---
title: "Emergency Self Destruction of LUKS in Kali"
date: Mon, 06 Jan 2014 00:00:00 +0000
draft: false
type: posts
---
# Emergency Self Destruction of LUKS in Kali

https://www.kali.org/blog/emergency-self-destruction-luks-kali/images/kali-luks-self-destruct.jpg



Kali Linux Full Disk Encryption As penetration testers, we often need to travel with sensitive data stored on our laptops. Of course, we use full disk encryption wherever possible, including our Kali Linux machines, which tend to contain the most sensitive materials. Setting up full disk encryption with Kali is a simple

### Kali Linux Full Disk Encryption

As [penetration testers](https://www.offsec.com/cyberversity/penetration-testing/), we often need to travel with sensitive data stored on our laptops. Of course, we use full disk encryption wherever possible, including our Kali Linux machines, which tend to contain the most sensitive materials.

Setting up [full disk encryption with Kali](https://www.kali.org/docs/installation/hard-disk-install/) is a simple process. The Kali installer includes a straightforward process for setting up encrypted partitions with LVM and LUKS. Once encrypted, the Kali operating system requires a password at boot time to allow the OS to boot and decrypt your drive, thus protecting this data in case your laptop is stolen. Managing decryption keys and partitions is done using the cryptsetup utility.

### Nuking our Kali Linux FDE Installation

A couple of days ago, one of us had the idea of adding a “nuke” option to our Kali install. In other words, having a boot password that would destroy, rather than decrypt, the data on our drive. A few Google searches later, we found an old [cryptsetup patch](http://lxer.com/module/newswire/view/103692/index.html) by Juergen Pabel which does just that, adding a “nuke” password to cryptsetup, which when used, deletes all keyslots and makes the data on the drive inaccessible. We ported this patch for a recent version of cryptsetup and posted it on [GitHub](https://gitlab.com/kalilinux/packages/cryptsetup-nuke-keys).

### Testing the LUKS Nuke Patch

This feature isn’t implemented yet in Kali as we wanted to gather some user feedback before applying this patch to base images. If you’d like to try it our yourself, these are the build instructions. Start by running an LVM encrypted installation in Kali and set a decryption password. Once done, download the cryptsetup package source and apply our patch to it. Proceed to build the patched package as follows:

```console
root@kali:~# apt-get source cryptsetup
root@kali:~# git clone https://gitlab.com/kalilinux/packages/cryptsetup-nuke-keys
root@kali:~# cd cryptsetup-1.6.1/
root@kali:~/cryptsetup-1.6.1# patch -p1 < ../cryptsetup-nuke-keys/cryptsetup_1.6.1+nuke_keys.diff
patching file lib/libcryptsetup.h
patching file lib/luks1/keymanage.c
patching file lib/setup.c
patching file src/cryptsetup.c
root@kali:~/cryptsetup-1.6.1# dpkg-buildpackage -b -uc
```

Once the package has built, install the cryptsetup packages to get our **nuke** option implemented:

```console
root@kali:~/cryptsetup-1.6.1# ls -l ../*crypt*.deb
-rw-r--r-- 1 root root 149430 Jan 4 21:34 ../cryptsetup_1.6.1-1kali0_amd64.deb
-rw-r--r-- 1 root root 250616 Jan 4 21:34 ../cryptsetup-bin_1.6.1-1kali0_amd64.deb
-rw-r--r-- 1 root root 105226 Jan 4 21:34 ../libcryptsetup4_1.6.1-1kali0_amd64.deb
-rw-r--r-- 1 root root 49580 Jan 4 21:34 ../libcryptsetup-dev_1.6.1-1kali0_amd64.deb
root@kali:~/cryptsetup-1.6.1# dpkg -i ../libcryptsetup*.deb
root@kali:~/cryptsetup-1.6.1# dpkg -i ../cryptsetup*.deb
```

Now that our patched cryptsetup package has been installed, we can go ahead and add a “nuke” key to our setup:

```console
root@kali:~# cryptsetup luksAddNuke /dev/sda5
Enter any existing passphrase: (existing passphrase)
Enter new passphrase for key slot: (nuke passphrase)
```

### Hey Dude, Where’s my Drive?

On any subsequent reboots, you will be asked for the LUKS decryption password each time as usual. If for whatever reason, you were to enter the nuke password, the saved keys would be purged rendering the data inaccessible. Should we implement this patch in the cryptsetup package? Let us know what you think via this quick poll. We’ll keep this poll open for a couple of weeks and keep you posted with any further developments of this feature:

```plain
Cryptseup Nuke Option in Kali
Add nuke features to cryptsetup ?*
- [ ] Yes, add this feature!
- [ ] Stop bothering me
- [ ] No, leave cryptsetup alone.
```

**Update:** The nuke patch has been introduced to Kali Linux and is available by default in [Kali Linux v1.0.6](https://www.kali.org/blog/kali-linux-1-0-6-release/).

**Update:** We’ve posted an example use-case for the Nuke feature in a later “[How to nuke your encrypted Kali Linux installation](https://www.kali.org/blog/nuke-kali-linux-luks/)” blog post.

**Update:** As of July 2019, Kali Linux no longer ships this cryptsetup patch, instead we introduced a cryptsetup-nuke-password package that provides a similar feature without modifying cryptsetup:

```console
root@kali:~# apt install cryptsetup-nuke-password
root@kali:~# dpkg-reconfigure cryptsetup-nuke-password
```

#### [Source](https://www.kali.org/blog/emergency-self-destruction-luks-kali/)

