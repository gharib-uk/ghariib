---
title: "<div>Microsoft Defender XDR demonstrates 100% detection coverage across all cyberattack stages in the 2024 MITRE ATT&CK® Evaluations: Enterprise​​</div>"
date: 2025-01-02
categories: 
  - "security"
---

## Delivering industry-leading detection for a sixth consecutive year

For the sixth year in a row, Microsoft Defender XDR demonstrated industry-leading extended detection and response (XDR) capabilities in the independent **MITRE ATT&CK® Evaluations: Enterprise.** The cyberattack used during the detection test highlights the importance of a unified XDR platform and showcases Defender XDR as a leading solution for securing your multi-operating system estate, with the following results:

<figure>

![Three charts showing Microsoft’s technique coverage across the three different attack scenarios for MITRE’s Detection test.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture1-4-1024x576.webp)

<figcaption>

_Figure 1. Diagram of Microsoft Defender XDR’s MITRE Tactics, Techniques, and Procedures (TTP) coverage for all cyberattack stages in Detection._

</figcaption>

</figure>

- **Achieved industry-leading, cross-platform detection**: 100% technique level detections across all attack stages for Linux and macOS threats leveraging our new extended Berkeley Packet Filter (eBPF) Linux sensor and macOS behavioral monitoring engine that delivers rich actionable content.

- **Delivered zero false positives**, providing powerful security without overwhelming the security operations center (SOC). Defender XDR accurately alerted on and blocked only malicious activity every time so the SOC can focus their limited time and resources on responding to real cyberthreats at hand. Key to this result are critical cross-platform capabilities like remote encryption detection for gaining deeper visibility into the cyberattacker’s machines and behavior monitoring for detecting emerging threats on macOS.

- **Equips the SOC with powerful technology like Microsoft Security Copilot**, the industry’s first generative AI for security, to thwart attacks with contextual insight and speed with capabilities like script analysis that translates obfuscated PowerShell scripts into intuitive explanations of a script’s role in the cyberattack.

- **Deep visibility into remote encryption,** providing unprecedented visibility into encryption attempts originating from remote machines that might not even be onboarded to Defender XDR and putting an end to an advanced cyberattack vector being used in over 70% of recent ransomware cases.¹

![Decorative moving image with various dots](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/MSFT_M365_Apr_SecurityGIF11_Blog_GIF_240410_FINAL.gif)

## Microsoft Defender XDR

Supercharge your SecOps effectiveness with XDR.

Learn more

Defender XDR is the industry’s broadest natively integrated XDR platform spanning **endpoints, hybrid identities, email, collaboration tools, software as a service (SaaS) apps, and data** with centralized visibility, powerful analytics, and automatic attack disruption, a powerful response capability unique to Microsoft. 

>  _A note on this year’s emulation: It is Microsoft’s opinion that the Protection test does not mirror realistic cyberthreats that organizations face. The Protection test methodology differed significantly from the Detection test that emulated an end-to-end attack scenario reflective of the cyberthreat landscape.  See our statement below._ 

## Customer reality is core to Microsoft’s testing approach

Microsoft Security’s mission is to build a safer world while enabling all organizations, users, and services to be as productive as possible. On the ground this means equipping security analysts with a holistic, actionable view of the cyberthreat landscape to minimize time to remediate legitimate bad actors.

As we develop our product, we strive to find the right balance between providing industry-leading security while ensuring under-sourced security operations teams are not flooded with false positives. We hold ourselves accountable for delivering on this goal by regularly participating in product evaluations to identify gaps and improve our products. This year, our conclusion from the MITRE protection test is that it was designed to evade protection mechanisms to the extent that it is unrepresentative of an actual cyberattack, a methodology that Microsoft disagrees with.

The core issue is the micro-testing methodology, which is inconsistent with how cyberattackers typically operate, moving laterally within organizations by gaining access to identities and privileges over time. These broader signals are critical for distinguishing between benign and malicious activities so we can balance protecting organizations from cyberattacks while supporting the broadest set of benign use cases across a massive customer base worldwide.

