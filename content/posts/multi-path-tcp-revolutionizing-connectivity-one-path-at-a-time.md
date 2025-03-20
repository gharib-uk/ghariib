---
title: "Multi-Path TCP: revolutionizing connectivity, one path at a time"
date: 2025-01-03
categories: 
  - "general"
---

The Internet is designed to provide multiple paths between two endpoints. Attempts to exploit multi-path opportunities are almost as old as the Internet, culminating in RFCs documenting some of the challenges. Still, today, virtually all end-to-end communication uses only one available path at a time. Why? It turns out that in multi-path setups, even the smallest differences between paths can harm the connection quality due to packet reordering and other issues. As a result, Internet devices usually use a single path and let the routers handle the path selection.

There is another way. Enter Multi-Path TCP (MPTCP), which exploits the presence of multiple interfaces on a device, such as a mobile phone that has both Wi-Fi and cellular antennas, to achieve multi-path connectivity.

MPTCP has had a long history — see the Wikipedia article and the spec (RFC 8684) for details. It's a major extension to the TCP protocol, and historically most of the TCP changes failed to gain traction. However, MPTCP is supposed to be mostly an operating system feature, making it easy to enable. Applications should only need minor code changes to support it.

There is a caveat, however: MPTCP is still fairly immature, and while it can use multiple paths, giving it superpowers over regular TCP, it's not always strictly better than it. Whether MPTCP should be used over TCP is really a case-by-case basis.

In this blog post we show how to set up MPTCP to find out.

## Subflows

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3r8AP5BHbvtYEtXmYSXFwO/36e95cbac93cdecf2f5ee65945abf0b3/Screenshot_2024-12-23_at_3.07.37_PM.png)

Internally, MPTCP extends TCP by introducing "subflows". When everything is working, a single TCP connection can be backed by multiple MPTCP subflows, each using different paths. This is a big deal - a single TCP byte stream is now no longer identified by a single 5-tuple. On Linux you can see the subflows with `ss -M`, like:

```
marek$ ss -tMn dport = :443 | cat
tcp   ESTAB 0  	0 192.168.2.143%enx2800af081bee:57756 104.28.152.1:443
tcp   ESTAB 0  	0       192.168.1.149%wlp0s20f3:44719 104.28.152.1:443
mptcp ESTAB 0  	0                 192.168.2.143:57756 104.28.152.1:443
```

Here you can see a single MPTCP connection, composed of two underlying TCP flows.

## MPTCP aspirations

Being able to separate the lifetime of a connection from the lifetime of a flow allows MPTCP to address two problems present in classical TCP: aggregation and mobility.

- **Aggregation**: MPTCP can aggregate the bandwidth of many network interfaces. For example, in a data center scenario, it's common to use interface bonding. A single flow can make use of just one physical interface. MPTCP, by being able to launch many subflows, can expose greater overall bandwidth. I'm personally not convinced if this is a real problem. As we'll learn below, modern Linux has a BLESS-like MPTCP scheduler and macOS stack has the "aggregation" mode, so aggregation should work, but I'm not sure how practical it is. However, there are certainly projects that are trying to do link aggregation using MPTCP.
    
- **Mobility**: On a customer device, a TCP stream is typically broken if the underlying network interface goes away. This is not an uncommon occurrence — consider a smartphone dropping from Wi-Fi to cellular. MPTCP can fix this — it can create and destroy many subflows over the lifetime of a single connection and survive multiple network changes.
    

Improving reliability for mobile clients is a big deal. While some software can use QUIC, which also works on Multipath Extensions, a large number of classical services still use TCP. A great example is SSH: it would be very nice if you could walk around with a laptop and keep an SSH session open and switch Wi-Fi networks seamlessly, without breaking the connection.

MPTCP work was initially driven by UCLouvain in Belgium. The first serious adoption was on the iPhone. Apparently, users have a tendency to use Siri while they are walking out of their home. It's very common to lose Wi-Fi connectivity while they are doing this. (source) 

## Implementations

Currently, there are only two major MPTCP implementations — Linux kernel support from v5.6, but realistically you need at least kernel v6.1 (MPTCP is not supported on Android yet) and iOS from version 7 / Mac OS X from 10.10.

Typically, Linux is used on the server side, and iOS/macOS as the client. It's possible to get Linux to work as a client-side, but it's not straightforward, as we'll learn soon. Beware — there is plenty of outdated Linux MPTCP documentation. The code has had a bumpy history and at least two different APIs were proposed. See the Linux kernel source for the mainline API and the mptcp.dev website.

