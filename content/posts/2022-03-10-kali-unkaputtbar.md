---
title: "Kali Unkaputtbar"
date: Thu, 10 Mar 2022 00:00:00 +0000
draft: false
type: posts
---
# Kali Unkaputtbar

https://www.kali.org/blog/unkaputtbar/images/banner-unkaputtbar.jpg



 Adjective (German) unkaputtbar (comparative unkaputtbarer, superlative am unkaputtbarsten) From un- + kaputt + -bar. (colloquial) indestructible, unbreakable Diese Flasche ist unkaputtbar. ― This bottle is indestructible. With our 2022.1 release, we promised something big for you bare-metal installers and here it is. With no further ado, we present to you: Kali Unkaputtbar Summary: Unkaputtbar brings Virtual Machines&rsquo;

> ##### Adjective (German)
> 
> **[unkaputtbar](https://en.wiktionary.org/wiki/unkaputtbar)** (comparative unkaputtbarer, superlative am unkaputtbarsten)
> 
> From _un-_ + _kaputt_ + _\-bar_.
> 
> 1.  (colloquial) indestructible, unbreakable _Diese Flasche ist **unkaputtbar**._ ― This bottle is **indestructible**.

With our [2022.1 release](https://www.kali.org/blog/kali-linux-2022-1-release/), we promised something big for you bare-metal installers and here it is. With no further ado, we present to you:

### Kali Unkaputtbar

_Summary: **Unkaputtbar brings Virtual Machines’ (VMs’) snapshot feature to bare-metal and injects some steroids**._

Have you ever wished you could travel back in time after deleting that important customer report or after installing a broken driver _(Nvidia?)_ just before heading into a board meeting? Well, you better read on, because now you can!

From zero to hero in 30 seconds (You _really_ want to enable video playback in your browser for this):

 Your browser does not support the video tag.

All it takes is installing Kali Linux version 2022.1 or newer with btrfs as the file system and to enable snapshotting after installation and you will get:

-   automatic snapshots with APT installations or removals
-   automatic snapshots on every boot
-   automatically created Kali Linux specific btrfs subvolume layout
-   new boot menu allowing you to boot into snapshots
-   ability to browse the file content of snapshots and copy files across
-   perform diffs between snapshots and restore individual files

But don’t take our word for it, see for yourself what [Kali Unkaputtbar](https://www.kali.org/docs/installation/btrfs/) can do.

* * *

Unkaputtbar features
--------------------

#### Boot snapshot

Booting into snaphots from the GRUB boot menu:

[![](https://www.kali.org/blog/unkaputtbar/images/btrfs-50-rollback1.png)](https://www.kali.org/blog/unkaputtbar/images/btrfs-50-rollback1.png)

#### Diff snapshots

Using snapper to generate diffs between snapshots:

[![](https://www.kali.org/blog/unkaputtbar/images/btrfs-60-diff1.png)](https://www.kali.org/blog/unkaputtbar/images/btrfs-60-diff1.png)

#### Browse snapshots

You can even browse the content of snapshots:

[![](https://www.kali.org/blog/unkaputtbar/images/btrfs-70-browse1.png)](https://www.kali.org/blog/unkaputtbar/images/btrfs-70-browse1.png)

#### Additional automatic snapshots

Configuring additional automatic snapshots such as of your home drive takes mere seconds:

[![](https://www.kali.org/blog/unkaputtbar/images/btrfs-030-snapper-config2.png)](https://www.kali.org/blog/unkaputtbar/images/btrfs-030-snapper-config2.png)

* * *

Install Unkaputtbar
-------------------

Can’t wait to get your hands on it? Well don’t ;-)

Download our latest image from [here](https://www.kali.org/get-kali/#kali-installer-images) and enable automatic snapshots as described in our [dedicated documentation](https://www.kali.org/docs/installation/btrfs/). That page also contains all the essential information to take full advantage of Kali Unkaputtbar.

* * *

**Special Thanks**:

Kali Unkaputtbar is made possible by software from an incredible bunch of people and we would like to express our heartfelt thanks to [Antynea](https://github.com/Antynea), [Ricardo Vieira](https://github.com/ricardomv), and our friends at [openSUSE](https://www.opensuse.org/) for their tireless work on [grub-btrfs](https://pkg.kali.org/pkg/grub-btrfs), [snapper-gui](https://pkg.kali.org/pkg/snapper-gui), & [snapper](https://pkg.kali.org/pkg/snapper) respectively.

#### [Source](https://www.kali.org/blog/unkaputtbar/)

<br/>
---
