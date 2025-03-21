---
title: "xz-utils backdoor how to get started"
date: Mon, 01 Apr 2024 00:00:00 +0000
draft: false
type: posts
---
# xz-utils backdoor how to get started

https://www.kali.org/blog/xz-backdoor-getting-started/images/xz-backdoor-getting-started.jpg



Following the recent disclosure of a backdoor in upstream xz/liblzma, we are writing this &ldquo;get started&rdquo; kind of blog post. We will explain how to setup an environment with the backdoored version of liblzma, and then the first commands to run to validate that the backdoor is installed. All in

Following the recent disclosure of a [backdoor in upstream xz/liblzma](https://www.openwall.com/lists/oss-security/2024/03/29/4), we are writing this “get started” kind of blog post. We will explain how to setup an environment with the backdoored version of liblzma, and then the first commands to run to validate that the backdoor is installed. All in all, it should just take a few minutes, and there’s no learning curve, it’s all very simple.

This blog post is aimed at all the enthusiasts that are following the news as the events unfold, and who are eager to have their hands on the keyboard, running a few commands in a terminal rather than just reading about it. This is really beginner level, and we’ll just reproduce the easiest findings that were reported in the initial disclosure. Nothing groundbreaking here, sorry ;)

Setting up the environment
--------------------------

First thing first: we’re going to need a Virtual Machine (or VM for short). The fastest is probably to just download a pre-built image from the [Kali Linux download page](https://www.kali.org/get-kali/#kali-virtual-machines), either the [current 2024.1 release](https://www.kali.org/blog/kali-linux-2024-1-release/) or the latest weekly image, at your preference.

When the image is downloaded, let’s start it. Don’t know how? We have documentation for each type of image: [VirtualBox](https://www.kali.org/docs/virtualization/import-premade-virtualbox/), [VMware](https://www.kali.org/docs/virtualization/import-premade-vmware/) and [Hyper-V](https://www.kali.org/docs/virtualization/import-premade-hyperv/). For [QEMU](https://www.kali.org/docs/virtualization/install-qemu-guest-vm/), its simple enough to create a new VM.

Now our VM is up and running, so we’re going to download and install a version of `liblzma` that contains the backdoor. Even though the package was pulled out of Linux distributions, it’s still widely available on the Internet. For this how-to, we’re going to get it from the [Debian snapshot service](https://snapshot.debian.org/). Since Kali is based on Debian, and liblzma only depends on the libc, it’s Ok to install the Debian package in Kali, we shouldn’t run into any incompatibility issue.

A note for clarity: xz-utils is the name of the upstream repository, it provides the well-known command `xz` to compress and decompress files, but it also provides the library `liblzma` , which is the compromised library that everyone is talking about at the moment. And it is via this library that a backdoor gets added to the SSH daemon… Clear?

The upstream versions `5.6.0` and `5.6.1` of xz-utils are known to contain the backdoor, so let’s grab the Debian package `5.6.1-1`.

Within the VM, let’s open a terminal and get it with:

```console
kali@kali:~$ wget https://snapshot.debian.org/archive/debian/20240328T025657Z/pool/main/x/xz-utils/liblzma5_5.6.1-1_amd64.deb
```

And now let’s install the package:

A word of caution for those who are not paying attention: below, we are purposefully installing a package that contains a backdoor! Obviously you are running those steps in a Virtual Machine, and this Virtual Machine is not exposed to the Internet.

```console
kali@kali:~$ sudo apt-get install --allow-downgrades --yes ./liblzma5_5.6.1-1_amd64.deb
```

Next step is to start (or restart) the SSH daemon:

```console
kali@kali:~$ sudo systemctl restart ssh
```

What’s next? Let’s find out!

Confirm that liblzma is compromised
-----------------------------------

First, we can detect if the version of liblzma contains the backdoor, thanks to a script from Vegard Nossum, that was [provided in the disclosure](https://www.openwall.com/lists/oss-security/2024/03/29/4).

Let’s create the script:

```console
kali@kali:~$ cat << 'EOF' > detect.sh
#! /bin/bash
set -eu
# find path to liblzma used by sshd
path="$(ldd $(which sshd) | grep liblzma | grep -o '/[^ ]*')"
# does it even exist?
if [ "$path" == "" ]
then
echo probably not vulnerable
exit
fi
# check for function signature
if hexdump -ve '1/1 "%.2x"' "$path" | grep -q f30f1efa554889f54c89ce5389fb81e7000000804883ec28488954241848894c2410
then
echo probably vulnerable
else
echo probably not vulnerable
fi
EOF
```

Make it executable, and then run it:

```console
kali@kali:~$ chmod +x detect.sh
kali@kali:~$
kali@kali:~$ ./detect.sh
probably vulnerable
```

The output from the command above should be `probably vulnerable`, meaning that the backdoor was detected in the library.

But wait, how does that work? The command `hexdump -ve '1/1 "%.2x"' <<file>>` will dump a file in hexadecimal form, without any formatting, just a looooong hexa string. The script does that with liblzma, and then matches a pattern (also in hexadecimal form) that belongs to the exploit. That’s all there is to it, and it’s enough to detect it.

Confirm that the SSH daemon is slower than usual
------------------------------------------------

First, for this test we need to make sure that password authentication is disabled, in the settings of the SSH daemon:

```console
kali@kali:~$ sudo sed -E -i 's/^#?PasswordAuthentication .*/PasswordAuthentication no/' /etc/ssh/sshd_config
```

Then restart the daemon:

```console
kali@kali:~$ sudo systemctl restart ssh
```

And now, let’s try to login as a non existant user, and time it:

```console
kali@kali:~$ time ssh nonexistant@localhost
nonexistant@localhost: Permission denied (publickey).
real 0.31s
user 0.05s
sys 0.00s
cpu 17%
```

There’s no “right value” here, as it’s highly dependent on your particular setup. However, what we want is to get an idea of how much time it takes, so let’s run the command a couple of times, to make sure that the results are consistent. In my tests, results are indeed very consistent, I get `real 0.30s` almost all the time.

Now let’s re-install the non-backdoored version of liblzma:

```console
kali@kali:~$ sudo apt update && sudo apt install --yes liblzma5
[...]
Get:1 http://http.kali.org/kali kali-rolling/main amd64 liblzma5 amd64 5.6.1+really5.4.5-1 [240 kB]
[...]
```

At the time of this writing, the version of the `lzma5` package in Kali rolling is `5.6.1+really5.4.5-1`, as shown above.

Now, let’s try the SSH login again, and time it:

```console
kali@kali:~$ time ssh nonexistant@localhost
nonexistant@localhost: Permission denied (publickey).
real 0.13s
user 0.05s
sys 0.00s
cpu 41%
```

As we can see, the difference in timings is pretty clear, it’s much faster without the backdoor!

Acknowledgments
---------------

As said in the introduction, this blog post is nothing new, it’s merely a step-by-step to reproduce some findings from the [original disclosure](https://www.openwall.com/lists/oss-security/2024/03/29/4). All the credits (massive credits actually) go to Andres Freund for the fantastic work and detailed report, and Vegard Nossum for the `detect.sh` script.

#### [Source](https://www.kali.org/blog/xz-backdoor-getting-started/)

