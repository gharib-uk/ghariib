---
title: "A Journey From sudo iptables To Local Privilege Escalation"
date: Fri, 20 Sep 2024 13:30:00 +0000
draft: false
type: posts
categories: 
- 
---
# A Journey From sudo iptables To Local Privilege Escalation

<br/>

<br/>
![](https://www.shielder.com/img/blog/sudo_iptables_og.jpg)

TL;DR
-----

A low-privileged user on a Linux machine can obtain the `root` privileges if:

-   They can execute `iptables` and `iptables-save` with `sudo` as they can inject a fake `/etc/passwd` entry in the comment of an `iptables` rule and then abusing `iptables-save` to overwrite the legitimate `/etc/passwd` file.
-   They can execute `iptables` with `sudo` and the underlying system misses one of the kernel modules loaded by `iptables`. In this case they can use the `--modprobe` argument to run an arbitrary command.

Intro
-----

If you’ve ever played with _boot2root_ CTFs (like Hack The Box), worked as a penetration tester, or just broke the law by infiltrating random machines (NO, DON’T DO THAT), chances are good that you found yourself with a low-privileged shell - `www-data`, I’m looking at you - on a Linux machine.

#### [Source](https://www.shielder.com/blog/2024/09/a-journey-from-sudo-iptables-to-local-privilege-escalation/)

<br/>
---
