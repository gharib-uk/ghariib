---
title: "Tracking and Fixing an Installer Bug"
date: Thu, 29 Aug 2013 00:00:00 +0000
draft: false
type: posts
---
# Tracking and Fixing an Installer Bug
https://www.kali.org/blog/tracking-fixing-installer-bugs/images/kali-installer-bug.jpg
<br/>

<br/>
A little while back, a bug with the LVM encrypted install in Kali Linux 1.0.4 was reported in our bug tracker. This bug was high priority in our TODO as encrypted installs are an important feature in our industry so we wanted to squash this bug ASAP. This article will
<br/>
A little while back, a bug with the LVM encrypted install in Kali Linux 1.0.4 was [reported](https://bugs.kali.org/view.php?id=443) in our [bug tracker](https://www.kali.org/docs/community/submitting-issues-kali-bug-tracker/). This bug was high priority in our TODO as encrypted installs are an important feature in our industry so we wanted to squash this bug ASAP. This article will describe the process of debugging, identifying, and fixing this bug in Kali, and ultimately in Debian as well.

The bug itself was weird; installing Kali with the “LVM Encrypted” option would result in a failed boot once the installation was done:

[![](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/kali-lvm-boot-bug.png)](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/kali-lvm-boot-bug.png)

The work-around suggested in the bug report indicated that the **/etc/crypttab** file was empty. By manually remounting the encrypted partition, repopulating it with the required parameters, and then updating the initramfs, the machine would boot successfully into the encrypted partition again. Most definately annoying and far from practical.

Now with the problem well defined, the solution seemed simple. Something was probably wrong with the way **/etc/crypttab** gets updated during the installation process. Our next step was to investigate the scripts that are responsible for this update and see if there are any bugs in the file update process. But how would you locate the exact script responsible for this update and how could we figure out what package it lives in?

To our rescue comes [DebianInstaller](https://wiki.debian.org/DebianInstaller/CheckOut). Using this set of scripts, we checked out the whole DebianInstaller source tree. This would allow us to search for the scripts that affect **/etc/crypttab** with much greater ease:

```console
root@kalima:~# svn co svn://anonscm.debian.org/svn/d-i/trunk debian-installer
root@kalima:~# cd debian-installer
root@kalima:~/debian-installer# scripts/git-setup
root@kalima:~/debian-installer# mr -p checkout
```

Once all the repositories had been checked out, we could simply grep for any files that reference the **/etc/crypttab** file as follows:

```console
root@kalima:~/debian-installer# grep -r '/etc/crypttab' * |grep -v ^manual
...
packages/partman-crypto/finish.d/crypto_config:# dm-crypt: creates /etc/crypttab entries
packages/partman-crypto/finish.d/crypto_config: echo "$target $source $keyfile $opts" >> /target/etc/crypttab
...
root@kalima:~/debian-installer#
```

We see above that it’s the “**crypto\_config**” script that writes to **/etc/crypttab**, which is located in the **partman-crypto** package.

Ideally, we would like to debug this script and see where the problem is, but how would you do this in a live installation media? The answer is relatively simple - we just had to pop open a command prompt during the installation process. The trick is to invoke our debugging shell (by pressing CTRL+ALT+F2) during the right stage of the installation - in our case we needed to interrupt the installer before the **crypto\_config** script was run but after the **partman-crypto** udeb was installed, so the beginning of the partitioning process would be a good spot. We proceeded to edit the **/lib/partman/finish.d/55\_crypto\_config** and added “**set -x**” at the start of the script:

[![](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/partman-script-debug.png)](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/partman-script-debug.png)

We then let the installer do its thing and just before the installation completed, we took a peek at **/var/log/syslog** in another shell. To our surprise, we saw that the **/etc/crypttab** file \*was\* being updated, contrary to our initial beliefs, as can be seen in the syslog of the installation. **WTH**:

```plain
Aug 28 21:57:42 main-menu[954]: (process:9810): crypttab_add_entry
Aug 28 21:57:42 main-menu[954]: (process:9810): /dev/sda5
Aug 28 21:57:42 main-menu[954]: (process:9810): /var/lib/partman/devices/=dev=sda/256901120-160041009151
Aug 28 21:57:42 main-menu[954]: (process:9810): /dev/mapper/sda5_crypt
...
Aug 28 21:57:42 main-menu[954]: (process:9810): echo
Aug 28 21:57:42 main-menu[954]: (process:9810): sda5_crypt UUID=6250dbca-648b-4848-9132-cfa900ab5874 none luks
```

This is where we started scratching our heads. If the problem was not in the writing of this file (as we expected), then why was there an empty **/etc/crypttab** file after the installation? Perhaps the problem was not in **partman-crypto** after all, but in how **live-build** generates our ISOs? We tested this theory of ours by using a Kali mini installation ISO (not built via live-build) and noticed that the LVM encrypted installs were working fine when using that installation media.

We know that the live-installer uses _tar_ to copy the whole live filesystem into a mounted **/target** directory and it assumes that the filesystems are empty, which is mostly true since they were just created by partman. This means that any pre-existing file can be overwritten if they are also in the live image, which was happening to **/etc/crypttab** in this case.

Further examination revealed that the problem was in **live-installer**, whereby it overwrites the generated **/etc/crypttab**. The live-installer already has some provisions to not overwrite **/etc/fstab**, so it’s just a matter of generalizing that rule and including the **/etc/crypttab** file as well:

```console
$ diff --git a/debian/live-installer.postinst b/debian/live-installer.postinst
index 9a39d8d..bc40b84 100644 (file)
--- a/debian/live-installer.postinst
+++ b/debian/live-installer.postinst
@@ -8,6 +8,8 @@ db_capb backup
# Architecture and OS detection
ARCH=`udpkg --print-architecture`
OS=`udpkg --print-os`
+# Files that must not be overwritten by copy of live system
+FILES_TO_PRESERVE="/etc/fstab /etc/crypttab"
NEWLINE="
"
@@ -34,11 +36,12 @@ install_live_system () {
# symlinks there.
rmdir /target/var/lock /target/var/run 2>/dev/null || true
- # Backup pre-existing /etc/fstab as it will be overwritten by the
- # copy of the live system
- if [ -e /target/etc/fstab ] && [ ! -e /target/etc/fstab.live-installer ]; then
- mv /target/etc/fstab /target/etc/fstab.live-installer
- fi
+ # Backup files that should not be overwritten by the copy
+ for f in $FILES_TO_PRESERVE; do
+ if [ -e /target$f ] && [ ! -e /target/${f}.live-installer ]; then
+ mv /target$f /target${f}.live-installer
+ fi
+ done
for place in $PLACES; do
[ ! -e $place ] && continue
@@ -83,10 +86,12 @@ install_live_system () {
eval ${SUPPORT}_teardown
done
- # Restore the fstab file created by d-i
- if [ -e /target/etc/fstab.live-installer ]; then
- mv /target/etc/fstab.live-installer /target/etc/fstab
- fi
+ # Restore important configuration files
+ for f in $FILES_TO_PRESERVE; do
+ if [ -e /target${f}.live-installer ]; then
+ mv /target${f}.live-installer /target$f
+ fi
+ done
if [ ${PLACE_FOUND} -eq 0 ]; then
error "Could not find any live images"
```

The above patch fixed the issue for us, allowing encrypted LVM installs to complete and boot successfully. As with any Debian bugs we encounter, we [send patches back to Debian](https://salsa.debian.org/installer-team/live-installer/-/commit/17678631e40f9fe31791bf90394fa81d87cce615) to improve the distribution we build upon. A fix for this installer bug will come out in our next point release ([1.0.5](https://bugs.kali.org/changelog_page.php)) next week. People generating their own ISO images though **live-build** will automatically receive the fixed package.

[![](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/installer-bug-fixed.png)](https://www.kali.org/blog/tracking-fixing-installer-bugs/images/installer-bug-fixed.png)

#### [Source](https://www.kali.org/blog/tracking-fixing-installer-bugs/)

<br/>
---
