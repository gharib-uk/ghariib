---
title: "Kaboxer - Kali Applications Boxer"
date: Tue, 25 May 2021 00:00:00 +0000
draft: false
type: posts
---
# Kaboxer - Kali Applications Boxer

https://www.kali.org/blog/introducing-kaboxer/images/kaboxer-banner-v1.jpg



On and off for the last 18 months we have been working on Kaboxer, and just before Kali 2021.1, it is ready to say &ldquo;Hello World&rdquo; (then it will start shipping you applications). TL;DR - What is this? What is the name about, Kaboxer? Kali Applications Boxer What does that mean? Apps in

On and off for the last 18 months we have been working on **Kaboxer**, and just before Kali 2021.1, it is ready to say “Hello World” _(then it will start shipping you applications)_.

TL;DR - What is this?
---------------------

**What is the name about, Kaboxer?** Kali Applications Boxer

**What does that mean?** Apps in containers, for packages (a way forward for applications that are hard to package properly). But instead of being stand alone containers, they are integrated into the standard Kali package management systems and can be installed/removed through standard apt commands.

**Okay. But what does Kaboxer do?** Not every tool is easy to package. There are various criteria to meet, at times some crazy dependency trees or peculiar system modifications. You may need to use a legacy library, or you may need to change a configuration of something that would break another application. What do you do? We work with tool authors to try and make it easier, or we spend many late nights trying to get it to fit or we are just unable to package it.

Enter Kaboxer. Using containers we can put in complex non-standard package into a container and integrate it with the rest of the operating system, and bundle it up into the packaging eco-system. This means you can apt-install a Kaboxer program and use it without needing to take any special steps.

**How does Kaboxer benefit me?** Kaboxer has a few use-cases, depending on who is using it:

-   For people using Kali Linux, it is transparent so you will not notice when you are using it _(which is why you may not see it as “a big deal”)_. You just get more tools!
-   For us Kali developers, this is a game changer.
-   For other Debian packagers, [this may pique your attention](https://www.kali.org/docs/development/packaging-apps-with-kaboxer/).
-   For tool authors _(who want their software in Kali)_, there is hope for you yet ;-)

**What’s the down side to Kaboxer?** The size of the application will be larger because it will carry the normal overhead of having to use containers. While the installed package will be small, its installation will download the required container which will consume up to several hundreds of megabytes even for a simple application.

**What is going to happen because of Kaboxer?** We hope to start to include more tools into Kali Linux that were previously not packable, and have you not realize that you are using them via Kaboxer. Unfortunately, such tools will not make it into our default installation as the size increase for the ISO images would be too significant.

* * *

Overview
--------

There are various tools which would be a benefit to Kali users, but suffer from problems that make them hard to ship properly as `*.deb` packages. This could be because, the tools:

-   Are not developed with packaging and system integration in mind. They assume they can install specific versions of libraries, or patch libraries, or download pieces of software at runtime rather than at install time. This is against packaging standards and also is bad software engineering practice.
-   May feel they are entitled to do whatever they please with the operating system or other applications. These actions should not be allowed and the software needs to be isolated. We have seen actions such as:
    -   Creating users with specific UIDs/GIDs.
    -   Using paths that go against the Filesystem Hierarchy Standard (FHS).
    -   Using TCP or UDP ports that are usually affected to other services.
    -   Reconfiguring existing services.
-   Interact with external servers (maybe by a insecure method), thus the software itself cannot be fully trusted. As a result, it may be a good idea to isolate such software from valuable or sensitive data that may be present on the system.

A way to provide the isolation required from above would be to use containerization. Containers allow running an application in an isolated environment, with drastically reduced risks of unplanned interaction with the rest of the system (users, services, other applications, existing files, specific versions of libraries, etc.).

* * *

Design choices
--------------

While we are not excluding to support other containerization solutions, we have opted to start with Docker. It is well-known, widely used, and benefits from a large eco-system of images, thus ensuring its long term viability. Docker containers can be configured in many ways to achieve the various integrations that we need with the host system or even between multiple containers.

The value of Kaboxer is in how it makes it easy to tie together docker containers with the host system, through the usual docker features such as mount points and port redirections, but also through integration with desktop menu entries. All those integrations, as well as the instructions to build or retrieve the docker image, are specified in a single YAML file.

It is that single YAML file that is shipped in the `.deb` files that we provide in Kali and the post installation script of those packages will transparently download the image so that the application is ready to run afterwards.

### Build of Docker images

The build of the docker images is also mediated by Kaboxer but there’s nothing magic, it boils down to calling `docker build` on a specific `Dockerfile` with a few variables.

