---
title: "<div>New VPN Backdoor</div>"
date: 2025-01-29
categories: 
  - "backdoors"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "malware"
  - "security"
  - "security-awareness"
  - "vpn"
  - "vulnerabilities"
---

A newly discovered VPN backdoor uses some interesting tactics to avoid detection:

> When threat actors use backdoor malware to gain access to a network, they want to make sure all their hard work can’t be leveraged by competing groups or detected by defenders. One countermeasure is to equip the backdoor with a passive agent that remains dormant until it receives what’s known in the business as a “magic packet.” On Thursday, researchers revealed that a never-before-seen backdoor that quietly took hold of dozens of enterprise VPNs running Juniper Network’s Junos OS has been doing just that...

A newly discovered VPN backdoor uses some interesting tactics to avoid detection:

> When threat actors use backdoor malware to gain access to a network, they want to make sure all their hard work can’t be leveraged by competing groups or detected by defenders. One countermeasure is to equip the backdoor with a passive agent that remains dormant until it receives what’s known in the business as a “magic packet.” On Thursday, researchers revealed that a never-before-seen backdoor that quietly took hold of dozens of enterprise VPNs running Juniper Network’s Junos OS has been doing just that.
> 
> J-Magic, the tracking name for the backdoor, goes one step further to prevent unauthorized access. After receiving a magic packet hidden in the normal flow of TCP traffic, it relays a challenge to the device that sent it. The challenge comes in the form of a string of text that’s encrypted using the public portion of an RSA key. The initiating party must then respond with the corresponding plaintext, proving it has access to the secret key.
> 
> The lightweight backdoor is also notable because it resided only in memory, a trait that makes detection harder for defenders. The combination prompted researchers at Lumin Technology’s Black Lotus Lab to sit up and take notice.
> 
> \[…\]
> 
> The researchers found J-Magic on VirusTotal and determined that it had run inside the networks of 36 organizations. They still don’t know how the backdoor got installed.

Slashdot thread.

Go to Source