For example, MITRE used “micro emulations” starting with a highly privileged user and applications signed by a trusted certificate  to conduct cyberattack steps in isolation without adequate context. Signed apps executed by privileged users is a benign scenario we see on thousands of Windows machines a day. Using a trusted certificate isn’t suspicious unless the associated user was compromised—context that the MITRE test lacked. Nor were there signals provided to enable us to determine that the certificate in the trusted root authority had been compromised or was seen to be signing malicious applications.

Microsoft will not implement the test’s recommendations as they do not reflect cyberattack patterns on customer environments. Doing so would cause outages for legitimate customer scenarios.

We appreciate the ongoing collaborative dialogue with MITRE on the topic of testing methodology and look forward to our continued partnership into the future.

## How Microsoft fended off adversaries in the Detection test

In previous evaluations, MITRE scoped emulated behaviors to a specific cyberthreat actor group, like Secret Blizzard. This year, MITRE has added ransomware as an attack category informing a range of malicious behaviors carried out against Windows and Linux. For the macOS portion of the emulation, MITRE applied adversarial behaviors inspired by cyberthreat actors that the Democratic People’s Republic of North Korea (DPRK) sponsors. Microsoft Threat Intelligence tracks these groups at a granular level, for example, Sapphire Sleet, Ruby Sleet, Moonstone Sleet, and others that commonly escalate privileges and target user credentials on macOS.

<figure>

![Image showing bar charts comparing the MITRE TTP coverage for all participating vendors in this year’s MITRE Detection test. ](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/er6_overall_final-1024x539.webp)

<figcaption>

_Figure 2. Diagram of participating vendors’ TTP coverage for all cyberattack stages in Detection_

</figcaption>

</figure>

Let’s take a closer look at how Microsoft Defender XDR once again achieved industry-leading results in this year’s MITRE evaluation and how Microsoft is shaping the future of security to respond to the most prevalent cyberthreats like ransomware.

## A leader in detection for every cyberattack stage: 100% technique level detections for Linux and macOS cyberthreats

<figure>

![Image showing bar charts comparing the MITRE TTP coverage for all participating vendors for Linux and macOS in year’s MITRE Detection test.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/ER6_Mac_PLUS_linux-1024x539.webp)

<figcaption>

F_igure 3. Diagram of Microsoft Defender XDR’s MITRE TTP coverage for Linux and macOS in Detection_

</figcaption>

</figure>

Organizations often have diverse digital estates spanning multiple operating systems, which is why Microsoft invests heavily in ensuring detection for all major operating systems is both accurate and actionable. Microsoft’s industry-leading cross-platform results are driven by a combination of continuous investments, such as:

_1\. Extending our generative AI solution, Security Copilot, beyond Windows._

Security Copilot is the only security AI product that combines a specialized language model with security-specific capabilities from Microsoft. These capabilities incorporate a growing set of security-specific skills informed by our unique global threat intelligence and more than 78 trillion daily signals. Summarizing incidents, guiding response actions, using natural language for advanced threat hunting, and analyzing obfuscated PowerShell scripts are just some of the ways Security Copilot helps analysts accelerate workflows and gain new skills. In this evaluation, script analysis played a key role for macOS where we see human-readable explanations alongside the code as well as MITRE Tactics, Techniques, and Procedures (TTPs). This way analysts can quickly understand how the adversary is using the file or script.

<figure>

![A screenshot showing Security Copilot analyzing malicious script that was used in the MITRE emulation.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture2-2-1024x518.webp)

<figcaption>

_Figure 4: Step 2.5 – System Services – launchctl (T1569.001), alongside Security Copilot script analysis for macOS that makes alerts more actionable_

</figcaption>

</figure>

_2\. Delivering enhanced behavioral monitoring capabilities to detect emerging cyberthreats even earlier on macOS._

Effective security is about the quality and actionability of detections, not just the quantity. These principles guide how we’ve built industry-leading security across Windows, Linux, and macOS. Let’s look at step Mac 4.08 Credentials from Password Stores: Keychain by a suspicious file as an example. Keychain-related file access happens often on macOS, even when a machine is idle. On average, these files may be accessed well over 400 times per hour. This level of activity is normal for many popular applications, such as OneDrive, Adobe Creative Cloud, and the built-in macOS apps. However, sorting out normal versus suspicious access poses a significant challenge for many vendors. We gain this deeper analysis through a combination of advanced behavior monitoring and content scanning, along with Microsoft’s exclusive threat intelligence. This approach helps pinpoint genuinely suspicious access, like those from us.zoom.ZoomHelperTool, providing analysts with the precise data they need to respond effectively.