## Linux as a server

Conceptually, the MPTCP design is pretty sensible. After the initial TCP handshake, each peer may announce additional addresses (and ports) on which it can be reached. There are two ways of doing this. First, in the handshake TCP packet each peer specifies the "_Do not attempt to establish new subflows to this address and port_" bit, also known as bit \[C\], in the MPTCP TCP extensions header.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/bT8oz3wxpw7alftvdYg5n/b7614a4d10b6c81e18027f6785391ede/BLOG-2637_3.png)

_Wireshark dissecting MPTCP flags from a SYN packet._ _Tcpdump does not report_ _this flag yet._

With this bit cleared, the other peer is free to assume the two-tuple is fine to be reconnected to. Typically, the **server allows** the client to reuse the server IP/port address. Usually, the **client is not listening** and disallows the server to connect back to it. There are caveats though. For example, in the context of Cloudflare, where our servers are using Anycast addressing, reconnecting to the server IP/port won't work. Going twice to the IP/port pair is unlikely to reach the same server. For us it makes sense to set this flag, disallowing clients from reconnecting to our server addresses. This can be done on Linux with:

```
# Linux server sysctl - useful for ECMP or Anycast servers
$ sysctl -w net.mptcp.allow_join_initial_addr_port=0
```

There is also a second way to advertise a listening IP/port. During the lifetime of a connection, a peer can send an ADD-ADDR MPTCP signal which advertises a listening IP/port. This can be managed on Linux by `ip mptcp endpoint ... signal`, like:

```
# Linux server - extra listening address
$ ip mptcp endpoint add 192.51.100.1 dev eth0 port 4321 signal
```

With such a config, a Linux peer (typically server) will report the additional IP/port with ADD-ADDR MPTCP signal in an ACK packet, like this:

```
host > host: Flags [.], ack 1, win 8, options [mptcp 30 add-addr v1 id 1 192.51.100.1:4321 hmac 0x...,nop,nop], length 0
```

It's important to realize that either peer can send ADD-ADDR messages. Unusual as it might sound, it's totally fine for the client to advertise extra listening addresses. The most common scenario though, consists of either nobody, or just a server, sending ADD-ADDR.

Technically, to launch an MPTCP socket on Linux, you just need to replace IPPROTO\_TCP with IPPROTO\_MPTCP in the application code:

```
IPPROTO_MPTCP = 262
sd = socket(AF_INET, SOCK_STREAM, IPPROTO_MPTCP)
```

In practice, though, this introduces some changes to the sockets API. Currently not all setsockopt's work yet — like `TCP_USER_TIMEOUT`. Additionally, at this stage, MPTCP is incompatible with kTLS.

## Path manager / scheduler

Once the peers have exchanged the address information, MPTCP is ready to kick in and perform the magic. There are two independent pieces of logic that MPTCP handles. First, given the address information, MPTCP must figure out if it should establish additional subflows. The component that decides on this is called "Path Manager". Then, another component called "scheduler" is responsible for choosing a specific subflow to transmit the data over.

Both peers have a path manager, but typically only the client uses it. A path manager has a hard task to launch enough subflows to get the benefits, but not too many subflows which could waste resources. This is where the MPTCP stacks get complicated. 

## Linux as client

On Linux, path manager is an operating system feature, not an application feature. The in-kernel path manager requires some configuration — it must know which IP addresses and interfaces are okay to start new subflows. This is configured with `ip mptcp endpoint ... subflow`, like:

```
$ ip mptcp endpoint add dev wlp1s0 192.0.2.3 subflow  # Linux client
```

This informs the path manager that we (typically a client) own a 192.0.2.3 IP address on interface wlp1s0, and that it's fine to use it as source of a new subflow. There are two additional flags that can be passed here: "backup" and "fullmesh". Maintaining these `ip mptcp endpoints` on a client is annoying. They need to be added and removed every time networks change. Fortunately, NetworkManager from 1.40 supports managing these by default. If you want to customize the "backup" or "fullmesh" flags, you can do this here (see the documentation):

```
ubuntu$ cat /etc/NetworkManager/conf.d/95-mptcp.conf
# set "subflow" on all managed "ip mptcp endpoints". 0x22 is the default.
[connection]
connection.mptcp-flags=0x22
```

Path manager also takes a "limit" setting, to set a cap of additional subflows per MPTCP connection, and limit the received ADD-ADDR messages, like: 

