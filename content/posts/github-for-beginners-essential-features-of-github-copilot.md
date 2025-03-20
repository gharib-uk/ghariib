---
title: "GitHub for Beginners: Essential features of GitHub Copilot"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Get the most out of Copilot with code completion, inline chat, slash commands, Copilot code review, and more.

The post GitHub for Beginners: Essential features of GitHub Copilot appeared first on The GitHub Blog.

Welcome back to our second GitHub for Beginners series, where we are diving into the world of GitHub Copilot. In our previous episode, we introduced you to GitHub Copilot and gave you some guidance on getting started. Hopefully you’ve had a chance to give it a try! Today we’re going to be looking at some of the essential features of Copilot, and provide you with tips on how to make the most out of your AI coding assistant.

<iframe loading="lazy" class="position-absolute top-0 left-0 width-full height-full" src="https://www.youtube.com/embed/b5xcWdzAB5c?feature=oembed" title="YouTube video player" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen frameborder="0"></iframe>

For the demos in this series, we’re using GitHub Copilot in Visual Studio Code.

Copilot is available in other IDEs, but the available functionality may vary depending on your environment.

## What is GitHub Copilot?

First, let’s go over a quick refresher. GitHub Copilot is an AI pair programmer that helps you write code. It is a generative AI, which means that it is capable of creating new content based on what it has learned. This also means that if you are following along through any demos, Copilot might offer different solutions in response to prompts. This is expected.

To use Copilot, you must have a GitHub account and a Copilot license. You can get started with a free license by visiting this link.

Once you have access, you need to install the extension in your editor, authenticate it, and you’ll be good to go. For more introductory details, check out our first blog in the series.

Now let’s take a look at some of the most essential features.

## Code completion

Let’s say you wanted to create a rock, paper, scissors game in Python. First, create a new file called `rock_paper_scissor.py`. At the top of the file, add the following comment:

```plaintext
Create a Rock Paper Scissors game where the player inputs their choice and plays against a computer that randomly selects its move, with the game showing who won each round. Add a score counter that tracks player and computer wins, and allow the game to continue until the player types ‘quit’.
```

Press the Enter key a couple of times. You will notice some gray, _italicized_ text appearing after your cursor. This is called **ghost text**, and is the suggestion provided from GitHub Copilot. You can press Tab to accept the suggestion. Continue accepting lines until the suggestion defines a function (that is, starts with the `def` keyword).

https://github.blog/wp-content/uploads/2025/03/01-ghost-text.mp4#t=0.001

When you press the Enter key after the function definition, you’ll see ghost text defining the main function. Hover over the ghost text with your cursor to see different menu options available to you. You can accept a single word, tab through a few completions, or accept the current suggestion. You can also click on the three dots and select “Open Completions Panel.”

https://github.blog/wp-content/uploads/2025/03/02-completions-panel.mp4#t=0.001

The Completions panel shows a list of the many ways this code could be implemented. Scroll through the options and choose the suggestion that you want to use. Once you’ve decided on a suggestion, click the associated “Accept suggestion” button. This populates the code into your file.

Finally, add a line to invoke the function. Remember that because Copilot is a generative AI, it will not always provide the same suggestions, even with the same prompt!

After adding the code, open your terminal and enter `python rock_paper_scissor.py` to play the game. Try it out as long as you like and enter `quit` when you’ve decided you’re done.

