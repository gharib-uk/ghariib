---
title: "IntelliJ IDEA 2024.3.2 Is Out"
date: 2025-01-17
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

We’ve just released the next minor update for IntelliJ IDEA 2024.3 – v2024.3.2. You can update to this version from inside the IDE, via the Toolbox App, or by using snaps for Ubuntu. You can also download it from our website. This release includes the following improvements: To find out more about the resolved issues, \[…\]

We’ve just released the next minor update for IntelliJ IDEA 2024.3 – v2024.3.2.

You can update to this version from inside the IDE, via the Toolbox App, or by using snaps for Ubuntu. You can also download it from our website.

This release includes the following improvements:

- Using certain third-party plugins no longer causes the font settings to reset to default. \[IJPL-157487\]

- It’s again possible to compile Java 7 projects with Javac version 7. \[IDEA-361854\]

- When establishing an SSH connection with a specified deployment path, the IDE no longer mistakenly sets the working directory to the host’s root directory. \[IJPL-171619\]

- The IDE no longer incorrectly reports valid JPQL syntax as errors. \[IDEA-360902, IDEA-364513, IDEA-244155\]

- JPA Buddy’s _New Association Attribute_ dialog option for `@OneToMany` once again works as expected. \[IDEA-359970\]

- When the IDE window size is maximized on macOS Sequoia, it stays maximized upon IDE reopening. \[IJPL-164502\]

- The IDE correctly displays the GitHub pull request timeline with mannequins and deleted participants. \[IJPL-79734\]

- The _Collection Presentation_ window properly displays nested collections in the debugger. \[IDEA-363360\]

- Each JetBrains IDE binary on macOS is now assigned a specific UUID, ensuring that different applications are no longer treated as single entities in the system settings. This helps prevent issues such as JetBrains Gateway being unable to connect to remote hosts. \[IJPL-172978, IJPL-173908\]

To find out more about the resolved issues, please refer to the release notes. 

If you encounter any issues or would like to make a suggestion or a feature request, please submit them to our issue tracker.

Happy developing!

Go to Source