<figure>

![A screenshot of the Defender XDR portal showing deeper context around a particular part of the attack pertaining to macOS.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture3-1-1024x671.webp)

<figcaption>

_Figure 5. MacOS and our cross-platform customers also receive the full context richness provided to Windows around what a malicious file capabilities are which includes a list of MITRE TTPs, strings, imports, and many other file attributes to provide comprehensive context of a cyberattack within a singular portal experience._  

</figcaption>

</figure>

<figure>

![A screenshot of the Defender XDR portal showing an even deeper context analysis on a macOS suspicious file alert.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture4-2-1024x676.webp)

<figcaption>

_Figure 6. macOS suspicious file alert with a clear description and information on why us.zoom.ZoomHelperTool was considered a suspicious file. Generated Information generated by multivariate machine learning models_.

</figcaption>

</figure>

## Zero false positives across Linux, macOS, and Windows

When benign activities are flagged as malicious, security analysts end up wasting time and resources investigating. At a scale of potentially hundreds to thousands of alerts a day, false positives quickly lead to team burnout and eroded trust in security measures. This year, MITRE introduced a false positive metric by weaving in innocuous actions like legitimate file-sharing in the cyberattack steps to see if evaluated solutions would generate unnecessary alerts. Microsoft employs machine learning-based detections, only alerting on anomalous activity that seems to originate from malicious intent. This approach is how we deliver powerful security without overwhelming the SOC.

Microsoft’s dedication to protection with minimal false positives is evident in regularly occurring, public antivirus assessments conducted by endpoint testing authorities like AV-Comparatives, AV-Test, and SE Labs.

<figure>

![A bar chart comparing the performance of participating vendors in generating false positives in the MITRE Detection test.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/ER6_FPs_fin_2.webp)

<figcaption>

_Figure 7. Number of false positives generated in this year’s MITRE evaluation._

</figcaption>

</figure>

## Deep visibility into remote encryption attempts 

Since 2022, Microsoft has observed a spike in cyberattackers using remote encryption, where a cyberattacker uses a compromised device to encrypt other devices in a given network. As the latest Microsoft Digital Defense Report points out, 70% percent of successful human-operated ransomware cyberattacks have applied this technique. Gaining insight into a cyberattacker’s machine is typically a blind spot for many antivirus and endpoint detection and response solutions. Defender XDR, however, provides analysts with this critical visibility so that even if an unmanaged device is compromised, it can protect your hybrid organization from advanced cyberattacks like ransomware.

<figure>

![A screenshot of the Defender XDR portal showing visibility into a remote device used in the attack, evidence of Microsoft’s ability to protect against it. ](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture7-1.webp)

<figcaption>

_Figure 8. Step 16.36 – Data Encrypted for Impact (T1486)_

</figcaption>

</figure>

## Empowering defenders with the security they need

As the cyberthreat landscape rapidly evolves, Microsoft is committed to empowering defenders with industry-leading, cross-platform XDR. Our evaluation philosophy is to reflect the real world by configuring the product as customers would in line with industry best practices. In the MITRE Evaluations, as with all simulations, Microsoft Defender XDR achieved industry-leading results without manual processing or fine-tuning and can be run in customer environments without generating an untenable number of false positives. Microsoft’s commitment to delivering cybersecurity while minimizing false positives is reflected in regularly occurring public evaluations.   

We thank MITRE Engenuity for the opportunity to contribute to and participate in this year’s evaluation. 

## Learn more

To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.

* * *

1Microsoft Digital Defense Report 2024

The post Microsoft Defender XDR demonstrates 100% detection coverage across all cyberattack stages in the 2024 MITRE ATT&CK® Evaluations: Enterprise​​ appeared first on Microsoft Security Blog.

Go to Source
