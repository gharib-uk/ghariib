---
title: "<div>Why content providers need IPv6</div>"
date: 2025-03-19
categories: 
  - "general"
---

IPv4 is an expensive resource. However, many content providers are still IPv4-only. The most common reason is that IPv4 is here to stay and IPv6 is an additional complexity.1 This mindset may seem selfish, but there are compelling reasons for a content provider to enable IPv6, even when they have enough IPv4 addresses available for their needs.

Disclaimer

It’s been a while since this article has been in my drafts. I started it when I was working at Shadow, a content provider, while I now work for Free, an internet service provider.

# Why ISPs need IPv6?

Providing a public IPv4 address to each customer is quite costly when each IP address costs US$40 on the market. For fixed access, some consumer ISPs are still providing one IPv4 address per customer.2 Other ISPs provide, by default, an IPv4 address shared among several customers. For mobile access, most ISPs distribute a shared IPv4 address.

There are several methods to share an IPv4 address:3

NAT44

The customer device is given a private IPv4 address, which is translated to a public one by a service provider device. This device needs to maintain a state for each translation.

464XLAT and DS-Lite

The customer device translates the private IPv4 address to an IPv6 address or encapsulates IPv4 traffic in IPv6 packets. The provider device then translates the IPv6 address to a public IPv4 address. It still needs to maintain a state for the NAT64 translation.

Lightweight IPv4 over IPv6, MAP-E, and MAP-T

The customer device encapsulates IPv4 in IPv6 packets or performs a stateless NAT46 translation. The provider device uses a binding table or an algorithmic rule to map IPv6 tunnels to IPv4 addresses and ports. It does not need to maintain a state.

<figure>

![Solutions to share an IPv4 address](https://d2pzklc15kok91.cloudfront.net/images/rfc9313.0517281a53fda5.svg)

<figcaption>

Solutions to share an IPv4 address across several customers. Some of them require the ISP to keep state, some don't.

</figcaption>

</figure>

All these solutions require a translation device in the ISP’s network. This device represents a non-negligible cost in terms of money and reliability. As half of the top 1000 websites support IPv6 and the biggest players can deliver most of their traffic using IPv6,4 ISPs have a clear path to reduce the cost of translation devices: **provide IPv6 by default to their customers**.

# Why content providers need IPv6?

Content providers should expose their services over IPv6 primarily to **avoid going through the ISP’s translation devices**. This doesn’t help users who don’t have IPv6 or users with a non-shared IPv4 address, but it provides a better service for all the others.

Why would the service be better delivered over IPv6 than over IPv4 when a translation device is in the path? There are two main reasons for that:5

1. Translation devices introduce additional latency due to their geographical placement inside the network: it is easier and cheaper to only install these devices at a few points in the network instead of putting them close to the users.
    
2. Translation devices are an additional point of failure in the path between the user and the content. They can become overloaded or malfunction. Moreover, as they are not used for the five most visited websites, which serve their traffic over IPv6, the ISPs may not be incentivized to ensure they perform as well as the native IPv6 path.
    

Looking at Google statistics, half of the users reach Google over IPv6. Moreover, their latency is lower.6 In the US, all the nationwide mobile providers have IPv6 enabled.

For France, we can refer to the annual ARCEP report: in 2022, 72% of fixed users and 60% of mobile users had IPv6 enabled, with projections of 94% and 88% for 2025. Starting from this projection, since all mobile users go through a network translation device, content providers can deliver a better service for 88% of them by exposing their services over IPv6. If we exclude Orange, which has 40% of the market share on consumer fixed access, enabling IPv6 should positively impact more than 55% of fixed access users.

* * *

In conclusion, content providers aiming for the best user experience _should_ expose their services over IPv6. By avoiding translation devices, they can ensure fast and reliable content delivery. This is crucial for latency-sensitive applications, like live streaming, but also for websites in competitive markets, where even slight delays can lead to user disengagement.

* * *

1. A way to limit this complexity is to build IPv6 services and only provide IPv4 through reverse proxies at the edge. ↩︎
    
2. In France, this includes non-profit ISPs, like FDN and Milkywan. Additionally, Orange, the previously state-owned telecom provider, supplies non-shared IPv4 addresses. Free also provides a dedicated IPv4 address for customers connected to the point-to-point FTTH access. ↩︎
    
3. I use the term NAT instead of the more correct term NAPT. Feel free to do a mental substitution. If you are curious, check RFC 2663. For a survey of the IPv6 transition technologies enumerated here, have a look at RFC 9313. ↩︎
    
4. For AS 12322, Google, Netflix, and Meta are delivering 85% of their traffic over IPv6. Also, more than half of our traffic is delivered over IPv6. ↩︎
    
5. An additional reason is for fighting abuse: blacklisting an IPv4 address may impact unrelated users who share the same IPv4 as the culprits. ↩︎
    
6. IPv6 may not be the sole reason the latency is lower: users with IPv6 generally have a better connection. ↩︎
