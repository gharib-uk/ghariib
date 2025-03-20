---
title: "<div>What’s new in Cloudflare: MASQUE now powers 1.1.1.1 & WARP apps, DEX now generally available with Remote Captures</div>"
date: 2025-01-02
categories: 
  - "general"
---

At Cloudflare, we are constantly innovating and launching new features and capabilities across our product portfolio. Today’s roundup blog post shares two exciting updates across our platform: our cross-platform 1.1.1.1 & WARP applications (consumer) and device agents (Zero Trust)  now use MASQUE, a cutting-edge HTTP/3\-based protocol, to secure your Internet connection. Additionally, DEX is now available for general availability.

## Faster and more stable: our 1.1.1.1 & WARP apps now use MASQUE by default

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6CghJvmC5DBnhKLM36MY3O/ecf722a9d9b5a4e4a048afea06237749/image1.png)

We’re excited to announce that as of today, our cross-platform 1.1.1.1 & WARP apps now use MASQUE, a cutting-edge HTTP/3\-based protocol, to secure your Internet connection.

As a reminder, our 1.1.1.1 & WARP apps have two main functions: send all DNS queries through 1.1.1.1, our privacy-preserving DNS resolver, and protect your device’s network traffic via WARP by creating a private and encrypted tunnel to the resources you’re accessing, preventing unwanted third parties or public Wi-Fi networks from snooping on your traffic.

There are many ways to encrypt and proxy Internet traffic — you may have heard of a few, such as IPSec, WireGuard, or OpenVPN. There are many tradeoffs we considered when choosing a protocol, but we believe MASQUE is the future of fast, secure, and stable Internet proxying, it aligns with our belief in building on top of open Internet standards, and we’ve deployed it successfully at scale for customers like iCloud Private Relay and Microsoft Edge Secure Network.

### Why MASQUE?

**MASQUE** is a modern framework for proxying traffic that allows a variety of application protocols, including HTTP/3, to utilize QUIC as their transport mechanism. That’s a lot of acronyms, so let's make sure those are clear. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6XkQ3rF8oo8JaG0Iujskia/6383b0c0bce36a94298960c163495843/image4.png)

**QUIC** is a general-purpose transport protocol and Internet standard that operates on top of UDP (instead of TCP), is encrypted by default, and solves several performance issues that plagued its predecessors. **HTTP/3** is the latest version of the HTTP protocol, defining the application-layer protocol that runs on top of QUIC as its transport mechanism. MASQUE is a set of mechanisms for tunneling traffic over HTTP. It extends the existing HTTP CONNECT model, to allow tunneling UDP and IP traffic. This is especially efficient when combined with the QUIC’s unreliable datagram extension.

For example, we can use MASQUE’s CONNECT-IP method to establish a tunnel that can send multiple concurrent requests over a single QUIC connection:

```
HEADERS
:method = CONNECT
:protocol = connect-ip
:scheme = https
:path = /.well-known/masque/ip/*/*/
:authority = example.org
capsule-protocol = ?1
```

The benefit these protocols have for the quality and security of everyone’s Internet browsing experience is real. Earlier transport protocols were built before the advent of smartphones and mobile networks, so QUIC was designed to support a mobile world, maintaining connections even in poorly connected networks, and minimizing disruptions as people switch rapidly between networks as they move through their day. Leveraging HTTP/3 as the application layer means that MASQUE is more like “normal” HTTP traffic on the Internet, meaning that it is easier to support, is compatible with existing firewall and security rules, and that it supports cryptographic agility (i.e. support for post-quantum crypto), making this traffic more secure and resilient in the long term.

### Get started now 

All new installations of our 1.1.1.1 & WARP apps support MASQUE, including iOS, Android, macOS, Windows, and Linux, and we’ve started to roll it out as the preferred protocol over WireGuard. On mobile, to check if your connection is already secured over MASQUE, or change your device’s default option, you can toggle this setting via _Advanced > Connection options > Tunnel protocol:_

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3c7lAh7C5huXDUYt4v7B7w/a089967f8d9d668b2ded321f40b35cf4/Screenshot_2024-12-23_at_18.26.20.png)

_Protocol connection options shown here on the iOS app_

We offer the following options: 

- **Auto**: this allows the app to choose the protocol.
    
- **MASQUE**: always use MASQUE to secure your connection.
    
- **WireGuard**: always use WireGuard to secure your connection.
    

On desktop versions, you can switch the protocol by using the WARP command-line interface. For example:

```
warp-cli tunnel protocol set WireGuard
warp-cli tunnel protocol set MASQUE
```

With this rollout, we're excited to see MASQUE deliver increased performance and stability to millions of users. Download one of the WARP apps today!

## DEX now Generally Available: Announcing detailed device visibility with DEX Remote Captures

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2RkuqjgXZh8tmoj4W1narK/baaf61dcde00bbfa4cef71e5dbd2cc23/image2.png)

_Following the successful beta launch of Digital Experience Monitoring (DEX), we are thrilled to announce the general availability of DEX, along with new Remote Captures functionality._

In today's hyper distributed environment, user experience is paramount. Recurring performance problems can lead to decreased user satisfaction, lost productivity, and damaged brand reputation.  Digital Experience Monitoring (DEX) offers a comprehensive solution to these challenges. Previous blog posts have discussed the solution and its capabilities. (_Introducing Digital Experience Monitoring__,_ _Understanding end user-connectivity and performance with Digital Experience Monitoring, now available in beta__,_ _What's new in Cloudflare One: Digital Experience monitoring notifications_)

### Introducing Remote Captures: PCAP and WARP Diag

Imagine this: an end user is frustrated with a slow application, and your IT team is struggling to pinpoint the root cause. Traditionally, troubleshooting such issues involved contacting the end user and asking them to manually collect and share network traffic data. This process is time-consuming, prone to errors, and often disruptive to the end user's workflow.

Building upon the capabilities of DEX, we are excited to introduce Remote Captures, a powerful new feature that empowers IT admins to gain unprecedented visibility into end-user devices and network performance. DEX now introduces Remote Captures, a powerful new feature that empowers IT admins to remotely initiate network packet captures (PCAP) and WARP Diag logs directly from your end users’ devices and capture diagnostic information automatically from our device client. This streamlined approach accelerates troubleshooting, reduces the burden on end users, and provides valuable insights into network performance and security.

### Why Remote Captures?

Remote Captures offer several key advantages. By analyzing detailed network traffic, IT teams can quickly pinpoint the root cause of network issues. Furthermore, granular network data empowers security teams to proactively detect and investigate potential threats. Finally, by identifying bottlenecks and latency issues, Remote Captures enable organizations to optimize network performance for a smoother user experience.

### How Remote Captures work

Initiating a Remote Capture is straightforward. First, select the specific device you wish to troubleshoot. Then, with a few simple clicks, start capturing network traffic and/or WARP Diag data. Once the capture is complete, download the captured data and utilize your preferred tools for in-depth analysis.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5NWQAhlUK8OQvuydQV0lb7/d93f6792e897120aa5e2f837a6ec7786/image3.png)

### Get started today

DEX Remote Captures are now available for Cloudflare One customers. They can be configured by going to Cloudflare Dashboard >  Zero Trust > DEX > Remote Captures, and then selecting the device you wish to collect from. For more information, refer to Remote captures. This new capability highlights just one of the many ways our unified SASE platform helps organizations find and fix security issues across SaaS applications. Try it out now using our free tier to get started.

## Never miss an update 

We hope you enjoy reading our roundup blog posts as we continue to build and innovate. Stay tuned to the Cloudflare Blog for the latest news and updates.

Go to Source
