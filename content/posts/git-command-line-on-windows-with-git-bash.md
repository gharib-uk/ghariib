---
title: "<div>Git command line on Windows with Git Bash</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Git commands allow developers to manage different versions of code and collaborate as a team. If you're in a Windows environment, you may have heard of Git Bash, a Bash terminal emulator that includes a Windows-friendly version of Git. Discover everything you need to know about installing Git Bash in this guide.

## How does Git Bash work?

Git Bash is an application that you can install on Windows operating systems using Git for Windows. This application acts as an emulator to use the Git version control tool on a Bash command terminal.

Bash is an acronym for Bourne Again SHell. SHell refers to the command terminal application of an operating system (OS). Bourne Again SHell is actually an upgraded version of Bourne SHell (also referred to as shell sh), the command line interface for UNIX developed by Stephen Bourne in 1977.

Bash is the default shell for Linux and MacOS operating systems. With Git Bash, Windows users can install Bash, run Bash commands and use Git commands.

## How to install Git Bash

To download Git Bash, it is necessary to install Git for Windows. To do this, go to the official Git for Windows website and click "Download" to install the full Git package. When the download is complete, open the .exe file and begin the installation.

To install Git Bash on Windows, please follow these step-by-step instructions:

1. Open the .exe file and click **Next**. Select the appropriate folder for the installation.
2. Accept the terms of use and click **Next** to start the installation.
3. In this step, select the components to install. The pre-selected settings are relevant, but you can change them according to your preferences. Click **Next** again.
4. Then, choose the editor you prefer to use with Git. The tool recognizes editors already installed on your computer.
5. A window is displayed with three settings of the PATH environment. Depending on your needs, choose whether Git should only be used by Git Bash or if you want to use it from other third-party software.
6. Finally, keep the default settings by clicking **Next** and install Git Bash by clicking **Install**.

## What are Bash commands?

First of all, the `pwd` (Print Working Directory) command allows you to view the absolute path. This means that it displays the path of the folder we are in at the time of typing the command.  
**Remember:** When you open the Git Bash terminal, you are in a folder on your computer. Usually, this is the folder with your username.

The `ls` command gives access to the list of files present in the current folder. You can also add options to the `ls` command with a dash `-`. For example, the `-l` option after `ls` lists the contents of a folder with more information about each file.

Bash also has a `cd` (Change Directory) command to move around your computer. To indicate the directory you want to go to, please specify the relative or absolute path after `cd`. The relative path is the location relative to the current directory while the absolute path is its location relative to the root folder.

## How to use Git Bash with GitLab

Using Git Bash with GitLab is like using the terminal emulator with another source code management platform. In order to push and retrieve your changes from GitLab, add the URL of your GitLab remote repository with the command: `git remote add origin <repository_url>`.

If your project is private, Git Bash asks you to authenticate yourself. Enter your credentials when the terminal requests your username and password. If you're having trouble logging in, check your authorization settings directly in GitLab.

Then use the basic Git commands like `git clone`, `git commit`, `git push`, `git branch`, as well as `git checkout`, to name a few. To learn more, visit our Git Cheat Sheet.

## Git Bash FAQ

**Are Git Bash and GitLab compatible?**

Yes. Using Git Bash with GitLab is similar to working with another source code management platform. Be sure to set up GitLab as a remote repository and authenticate yourself during the initial setup.

**Why use Git Bash?**

Git Bash acts as a terminal emulator to use the Git and Bash commands in a Windows environment.

**What's the point of a shell?**

Using a shell allows you to automate tasks through scripts, effectively control your computer and benefit from direct access to system functions.

## Read more

- What is Git version control?
- What's new in Git 2.47.0?
- Git pull vs. git fetch: What's the difference?

Git commands allow developers to manage different versions of code and collaborate as a team. If you're in a Windows environment, you may have heard of Git Bash, a Bash terminal emulator that includes a Windows-friendly version of Git. Discover everything you need to know about installing Git Bash in this guide.

## How does Git Bash work?

Git Bash is an application that you can install on Windows operating systems using Git for Windows. This application acts as an emulator to use the Git version control tool on a Bash command terminal.

Bash is an acronym for Bourne Again SHell. SHell refers to the command terminal application of an operating system (OS). Bourne Again SHell is actually an upgraded version of Bourne SHell (also referred to as shell sh), the command line interface for UNIX developed by Stephen Bourne in 1977.

Bash is the default shell for Linux and MacOS operating systems. With Git Bash, Windows users can install Bash, run Bash commands and use Git commands.

## How to install Git Bash

To download Git Bash, it is necessary to install Git for Windows. To do this, go to the official Git for Windows website and click "Download" to install the full Git package. When the download is complete, open the .exe file and begin the installation.

To install Git Bash on Windows, please follow these step-by-step instructions:

1. Open the .exe file and click **Next**. Select the appropriate folder for the installation.
2. Accept the terms of use and click **Next** to start the installation.
3. In this step, select the components to install. The pre-selected settings are relevant, but you can change them according to your preferences. Click **Next** again.
4. Then, choose the editor you prefer to use with Git. The tool recognizes editors already installed on your computer.
5. A window is displayed with three settings of the PATH environment. Depending on your needs, choose whether Git should only be used by Git Bash or if you want to use it from other third-party software.
6. Finally, keep the default settings by clicking **Next** and install Git Bash by clicking **Install**.

## What are Bash commands?

First of all, the `pwd` (Print Working Directory) command allows you to view the absolute path. This means that it displays the path of the folder we are in at the time of typing the command.  
**Remember:** When you open the Git Bash terminal, you are in a folder on your computer. Usually, this is the folder with your username.

The `ls` command gives access to the list of files present in the current folder. You can also add options to the `ls` command with a dash `-`. For example, the `-l` option after `ls` lists the contents of a folder with more information about each file.

Bash also has a `cd` (Change Directory) command to move around your computer. To indicate the directory you want to go to, please specify the relative or absolute path after `cd`. The relative path is the location relative to the current directory while the absolute path is its location relative to the root folder.

## How to use Git Bash with GitLab

Using Git Bash with GitLab is like using the terminal emulator with another source code management platform. In order to push and retrieve your changes from GitLab, add the URL of your GitLab remote repository with the command: `git remote add origin <repository_url>`.

If your project is private, Git Bash asks you to authenticate yourself. Enter your credentials when the terminal requests your username and password. If you're having trouble logging in, check your authorization settings directly in GitLab.

Then use the basic Git commands like `git clone`, `git commit`, `git push`, `git branch`, as well as `git checkout`, to name a few. To learn more, visit our Git Cheat Sheet.

## Git Bash FAQ

**Are Git Bash and GitLab compatible?**

Yes. Using Git Bash with GitLab is similar to working with another source code management platform. Be sure to set up GitLab as a remote repository and authenticate yourself during the initial setup.

**Why use Git Bash?**

Git Bash acts as a terminal emulator to use the Git and Bash commands in a Windows environment.

**What's the point of a shell?**

Using a shell allows you to automate tasks through scripts, effectively control your computer and benefit from direct access to system functions.

## Read more

- What is Git version control?
- What's new in Git 2.47.0?
- Git pull vs. git fetch: What's the difference?

Go to Source
