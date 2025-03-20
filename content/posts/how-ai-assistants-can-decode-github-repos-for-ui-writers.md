---
title: "How AI Assistants Can Decode GitHub Repos for UI Writers"
date: 2025-01-02
categories: 
  - "general"
---

_This ongoing_ _Docker Labs GenAI series_ _explores the exciting space of AI developer tools. At Docker, we believe there is a vast scope to explore, openly and without the hype. We will share our explorations and collaborate with the developer community in real time. Although developers have adopted autocomplete tooling like GitHub Copilot and use chat, there is significant potential for AI tools to assist with more specific tasks and interfaces throughout the entire software lifecycle. Therefore, our exploration will be broad. We will be releasing software as open source so you can play, explore, and hack with us, too._

Can an AI-powered assistant understand a GitHub repo enough to answer questions for UI writers?

![2400x1260 docker labs genai](https://www.docker.com/wp-content/uploads/2024/06/2400x1260_docker-labs-genai-1110x583.png "- 2400x1260 docker labs genai")

Across many projects, user-facing content is rendered based on some sort of client-side code. Whether a website, a game, or a mobile app, it’s critical to nail the text copy displayed to the user.

So let’s take a sample question: **Do any open PRs in this project need to be reviewed for UI copy?** In other words, we want to scan a GitHub repo’s PRs and gain intelligence about the changes included.

Disclaimer: The best practice to accomplish this at a mature organization would be to implement Localization (i18n), which would facilitate centralized user-facing text. However, in a world of AI-powered tools, we believe our assistants will help minimize friction for all projects, not just ones that have adopted i18n.

So, let’s start off by seeing what options we already have.

The first instinct someone might have is to open the new copilot friend in the GitHub nav

<figure>

![genai series 13 f1](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f1.png "- genai series 13 f1")

<figcaption>

Figure 1: Type / to search.

</figcaption>

</figure>

We tried to get it to answer basic questions, first: “How many PR’s are open?”

<figure>

![genai series 13 f2](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f2.png "- genai series 13 f2")

<figcaption>

Figure 2: How many PR’s are there open? The answer doesn’t give a number.

</figcaption>

</figure>

Despite having access to the GitHub repo, the Copilot agent provides less helpful information than we might expect.

<figure>

![genai series 13 f3](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f3.png "- genai series 13 f3")

<figcaption>

Figure 3: Copilot is powered by AI, so mistakes are possible.

</figcaption>

</figure>

We don’t even get a number like we asked, despite GitHub surfacing that information on the repository’s main page. Following up our first query with the main query we want to ask effectively just gives us the same answer

<figure>

![genai series 13 f4](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f4.png "- genai series 13 f4")

<figcaption>

Figure 4: The third PR is filesharing: add some missing contexts.

</figcaption>

</figure>

And, after inspecting the third PR in the list, it doesn’t contain user-facing changes. One great indicator for this web project is the lack of any clientside code being modified. This was a backend change so we didn’t want to see this one.

<figure>

![genai series 13 f5](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f5.png "- genai series 13 f5")

<figcaption>

Figure 5: The PR doesn’t contain user-facing changes.

</figcaption>

</figure>

So let’s try to improve this:

First prompt file

```
---
functions:
  - name: bash
	description: Run a bash script in the utilities container.
	parameters:
  	  type: object
  	  properties:
    	    command:
      	      type: string
      	description: The command to send to bash
	container:
    	  image: wbitt/network-multitool  
    	  command:
      	    - "bash"
      	    - "-c"
      	    - "{{command|safe}}"
  - name: git
	description: Run a git command.
	parameters:
  	  type: object
  	  properties:
    	    command:
      	      type: string
      	description: The git command to run, excluding the `git` command itself
	container:
  	  image: alpine/git
  	  entrypoint:
    	    - "/bin/sh"
  	  command:
    	    - "-c"
    	    - "git --no-pager {{command|safe}}"
---

# prompt system

You are a helpful assistant that helps the user to check if a PR contains any user-facing changes.

You are given a container to run bash in with the following tools:

  curl, wget, jq
and default alpine linux tools too.

# prompt user
You are at $PWD of /project, which is a git repo.

Checkout branch `{{branch}}`.

Diff the changes and report any containing user facing changes
```

This prompt was promising, but it ended up with a few blocking flaws. The reason is that using `git` to compare files is quite tricky for an LLM.

- `git diff` uses a pager, and therefore needs the `--no-pager` arg to send `stdout` to the conversation.
- The total number of files affected via `git diff` can be quite large.
- Given each file, the raw diff output can be massive and difficult to parse.
- The important files changed in a PR might be buried with many extra files in the diff output.
- The container has many more tools than necessary, allowing the LLM to hallucinate.

The agent needs some understanding of the repo to determine the sorts of files that contain user-facing changes, and it needs to be capable of seeing just the important pieces of information.

Our next pass involves a few tweaks:

- Switch to `alpine git` image and a file writer as the only tools necessary.
- Use `–files-only` and `–no-pager` args.

````
# ROLE assistant

The following files are likely to contain user-facing changes as they mainly consist of UI components, hooks, and API functionalities.

```
file1.ts
fil2.tsx
file3.tsx
...
```
Remember that this isn't a guarantee of whether there are user-facing changes, but just an indication of where they might be if there are any.
````

Remember that this isn’t a guarantee of whether there are user-facing changes, but just an indication of where they might be if there are any.

Giving the agent the tool `run-javascript-sandbox` allowed our agent to write a script to save the output for later.

<figure>

![genai series 13 f6](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f6.png "- genai series 13 f6")

<figcaption>

Figure 6: Folder called user-changes with files.txt.

</figcaption>

</figure>

To check out the final prompt here, use our Gist.

## Expert knowledge

This is a great start; however, we now need to inspect the files themselves for user-facing changes. When we started this, we realized that user-facing changes could manifest in a diverse set of “diff”s so we needed to include expert knowledge. We synced up with Mark Higson, a staff SWE currently working on the frontend platform here at Docker. Mark was able to help provide some key advice for what “user-facing” changes look like in many repos at Docker, so I baked the tips into the prompt.

### Straightforward approaches

Looking for changes in text nodes found in a JSX tree is the easiest example.

### JSX node with interpolation

```
<div>{functionReturningString()}</div>
```

If the result is a string, the result is probably user-facing, but the components that create the string could be elsewhere, so look for:

### Nuanced indicators

- **Standard user-facing components.** Example: notifications. If a notification’s props change, we can likely infer that it is a user-facing change.
- **Constructors for commonly used components.** Example: errors. If an `Error()` is constructed with a different argument, we know that error could show up differently.

The key for UI reviewers is the overall amount of text is changed, rather than layout.

So, despite not being able to catch everything that could be rendered, we will be able to deliver value by focusing on these areas.

## Diffing the files

There are a few approaches to finding changes in the files. To start, we’ll use `git` again.

Before including expert advice, `git diff` was hallucinating changes that weren’t actually represented in the output. After the expert advice, we are seeing much more focused outputs from the LLM.

Our second prompt just needs to read the user-facing files that we already saved, and then compare them to main. After a bit of trial and error with git, we found that a combination of `--no-color` and `--minimal` was able to give the LLM enough context without consuming too many tokens for each change.

There’s a lot of buzz around generating and reading diffs with LLMs. The teams at Aider and Cursor are both focusing on fine-tuning LLM’s using speculative decoding:

- https://github.com/Aider-AI/aider/issues/625
- https://arxiv.org/abs/2302.01318

For our use cases, however, we found that the average “big” PR at Docker was not a problem for traditional LLMs when given good advice and the right tools.

So here’s the second prompt:

```
## User-facing content

The following pieces of code are what we want to focus on for user-facing content:

Text Nodes in any JSX or TSX files.

Text Nodes in any HTML files.

Standard user-facing components. Example: Definitions/Constructors for notifications, toasts, and errors.

# prompt user

Can you tell me if there are any user-facing changes in {{branch}}?
```

To check out the full prompt here, use our Gist.

**Output**

_Note: Some full paths, branches, and diffs are trimmed or replaced with placeholders for this article._

````
# ROLE assistant

```json
{
  "args": ["checkout", "branch"]
}
```

# ROLE tool (git)

Already on 'branch'
Your branch is up to date with 'origin/branch'.

# ROLE assistant

```json
{
  "path": "/thread/user-changes/files.txt"
}
```

*entering tool*

# ROLE tool (read-file)

path1
path2
path3
...rest of paths

# ROLE assistant

```json
{
  "args": ["diff", "main", "--no-color", "--minimal", path1 path2 path3...]
}
```

*entering tool*

# ROLE tool (git)

<Diff output truncated>
# ROLE assistant

The following user-facing changes were detected between the "main" branch and the "branch" branch:

1. In `File1.tsx` file, the text "Sign In" was modified to "Sign in".

2. In `File2.ts` file, titles "Databases & Storage", "Monitoring & Observability", "Developer Tools", "New Extensions", were changed to "Databases & storage", "Monitoring & observability", "Developer tools", "New extensions" respectively.

3. In `File3.rtl.test.tsx.snap` and `ImageHeader/index.tsx files`, the text "Compressed Size " was changed to "Compressed size".

4. In `File4.rtl.test.tsx` and `ImageDetail/index.tsx files`, a test-case previously checking for the existence of "Compressed Size" now looks for "Compressed size".

5. In `File5.tsx file`, the button text "Add Collaborator" has been changed to "Add collaborator".

6. In `route.tsx file`, the collaborators table title "Current Collaborators" was changed to "Current collaborators".

7. In `File6.tsx` file, the description was updated.

8. In `File7.tsx`, `RepositoryOverview.tsx`, `RepositoryReadMe.tsx`, `TagsAndScans.tsx` files, the description when there is no category or description was updated.

9. In `File8.rtl.test.tsx`, `DeleteRepo.tsx`, the text for confirming repo deletion and the delete button text was adjusted.

10. In `route.tsx file`, the filter label "Trusted Content" was changed to "Trusted content".

11. In `File9.tsx`, the text "Your Organizations" has been changed to "Your organizations".

12. In `Component/index.tsx`, the search group label "Trusted Content" has been changed to "Trusted content".

13. In `file10.test.ts` file, a test-case was updated to check for the new text "Sign in".
```
````

Overall, this was a positive result:

<figure>

![genai series 13 f7](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f7.png "- genai series 13 f7")

<figcaption>

Figure 7: Mark’s response “Damn, that’s nice output (assuming it’s accurate).”

</figcaption>

</figure>

Follow up:

<figure>

![genai series 13 f8](https://www.docker.com/wp-content/uploads/2024/11/genai-series-13_f8.png "- genai series 13 f8")

<figcaption>

Figure 8: Mark’s response, “I checked, it’s accurate.”

</figcaption>

</figure>

Another example from a different PR:

```
The changes in the `branch` branch compared to `main` include several modifications that affect user-facing content, particularly in JSX and TSX files. Here is a summary of the key changes:

...

These changes primarily involve simplification and refactoring of the existing UI logic, possibly to streamline the user interaction by removing complexities related to feature flags and reducing the use of modals or conditional rendering for specific purchasing flows.
```

### **Try it yourself**

Here is a markdown file that you can paste into VSCode to try these prompts on your own branch. In the last line, update `my-branch` to one of your local branches that you’d like to review: https://gist.github.com/ColinMcNeil/2e8f25e2d4092f3c7a0ce8992d2e197c#file-readme-md

## Next steps

This is already a promising flow. For example, a tech writer could clone the git repo and run this prompt to inspect a branch for user-facing changes. From here, we might extend the functionality:

- Allow user input for PR to review without knowing the branch or git needing to use `git`.
- Automatic `git clone & pull` with `auth`.
- Support for larger >15 files changed PR by allowing agents to automate their tasks.
- “Baking” the final flow into CI/CD so that it can automatically assign reviewers to relevant PRs.

If you’re interested in running this prompt on your own repo or just want to follow along with the code, watch our new public repo and reach out. We also appreciate your GitHub Stars.

Everything we’ve discussed in this blog post is available for you to try out on your own projects. 

For more on what we’re doing at Docker, subscribe to our newsletter.

## Learn more

- Subscribe to the Docker Newsletter. 
- Learn about accelerating AI development with the Docker AI Catalog.
- Read the Docker Labs GenAI series.
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, AI/ML, Docker Labs GenAI series, GenAI
