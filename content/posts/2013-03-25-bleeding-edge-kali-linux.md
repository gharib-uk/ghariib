---
title: "Bleeding Edge Kali Linux"
date: Mon, 25 Mar 2013 00:00:00 +0000
draft: false
type: posts
---
# Bleeding Edge Kali Linux

https://www.kali.org/blog/bleeding-edge-kali-repositories/images/kali-bleeding-edge-repo.jpg



We&rsquo;ve been busy this week, still behind on our emails, but going strong with Kali development. We packaged some new tools which were pointed out by the community as missing, such as inguma, arachni, bully, lbd, uniscan, automater, as well as started to build a framework of libraries and patches

We’ve been busy this week, still behind on our emails, but going strong with Kali development. We packaged some [new tools](https://bugs.kali.org/) which were pointed out by the community as missing, such as **inguma**, **arachni**, **bully**, **lbd**, **uniscan**, **automater**, as well as started to build a framework of libraries and patches for bluetooth sniffing and ubertooth tools. We also fixed the Kali Menu to be editable again.

With over 300 tools in our repository, it’s close to impossible for us to keep \*every\* tool updated to the latest git version, all the time. A good example of this happed a few days ago, when we updated the **SET** package. Two days after we updated the package, we got a “tool upgrade” request for SET in our bug tracker, as our package was already outdated. Seems a bit extreme? Maybe not, **consider the following conundrum**:

You have an audit tommorrow, and you’re getting your tools ready. You boot Kali Linux up, run an “**apt-get update && apt-get upgrade**” to make sure you’re running the latest stable versions of _$your\_favorate\_tool_. After the update, you notice that _$your\_favorate\_tool_ has a package that was updated last week in our repositories. However, in the past week, some pretty awesome updates were added to _$your\_favorate\_tool_ upstream, and you really need those new features for the upcoming assessment. **What do you do ?**

### Worst Thing To Do.

Assuming you’re not using Kali as a “Throw Away Instance”, the worst thing to do in this case is to overwrite packaged files. For example, **the following commands to update a tool that gets updated frequently is a big no-no** and will ultimately break your install in the future:

```sh
# dont update tools like this!
cd /usr/share/
rm -rf $your_favorate_tool
git clone $your_favorate_tool
```

### OK To Do…but, Meh.

Since you **shouldn’t be messing with packaged files**, the most common option is to svn or git checkout _$your\_favorate\_tool_ in a temporary directory and use it from there as shown below. In most cases, all the dependencies needed for the updated tool will usually already exist in Kali. Alternatively, you could opt to [rebuild the source package](https://www.kali.org/docs/development/rebuilding-a-package-from-source/), which includes your updates and changes:

```sh
cd ~
mkdir work
cd work
git clone $your_favorate_tool
cd $your_favorate_tool
./your_favorate_tool
```

### Our Solution.

We said **\*close\*** to impossible, right? Seeing this clash between the update frequency of some tools and the need (of some people, not everyone) to “always have the latest revision of _$your\_favorate\_tool_”, we came up with an interesting solution. We’ve set up an opt-in “**Kali bleeding edge**” repository which contains daily builds for several useful and frequently updated tools. These repositories are still highly experimental (meaning we expect things to break from time to time until we get more feedback from the community). If you want to try this feature out, you are welcome to add the bleeding edge repository as shown below however, please remember that term “**bleeding edge**”. There’s a reason for the blood:

```sh
echo deb http://http.kali.org/kali kali-bleeding-edge main contrib non-free >> /etc/apt/sources.list
apt-get update
apt-get upgrade
```

We currently have a handful of tools in this repository, which include **se-toolkit** (set), **aircrack-ng**, **dnsrecon**, **sqlmap**, **rfidiot**, **beef-xss**, **libnfc**, **libfreefare**, **mfoc** and **mfcuk**. We expect this list to grow with time.

#### [Source](https://www.kali.org/blog/bleeding-edge-kali-repositories/)

