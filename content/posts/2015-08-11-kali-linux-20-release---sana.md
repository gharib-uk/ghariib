---
title: "Kali Linux 20 Release - Sana"
date: Tue, 11 Aug 2015 00:00:00 +0000
draft: false
type: posts
---
# Kali Linux 20 Release - Sana
https://www.kali.org/blog/kali-linux-2-0-release/images/kali-linux-2-0-release.jpg
<br/>

<br/>
Our Next Generation Penetration Testing Platform We&rsquo;re still buzzing and recovering from the Black Hat and DEF CON conferences where we finished presenting our new [Kali Linux Dojo](](/docs/development/dojo-mastering-live-build/), which was a blast. With the help of a few good people, the Dojo rooms were set up ready for the masses -
<br/>
Our Next Generation Penetration Testing Platform
------------------------------------------------

We’re still buzzing and recovering from the Black Hat and DEF CON conferences where we finished presenting our new \[Kali Linux Dojo\](\](/docs/development/dojo-mastering-live-build/), which was a blast. With the help of **a few good people**, the Dojo rooms were set up ready for the masses - where many **generated their very own Kali 2.0 ISOs for the first time**. But the excitement doesn’t end for us just yet. With the end of the cons, we now find ourselves smack in the middle of **the most significant release of Kali since 2013**. Today is the day that Kali 2.0 is officially released.

**So, what’s new in Kali 2.0?** There’s a new 4.0 kernel, now based on Debian Jessie, improved hardware and wireless driver coverage, support for a variety of Desktop Environments (gnome, kde, xfce, mate, e17, lxde, i3wm), updated desktop environment and tools - and the list goes on. But these bulletpoint items are essentially a **side effect of the real changes that have taken place** in our development backend. Ready to hear the real news? **Take a deep breath, it’s a long list**.

