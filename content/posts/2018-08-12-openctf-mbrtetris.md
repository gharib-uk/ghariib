---
title: "OpenCTF mbrtetris"
date: Sun, 12 Aug 2018 17:15:00 +0000
draft: false
type: posts
---
# OpenCTF mbrtetris

<br/>

<br/>
**Category:** Forensics

**Points:** 25

**Description:**

```
boot this on baremetal. - https://kajer.openctf.com/tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6
```

**File Download:** [tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6](https://aagallag.com/files/ctf_files/openctf/tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6)

I start by running the `file` command:

```
$ file tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6 
tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6: DOS/MBR boot sector; partition 1 : ID=0x7, start-CHS (0x0,33,3), end-CHS (0x1,124,22), startsector 2048, 20480 sectors
```

Ok, let’s try mounting this:

```
$ sudo mount tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6 /mnt
mount: /mnt: wrong fs type, bad option, bad superblock on /dev/loop19, missing codepage or helper program, or other error.
```

Hmm, I use some Google-fu, and I stumble upon this useful article on [major.io](https://major.io/2010/12/14/mounting-a-raw-partition-file-made-with-dd-or-dd_rescue-in-linux/). As the article suggests, let’s run fdisk to calculate the offset.

```
$ fdisk -l tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6 
Disk tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6: 12 MiB, 12582912 bytes, 24576 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x3e33db48

Device                                                Boot Start   End Sectors Size Id Type
tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6p1       2048 22527   20480  10M  7 HPFS/NTFS/exFAT
```

So the math for this should be: `2048*512 == 1048576`.

And finally, to successfully mount this:

```
sudo mount -o ro,loop,offset=1048576 tinytetris-2c5414f1f85397e1787ec31cca3f252ac5fb78a6 /mnt
```

And finally, let’s take a look at the contents of this image.

```
$ cd /mnt
$ sudo ls -la
total 725
drwxrwxrwx  1 root root   4096 Jun  8 12:34  .
drwxr-xr-x 26 root root   4096 Aug  9 09:46  ..
drwxrwxrwx  1 root root   4096 Jun  8 12:34  4
-rwxrwxrwx  1 root root 727552 Jul  4  2017  bigfile.exe
-rwxrwxrwx  1 root root    237 Jun  8 12:34  flag.pyc
drwxrwxrwx  1 root root      0 Jul  5  2017  kuku
drwxrwxrwx  1 root root      0 Jul  5  2017 'System Volume Information'
$ python flag.pyc 
s1mPl3_0n_linux_sux0rZ_oN_winb10w$
```

Flag
====

**s1mPl3\_0n\_linux\_sux0rZ\_oN\_winb10w$**

#### [Source](http://b0tchsec.com/2018/openctf/mbrtetris)

<br/>
---
