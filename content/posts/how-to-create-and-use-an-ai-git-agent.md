---
title: "How to Create and Use an AI Git Agent"
date: 2025-01-02
categories: 
  - "general"
---

_This ongoing_ _Docker Labs GenAI series_ _explores the exciting space of AI developer tools. At Docker, we believe there is a vast scope to explore, openly and without the hype. We will share our explorations and collaborate with the developer community in real time. Although developers have adopted autocomplete tooling like GitHub Copilot and use chat, there is significant potential for AI tools to assist with more specific tasks and interfaces throughout the entire software lifecycle. Therefore, our exploration will be broad. We will be releasing software as open source so you can play, explore, and hack with us, too._

In our past experiments, we started our work from the assumption that we had a project ready to work on. That means someone like a UI tech writer would need to understand Git operations in order to use the tools we built for them. Naturally, because we have been touching on Git so frequently, we wanted to try getting a Git agent started. Then, we want to use this Git agent to understand PR branches for a variety of user personas — without anyone needing to know the ins and outs of Git.

![2400x1260 docker labs genai](https://www.docker.com/wp-content/uploads/2024/06/2400x1260_docker-labs-genai-1110x583.png "- 2400x1260 docker labs genai")

## Git as an agent

We are exploring the idea that tools are agents. So, what would a Git agent do? 

Let’s tackle our UI use case prompt. 

Previously:

```
You are at $PWD of /project, which is a git repo.
Force checkout {{branch}}
Run a three-dot diff of the files changed in {{branch}} compared to main using --name-only.
```

A drawback that isn’t shown here, is that there is no authentication. So, if you haven’t fetched that branch or pulled commits already, this prompt at best will be unreliable and more than likely will fail (Figure 1):

<figure>

![Screenshot of Logs showing failure to authenticate.](https://www.docker.com/wp-content/uploads/2024/12/F1-no-auth-1110x221.png "Screenshot of Logs showing failure to authenticate. - F1 no auth")

<figcaption>

**Figure 1:** No authentication occurs.

</figcaption>

</figure>

Now:

```
You are a helpful assistant that checks a PR for user-facing changes.
1. Fetch everything and get on latest main.
2. Checkout the PR branch and pull latest.
3. Run a three-dot git diff against main for just files. Write the output to /thread/diff.txt.
```

This time around, you can see that we are being less explicit about the Git operations, we have the ability to export outputs to the conversation thread and, most importantly, we have authentication with a new prompt!

## Preparing GitHub authentication

**Note:** These prompts should be easily adaptable to other Git providers, but we use GitHub at Docker.

Before we can do anything with GitHub, we have to authenticate. There are several ways to do this, but for this post we’ll focus on SSH-based auth rather than using HTTPS through the CLI. Without getting too deep into the Git world, we will be authenticating with keys on our machine that are associated with our account. These keys and configurations are commonly located at `~/.ssh` on Linux/Mac. Furthermore, users commonly maintain Git config at `~/.gitconfig`. 

The `.gitconfig` file is particularly useful because it lets us specify carriage return rules — something that can easily cause Git to fail when running in a Linux container. We will also need to modify our SSH config to remove UseKeychain. We found these changes are enough to authenticate using SSH in Alpine/Git. But we, of course, don’t want to modify any host configuration.

We came up with a fairly simple flow that lets us prepare to use Git in a container without messing with any host SSH configs.

1. Readonly mounts: Git config and SSH keys are stored on specific folders on the host machine. We need to mount those in.  
    a. Mount `~/.ssh` into a container as `/root/.ssh-base readonly`.  
    b. Mount `~/.gitconfig` into the same container as `/root/.gitconfig`.
2. Copy `/root/.ssh-base` to `/root/.ssh` and make the new file readwrite.
3. Make necessary changes to config.
4. For the LLM, we also need it to verify the config is in the thread and the changes were made to it. In the event that it fails to make the right changes, the LLM can self-correct.
5. Copy the `.ssh` directory and `.gitconfig` to `/thread`.

All of this is baked in a prompt you can find and run on GitHub. Simply replace `<username>` with your system username (Figure 2).

<figure>

![Screenshot of mounts, showing storage of Git config and SSH keys.](https://www.docker.com/wp-content/uploads/2024/12/F2-mounts-1110x195.png "Screenshot of mounts, showing storage of Git config and SSH keys. - F2 mounts")

<figcaption>

**Figure 2:** Readonly mounts.

</figcaption>

</figure>

If you’re using the default behavior in our extension for VSCode, then the `/thread/` volume is ephemeral, so you would need to make sure your agent runs it before any Git tools. However, if you use the `Set prompt thread ID` command in VSCode, you can keep the `/thread` volume around under your own tag (Figure 3). 

Tagging the thread persists everything after the prompt completes, so you would only need to run the prompt once before going on to develop your Git tools.

<figure>

![Screenshot of setting the prompt thread ID.
](https://www.docker.com/wp-content/uploads/2024/12/F3-set-thread-1110x101.png "Screenshot of setting the prompt thread ID. - F3 set thread")

<figcaption>

**Figure 3:** Set the prompt thread ID.

</figcaption>

</figure>

If you opt to persist the thread, then this prompt would only need to be run once and the Git credentials will stick around (Figure 4).

<figure>

![Screenshot showing the stored configuration info.](https://www.docker.com/wp-content/uploads/2024/12/F4-Git-credentials-1110x316.png "Screenshot showing the stored configuration info. - F4 Git credentials")

<figcaption>

**Figure 4:** Stored credential data.

</figcaption>

</figure>

Now that we have our credentials ready, we need our Git tool to pick up on them.

```
container:
   image: alpine/git
   entrypoint:
     - /bin/sh
     - -c
   command:
     - "cp -r /thread/.ssh /root/.ssh && cp -r /thread/.gitconfig /root/.gitconfig && git --no-pager {{args|safe}}" 
```

Our container definition copies the SSH credentials and `.gitconfig` files to the correct directory before running Git commands.

````
Unpacking objects:  90% (9/10)
Unpacking objects: 100% (10/10)
Unpacking objects: 100% (10/10), 4.14 KiB | 141.00 KiB/s, done.
From github.com:docker/repo
   abcd123..abcd123  main           -> origin/main
 * [new tag]           branch/v2982.0.0 -> branch/v2982.0.0
# ROLE assistant
I have fetched all origin branches. The latest updates have been applied to your local repository, and any new tags have been downloaded.
```
Now, we can see our Git tool is authenticated as if it were us.
Running the agent now, it can:
## ROLE assistant
I have run the git fetch --all command, which retrieves updates from all remotes. If you need further assistance with your Git repository, feel free to ask!
```

# ROLE assistant
```json
{"request":"checkout main"}
```

## ROLE assistant
```json
{"args":"checkout main"}
```

## ROLE assistant
You are already on the 'main' branch, and it is up to date with 'origin/main'.
```

# ROLE assistant
```json
{"request":"checkout the branch branch and pull latest changes."}
```

...
## ROLE assistant
The branch branch has been successfully checked out, and it's already up to date with the latest changes.
...

# ROLE assistant
```json
{"request":"Run a three-dot git diff against main and write the output for just files to /thread/diff.txt."}
```

...

## ROLE assistant
```json
{"args":"diff main... --name-only","outfile":"/thread/diff.txt"}
```

*entering tool*

## ROLE tool (git)
file1.tsx
file2.tsx
...
## ROLE assistant

The three-dot Git diff against the main branch has been executed, and the output has been saved to /thread/diff.txt. This includes the names of files that have differences.
``
````

With the new Git agent, we have enabled our assistant to fetch new information from upstream, switch branches, and pull in new code to review. If we provide our agent with credentials that are limited to read-only access, this agent could be used by someone that does not have a local git install.

For more on what we’re doing at Docker, subscribe to our newsletter.

## Learn more

- Subscribe to the Docker Newsletter. 
- Learn about accelerating AI development with the Docker AI Catalog.
- Read the Docker Labs GenAI series.
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, AI/ML, Docker Labs GenAI series, GenAI Stack