![Screenshot of the output of rock paper scissors.py in the terminal](https://github.blog/wp-content/uploads/2025/03/03-game-terminal.png?w=614&resize=614%2C281)

Congratulations! You’ve just made a game with GitHub Copilot!

## Inline chat

Now that we have the base for our game, we want to improve it, and we’re going to use the inline chat feature of GitHub Copilot to accomplish this. In our demo, we had the following line:

```plaintext
player_choice = input('Rock, Paper, Scissors or Quit: ').lower()
```

However, we want players to have a little more flexibility. By highlighting this line of code and pressing either Ctrl + I or Command + I, we open up Copilot Inline Chat where we can enter a prompt. Let’s say we give Copilot this prompt:

```plaintext
allow the user to enter r for rock, p for paper, and s for scissors
```

Before sending this prompt to Copilot, notice the dropdown menu at the end of the prompt window. This dropdown enables you to choose which model you’d like to use to respond to the prompt. Copilot Chat uses different models, and new ones are being continuously added.

![multimodal in inline chat](https://github.blog/wp-content/uploads/2025/03/04-multi-model.png?w=1024&resize=1024%2C573)

After selecting your model, press enter, and watch as Copilot updates the code. Once the code is updated, you have the option to accept or discard the suggested changes. If you accept the changes, try running the game again in your terminal, and observe that now you can use single letters to make your choice.

The inline chat feature is really good at helping you apply quick fixes to your code. If you want to work at a deeper level, you can use the full Copilot Chat experience for that.

## Copilot Chat

GitHub Copilot Chat can help you with code explanations, building out full minimum viable products (MVPs), writing tests, fixing errors in your terminals, and much more. To showcase the capabilities of Copilot Chat, we’re going to use it to create a graphical user interface (GUI) for our rock, paper, scissors game.

First, click the chat icon in the left-hand bar of VS Code to open up the chat window. Enter the following prompt into the box provided:

```plaintext
Create a simple GUI using a library like Tkinter for the game
```

Note that as long as you keep your `rock_paper_scissor.py` file open, it will be listed at the bottom of the chat panel as the current file. This is known as the context, and it lets Copilot Chat know which game you are talking about. You could disable this by clicking the Close File icon shown after “Current file”. However, in this case, you want to make sure that Copilot Chat has the context of the correct file. The best practice is to let Copilot have the context of your currently-open file to generate relevant code.

![Screenshot of the chat window with an arrow pointing to the current file](https://github.blog/wp-content/uploads/2025/03/05-curr-file.png?w=420&resize=420%2C305)

Just like with the inline chat prompt window, you can choose the model you want to use to respond to your prompt. Let’s use the Claude 3.5 Sonnet model and send the request.

Copilot will respond in the chat window, and you will notice several things once it is complete. First, it identifies that it’s using the one file as a reference. Then it provides a plan of action and the code block. Finally, it provides the code citations that you can investigate if you choose to with the “View matches” link.

https://github.blog/wp-content/uploads/2025/03/06\_response.mp4#t=0.001

In the Chat panel, you have a few options available. Going from left to right, here are what the different icons do at the bottom of the suggestion:

- Have the code read aloud to you
- Retry the same prompt to get a different response
- Upvote the provided code
- Downvote the provided code
- Edit the suggestion by using Copilot

https://github.blog/wp-content/uploads/2025/03/07-buttons.mp4#t=0.001

Scroll up and examine the provided code block. When you hover over it with your cursor, three icons appear in the top-right corner. Going from left to right, this is what these buttons do:

- **Apply in Editor**: Apply changes quickly in your code file with a single click—no need for copying and pasting!
- **Insert At Cursor**: Insert the code block into your current cursor position in the file.
- **Copy**: Copy the generated code to your clipboard, so you can paste it in at your leisure.
- **More**: Open up a dropdown menu with options for “Insert into Terminal” and “Insert into New File”. This will paste the code into the indicated locations.

![Screenshot with the four buttons described above circled](https://github.blog/wp-content/uploads/2025/03/08-editor-buttons.png?w=1024&resize=1024%2C573)

For now, click the Apply in Editor button to update the code. This updates the entire file. At the top of the file, you see options to accept the changes, discard them, and show or hide them. Click Accept Changes to have the editor finalize the changes to your file.

Look at the changes and observe how your text-based game has now been changed to run with a GUI. Try running the code to see the changes in action!

## Slash commands

Slash commands are shortcuts to common prompts that we’ve found to be particularly helpful in day-to-day usage. You can use them in a Copilot Chat window to get explanations of code, fix code, create a new notebook, and more.

In your Copilot Chat window, type forward slash (/). As soon as you do, Copilot provides a list of possible commands that you can use. For example, you could use the `/explain` command to have Copilot provide a full explanation of the code in your current file. You could also use `/help` to get a list of available slash commands, as well as explanations of what they do. So if you’re ever stuck, just remember `/help`.

![Screenshot showing the list of commands that comes up when a user types /help.](https://github.blog/wp-content/uploads/2025/03/09-slash-commands.png?w=887&resize=887%2C388)

If you’d like to see some of the more common slash commands without opening your editor, refer to our Copilot Chat cheat sheet.

## Adding context

As we’ve already seen, context helps Copilot Chat provide answers that are targeted toward your specific goals. But what if you want to provide a context beyond your currently open file?

You can use variables in the chat interface to provide additional context to help Copilot understand your requests. For example, you could use the `#file` variable to specify a specific file or files that Copilot should use when it tries to resolve your prompt. Or you could use `#codebase` to search through your codebase.

https://github.blog/wp-content/uploads/2025/03/10\_file\_codebase.mp4#t=0.001

To see a list of chat variables, check out or Copilot Chat cheat sheet documentation.

## Chat participants

Think of chat participants as experts with certain specialties. You can invoke a chat participant by typing @ in the chat window, followed by the name of the participant you want to use. For example, `@workspace` invokes a participant who has knowledge about your entire workspace. You could use this to determine where a function is defined in your project, or if you wanted to generate a detailed README of your project.

Another frequently used participant is `@terminal`, who has context of all of the information in the terminal window integrated into VS Code. Perhaps the most common participant is `@github` who has knowledge of your repository, pull requests, issues, and can search the web.

![Screenshot showing some of the available chat participants](https://github.blog/wp-content/uploads/2025/03/11_participants.png?w=1024&resize=1024%2C452)

Our Copilot Chat cheat sheet also lists some of the most common chat participants that folks use when interacting with Copilot Chat.

## Copilot Edits

Copilot Edits is one of the most recent and exciting features added to GitHub Copilot. With Copilot Edits, you can start a code session quickly to iterate on code changes using natural language. It allows you to edit multiple files at the same time, and combines the conversation flow of Copilot Chat with the fast feedback of inline chat together in one experience.

Let’s use Copilot Edits to add a detailed scoreboard to the game we already have. To get started, click the GitHub Copilot button in VS Code to pull up the Copilot menu. Select “Open Copilot Edits”. In the box provided, enter the following prompt.

```plaintext
Add a detailed scoreboard to track and display the number of ties, player wins, and computer wins for each round.
```

![Screenshot with an arrow pointing the Copilot icon and displaying the dropdown menu that appears when you click it.](https://github.blog/wp-content/uploads/2025/03/12-copilot-edits.png?w=877&resize=877%2C394)

Choose the model you want to use (we are continuing to use Claude 3.5 Sonnet), and then send the prompt. Once you do, you’ll see responses being streamed in. For now, just click “Accept” and run the game. See the GUI has been updated to keep track of stats for the game as you play.

Remember to review all suggestions from any AI developer tool you use

As with any generative AI, you should always carefully review any suggestions it makes before accepting them. There’s a reason we refer to GitHub Copilot as an assistant. It is meant to augment your abilities, not serve as a replacement for your skills.

This example only has a single file in the project. Copilot Edits becomes even more powerful when you use it to edit multiple files at the same time. We’ll demonstrate that in another episode.

## Commit messages

Now that we’ve finished our game, it’s time to send it up to GitHub, so that the team can review it. In VS Code, click the source control button. Stage your changes and then click the button on the right-hand side of the commit message box that looks like a set of sparkles. This button uses GitHub Copilot to generate a commit message.

https://github.blog/wp-content/uploads/2025/03/13\_commits.mp4#t=0.001

## Copilot code review

You can take this a step further and have Copilot review your code before sending it to the rest of the team. To do this, highlight a section of code in your editor. A sparkle icon will appear at the top-left of the highlighted section. Click this icon and select the “Review using Copilot” option.

![Select the 'Review using Copilot' option. ](https://github.blog/wp-content/uploads/2025/03/14-copilot-review.png?w=844&resize=844%2C313)

Copilot provides you with an initial review of your code before you push it and ask for a final review from your team. You can also get this Copilot review through github.com, if that’s your preference.

## JetBrains IDE features

Not all IDEs are identical when it comes to GitHub Copilot, and different IDEs have different functionalities available. If you are using JetBrains IDEs, you have access to Copilot Code Complete and Copilot Chat.

## Your next steps

We just went over some of the most essential features of GitHub Copilot that will help you make the most of our AI pair programmer. As you can see, it’s a very powerful coding assistant that does more than just code completion.

- Don’t forget that you can use GitHub Copilot for free.
- Make sure to check out our best practices for using GitHub Copilot.
- If you have any questions, pop them in the GitHub Community thread and we’ll be sure to respond.
- Join us for the next part of the series where we’ll walk through prompt engineering with GitHub Copilot.

Happy coding!

**Want to try out some of the features for yourself?**  
Give GitHub Copilot a try!

The post GitHub for Beginners: Essential features of GitHub Copilot appeared first on The GitHub Blog.

Go to Source
