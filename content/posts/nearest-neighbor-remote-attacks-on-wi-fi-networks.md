---
title: "Nearest Neighbor: remote attacks on Wi-Fi networks"
date: 2025-01-03
categories: 
  - "cybersecurity"
  - "security"
---

How the Nearest Neighbor tactic can be used in remote attacks on an organization's wireless network — and how to protect yourself against this threat.

From the perspective of information security, wireless networks are typically perceived as something that can be accessed only locally — to connect to them, an attacker needs to be physically close to the access point. This significantly limits their use in attacks on organizations, and so they are perceived as relatively risk-free. It’s easy to think that some random hacker on the internet could never simply connect to a corporate Wi-Fi network. However, the newly emerged Nearest Neighbor attack tactic demonstrates that this perception is not entirely accurate.

Even a well-protected organization’s wireless network can become a convenient entry point for remote attackers if they first compromise another, more vulnerable company located in the same building or a neighboring one. Let’s delve deeper into how this works and how to protect yourself against such attacks.

## A remote attack on an organization’s wireless network

Let’s imagine a group of attackers planning to remotely hack into an organization. They gather information about the given company, investigate its external perimeter, and perhaps even find employee credentials in databases of leaked passwords. But they find no exploitable vulnerabilities. Moreover, they discover that all of the company’s external services are protected by two-factor authentication, so passwords alone aren’t sufficient for access.

One potential penetration method could be the corporate Wi-Fi network, which they could attempt to access using those same employee credentials. This applies especially if the organization has a guest Wi-Fi network that’s insufficiently isolated from the main network — such networks rarely use two-factor authentication. However, there’s a problem: the attackers are on the other side of the globe and can’t physically connect to the office Wi-Fi.

This is where the Nearest Neighbor tactic comes into play. If the attackers conduct additional reconnaissance, they’ll most likely discover numerous other organizations whose offices are within the Wi-Fi signal range of the target company. And it’s possible that some of those neighboring organizations are significantly more vulnerable than the attackers’ initial target.

This may simply be because these organizations believe their activities are less interesting to cyberattack operators — leading to less stringent security measures. For example, they might not use two-factor authentication for their external resources. Or they may fail to update their software promptly — leaving easily exploitable vulnerabilities exposed.

One way or another, it’s easier for the attackers to gain access to one of these neighboring organizations’ networks. Next, they need to find within the neighbor’s infrastructure a device connected to the wired network and equipped with a wireless module, and compromise it. By scanning the Wi-Fi environment through such a device, the attackers can locate the SSID of the target company’s network.

Using the compromised neighboring device as a bridge, the attackers can then connect to the corporate Wi-Fi network of their actual target. In this way, they get inside the perimeter of the target organization. Having achieved this initial objective, the attackers can proceed with their main goals — stealing information, encrypting data, monitoring employee activity, and more.

## How to protect yourself against the Nearest Neighbor attack

It’s worth noting that this tactic has already been used by at least one APT group, so this isn’t just a theoretical threat. Organizations that could be targeted by such attacks should start treating the security of their wireless local area networks as seriously as the security of their internet-connected resources.

To protect against the Nearest Neighbor attack, we recommend the following:

- Ensure that the guest Wi-Fi network is truly isolated from the main network.
- Strengthen the security of corporate Wi-Fi access — for instance, by using two-factor authentication with one-time codes or certificates.
- Enable two-factor authentication — not only for external resources but also for internal ones, and, in general, adopt the Zero Trust security model.
- Use an advanced threat detection and prevention system, such as Kaspersky Next XDR Expert.
- If you lack highly qualified in-house cybersecurity specialists, make use of external services such as Managed Detection and Response and Incident Response.

Go to Source
