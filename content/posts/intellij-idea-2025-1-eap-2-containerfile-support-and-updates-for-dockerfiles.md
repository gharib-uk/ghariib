---
title: "IntelliJ IDEA 2025.1 EAP 2: Containerfile Support and Updates for Dockerfiles"
date: 2025-01-23
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

IntelliJ IDEA 2025.1 EAP 2 is out! With a focus on improving workflows for environments like Docker containers and other remote solutions, this build introduces updates that simplify setup and enhance productivity in these scenarios. You can now download this version from our website, update directly from within the IDE, use the free Toolbox App, \[…\]

IntelliJ IDEA 2025.1 EAP 2 is out!  
  
With a focus on improving workflows for environments like Docker containers and other remote solutions, this build introduces updates that simplify setup and enhance productivity in these scenarios.

You can now download this version from our website, update directly from within the IDE, use the free Toolbox App, or install it via snap packages for Ubuntu. 

Download IntelliJ IDEA 2025.1 EAP 2

Try the new updates delivered with the second EAP build for yourself! 

## Remote development environments 

### Containerfile support 

Container ecosystems have been evolving beyond Docker-centric workflows, with tools like Podman and Buildah favoring Containerfile as a neutral alternative. However, IDE support, which is typically tied to Dockerfile, often lagged behind. That created friction, forcing developers to either rename Containerfile to Dockerfile, lose Podman-specific best practices, or just plow through with basic text editing. 

Now, JetBrains IDEs come with built-in Containerfile recognition. This might seem like a relatively minor enhancement, but it really contributes to a smooth developer experience for anyone juggling Docker, Podman, and Buildah in the same environment.

It is no longer necessary to keep separate versions of your build files just so your IDE doesn’t treat them as plain text. And for new hires or open-source contributors using Podman straight from the get-go, it provides enhanced clarity.

Syntax highlighting, linting, and snippet suggestions are fully supported, reducing errors, speeding up debugging, and improving clarity – especially for new hires or contributors using Podman. Now, it doesn’t matter if you switch engines or if half the team uses Docker while the other half uses Podman – everyone can work with the same file, recognized by the same tools.

![image.png](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfOPB05M_qJxC8Zxv-0AwdRIRx1NQa_3u5oELJ4miKnEXWs7fs6Y2GGRBbLUQM7lOikq_RjgZ-eIrDWAD4PAAj8PP-XtouH4PGdVij4om6B5WaQezNYN6qjS0gy55KL9WbRkDTfdA?key=W3y1-U-bXo4mtQ_gY7TGf7nc)

### Support for lowercase instructions in Dockerfiles 

IntelliJ IDEA 2025.1 EAP 2 brings enhanced Dockerfile support, allowing you to write directives in lowercase in addition to the conventional uppercase. Previously, the IDE recognized commands like `FROM`, `RUN`, and `COPY` primarily as Dockerfile instructions. Now, you’re also free to use the lowercase `from`, `run`, and `copy`, too.

Although Docker itself is case-insensitive with regard to instructions, uppercase has historically been used to improve readability and to distinguish instructions from arguments. However, alternative casing styles might be preferred to accommodate specific commands, plugins, corporate standards, or personal preferences. With this update, you can adhere to your preferred conventions without risking missing highlights or encountering misleading warnings from the IDE.

![image.png](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZLAC84YXFVcLGtALThkrEo5omPUz_kfz8ijnR8UWo5ceyxQt80ijTLdb4UEXG5f9_wRrL22ZhB96Pra8OG5h0-AUNOic_ICLbSmLIzkrE7cdTKJ_CPPwRC-HOfwbkcSW_zSCd?key=W3y1-U-bXo4mtQ_gY7TGf7nc)

### New inspection for reliable `ENTRYPOINT` initialization with exec

We’ve introduced a new Dockerfile inspection that ensures your `ENTRYPOINT` is correctly initiated with `exec`. Using `exec` allows signals sent via `docker stop` to reach the main process directly, preventing lingering or improperly terminated processes. If you omit `exec`, your application may run as a child process and fail to receive signals like `SIGTERM`, making shutdown unreliable. This inspection highlights incorrect `ENTRYPOINT` usage and guides you toward best practices, helping you maintain cleaner Dockerfiles and more robust container lifecycles.

![image1.png](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfLwUkTZQmg3eBuqu7Ujbhjkzh9_-wxKxlLssfyum8ERUAG2G6cY-cIRwsSZY1bQG-PSfw0uAwGhykJD5-OQfC80uSMRfFpdCw995TfiZsBvErPWic5KUiq-dX8Dbzg9pB7fyu7?key=W3y1-U-bXo4mtQ_gY7TGf7nc)

These are the key updates for this week. For the complete list of changes, refer to the release notes.

We’d love for you to explore the new features and share your feedback. Let us know your thoughts and ideas in the comments below, or get in touch with us on X. If you encounter any issues, please report them through our issue tracker.

Happy developing!

Go to Source