It is up to the packager to write that Dockerfile but that step can be trivial when the upstream project already has a Dockerfile or when it provides a ready-to-use docker image.

### Publication of Docker images

This step is so boring that we have automated it with GitLab CI. Every time that we make a change to a repository dedicated to a “kaboxed” application, such as covenant, [GitLab CI](https://gitlab.com/kalilinux/packages/covenant-kbx/-/pipelines) will rebuild the associated docker image and store it in its [image registry](https://gitlab.com/kalilinux/packages/covenant-kbx/container_registry).

### Integration of the images into the system

Once the app is containerized, we still need to make it available to the user in a seamless way, in a manner that ideally wouldn’t even be noticeable. The user shouldn’t even have to know that the app runs in a container.

We already explained that users continue to interact with Kali packages to install and remove the containerized applications, even though those packages are mostly empty shells running Kaboxer commands in the various maintainer scripts. They do also provide `.desktop` files so that the applications can be started from the usual desktop menu, and command-line helpers so that they can be started from a terminal without having to know about Kaboxer.

To be able to run docker containers, the users need some elevated permissions: we modified the Kali installer to grant those permissions by default to users created during the initial installation process. For other users, they will have to be added to the Kaboxer group (`adduser $USER kaboxer`).

Users obviously want their application data to be retained so Kaboxer has facilities to configure volumes shared between the host and the container thus providing persistence even if containers are short-lived. And then depending on the kind of application, you likely need more specific integrations:

-   For GUI applications, we need the host X11 socket to be accessible.
-   For web applications, we want to expose the HTTP port and start the web browser on the appropriate URL.

Those basic needs are covered with the current Kaboxer features but it seems likely that other kind of integrations will be required in the future.

* * *

If you still want to learn more about Kaboxer, please see its [homepage (plus source code)](https://gitlab.com/kalilinux/tools/kaboxer/), and [our documentation (with “Hello World” example)](https://www.kali.org/docs/development/packaging-apps-with-kaboxer/).

For examples of “real world” application, you can look at our first “Kaboxed apps”:

-   [Covenant](https://pkg.kali.org/pkg/covenant-kbx/), a framework to highlight the attack surface of .NET. Covenant comes as a server that is started in the background, plus a Web app that runs in the browser.
-   [Firefox Developer Edition](https://pkg.kali.org/pkg/firefox-developer-edition-kbx), is a web browser and we picked it as it is a complex large GUI application.
-   [Zenmap](https://pkg.kali.org/pkg/zenmap-kbx), the official NMAP GUI. Zenmap relies on deprecated [Python 2](https://www.kali.org/blog/python-2-end-of-life/) libraries that are not available in Kali Linux.

Want to get your hands dirty and give it a try?

```console
kali@kali:~$ sudo apt update && sudo apt -y install covenant-kbx
...
kali@kali:~$
kali@kali:~$ covenant-kbx
Usage: covenant-kbx start|stop
kali@kali:~$
kali@kali:~$ covenant-kbx start
>>> Initializing user data in ~/.local/covenant/data
>>> Starting covenant
Please wait during the start, it can take a long time...
>>> Opening https://127.0.0.1:7443 with a web browser
covenant/default started
Press ENTER to exit
kali@kali:~$
kali@kali:~$ ss -at | grep 7443
LISTEN 0 4096 0.0.0.0:7443 0.0.0.0:*
kali@kali:~$
```

_Do not forget to open up `https://localhost:7443` in a web browser!_

If you would like to start exploring Kaboxer itself and see what is happening under the hood:

```console
kali@kali:~$ kaboxer
usage: kaboxer [-h] [-v] {run,start,stop,get-meta-file,get-upstream-version,prepare,upgrade,list,ls,build,install,clean,push,save,load,purge} ...
kaboxer: error: the following arguments are required: action
kali@kali:~$
kali@kali:~$ kaboxer ls
App Installed version Available version Packaging revision from YAML Packaging revision from image
-------- ------------------- ------------------- ------------------------------ -------------------------------
covenant 0.6 - 1 1
kali@kali:~$
```

Lastly, you can track what programs are using Kaboxer in Kali by searching packages ending with `-kbx`:

```console
kali@kali:~$ apt-cache search --names-only '\-kbx

#### [Source](https://www.kali.org/blog/introducing-kaboxer/)


covenant-kbx - .NET command and control framework
firefox-developer-edition-en-us-kbx - Mozilla Firefox web browser - Developer Edition - en-US
zenmap-kbx - The Network Mapper Front End
kali@kali:~$
```

#### [Source](https://www.kali.org/blog/introducing-kaboxer/)