```
$ ip mptcp limits set subflow 4 add_addr_accepted 2  # Linux client
```

I experimented with the "mobility" use case on my Ubuntu 22 Linux laptop. I repeatedly enabled and disabled Wi-Fi and Ethernet. On new kernels (v6.12), it works, and I was able to hold a reliable MPTCP connection over many interface changes. I was less lucky with the Ubuntu v6.8 kernel. Unfortunately, the default path manager on Linux client only works when the flag "_Do not attempt to establish new subflows to this address and port_" is cleared on the server. Server-announced ADD-ADDR don't result in new subflows created, unless `ip mptcp endpoint` has a `fullmesh` flag.

It feels like the underlying MPTCP transport code works, but the path manager requires a bit more intelligence. With a new kernel, it's possible to get the "interactive" case working out of the box, but not for the ADD-ADDR case. 

## Custom path manager

Linux allows for two implementations of a path manager component. It can either use built-in kernel implementation (default), or userspace netlink daemon.

```
$ sysctl -w net.mptcp.pm_type=1 # use userspace path manager
```

However, from what I found there is no serious implementation of configurable userspace path manager. The existing implementations don't do much, and the API seems immature yet.

## Scheduler and BPF extensions

Thus far we've covered Path Manager, but what about the scheduler that chooses which link to actually use? It seems that on Linux there is only one built-in "default" scheduler, and it can do basic failover on packet loss. The developers want to write MPTCP schedulers in BPF, and this work is in-progress.

## macOS

As opposed to Linux, macOS and iOS expose a raw MPTCP API. On those operating systems, path manager is not handled by the kernel, but instead can be an application responsibility. The exposed low-level API is based on `connectx()`. For example, here's an example of obscure code that establishes one connection with two subflows:

```
int sock = socket(AF_MULTIPATH, SOCK_STREAM, 0);
connectx(sock, ..., &cid1);
connectx(sock, ..., &cid2);
```

This powerful API is hard to use though, as it would require every application to listen for network changes. Fortunately, macOS and iOS also expose higher-level APIs. One example is nw\_connection in C, which uses nw\_parameters\_set\_multipath\_service.

Another, more common example is using `Network.framework`, and would look like this:

```
let parameters = NWParameters.tcp
parameters.multipathServiceType = .interactive
let connection = NWConnection(host: host, port: port, using: parameters) 
```

The API supports three MPTCP service type modes:

- _Handover Mode_: Tries to minimize cellular. Uses only Wi-Fi. Uses cellular only when Wi-Fi Assist is enabled and makes such a decision.
    
- _Interactive Mode_: Used for Siri. Reduces latency. Only for low-bandwidth flows.
    
- _Aggregation Mode_: Enables resource pooling but it's only available for developer accounts and not deployable.
    

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/47MukOs6bhCMOkO1JL15sP/7dd75417b855b681bde504122d5af01e/Screenshot_2024-12-23_at_2.59.51_PM.png)

The MPTCP API is nicely integrated with the iPhone "Wi-Fi Assist" feature. While the official documentation is lacking, it's possible to find sources explaining how it actually works. I was able to successfully test both the cleared "_Do not attempt to establish new subflows"_ bit and ADD-ADDR scenarios. Hurray!

## IPv6 caveat

Sadly, MPTCP IPv6 has a caveat. Since IPv6 addresses are long, and MPTCP uses the space-constrained TCP Extensions field, there is not enough room for ADD-ADDR messages if TCP timestamps are enabled. If you want to use MPTCP and IPv6, it's something to consider.

## Summary

I find MPTCP very exciting, being one of a few deployable serious TCP extensions. However, current implementations are limited. My experimentation showed that the only practical scenario where currently MPTCP might be useful is:

- Linux as a server
    
- macOS/iOS as a client
    
- "interactive" use case
    

With a bit of effort, Linux can be made to work as a client.

Don't get me wrong, Linux developers did tremendous work to get where we are, but, in my opinion for any serious out-of-the-box use case, we're not there yet. I'm optimistic that Linux can develop a good MPTCP client story relatively soon, and the possibility of implementing the Path manager and Scheduler in BPF is really enticing. 

Time will tell if MPTCP succeeds — it's been 15 years in the making. In the meantime, Multi-Path QUIC is under active development, but it's even further from being usable at this stage.

We're not quite sure if it makes sense for Cloudflare to support MPTCP. Reach out if you have a use case in mind!

_Shoutout to_ _Matthieu Baerts_ _for tremendous help with this blog post._

Go to Source
