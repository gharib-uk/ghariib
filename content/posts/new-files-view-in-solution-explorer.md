---
title: "New Files View in Solution Explorer"
date: 2025-01-19
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

In modern development, repositories are much more than just .NET solutions. From TypeScript end-to-end tests and Docker compose files to global.json and Playwright configs – your codebase contains numerous essential files that live outside of your C# projects.While these files are fully supported in Rider, they can be hard to find when viewing your solution \[…\]

In modern development, repositories are much more than just .NET solutions. From TypeScript end-to-end tests and Docker compose files to global.json and Playwright configs – your codebase contains numerous essential files that live outside of your C# projects.While these files are fully supported in Rider, they can be hard to find when viewing your solution structure.

This is why we’re introducing an enhanced _Files_ view in the EAP 1 release for Rider 2025.1. 

Download Rider 2025.1 EAP 1

## What’s new?

The reimagined _Files_ view brings three key improvements:

First, we’ve elevated the _Files_ tab to be a true companion to the _Solution_ view. You’ll find them side-by-side in one tool window, making it easier to switch contexts. Need to check your e2e tests in the tests folder? Looking for that CI configuration in `.github`? The _Files_ view puts everything at your fingertips.

Second, we’ve shifted the view’s starting point to your repository root instead of the directory that contains the current solution file. This can be especially valuable for full-stack projects, where you might have:

- A backend folder with multiple .NET solutions

- A frontend directory containing your React, Angular, or Vue code

- Docker configurations and Kubernetes manifests

- Database migrations and scripts

- API documentation and Swagger files

Finally, we’ve added intelligent folder highlighting to speed up navigation:

- Development folders like `src` and `tests` stand out for quick access.

- System folders like `.git` fade into the background.

- Special directories like `.github` get distinct styling.

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/RD-251-Repo-View-FULL.jpg)

## How to try it 

To make sure the _Files_ tab is displayed by default, go to Rider’s _Settings/Preferences | Advanced Settings | Project View_ and check the _Enable Repository view in Explorer toolwindow_ box.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfI00tUweV3rPjyL8GW4auWvk-6RCML9tdZqWUmxI_Vqm6J9noIo3hC3QXjdrFKOFVFSX4SsmltgueWEULvmXUbnNY40yDFwM5zTMwjRpTWAPxOilqGYSkBHvc4Rag8AbCGP_OGYQ?key=YuHtOg_vIk2UBEjYskzBl4L9)

Then restart Rider, and you’ll see your full-stack repository in a whole new light!

## **Help us shape this feature**

This is an early implementation, and your experience matters. We’d love to hear how the new _Files_ view fits into your workflow. Share your thoughts in the comments below – what works, what doesn’t, and what you’d like to see next.

Download Rider 2025.1 EAP 1

Go to Source
