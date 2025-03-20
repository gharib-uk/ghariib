---
title: "Whats New in Kali Linux"
date: Wed, 13 Mar 2013 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Whats New in Kali Linux
https://www.kali.org/blog/kali-linux-whats-new/images/kali-whats-new.jpg
<br/>

<br/>
Enter Kali Linux
----------------

“**So, what’s the difference between [BackTrack](https://www.backtrack-linux.org/) and Kali?**” you might be asking. Unfortunately for us, that’s not a simple question to answer. It’s a mix between “everything” and “not much”, depending on how you used BackTrack.

From an end user perspective, the most obvious change would be the switch to Debian and an FHS-compliant system. What this means is that instead of having to navigate through the **/pentest** tree, you will be able to call any tool from anywhere on the system as every application is included in the system path. However, there’s much hidden magic in that last sentence. I’ll quickly list some of the new benefits of this move.

### Streaming Security and Package Updates From Debian

Our new streamlined repositories synchronize with the [Debian](https://www.debian.org/) repositories 4 times a day, constantly providing you with the latest package updates and security fixes available.

### Debian Compliant Packaging of Each Tool in Kali

This is where we’ve been spending most of our time and effort. Relentlessly packaging dozens of useful tools, painstakingly making sure our packages are Debian compliant.

### Long Term Packaging and Maintenance of High Profile Tools

Many of the tools in our toolbox need to be “bleeding edge”. This means we have take on the task of packaging and maintaining upstream versions of many tools, so that our users are constantly kept up to date where it matters.

### Streamlined Development Process

As our source packages are now also Debian compliant, you can quickly and easily get the required sources of each tool, then modify and rebuild them with a couple of commands.

### Bootstrap Builds and ISO Customizations

One of the many benefits of our move to a Debian compliant system, is the ability to Bootstrap a Kali Installation/ISO directly from our repositories. This means that you can easily [build your own customizations of Kali](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/), as well as perform [enterprise network installs](https://www.kali.org/docs/installation/network-pxe/) from a local or remote repository.

### Automating Kali Installations

Kali Linux installations can now be automated using pre-seed files. This allows for enterprise wide customization and deployment on multiple systems.

### Real ARM Development

BackTrack 5 brought with it new support for ARM hardware. Our ARM build-bot was a modified Motorola Xoom tablet, which suffice to say, didn’t last for long. To help remedy this, [OffSec](https://www.offsec.com/) has donated a Calxeda ARM cluster to our project, allowing reliable and long term development of Kali Linux ARM images.

### Complete Desktop Environment Flexibility.

Our new build and repository environments allow for complete flexibility in generating your own updated Kali ISOs, with any desktop environment you like. Do you prefer KDE? LXDE? XFCE? Anything else? Then [change your Kali desktop environment](https://www.kali.org/docs/development/live-build-a-custom-kali-iso/) yourself.

### Seamless Upgrades Between Future Major Versions

Another benefit derived from the move to a Debian compliant system is the ability to seamlessly upgrade future major version of Kali. No longer will you have to reinstall your penetration testing machine due a new version of Kali coming out.

With all these changes (and many more), you can see why we’re so excited about this release. Go ahead and give Kali a spin. Head on to the [documentation area](https://www.kali.org/docs/) for some setup guides, and then over to our [forums](https://forums.kali.org/) and join the new Kali community!

#### [Source](https://www.kali.org/blog/kali-linux-whats-new/)

<br/>
---
