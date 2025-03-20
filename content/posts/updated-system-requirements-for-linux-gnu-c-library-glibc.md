---
title: "Updated System Requirements for  Linux GNU C Library (glibc)"
date: 2025-01-04
categories: 
  - "general"
  - "linux"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "software"
---

Starting with v2025.1, IntelliJ-based IDEs will require glibc 2.28 or higher on Linux x64 systems. This change is being made to address potential security vulnerabilities associated with building and running our software on outdated systems and to ensure our products evolve with modern frameworks and technologies. By enforcing a more recent glibc version, we aim to \[…\]

Starting with v2025.1, IntelliJ-based IDEs will require glibc 2.28 or higher on Linux x64 systems. This change is being made to address potential security vulnerabilities associated with building and running our software on outdated systems and to ensure our products evolve with modern frameworks and technologies. By enforcing a more recent glibc version, we aim to provide a more secure and stable development environment.

## Why this change is necessary

IntelliJ-based IDEs run on our custom JetBrains Runtime (JBR). However, the Linux system used to build JBR has reached end-of-life, meaning the operating system will no longer receive critical security updates, posing a risk to the integrity and security of our software.  
  
In order to continue delivering high-quality products, we must transition to a more recent and supported Linux x64 build platform. Since the new build platform has a more recent version of glibc, the system requirements for IntelliJ-based IDEs on Linux will change accordingly.

## How this affects you

If you are running your IDE on an x64 Linux distribution with glibc 2.28 or newer, no action is required.

If you are running your IDE on a system with a version of glibc lower than 2.28, whether it’s a desktop or a remote development environment, the system must be updated to glibc 2.28 or newer to run v2025.1. 

If you prefer to keep using your current IDE version (prior to v2025.1), there is no need to update your glibc.

## How to check if your system meets this requirement

To check your current glibc version, run the ldd –version command in the terminal.

## Update details

glibc, the GNU C library, is a prevalent implementation of the essential library that allows applications to interact with the underlying Linux kernel. Many Linux distributions are based on glibc, with several utilizing an alternative implementation, musl libc.

All versions of glibc are backward-compatible, meaning that it is possible to use a more recent version than the one mandated by the system requirements. This minimal version is determined by the software itself and the environment in which that software was built. For example, software built on a system with glibc 2.29 is guaranteed to run on a system with glibc 2.35, but it may break on a system with version 2.28.

The Linux system where JetBrains Runtime used to be built resulted in a dependency on glibc 2.17. That build system recently reached its end-of-life, meaning that even important security patches will not be delivered for it. The new build platform has glibc version 2.28 and will be supported longer, pushing future requirement bumps further away.

To continue delivering secure updates to existing product versions, such as, for example, v2023.2, our team has switched the build platform for those versions to a commercially supported Linux version. That operating system will be maintained until the end of 2025, thus providing for a smooth transition to a more modern glibc.

## References

- glibc – the GNU C Library

- glibc releases

- IntelliJ IDEA system requirements

- CentOS 7 EOL announcement 

- Musl libc – an alternative to glibc

- JetBrains Runtime releases

## Feedback 

We recognize that this change is significant, and we genuinely value your feedback. Your insights and suggestions are important to us as we strive to make this transition as seamless and beneficial as possible. Please feel free to share your thoughts and comments in the comment section below or in our issue tracker (for instance, in JBR-7665).

Go to Source
