---
title: "Happy 10th anniversary Kalis story so far"
date: Wed, 29 Mar 2023 00:00:00 +0000
draft: false
type: posts
---
# Happy 10th anniversary Kalis story so far

https://www.kali.org/blog/10-years/images/banner-kali-10-year.jpg



 Wednesday 13th, March 2013, 10 years ago, Kali Linux v1.0 was first released. Today we want to celebrate Kali&rsquo;s 10th anniversary! Time has flown. And gosh, a lot has changed since then! They grow up so fast! This is the story of how Kali came to be, and some of the challenges along

Wednesday 13th, March 2013, 10 years ago, Kali Linux v1.0 was [first released](https://www.kali.org/docs/introduction/press-release/). Today we want to celebrate Kali’s 10th anniversary!

Time has flown. And gosh, a lot has changed since then! _They grow up so fast!_

This is the story of how Kali came to be, and some of the challenges along the way.

Yesterday is History: The Past
------------------------------

How did we get to where we are today? There is a quick answer, and a not so quick answer.

**[Quick](https://www.kali.org/releases/) history lesson**

It all began in 2004, with **Whoppix**, a security operating system based on Knoppix. This lead into **WHAX** in 2005, which used Slax. In 2006, **BackTrack Linux** happened which was based initially on Slax, then moved to Ubuntu. Every one of these OSes and its changes were done to solve different problems. Using everything which was learnt, **Kali Linux** was born. A fresh start in March 2013.

**[Longer](https://www.kali.org/docs/introduction/kali-linux-history/) history lesson**

_Knoppix - Initial two weeks work_

Whoppix _(White-Hat and knOPPIX)_ came about as the founder, @Muts, was doing an in-person air-gap network penetration test lasting for two weeks in 2004. It was a government contract, and he was not allowed to bring in his own laptop nor allowed to install any software on their machines. So every day, he was only allowed to take in software on a CD-ROM, before it was destroyed at the end of each day. At the time, Live CDs were the “in thing”. They would allow you to run a Linux OS completely off a disk, using RAM as a temporary HDD, without leaving a trace behind. Knoppix was chosen as the base OS which is Debian-based. He created one for this assessment and pre-loaded it with various tools that he believed he would need for the job. Each night, he would go back, tweak it by adding more tools or bug fixes. After the assessment was over, he cleaned it up and shared it online on a forum in August 2004. He then left for a vacation. Upon getting back he checked the logs to see the download numbers, and could not believe that it was so popular! He started to get requests from people, asking for tools to included as well as bug reports.

[![whoppix 2.x](https://www.kali.org/blog/10-years/images/Whoppix2-1.png)](https://www.kali.org/blog/10-years/images/Whoppix2-1.png)

* * *

_Slax - Starting to take it seriously_

What had started off as a “quick small thing” for single assessment, had started to gain traction. With Whoppix growing in popularity, it was becoming harder to develop for. At the time, Slax had a more mature toolchain for generating Live-CDs and working with OverlayFS. Thus it was a better suited option. _Editors note: We cannot say for certain, but the image file size may have become an issue. With more tools wanting to be added, space was running out. This was at a time when CD-R were at their peak, giving you 650-700 MB and USB media was not yet on the scene. By switching to Slax, the base OS size dropped, and allowed for more custom packages to be included as well as tighter compression (LZMA). Overall, this gave a lot more space to grow._

[![Whax 3.0](https://www.kali.org/blog/10-years/images/Whax3-5.png)](https://www.kali.org/blog/10-years/images/Whax3-5.png)

* * *

_Merging into BackTrack_

At the same time, there was a similar project happening over at remote-exploit, **Auditor Security Collection** (based on Knoppix), which first started in 2005. Auditor and WHAX had similar goals, but different strengths. At the time, cooperation on “Open-Source projects” was very different to what it is today, as it was “I made this thing, I’m sharing it.” It was more a few large players contributed, rather being able to accept work from lots of smaller submissions. After a bit of discussion between the authors, it made sense rather than both projects tackling the same problems, for the projects to merge together. This created BackTrack in May 2006. Initially, it was still based on Slax, but moved to Ubuntu later on. _Editors note: We cannot say for certain, where the name “BackTrack” originated from. At the time, when internally debugging problems, the phrase “Back tracking through the logs” got said frequency. We cannot say which came first, the phrase or the name._

[![BackTrack 1](https://www.kali.org/blog/10-years/images/BackTrack1-3.png)](https://www.kali.org/blog/10-years/images/BackTrack1-3.png)

* * *

_OffSec’s Start_

Walking around Black Hat USA 06, the team noticed how many people were using their project, and offering training using it. After listening to a few sales pitches, and training offerings, the team noticed just how many people were getting it wrong. As a result, [OffSec (previously known as Offensive Security)](https://www.offsec.com/), was created. Training from the people who made the tools. Who else would know it better?!

* * *

_USB - Live-Boot & Persistent_

BackTrack now had a stable, mature Live-CD project and it was exactly what was needed for the problem which was air-gap networks at the time. Whilst these Live-CDs were the answer for some scenarios, the team wanted to expand into more areas. The cost of USB flash drives/sticks had came down dramatically, as a result were more freely available. There was then a shift to “Live-Boot” (either CDs or USBs). The next item to solve would be getting their data to be “persistent” rather than losing it when powered off. Enter BackTrack 3 in June 2008.

_Login sound_

* * *

_System Updates_

A few months later, the team was once again at Black Hat USA & DEF CON, and were really excited by how many people were using their creation, and got to see first hand how people were using it. _DEF CON was also aware, as they were tracking user’s user-agents in web requests!_ Times were different to how it is now. It was common for _big_ exploits to make an appearance around these security conferences. 2008 was no exception. Walking about, the team could see first hand how many people were vulnerable, and they were helpless as they had no way of pushing out a patch. A drawback with Live-Boot is that in order to update to the latest release, you need to replace the whole OS. A complete re-install. Even with persistence for USBs, it is more for user data, rather than packages. Even if you installed BackTrack, there was not an update mechanism as the infrastructure was not in place to support it. You could not get the latest version. You would have to do a complete re-install. The team knew what their next problem was to solve: system updates.

As the project needs changed, the team had started hitting technical limitations of using Slax as the base, and started to look for an alternative underlying OS. They needed a way for end-users to easily update their system, without doing a complete re-install. Ubuntu was making a lot of positive noise, was gaining a lot of traction as it was popular with end-users, and had good development tools. It allowed for package updates to easily be applied to people systems. In February 2009, at Shmoocon, BackTrack 4 “Beta” was released using Ubuntu.

This change signals the start of BackTrack becoming a “real OS” - a traditional fully feature distribution. Previously when there was a major change, there would be a new name for the project. However, because BackTrack was getting to be known, it had grown legs, started appearing in the media, it was becoming ingrained in pop culture, they wanted to keep the momentum going.

* * *

_Installer_

In various releases prior, there was an installer included, which got carried over from the underlying base OS (Slax). However, it was not straight forward to use, and it was expected that certain commands were ran prior to execution. This, plus not allowing for any customization meant it was dropped in later releases.

[![BackTrack 3](https://www.kali.org/blog/10-years/images/BackTrack3-3.png)](https://www.kali.org/blog/10-years/images/BackTrack3-3.png)

With computer hardware getting more powerful each year, using virtual machine became more accessible. Users then started to want to be able to install BackTrack. The community started to contribute guides on how to manually install BackTrack, as well as creating an official tutorial via the terminal. The goal was now clear, BackTrack needed an “easy” installer. A graphical one happened in BackTrack 4 “Pre-Final” in June 2009.

* * *

_Domain_

The team knew how much BackTrack was growing in popularity, and as they did not switch the project name when using Ubuntu, it was time to create its own place on the Internet. With the launch the first stable release of BackTrack 4 in January 2010, the project got its own domain ([backtrack-linux.org](https://www.backtrack-linux.org/)) and moved off remote-exploit.

[![BackTrack 4](https://www.kali.org/blog/10-years/images/BackTrack4-4.png)](https://www.kali.org/blog/10-years/images/BackTrack4-4.png)

* * *

_System Upgrades_

When the team started work switching from Slax to Ubuntu, they grabbed the latest release at the time (8.10 - Intrepid Ibex). As this was not a “Long-Term Support” (LTS) release, upstream would only support it until April 2010 - one year & six months. This means, towards the end of this version, updates became fewer, packaging became harder, and workload increased. Things were not as stable as when it was first released.

It was not until May 2011 that the next major version of BackTrack got released, which also happens to be the last, BackTrack 5. This time, it was based on Ubuntu 10.04 (Lucid Lynx) LTS. This gave three years of support from upstream.

[![BackTrack 5](https://www.kali.org/blog/10-years/images/BackTrack5-3.png)](https://www.kali.org/blog/10-years/images/BackTrack5-3.png)

Upgrading from BackTrack 4 to BackTrack 5 meant doing a complete re-install (two other Ubuntu in-between, `9.xx`!). Even though there was a packaging update system in-place, there was not an upgrade path. This was still the same behavior as when using Slax. _Same mentality as Live-boot: replace the OS for each release, not upgrade._ It did not make for a good end-user experience. All their data, any custom programs they installed, all the customizations to their machines, would be lost when switching to the latest version. _As a result, it meant that people were reluctant to upgrade, as it would be a lengthy process for them._ The team had their new challenge to try and overcome.

* * *

_Final Fresh Start_

For the next few years, the team was busy working away, doing what they do best. However, they started to run into various problems again. As these problems started to accumulate, it was clear that a re-build was required once again, _for the final time_. As so much had changed, the project would not get away with not swapping names. They needed to drive home just how much change there was. BackTrack Linux became Kali Linux in March 2013.

_You can read more of [technical limitations here](https://www.kali.org/blog/10-years/#backtrack-6), as well as [why the name, Kali](https://www.kali.org/blog/10-years/#kalis-name)._

[![Kali 1.0](https://www.kali.org/blog/10-years/images/Kali1.0-2.png)](https://www.kali.org/blog/10-years/images/Kali1.0-2.png)

Kali 1.0 (Moto) [first saw the light of day](https://www.kali.org/docs/introduction/press-release/) at [Black Hat Europe 2013](https://www.blackhat.com/eu-13/) and was based on Debian 7. With the new infrastructure in place, building and publishing images became more automated and simplified. As a result, there were multiple revisions made to address bug fixes and even a minor update. This gave a better experience for users, as they were not having to wait months for an updated image.

[![Kali 1.1](https://www.kali.org/blog/10-years/images/Kali1.1-2.png)](https://www.kali.org/blog/10-years/images/Kali1.1-2.png)

The team then waited for Debian 8 to be released, before pushing out Kali 2.0 (Sana) in August 2015 (at [Black Hat USA 15](https://www.blackhat.com/us-15/briefings.html#the-kali-linux-dojo-workshop-#1-rolling-your-own-generating-custom-kali-linux-20-isos) & [DEF CON 23](https://defcon.org/html/defcon-23/dc-23-kali-dojo.html)). This time, for the first time, people could upgrade their systems between major project updates! It did require a very minor alteration for the end-user to be able to do this _(updating apt’s source to use the new codename)_.

[![Kali 2.0](https://www.kali.org/blog/10-years/images/Kali2.0-3.png)](https://www.kali.org/blog/10-years/images/Kali2.0-3.png)

* * *

_Moving to Rolling_

Overall, the feedback received for Kali 2.0 was positive, and it was a success. One of the reasons it was so well received was because of the updated versions of most packages. In information security (infosec) there is the need to be on the latest version. This is often because:

-   Being a developer, you may need the latest feature which has just been added.
-   _Being a system administrator, a patch could contain a security update to stop a vulnerability._

Writing exploits or developing infosec tools is no exception, they often need to have access to the latest libraries. Debian is stable, and that is because they only update their stable release about every two years, in order for packages to then be the latest tested version. _They will manually “backport” updates to patch any security vulnerabilities for older versions in order to fix that version of the package_

As two years is a very long time in infosec to wait to get an update, users were following bad practices in order to attempt to get the latest version. At times, they would break their setup in the process. The team had their new challenge to try and overcome!

To help understand why, it may take up to two years to get a package updated in stable. However, that does not mean its not ready before then as there is a whole workflow behind it. There is a branch called “testing”, which is for packages that are ready to into stable, but only when the time is ready for every package. A package will start in “unstable”, and once it passes Debian’s strict package testing process will it make it into testing. There are numerous packages upgrades in testing that never make it into stable, which would be in a stable state to release. This felt like a good balance of being stable and up-to-date for Kali team.

As soon as Kali 2.0 was out of the door, the Kali team knew what they had to do. Move from “Debian stable” to “Debian testing”. 5 months later, January 2016 Kali become a rolling distribution with Kali 2016.1.

[![Kali 2019.4](https://www.kali.org/blog/10-years/images/Kali2019.4-4.png)](https://www.kali.org/blog/10-years/images/Kali2019.4-4.png)

From this point forwards, the Kali team would take a “snapshot” of the network repository, test and then release that. This is because updates are continuously happening from “Debian testing”.

Snapshots are now taken about once a quarter, giving four major releases a year, rather than doing it a few months after Debian’s stable release once every two years.

[![Kali 2020.4](https://www.kali.org/blog/10-years/images/Kali2020.4-2.png)](https://www.kali.org/blog/10-years/images/Kali2020.4-2.png)

* * *

_Believe it or not, there is even more to the story where can go into more detail. If this is something you would like, let us know!_

### Dragon Logo

Throughout the projects, one item has been constant. The logo. The dragon. To the point that the logo is potentially better known than the project itself!

What originally started out to be a competition, originating on the Whoppix forum, the idea was “best graphic will be the next wallpaper”. The first submission that was awarded the winner, was the dragon. We liked it so much, we never re-opened the competition for the next version!

What we did not know at the time, was that the submission was plagiarized. It was taken from deviantart without the authors permission. We tracked down the rightful author and bought out the rights to it for Kali Linux.

The design of the dragon has been slightly tweaked over the years, by altering here and there such as making it thinner in places _(put on a diet!)_, curving the tail a little more etc. But we have always been careful to protect the iconic image as it is one of the most recognizable logos in this space. And to answer the question, we have no plans to ever replace the dragon logo.

[![Dragons](https://www.kali.org/blog/10-years/images/dragons.png)](https://www.kali.org/blog/10-years/images/dragons.png)

_Left, Old. Right, New_

### BackTrack 6?

Rather than releasing BackTrack 6, Kali 1.0 (Moto) came out instead. But why? With BackTrack 3 -> 4 (Slax to Ubuntu), why not with BackTrack 5 -> 6 (Ubuntu -> Debian)? Why was there such a fresh start and not keep the momentum going?

Well, a few new problems had started to arise, some technical, others not, and the team knew that one day they would need to find for solutions, such as:

**Upstream origin**

Ubuntu worked well for a number of years, however the team were increasingly finding that they were running into technical issues, causing limitations. At the time, if someone found an issue with a package on BackTrack, if it was not something custom to BackTrack, it would need to go “upstream” to Ubuntu. As Ubuntu uses Debian as its base, if they have not forked the package, it would need to then go to Debian to solve it. Then when a patch is created, it would need to be pushed into Debian, to be pulled into Ubuntu, to be pulled into BackTrack. This was a lengthy procedure. Otherwise, the team would have to fork a package, thus taking on ownership and responsibility of maintenance, to include a patch. With more packages requiring forks, more time was spent maintaining the OS rather than security focused aspects, which took the fun out of the team as it became more of a nightmare. The decision was made to cut out the middle layer, to use Debian as the base, rather than Ubuntu.

**Upstream direction**

By also using another OS as your base, you often follow their direction for their project. You can alter, but it does add additional complexity when synchronizing (think `git rebase`). This takes time and focus away from what makes BackTrack, BackTrack! Ubuntu had announced a major desktop environment change. Going from GNOME as their default to something they had created, Unity (and Mir). At the time, it had caused a bit of controversy. We also knew that certain tools would not be compatible. _Editors note: Whoppix & Whax both used KDE as the default Desktop Environment, and we switched to GNOME when using Ubuntu._

We also did not know just how difficult it ended up being to fully customise Ubuntu. At the time the Ubuntu developers had put various restrictions in place to stop things which we needed to-do. We believe this was because they were trying to make a OS that was user-friendly for people first starting out, rather than for people who were experience with Linux, who would be trying to-do a penetration test.

**Stability**

If we were going to be using Debian, it is best to follow their rules. Therefore we needed to follow “the correct Debian standard for packaging”. This meant we had to abide by Linux’s Filesystem Hierarchy Standard (FHS). This was introduced right back in the start with Whoppix so people had gotten used to `/pentest` directory, and liked it. However, it was a reason why the project had stability issues, thus users may have issues upgrading. It was not the best way to manage or maintain packages. The upside is, programs got added to `$PATH`, allowing you to call a program from any location!

The lack of FHS was hurting us in many different ways. Because of packages not being in the expected Linux location, we had to manually create links for libraries. This was of course a dependency nightmare. As you then expected, it was painful to update packages due to the amount of work, thus tools became outdated.

This was also a reason why you should not have added BackTrack’s network repository to another OS! Plus if you started to try an install things outside of BackTrack’s network repository, it was only too easy to have a knock on effect breaking something else. It was not right to be so worry to perform an update, as simple updates may break installs.

**Package maintenance**

Because of Debian’s standards, for end users, it helped to make packages more stable as well has having defined path for updates and upgrades. For us developers, maintaining a package also became easier. As a result, we can:

-   Look at a package’s metadata, so we know where the code upstream is (where it lives)
-   Have alerts when there has been an updated release out upstream
-   Use tools that are design to download and updated code
-   Import the update code
-   Update any necessary metadata, keeping it up-to-date and relevant
-   _And include the original archive (in another git branch)_

So all we have to do is build the package locally, test it to make sure it works. Then sign off on it, by adding our digital signatures to the packages (so it came from us and not tampered/altered by any malicious party), before uploading the package source to the build bots, which will re-compile it for every supported architecture. Simple!

This mature workflow helps prevent all the odd weird edge cases that were causing users machines to break by doing simple updates. This helps make the system more robust.

**Customization**

The above talks about packages, but another item to call out is the images. We could then be used to bootstrap builds, allowing for custom images to be generated, such as giving the flexibility for switching between desktop environments creation (GNOME? KDE? Xfce? Something else? Go for it!). Plus, it would support pre-seed answer files during setup, allowing to do unattended fully automate installs. This is more of a correct way of doing things, rather than a hacky job _e.g. generate the files, rather than relying on post-install scripts to modify afterwards._

**ARM - multi architecture**

Since BackTrack 4, the option was there for ARM support. We then experimented and released ARM support with BackTrack 5. At this point, we knew this was something we wanted to explore more going forwards. At the time, there was a lot of manual work involved, creating chroots for each device (started with Motorola Xoom). With BackTrack, we were forced to build the image, on the device itself, which does not scale well at wanting to support more device. Debian would not only allow for us to do that, but streamlined the process. At this point, we were able to treat ARM as a first-class citizen going forwards. This has been accelerated with [Ampere](https://www.kali.org/blog/ampere/)’s infrastructure support.

**Infrastructure**

With BackTrack, we were using subversion for our control system. We were starting to get requests for governments, as they wanted to use our OS internally. However, there was were sticking points. We needed to digitally sign our packages, and they needed to be able to compile the OS themselves internally. As we were scrapping it all and starting from scratch when starting with Kali, we moved to Git and used GPG to tag releases, at the same time, published all our build-scripts allowing anyone to build Kali on any platform. This made Kali truly open-source.

At the same time as the switch, we did some big internal changes. We originally set up our own public git server, but since then we have out grew it now migrated to [GitLab](https://about.gitlab.com/blog/2021/02/18/kali-linux-movingtogitlab/) and taken it to the next level. The team was growing, and we needed to start to scale, rather than having a few bottlenecks points. Another upside of this was to allow people to get source code of packages .

The team had really never used Ubuntu before, as a result, took multiple attempts to get a functioning eco-system (may not stretch as far as saying working systems!). This time, we did not want to make this mistake again, so we consulted the guy who _literally_ wrote the Debian handbook to get his advice and guidance to understand Debian’s way of doing things correctly from day 1.

We also setup dedicated build boxes, on various different architecture (as we wanted to support ARM). This allowed us for “CI/CD” for packages being built automatically, as well as nightly testing of images and system upgrades. We also setup unit testing for packages, allowing for testing to happen when importing from Debian.

**Setup**

We knew how important it was to have a functioning setup process, and it needs to be able to cover a lot of different scenarios. With the switch to Debian, we were able to add:

-   Full language support - Kali is not limited to English!
-   Accessibility support - We were contacted by multiple blind penetration testers
-   Straight to setup - We could now install straight from grub without having to boot into a live environment first

**Summary**

Debian was picked as its one of the largest Linux distributions, community driven (thus open-source), and supports many different architectures & hardware. Rather than using an OS which is forked from Debian, thus adds another layer of complexity of who is “upstream,” it makes sense to go straight to the source. Then following Debian standards & polices had huge advantages for us.

The downside would mean a complete redesign & restructure. And we mean a complete re-do over. Everything from our infrastructure (package & image build bots, network mirror handling etc), the base operating system, our packages. Complete re-write. We stop focusing on BackTrack to focus secretly working away on Kali which took over a year.

Needless to say, it was a LOT of work, but it was completely worth doing it. Both from development team, streamlining so many aspects to giving a greater improvement to user experience.

* * *

If you want even more, you can look at at the original blog posts, as well as watch some [talks](https://www.kali.org/blog/10-years/#talks) about it:

-   [BackTrack Reborn – Kali Linux](https://www.offsec.com/offsec/backtrack-reborn-kali-linux/)
-   [Kali Linux 1.0 Release - Moto - The Birth of Kali Linux](https://www.kali.org/blog/kali-linux-1-0-0-release/)
-   [What’s New in Kali Linux?](https://www.kali.org/blog/kali-linux-whats-new/)

### Kali’s Name

**Why is the name “Kali Linux”?**

“Kali” was already a part of a team members vocabulary, and when it was put forwards internally, everyone liked it. There are various incorrect claims online of why we picked it (such as it standing for something) but really, we just liked it. Yes, it also means “strong” in Swahili, and is also a Hindu goddess. **We. Just. Liked it.**

**How did you pick the name?**

When coming up with the name, we knew we needed something that was cool, catchy, and unique. We did not want to step on any other projects toes in infosec realm, or even IT in general.

One evening at a Black Hat USA, the team was sitting around and trying to come up with a new name. They started coming up with initials of phrases, or merging abbreviations and even looked at coming up with a completely new word. Without any success, one of the team members instinctively went to what they do when naming a new pet; Pantheon names. After making a list of of various Deities from as many mythologies, the team started looking up to see which had not yet been used in the Industry. Whilst there was a few terms the team really like, they were already in use by other projects. After looking up over 70 suggestions, only one really stood out, “Kali”. Kali is a Hindu Goddess of “Destruction and Rebirth”. It could not have been any better fitting; with one project ending, a new one starting. One of the team members ears twitched when they heard it as a suggestion, as they spoke Swahili, where “Kali” means “Strong”. After a little bit more understanding of the word, there is also a martial art also called “Kali”. Its style? All offense, no defense. It felt like it was a sign, everything was pointing to “Kali”. It was the only name which came up strong, and there was not a reason not to use it. The team stopped the hunt for a name.

The word Kali has a lot of meaning to many different people. _Everyone is right, as it means something different to everyone._ **We. Just. Liked it.**

### Screenshot Tour

They say “a picture is worth a thousand words”, so here is a trip down memory lane that’s worth £59,000:

-   Whoppix 2.x ([#1](images/Whoppix2-1.png), [#2](images/Whoppix27-1.png), [#3](images/Whoppix27-2.png), [#4](images/Whoppix27-3.png), [#5](images/Whoppix27-4.png))
-   Whax 3.0 ([#1](images/Whax3-1.png), [#2](images/Whax3-2.png), [#3](images/Whax3-3.png), [#4](images/Whax3-5.png), [#5](images/Whax3-5.png), [#6](images/Whax3-6.png))
-   BackTrack 1 ([#1](images/BackTrack1-1.png), [#2](images/BackTrack1-2.png), [#3](images/BackTrack1-3.png), [Wallpaper](images/BackTrack1-wallpaper.jpg))
-   BackTrack 2 ([#1](images/BackTrack2-1.png), [#2](images/BackTrack2-2.png), [#3](images/BackTrack2-3.png))
-   BackTrack 3 ([#1](images/BackTrack3-1.png), [#2](images/BackTrack3-2.png), [#3](images/BackTrack3-3.png), [Wallpaper #1](images/BackTrack3-wallpaper1.jpg), [Wallpaper #2](images/BackTrack3-wallpaper2.png))
-   BackTrack 4 ([#1](images/BackTrack4-1.png), [#2](images/BackTrack4-2.png), [#3](images/BackTrack4-3.png), [#4](images/BackTrack4-4.png))
-   BackTrack 5 ([#1](images/BackTrack5-1.png), [#2](images/BackTrack5-2.png), [#3](images/BackTrack5-3.png))
-   Kali 1.0.x ([#1](images/Kali1.0-1.png), [#2](images/Kali1.0-2.png))
-   Kali 1.1.x ([#1](images/Kali1.1-1.png), [#2](images/Kali1.1-2.png))
-   Kali 2.x ([#1](images/Kali2.0-1.png), [#2](images/Kali2.0-2.png), [#3](images/Kali2.0-3.png))
-   Kali 2019.4 ([#1](images/Kali2019.4-1.png), [#2](images/Kali2019.4-2.png), [#3](images/Kali2019.4-3.png), [#4](images/Kali2019.4-4.png))
-   Kali 2020.2 ([#1](images/Kali2020.2-1.png), [#2](images/Kali2020.2-2.png), [#3](images/Kali2020.2-3.png), [#4](images/Kali2020.2-4.png))
-   Kali 2020.4 ([#1](images/Kali2020.4-1.png), [#2](images/Kali2020.4-2.png))
-   Kali 2021.2 ([#1](images/Kali2021.2-1.png), [#2](images/Kali2021.2-2.png))
-   Kali 2022.1 ([#1](images/Kali2022.1-1.png), [#2](images/Kali2022.1-2.png), [#3](images/Kali2022.1-3.png))
-   Kali 2023.1 ([#1](images/Kali2023.1-1.png), [#2](images/Kali2023.1-2.png), [#3](images/Kali2023.1-3.png), [#4](images/Kali2023.1-4.png), [#5](images/Kali2023.1-5.png))
-   Shells ([#1](images/shell-history.png))

Today: The Present
------------------

And today, we have some presents to share with you! We are celebrating 10 years, with various different items.

[![](https://www.kali.org/blog/10-years/images/kali-cake-transparent.png)](https://www.kali.org/blog/10-years/images/kali-cake-transparent.png)

### Kali 2023.1

First up, was the release of [Kali 2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/). We managed to release exactly on the 10 year anniversary date! We try and not publicly give release dates, as there are many moving parts (outside of our control). This release was no exception, with multiple things breaking daily (Debian installer when trying to build the final RC image, Hugo image altering in our CI pipeline, and fall out from Python 3.11).

[![Kali 2023.1](https://www.kali.org/blog/10-years/images/Kali2023.1-3.png)](https://www.kali.org/blog/10-years/images/Kali2023.1-3.png)

With this comes various features and changes, include the initial technical preview launch of “Kali Purple”. Kali has been doing 10 years of offensive (18.5 from Whoppix!), now Kali is starting to help the defensive side. You can read more about it in the release notes of [Kali 2023.1](https://www.kali.org/blog/kali-linux-2023-1-release/), [Kali Purple’s documentation](https://gitlab.com/kalilinux/kali-purple/documentation), as well as watch the following talk from [Adversary Village at RSAC 2023](https://www.youtube.com/watch?v=3UMxOsdexK8).

### Online Puzzle

We also have an online event in the form of a Jeopardy Capture The Flag (CTF) happening over at [10year.kali.org](https://10year.kali.org/). The puzzles are designed by the same people who where the first to solve the TV show Mr. Robot “Alternate Reality Game” (ARG) - [Mr. Robot ARG Society](https://www.kali.org/blog/mr-robot-arg-society/). Enjoy and hope it keeps you busy for a while!

This challenge will only be “online” for two weeks, but we will offer it up afterwards to be able to download and run offline. _Check back on [10year.kali.org](https://10year.kali.org/) after closing to get the instructions._

### Song

We have once again worked with Uzimon, and now have a Kali tune! Give `Going Back to Kali` a listen to, and feel free to download, use, and remix. Enjoy!

[Audio (mp3)](audio/Uzimon%20-%20Going%20Back%20To%20Kali.mp3), [Video (mp4)](videos/Uzimon%20-%20Going%20Back%20To%20Kali.mp4) and [Lyrics](static/Uzimon%20-%20Going%20Back%20To%20Kali.txt)

In case you [missed](https://www.kali.org/blog/kali-linux-1-1-0-release/) the first songs:

-   OffSec: [Uzimon - OffSec Say Try Harder!](https://www.offsec.com/offsec/say-try-harder/)
-   OffSec: [Uzimon - Call OffSec](https://www.offsec.com/offsec/happy-holidays-from-offsec/)
-   BackTrack: [Infected Mushroom - Project 100](https://web.archive.org/web/20170606153104/http://www.backtrack-linux.org:80/backtrack/backtrack-5-are-you-infected-yet/)

### Sneakers

Kali is now going sneakernet! No, really! We have Kali on sneakers.

This is a very unique and a collectors item. These are Jordans, with Italian Nubuck and genuine Python _(not Python 3.11, but the real thing!)_. As these are for a limited time, as well as being handmade item, _do expect the price tag to match it_. Please note: we are not on commission, we are not getting a cut of any sales price. 100% is going to the shoe maker.

You can put your pre-order in here **No longer taking orders**. [You can view the mock ups still](https://web.archive.org/web/20230316175151/https://hvnd.studio/products/kali-pre-order).

[![Kali Shoe](https://www.kali.org/blog/10-years/images/kali-shoe.jpg)](https://www.kali.org/blog/10-years/images/kali-shoe.jpg)

### PEN-200/PWK Refresh

From an OffSec side of things, they have had a trick up their sleeve and launched the refreshed [PEN-200 (Pentesting With Kali)](https://www.offsec.com/courses/pen-200/) course. Everything from [new material](https://www.offsec.com/wp-content/uploads/2023/03/V1.Regular-Syllabus-PDF.pdf) to a new format style of lab machines, but still keeping OSCP certificate.

For more information, please see [their blog](https://www.offsec.com/offsec/pen-200-2023/) post, [PDF](https://www.offsec.com/wp-content/uploads/2023/03/pen200-ebook-2023.pdf), and [FAQs](https://help.offsec.com/hc/en-us/articles/12483872278932-PEN-200-FAQ).

### AMAs

Not only did we also do what has became our normal post-launch [Discord](https://discord.kali.org/) session, we also did a [“Ask Me Anything” (AMA)](https://www.reddit.com/r/offensive_security/comments/11swldq/hi_im_g0tm1lk_lead_developer_for_kali_linux/), over on Reddit last week. You can read back over people’s questions and the teams answers.

If you missed it and you have any burning questions you would like the team to answer, feel free to stop by for the next Discord session. We are aiming to schedule them the first Tuesday after each Kali release. See you for Kali 2023.2!

* * *

Unfortunately, due to time constraints and personal events, we were unable to get our “kali4kids” ready in time. We were hoping to have this year’s special something early, but rather than rushing and reducing the quality, we are delaying it. Trust us, its going to be worth the wait. _Its also not an April Fools, its not being released on 1st April - promise!_

Tomorrow: The Future
--------------------

So, where do we see Kali in the next 10 years? Good question! In short: _We do not know._

10 years ago we could not have predicted where we are today. And the same goes for now, we cannot predict the next 10 years.

What we can say is, we will continue with what we have already done and be responsive to the industry, pentesting, & market with how it develops over time with the goal of being at the forefront.

* * *

We need to be responsive to items which come up. Both from an upstream side of things (Debian), as well as the infosec industry.

With software, code gets updated. A perfect recent example of that is Python. Debian is getting ready to do its next stable release this summer. As a result, there are [various stages](https://release.debian.org/bookworm/freeze_policy.html) in the release cycle to make this happen. Package maintainers are trying to make sure they have the latest version in, and as bug free as possible. Python is such a core part of an OS as so many scripts and packages rely on it, it is included in their first freeze. Python 3.11 has been “rushed” to make it into the upcoming stable release, as there are real improved performance. This then has a knock on-effect, as not everything may be compatible, either in Debian or Kali. After the updated Python was accepted, packages then get updated, pushed and tested against. There is a lot, and it does not happen all at once, but rather as each package then gets updated afterwards. As a result, bugs may trickle in as things change. _We also like to try and keep as close to Debian as possible, expect for where it makes sense to benefit us and our users. This means we can put more time into doing things that make Kali… Kali!_

With technology, trends change. Live-CD were once “the in thing”, then persistent USB was “it”, which moved onto VMs, which gave way to containers, and currently “cloud” is in. Who knows what’s next!

In infosec, trends change as technology changes, software stacks change, attack surface changes, and defenses improve. As a result, we need to be able to adapt. At one stage, Wireless hacking “was the thing”, so we needed to support injection on as many cards as possible. Today, it is more “Command and Control”, “Cloud”, “Living off the Land”, and “Endpoint Detection and Response”. We do not know what tomorrow will bring, but we need to be ready to react to it.

* * *

We have built up various internal technical debt, that we need to pay off. _Luckily nothing is more than 10 years old!_

An item which we are able to talk about, is switching out our mirror redirector to a newer solution. We have had this since the start of Kali, and whilst it does work, it is becoming more feature limiting and unable to integrate with newer stacks.

Refreshing our CI solution we are using for our image building is another item which is long overdue. This will help detect build issues with our images and platforms quicker.

Once we have reduced as much of the debt as we can, we will be in a much stronger position to be able to push forwards, and quicker than if we were to do any future items now. We are aiming then to ride on the development momentum, to build excitement up in the community.

* * *

Like Kali Purple, Kali is going defensive. This is a new area for Kali, currently we have only scratched the surface. We want to go deeper and break into more infosec areas as well!

We are looking for ideas and suggestions to please come and get involved with what you would like to see Kali Purple turn into.

* * *

Kali is an Open-Source project. Previously we have been doing things behind closed doors, but we have been doing more in the public eye. But we can do more. We want to do even more in the open as possible. We are wanting to feed and help community contributions grow.

* * *

…One thing that is certain however, we do not plan on changing the projects name. Kali is here to stay!

Timelines
---------

Over the years there has been some activity in different areas. Below we have pieced together some things which may be of interest to some of you.

### Talks

Over the years, the team has given various different talks about the projects. They could be more of a deep dive of a certain platform or feature, but below are ones more of a higher level overview:

-   2013-07-17: [OISF 2013 Martin Bos Kali Linux Backtrack Linux reborn](https://www.youtube.com/watch?v=zflPxpmxk-o) @ Ohio Information Security Forum
-   2017-12-05: [Kali Linux’s Experience of a Derivative Tracking Debian Testing](https://www.youtube.com/watch?v=E-rs7OXH7Ls) @ Debconf 16

### Trailer Videos

Over the years, we have done videos to show off the project. Here is a collection:

-   2011-05-05: [BackTrack 5 - Penetration Testing Distribution](https://vimeo.com/23347352)
-   2013-01-19: [BackTrack Reborn - Kali Linux Teaser](https://vimeo.com/57742213)
-   2015-07-01: [Kali 2.0 Teaser - Kali Sana](https://vimeo.com/132329259)
-   2017-08-22: [Kali 2017.1 - KDE Desktop Fun](https://vimeo.com/346920378)

### Hacked

We know everyone is a target, but also being apart of the industry we paint ourselves as a bigger target. From time-to-time we have made slip ups. Lucky, as far as we know, it never has been anything major, critical or user-facing:

-   2010-12-25: “Happy Ninja” low privileged shell by compromising another vHost on the same machine via an outdated vBulletin plug-in
    -   Their eZine: [Owned and Exposed #2](https://www.exploit-db.com/papers/15823)
-   _2012-04-11 - Invalid [0-day privilage escalation in BackTrack](https://web.archive.org/web/20120414172307/http://www.backtrack-linux.org:80/backtrack/backtrack-0day-privilege-escalation) advisory_
    -   _Really, it was WICD privilege escalation, BackTrack was already running as root, and it was more of a PR stunt_
-   2013-05-01: OffSec launches Bug Bounty program, covering Kali Linux
-   2014-04-30: “The GreaT TeAm (TGT)"’s defacement of `lists.kali.org` (via Heartbleed/CVE-2014-0160)
    -   Low traffic, rarely used 3rd party hosted public mailing list
-   2016-03-31: “Team Bad Dream” gained moderation privileges on `forums.kali.org` (socially engineered), which lead to failed defacement on `bugs.kali.org` (suspect re-used credentials)
    -   Also suspect impersonating user on IRC (failed attempt at requesting additional access to new services)
    -   Limited only to community support portals
-   2021-12-08: “0x0 keeper” used CVE-2021-43798 Grafana directory traversal (via OffSec’s bug bounty program)
    -   Used for system monitoring and alerts

### April Fools

We like a bit of fun, and have been known to pull pranks over the years:

-   2010-04-01: [Backcrack-ng v1.1](https://web.archive.org/web/20100407115253/http://www.backtrack-linux.org:80/backtrack/aircrack-ng-aquired-by-the-backtrack-team)
-   2021-04-01: [kali4kids](https://twitter.com/kalilinux/status/1377659731913871362)
-   2022-04-01: [Hollywood mode](https://www.kali.org/blog/kali-linux-2022-2-release/)

#### Kali4Kids

What [started out as a poster](https://www.kali.org/blog/kali-everywhere/) put out by a government agency that [did not give the message they were expecting](https://www.zdnet.com/article/uk-police-distance-themselves-from-poster-warning-parents-to-report-kids-for-using-kali-linux/).

In response, the community started to share how [Kali is for the children](https://twitter.com/kalilinux/status/1229906554079645696). With how much it was being used, it was shortened to “Kali For Kids”. So we figured, [what would Kali look like IF it was made for kids](https://web.archive.org/web/20210401163106/https://kids.kali.org/).

“kali4kids” then became the internal name for next year’s April Fools joke. We have got enough ideas for things we want to try all the way to 2039!

### Kali Dojo

The term “Dojo” relates to the martial arts, where you practised it. As there has always been a underlying theme of martial arts, it felt right to use. Kali dojo, was a series of workshops given at mostly conferences. They would be hands on exercises, doing features with the OS that makes Kali, Kali:

-   2014-08-07: [Black Hat USA 2014](https://www.blackhat.com/us-14/kali-linux-dojo.html)
-   2014-09-24: [BruCon 2014](http://2014.brucon.org/index.php/Training_Kali_Linux_Dojo.html)
-   2014-09-27: [DerbyCon 2014](https://www.youtube.com/watch?v=EhKeNfldif8&t=684s)
-   2015-08-05: [Black Hat USA 2015](https://www.blackhat.com/us-15/briefings.html#the-kali-linux-dojo-workshop-#1-rolling-your-own-generating-custom-kali-linux-20-isos)
-   2015-08-07: [DEF CON 23](https://defcon.org/html/defcon-23/dc-23-kali-dojo.html)
-   2016-03-24: _[IRC](https://www.offsec.com/offsec/what-it-means-to-be-an-oscp-reloaded/)_
-   2016-08-04: [Black Hat USA 2016](https://www.blackhat.com/us-16/kali-linux.html)
-   2016-08-31: [Ekoparty 12](https://ekoparty.blogspot.com/2016/08/the-kali-linux-dojo-penetration-testing.html)
-   2017-07-27: [Black Hat USA 2017](https://www.blackhat.com/us-17/kali-linux.html)
-   2017-07-29: [DEF CON 25](https://www.wallofsheep.com/blogs/news/introducing-hands-on-workshops-at-the-packet-hacking-village#kalilinux)
-   2018-08-09: [Black Hat USA 2018](https://twitter.com/kalilinux/status/1027599759257743360)
-   2018-08-11: [DEF CON 26](https://www.wallofsheep.com/pages/dc26#jlong)
-   2018-10-25: [Wild West Hackin’ Fest 2018](https://wwhf18.sched.com/event/Foa3/workshop-kali-linux-dojo-registration-required)
-   2018-03-21: [InfoSec World 2018](https://web.archive.org/web/20171116235734/http://infosecworld.misti.com/agenda/agenda-at-a-glance)

Online videos of 2015 dojos:

-   2015-02-25: [Kali Dojo 02 - Building Custom Kali ISOs using Live Build](https://vimeo.com/120611508)
-   2015-02-26: [Kali Dojo 03 - Kali Linux USB Persistence & Encryption](https://vimeo.com/120724441)
-   2015-03-06: [Kali Dojo 04 - Kali on a Raspberry Pi with LUKS Disk Encryption](https://vimeo.com/121449299)

#### [Source](https://www.kali.org/blog/10-years/)

