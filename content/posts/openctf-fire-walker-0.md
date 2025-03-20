---
title: "OpenCTF : fire walker 0"
date: 2025-03-19
categories: 
  - "general"
---

**Category:** Network

**Points:** 50

**Description:**

```
Flag: http://172.31.2.97:20621/flag-d12bb978.txt Firewall rules: https://scoreboard.openctf.com/firewalker_0-acaceaa807e20591173451a7a824a23f2728563b
```

**File Download:** firewalker\_0-acaceaa807e20591173451a7a824a23f2728563b

The goal of the fire walker challenges is straight-forward, download the flag file from the provided http URL. The trick is, there are firewall rules that will prevent you from simply running wget, curl, or opening the URL in your favorite web browser. So let’s take a look at the firewall rules:

```
$ cat port_20621_rules.txt 
Chain PORT_20621 (1 references)
target     prot opt source               destination         
REJECT     tcp  -- anywhere             anywhere             tcp spts:1024:65535 reject-with icmp-admin-prohibited
```

So what these rules are describing, is that if the source port is in the range of 1024-65535, the packet will be rejected. For those new to networking, every TCP connection has a source port and a destination port. You are likely already aware of common destination port number used for servers, for example, HTTP uses port `80` by default. In this challenge, port `20621` is the destination port for our HTTP traffic. However, the source port is randomly selected by your client. And because ports under 1024 generally require elevated privellages, most, if not all, programs will never select a port below 1024. However, both `nc` and `curl` allow you to manually specify the port number. I decided to use `curl` because this allowed me to avoid manually building the HTTP GET request. Be sure to run the command with `sudo`.

```
$ sudo curl --local-port 22 http://172.31.2.97:20621/flag-d12bb978.txt > flag-d12bb978.txt
$ cat flag-d12bb978.txt 
pr1vil3ge_h4s_its_privile9e5
```

# Flag

**pr1vil3ge\_h4s\_its\_privile9e5**
