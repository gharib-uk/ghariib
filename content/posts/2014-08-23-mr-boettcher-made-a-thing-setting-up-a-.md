---
title: "Mr Boettcher made a thing Setting up a proper Debian install"
date: Sat, 23 Aug 2014 03:09:42 +0000
draft: false
type: posts
categories: 
- 
---
# Mr Boettcher made a thing Setting up a proper Debian install

<br/>

<br/>
Mr. Boettcher made a thing!  He created a video that highlights how to install Linux securely in a VM.  His next video will be how to setup OWASP's WebGoat to test for vulnerable web apps.  He noticed that documentation is a bit sparse, and often contradictory, so he wanted to help other folks who are having issues to get a proper install.

You will need an Network Install ISO of Debian, and you will need either VMware Player or Workstation.

His notes are below... Enjoy!

Secure the Goat #1 - Goat Pen  
  
Create a directory where you will put the VM.  We'll call it 'goat'.  
Download the Debian Network Install ISO and place it in the 'goat' directory.  
  
Create a 'share' directory inside the goat directory  
Place a (test) file in the share directory  
In VMware Worstation create a new vm using a Debian ISO and run install  
  
Update the sudoers file  
$ su - root  
$ update-alternatives --config editor  
    change to vim.tiny by pressing 2 and enter  
$ visudo -f /etc/sudoers  
    copy the root line and add one for goat user  
  
In order to install vmware tools, we'll need to install these packages  
$ sudo apt-get install gcc linux-headers-$(uname -r) make  
  
For the vmware tools install to work properly, these simlinks are required  
$ cd /lib/modules/$(uname -r)/build/include/linux  
$ sudo ln -s ../generated/utsrelease.h  
$ sudo ln -s ../generated/autoconf.h  
  
Insert vmware tools virtual CD  
In the workstation menu select vm -> install vmware tools  
$ tar -C /tmp/ -zxvf /media/cdrom/VMwarTools...  
$ sudo /tmp/VMwareTools.../vmware-install.pl  
  
Show desktop icons  
$ gsettings set org.gnome.desktop.background show-desktop-icons true  
  
change resolution in menu at top:  
    applications/system tools/preferences/system settings/ then 'displays'  
  
in Workstation under vm/settings, set virtual machine shared folder  
  
remove ISO file, take snapshot

#### [Source](http://brakeingsecurity.com/mr-boettcher-made-a-thing-setting-up-a-proper-debian-install)

<br/>
---