[![](https://www.kali.org/blog/kali-linux-2-0-release/images/tools-slider.png)](https://www.kali.org/blog/kali-linux-2-0-release/images/tools-slider.png)

Kali Linux is Now a Rolling Distribution
----------------------------------------

One of the biggest moves we’ve taken to keep Kali 2.0 **up-to-date in a global, continuous manner,** is transforming Kali into a **rolling distribution.** What this means is that we are pulling our packages continuously from [Debian Testing](https://www.debian.org/devel/testing) (after making sure that all packages are installable) - essentially upgrading the Kali core system, while allowing us to take advantage of newer Debian packages as they roll out. This move is where our choice in Debian as a base system really pays off - we get to enjoy the stability of Debian, while still remaining _on the cutting edge_.

Continuously Updated Tools, Enhanced Workflow
---------------------------------------------

Another interesting development in our infrastructure has been the integration of an **upstream version checking system**, which alerts us when new upstream versions of tools are released (usually via git tagging). This script runs daily on a select list of common tools and keeps us alerted if a new tool requires updating. With this new system in place, **core tool updates will happen more frequently**. With the introduction of this new monitoring system, we will slowly start phasing out the “tool upgrades” option in our [bug tracker](https://bugs.kali.org/).

New Flavours of Kali Linux 2.0
------------------------------

Through our Live Build process, Kali 2.0 now natively supports **KDE, GNOME3, Xfce, MATE, e17, lxde and i3wm**. We’ve moved on to [GNOME 3](https://www.gnome.org/gnome-3/) in this release, marking the end of a long abstinence period. We’ve finally embraced GNOME 3 and with a few custom changes, it’s grown to be our favourite desktop environment. We’ve added custom support for multi-level menus, true terminal transparency, as well as a handful of useful gnome shell extensions. This however has come at a price - **the minimum RAM requirements for a full GNOME 3 session has increased to 768 MB**. This is a non-issue on modern hardware but can be detrimental on lower-end machines. For this reason, we have also released an official, **minimal Kali 2.0 ISO**. This “light” flavour of Kali includes a handful of useful tools together with the lightweight [Xfce](https://www.xfce.org/) desktop environment - a perfect solution for resource-constrained computers.

Kali Linux 2.0 ARM Images & NetHunter 2.0
-----------------------------------------

The whole **ARM** image section has been updated across the board with Kali 2.0 - including Raspberry Pi, Chromebooks, Odroids… The whole lot! In the process, we’ve added some new images - such as the latest **Chromebook Flip** - the little beauty here on the right. Go ahead, click on the image, take a closer look. Another helpful change we’ve implemented in our ARM images is including kernel sources, for easier compilation of new drivers.

We haven’t forgotten about **NetHunter**, our favourite mobile penetration testing platform - which also got an update and now includes Kali 2.0. With this, we’ve released a whole barrage of new NetHunter images for Nexus 5, 6, 7, 9, and 10. The **OnePlus One** NetHunter image has also been updated to Kali 2.0 and now has a much awaited **image for CM12 as well** - check the [OffSec NetHunter](https://www.kali.org/get-kali/#kali-mobile) page for more information.

[![](https://www.kali.org/blog/kali-linux-2-0-release/images/kali-asus-chrome-flipbook-1.png)](https://www.kali.org/blog/kali-linux-2-0-release/images/kali-asus-chrome-flipbook-1.png)

Updated VMware and VirtualBox Images
------------------------------------

OffSec, the **[information security training](https://www.offsec.com/courses-and-certifications/)** and **[penetration testing](https://www.offsec.com/penetration-testing/)** company behind Kali Linux, has put up new [VMware and VirtualBox Kali 2.0 images](https://www.kali.org/get-kali/#kali-vm) for those who want to try Kali in a virtual environment. These include 32 and 64 bit flavours of the GNOME 3 full Kali environment.

If you want to build your own virtual environment, you can consult our documentation site on how to install the various [virtual guest tools](https://www.kali.org/docs/general-use/) for a smoother experience.

[![](https://www.kali.org/blog/kali-linux-2-0-release/images/kali-vm-images.png)](https://www.kali.org/blog/kali-linux-2-0-release/images/kali-vm-images.png)

TL;DR. Where’s My Kali 2.0 Download?
------------------------------------

The tl;dr of this release is best explained by comparison: If Kali 1.0 was **focused on building a solid infrastructure** then Kali 2.0 is **focused on overhauling the user experience and maintaining updated packages and tool repositories**. Along with the arrival of 2.0 comes a whole lot of interesting updates… You can head down to our [Kali Linux 2.0 Download](https://www.kali.org/get-kali/) page to get the goodness for yourself.

Still TL; Still DR. How Do I Upgrade to Kali 2.0?
-------------------------------------------------

Yes, you **_can_** upgrade Kali 1.x to Kali 2.0! To do this, you will need to edit your source.list entries, and run a **dist-upgrade** as shown below. If you have been using incorrect or extraneous Kali repositories or otherwise manually installed or overwritten Kali packages outside of **apt**, your upgrade to Kali 2.0 may fail. This includes scripts like lazykali.sh, PTF, manual git clones in incorrect directories, etc. - All of these will clobber existing files on the filesystem and result in a failed upgrade. If this is the case for you, you’re better off reinstalling your OS from scratch.

Otherwise, feel free to:

```sh
cat << EOF > /etc/apt/sources.list
deb http://http.kali.org/kali sana main contrib non-free
deb http://security.kali.org/kali-security/ sana/updates main contrib non-free
EOF
apt-get update
apt-get dist-upgrade # get a coffee, or 10.
reboot
```

Metasploit Community / Pro no longer ships in Kali
--------------------------------------------------

At the request of Rapid7, we have **removed the Metasploit Community / Pro package** from Kali Linux and now host the Open-source **_metasploit-framework_** package only. For all of you who require Community or Pro, you will now need to **download it from Rapid7** and then register and submit your personal details in order to get a license. In addition, _the Rapid7 team no longer maintains the Metasploit package in Kali_, which has brought with it some substantial changes - we’ve moved to a “native” setup, where rather than bundling all the required software needed to run Metasploit in one big package, we use native dependencies within Kali to support the **_metasploit-framework_** package. This results in a **faster, smoother work experience and easier integration** with Metasploit dependencies. For more information about this, check out our [Metasploit Framework in Kali](https://www.kali.org/docs/tools/starting-metasploit-framework-in-kali/) documentation page.

### Starting up Metasploit Framework in Kali Linux 2.0

Due to the above-mentioned changes in the **_metasploit-framework_** package, there are some minor changes in how Metasploit is started in Kali - specifically, there **is no longer a _metasploit_ service**. This is how you start up the Metasploit Framework with database support in Kali Linux 2.0:

```sh
# Start the Postgresql Database
/etc/init.d/postgresql start
# Initialize the Metasploit Framework Database
msfdb init
# Run msfconsole
msfconsole
```

Your Kali 2.0 FU Just Got an Upgrade
------------------------------------

Kali Linux 2.0 is a serious step forward for us, as we **continuously improve the distribution**. We hope you enjoy the new look, features, tools, and workflow. As usual, you are invited to join our community via [forums](https://forums.kali.org/), [bug tracker](https://bugs.kali.org/), [Twitter](https://twitter.com/kalilinux), [Facebook](https://www.facebook.com/kalilinux), and of course, [IRC](https://www.kali.org/docs/community/kali-linux-irc-channel/). Lastly, if you haven’t seen our Kali 2.0 Teaser video, here it is!

#### [Source](https://www.kali.org/blog/kali-linux-2-0-release/)

<br/>
---
