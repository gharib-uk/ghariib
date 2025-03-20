---
title: "Shellshock a survey of Docker images"
date: 2014-10-15T13:39:00.000-07:00
draft: false
type: posts
categories: 
- API,docker,shellshock
---
# Shellshock a survey of Docker images

<br/>

<br/>
When I look at the whole [_Shellshock_](http://en.wikipedia.org/wiki/Shellshock_\(software_bug\)) debacle I am mostly sad. Sad that one can exploit a bug in a piece of software from 1989 to hack internet-connected devices in 2014. I always have this naive hope that maybe, just maybe, not everything is hopelessly broken - which of course gets crushed every other week.

  

Enough ranting. This blog post is about a small research I've run last week on Docker and _Shellshock_. No, sorry, this is not yet another "product X is vulnerable to _Shellshock_ if used in a dark night with a super moon" report. So, what is this about? To understand that, we need to do some homework.

  

### Docker Images and you

One of the core concepts of Docker is the difference between an Image and a Container. The TL;DR;, slightly inaccurate version (I should not use Virtual Machine in this context but all readers will be familiars with VMs) can be broken down in two points:

1.  An Image is a "base", read only virtual machine template and a "Container" is a writable instance of that machine;
2.  Images can be chained in a hierarchy of inheritance where a Base image is modified generating a child image and so on. This is "_the_ way" to build images, even though you can always build your own.

[![](https://docs.docker.com/terms/images/docker-filesystems-busyboxrw.png)](https://docs.docker.com/terms/images/docker-filesystems-busyboxrw.png)Here is an image deep linked from the [Docker documentation](https://docs.docker.com/terms/container/), which should make things clearer.

As you can see the Debian Base Image has a couple of children before finally generating a writable container. The logic of Docker is such that you should not be "changing" the base image but rather "building on top" of it, adding new components. You surely can, but that would kill one of the key benefits of Docker - citing [Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.0_Release_Notes/sect-Red_Hat_Enterprise_Linux-7.0_Release_Notes-Linux_Containers_with_Docker_Format-Advantages_of_Using_Docker.html): _Lightweight footprint and minimal overhead_.

  

Another important piece of information is that Docker maintains [its own repository of public images](https://registry.hub.docker.com/) that anyone can download and use. Docker has some rather complicated concepts for indexes and registries but they don't help us here: suffice to say that in practice lots of users will download images from this "official" repository.

  

Some important implications for us security folks:

  

-   If a base image has a security bug, it is at least possible (if not likely) that all the children will inherit the same bug;
-   The logic with which developers most likely approach this model is "I won't have to worry about the base image". This has been somehow [hinted at](http://www.activestate.com/blog/2013/06/solomon-hykes-explains-docker) and while some experienced developers will take the need for updates into consideration, not everyone will;
-   People will download and build upon a set of images from Docker's repository. No, we will not hack that, stop being evil!

  

### Yeah, OK. So, Shellshock and Docker?

Now that we have a shared passable understanding of how Images work in Docker, let's get to what I have done. Last week I wondered how many of the most popular Docker base images had been updated: Shellshock had significant press coverage, the kind of coverage that pushes _my mum_ to ask me about "that problem they are talking about in the news", so I figured that most of the main images would have been updated by now. Have them?

  

To find out, I whipped together a small Python script ([published on github](https://github.com/paradoxengine/docker-tester)) that downloads a list of Docker images in an host VM, downloads and runs a script on each one and then reports the results. Once I finally managed to get it working reliably (and I suspect Guido might have heard me cursing my inability with the language he designed as I longed for the forbidden PHP fruit) I run it against the 100 most popular Docker images published on Docker's repository. In a nutshell, my script simply downloads the image, runs [bashcheck](https://github.com/hannob/bashcheck) on it, and then reports back the results. Because of the way the integration is designed, it will only work on Debian based machines: this is an important point because it means that _all my results are likely underestimating the actual numbers_.

  

Many crashes of Virtual Box later, the results were back. 30 Images had at least one instance of the many bugs the Shellshock umbrella covered. The full results are in the repository with the script, and I'll summarize them later on, but a caveat first. There is no proof that containers using these images or derivates of those images can be exploited: the only thing my script detects is the lack of patching. Don't wear your tinfoil hats just yet.

  

Now, without further ado...

  

### Things I have learnt scanning the 100 most popular Docker images

-   30% of the top 100 images were still vulnerable to one of the shellshock bugs;
-   4 of the top 30 were vulnerable, 1 in the top 10 - so around 10% of the really popular images;
-   None of the vulnerable images were "official, Docker maintained images", but some were based on them: those images were still vulnerable because they were not _rebuilt_ after the patch had been applied on the base images. That is, using a base image that gets regularly updated is not enough;
-   Some of the vulnerable images have a consistent user base, or at least downloads. _asher/remote\_syslog_ has got almost 900.000 downloads;
-   Docker security team is really nice. I gave them an heads up (nothing for them to do here really, in terms of incident response, but a lot of long term work) and they were very direct about the issues and shared some nice insights. Thumbs up.

[![](http://4.bp.blogspot.com/-Cb2xz0H4jfw/VD7aIdrKqnI/AAAAAAAAqRU/fmJ4dKlSeJM/s1600/image%2B(2).png)](http://4.bp.blogspot.com/-Cb2xz0H4jfw/VD7aIdrKqnI/AAAAAAAAqRU/fmJ4dKlSeJM/s1600/image%2B\(2\).png)

A synthetic summary of the shellshock related bugs I've found scanning on October 9th 2014

  

### Things you should worry about

Pentesters never worry! If you are a pentester, you likely want to keep an eye out for usages of Docker images during a pentest. You might even want to ask container's configurations to discover vulnerabilities before you even start the test - it's wonderful to have bugs at day 0.

  

If you are on the other side of the security fence, though, Docker is coming for you: it's the new hotness and it's quite likely to pop up in your infrastructure in one form or another. The sooner you have a strategy in mind to update those containers, the better.

  

But wait, **didn't we use to have the very same problem with virtual machines a few years ago?** We still do. But we used to, too.  
However I think there are some subtle but important differences here. As an admin or security person, you can't just SSH in the machine and "apt-get upgrade" it, then save a new snapshot. There is a whole chain of images that might get forked in various points, where some of the nodes might even be escaping your control. Updating images is a very real, [known problem](https://github.com/docker/docker/issues/1724): the Docker security team told me they are looking into it so hopefully things are going to get better in the future, but for now you really want to have a story for managing updates. Possibly before the next Shellshock.

  

### My humble view on things that could be improved

I should start by saying that I don't know nearly enough about Docker's infrastructure to have a complete view - and that making posts where you have to provide no solution to the problems you find is much easier. However, I think I realized two or three things while working on this:

-   **Reporting bugs on Docker images is hard!** Some of these images have tens of thousands of users but no bug tracker or no clear way to report security bugs. In some cases I've opened an issue in github and hoped for the best. Providing some kind of built-in bug reporting feature would be a nice to have in the registry, or maybe this could be brewed in Dockerfiles?
-   **Old images are bad**! When you look at an image in Docker's repository you have a clear indication of when it was built (or at least committed). Check out the Properties of [itzg/minecraft-server](https://registry.hub.docker.com/u/itzg/minecraft-server/dockerfile/): it has been built before the Shellshock bug was even discovered and it's based on an official base image. Now, given that we know what base images are vulnerable to bugs and when, it should be possible to simply assume that all the images that have been built before that as potentially vulnerable as well;
-   **Custom images are a lot of work to maintain.** On one of my bug reports the maintainer of the image just said "sure, I'll rebuild". Since he was using an official Debian build as a base image, it's not a lot of work on his side. Had he used a completely custom OS, he'd have to do a standard upgrade, which takes more and more time and effort as the image ages.

  

### In conclusion...

A somewhat interesting percentage of images was found to be vulnerable during my tests, for a total of maybe a couple of millions downloads and thus potentially affected containers. The interesting takeaway for me, however, was that updating Docker images is subtly different and possibly more complex than updating VMs. I suspect this is something we'll have to deal with more and more in the future as containerized systems become widespread.

  
EDIT: I have been pointed [this blog post](http://blog.tutum.co/2014/10/09/protect-your-docker-containers-against-shellshock/) which does a detailed analysis of some of the official Base Images - I have only pulled the Latest tag for each Image, so they got more coverage there. From a quick skim, none of the images I've found to be vulnerable were based on the images they flag in the article.

#### [Source](http://blog.nibblesec.org/feeds/759400037292500711/comments/default)

<br/>
---
