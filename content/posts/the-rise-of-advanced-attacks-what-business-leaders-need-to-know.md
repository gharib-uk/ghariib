---
title: "The Rise of Advanced Attacks — What Business Leaders Need to Know"
date: 2025-03-19
---

Cyberthreats are evolving at an alarming rate, thanks to cybercriminals’ use of advanced technologies, like AI, to develop more sophisticated attacks that no one has ever seen before. Today, we’re seeing attackers leveraging automation, artificial intelligence and adaptive malware to exfiltrate data in ways that bypass standard detection mechanisms (i.e., security solutions that focus only on perimeter defenses).

One example of the latest tactics bad actors are using is called _Relayed Data Exfiltration via HTTP Headers_. This type of attack uses stealthy siphoning techniques to steal sensitive business data, customer information or intellectual property without triggering alarms. Unlike brute-force cyberattacks that immediately lock systems, these attacks are like a slow drip of stolen data.

The attackers hitch a ride through your HTTP headers, using them as a covert pathway to exfiltrate data out of your organization while disguising their activity as normal web traffic. Instead of sending stolen data directly to the attacker, small chunks of information are embedded in cleverly crafted domains and sent to trusted internet services. When these services process the domains, they unknowingly forward the hidden data to the attacker via DNS. Since many security tools don't inspect HTTP headers for hidden data exfiltration, attackers can easily steal your data using this innovative new technique.

<figure>

![Introducing Exfiltration Shield: prevent data exfiltration via DNS Relay Attacks.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-335878-1.png)

<figcaption>

Exfiltration Shield blocks data exfiltration by preventing DNS relay attacks via HTTP headers.

</figcaption>

</figure>

This is just one example of novel attacks being waged against unwitting victims. As businesses adopt cloud technologies and remote work, cybercriminals have more entry points to exploit, which means that businesses must rethink their security strategies to stay ahead of emerging threats.

### **Protecting Against the Latest Attack Techniques — The Proof Is in the Data**

That’s why Palo Alto Networks security services are always evolving to detect and neutralize the most stealthy and evasive threats before they can do harm. We do this through a combination of real-time monitoring, GenAI-based real-time threat detection and advanced threat intelligence, to stay ahead of threats. We then introduce new security enhancements to keep you protected. And we have the numbers to prove it.

Our latest advancements in CDSS show the impact of our continuous innovation and the increasingly complex threat environment:

- We’ve seen an **18% increase in events** analyzed daily, from an average ≤4.6 billion to now ≤5.43 billion, which includes benign and malicious activity across files, URLs, domains and network sessions.
- What’s most troubling is that **new and unique attacks have increased by ~4X** every day (increasing from ≤2.3 million to ≤8.95 million), which includes detections of new threats like relayed attacks.
- Of all these threats, we’re **blocking ~3X more attacks inline,** jumping from ≤11.3 billion to ≤30.9 billion each day, stopping them in real-time before they reach the network, endpoint or user, preventing damage before it even starts.

When security teams need to defend against an overwhelming volume of real-time threats, the ability to analyze, identify and block attacks faster and with greater precision is critical. With Palo Alto Networks, your security is constantly getting better. New features are being added to protect against the latest attack techniques without you needing to change your software. These improvements mean that security teams can detect, respond and prevent attacks at unprecedented scale and speed:

- **Reduced Risk of Compromise –** More threats are identified before they reach your network.
- **Greater Efficiency –** High-fidelity detection reduces time spent investigating unnecessary threats.
- **Stronger Overall Security Posture –** Inline blocking prevents attacks before damage occurs.

### The Next Evolution

In today’s rapidly evolving threat landscape, staying ahead isn’t just a goal, it’s a commitment.

With more accurate, real-time data, organizations can make informed security decisions, minimize exposure and prevent breaches before they happen. And as threats continue to evolve, so do we.

We introduced a new feature, Exfiltration Shield, that combines relay detection in Advanced Threat Prevention (ATP) with a fully qualified domain name (FQDN) validation in Advanced DNS (ADNS) to extract HTTP requests and verify domains in real time, preventing attackers from using trusted domains to exfiltrate data undetected. In essence, it stops attackers from using HTTP headers to exfiltrate data by blocking this technique in real time, closing yet another pathway for cybercriminals to exploit.

At Palo Alto Networks, keeping pace with evolving threats means continuously pushing the boundaries of security. With more data analyzed, more threats identified and more attacks stopped in real time, we’re delivering on our promise to keep you protected. And we’re not stopping here. Stay tuned for what’s next.

_Exfiltration Shield is now generally available (GA). If you’re a current ATP customer, you can start protecting your organization from data exfiltration immediately by_ _enabling inline cloud analysis_ _in your configuration. If you’re not yet an ATP customer, now is the time to upgrade._ _Contact a sales representative_ _to learn how you can get best-in-class security that stops evasive threats before they cause harm._

The post The Rise of Advanced Attacks — What Business Leaders Need to Know appeared first on Palo Alto Networks Blog.

Go to Source
