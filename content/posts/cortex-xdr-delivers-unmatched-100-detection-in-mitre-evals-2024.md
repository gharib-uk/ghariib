---
title: "Cortex XDR Delivers Unmatched 100% Detection in MITRE Evals 2024"
date: 2025-01-03
categories: 
  - "security"
---

Palo Alto Networks Cortex XDR made history in this year’s MITRE ATT&CK Evaluations as the first participant ever to achieve 100% detection with technique-level detail and no configuration changes or delays. Technique-level detections represent the gold standard, equipping security analysts with the precise information needed to identify an attack. This is the second year in a row Cortex XDR has achieved 100% detection with no configuration changes or delays, demonstrating unmatched consistency.

![2024 Enterprise ATT&CK Evaluations](https://www.paloaltonetworks.com/blog/wp-content/uploads/2024/12/Enterprise2024_glow-230x230.png)Cortex XDR achieved the highest prevention rate among all vendors with zero false positives – a critical distinction for endpoint security. At the prevention stage, even one false positive can disrupt legitimate business processes, impacting productivity.

This year’s evaluation was more rigorous than ever, incorporating false positive testing, macOS support and expanded Linux scenarios. Notably, vendor participation dropped from 29 to 19, with several prominent vendors opting out. Of the vendors tested, two-thirds detected less than 50% of the attack steps – underscoring the heightened rigor of this year’s evaluation.

We are incredibly proud of our world-class threat research and engineering teams for delivering exceptional endpoint security, empowering our customers to stay ahead of adversaries, like those simulated in this evaluation.

![Industry-Best Detection Coverage](https://www.paloaltonetworks.com/blog/wp-content/uploads/2024/12/detection-chart_1200x628-1-1-230x120.jpg)

### Cortex XDR: Industry-Best Endpoint Security Performance in MITRE ATT&CK Round 6

Cortex XDR excelled in both detection and prevention scenarios of the evaluation, setting a new benchmark for endpoint security and redefining what organizations should expect from their cybersecurity solutions.

In the three detection scenarios, Cortex XDR achieved a historic 100% technique-level detection of all simulated attack steps with no configuration changes or delayed detections. These results reaffirm our commitment to providing the most comprehensive defense for every major OS – Windows, macOS and Linux.

MITRE ATT&CK Evaluations also test prevention, assessing a solution’s ability to block attacks before they can cause damage. This is the essence of real-world endpoint security – prevent as much as possible and detect everything else quickly and accurately.

In Round 6, Cortex XDR prevented 8 out of 10 attack steps while maintaining zero false positives. False positives at the prevention stage can disrupt critical business operations with the potential for significant financial damage. Cortex XDR’s ability to combine unmatched prevention accuracy with zero disruptions makes it the ideal endpoint security solution for the world’s largest and most demanding organizations.

While there were two attack steps in the prevention test that didn’t meet the criteria for MITRE to count as a block, our actual action during these steps would have protected our customers and stopped a breach.

- In step three, the attack attempted an SSH connection from a suspicious host in China, which was supposed to be followed by MITRE’s attack step. We blocked the suspicious SSH connection, stopping the attack at an earlier stage.
- In step five, the attack attempted to encrypt data, and the encryption action was immediately reversed by the Cortex XDR agent. We stopped the attack, delivering the only outcome that matters, which is keeping customers safe.

![Top Prevention with Zero False Positives](https://www.paloaltonetworks.com/blog/wp-content/uploads/2024/12/prevention-chart_1200x628-1-230x120.jpg)

## What Is the MITRE ATT&CK Evaluation?

The MITRE ATT&CK Evaluation is the most rigorous test in cybersecurity, designed to measure how well endpoint security solutions detect and prevent real-world threats. MITRE ATT&CK studies and emulates attacks conducted by sophisticated adversaries, making it a true benchmark for security effectiveness.

This year’s Enterprise 2024 Evaluation focused on two distinct and highly relevant attack sources:

- **Ransomware**: Exploring behaviors common in ransomware campaigns, such as abusing legitimate tools, encrypting data and disabling critical services or processes. MITRE chose to emulate the techniques of two notorious ransomware as a service threat groups – CLOP and LockBit.
- **Democratic People's Republic of Korea (DPRK)**: Simulating attacks on macOS systems, inspired by the DPRK’s use of modular malware to elevate privileges and target credentials.

### How Does Palo Alto Networks Monitor Prominent Threats like Ransomware and DPRK?

As an extension of Unit 42, the Palo Alto Networks Cortex Threat Research team monitors the ever-evolving threat landscape and transforms its findings into actionable intelligence, directly enhancing Cortex XDR's detection and prevention capabilities.

Over the past year, as part of their daily efforts to study the tactics, techniques and procedures (TTPs) of threat actors, the team zeroed-in on DPRK-linked APT groups, uncovering new campaigns and malware used to infiltrate organizations worldwide.

The DPRK is known for its double-pronged cyberwarfare strategy, centered on espionage and large-scale cybercriminal activities aimed at generating revenue for the North Korean regime. Our research highlights the DPRK’s increasing focus on targeting macOS users and endpoints, along with persistent efforts to steal cryptocurrencies and sensitive information from organizations worldwide across various industries.

Highlights of the team’s research have been shared with the community and are available in the Unit 42 Threat Research Center, including the following research articles:

- Threat Assessment: North Korean Threat Groups
- Contagious Interview: DPRK Threat Actors Lure Tech Industry Job Seekers to Install New Variants of BeaverTail and InvisibleFerret Malware
- Gleaming Pisces Poisoned Python Packages Campaign Delivers PondRAT Linux and MacOS Backdoors
- Unraveling Sparkling Pisces’s Tool Set: KLogEXE and FPSpy

The team has also published several research articles on the most menacing ransomware gangs active in 2024. This research highlights the evolution of these groups, showcasing their increasing sophistication and aggression. The research further demonstrates how Cortex XDR’s detection and prevention capabilities can automatically disrupt these operations and stop the encryption of sensitive information.

To learn more about these ransomware groups, please see the following articles:

- Threat Assessment: Howling Scorpius (Akira Ransomware)
- From RA Group to RA World: Evolution of a Ransomware Group
- Threat Assessment: BianLian

### How Did the Test Change This Year?

This year’s MITRE ATT&CK Evaluation introduced significant changes to reflect the evolving threat landscape and better measure endpoint security effectiveness.

- **Expanded Endpoint Coverage**: For the first time, the test included a broader range of endpoint platforms, targeting Windows, Linux and macOS environments. This expanded scope ensured that solutions were tested against diverse operating systems, providing a more comprehensive view of defensive capabilities.
- **Inclusion of False Positives**: Another major addition was the inclusion of false positive tracking, a critical factor in real-world deployments. Vendors were not only evaluated on their ability to detect threats but also on avoiding inaccurate detections that could overwhelm security teams or disrupt business critical processes.

By incorporating these changes, MITRE executed its most challenging and realistic evaluation to date.

### Consistency Matters — Cortex XDR Delivers Exceptional Results Year After Year

Palo Alto Networks is committed to delivering the highest quality of cybersecurity technology to our customers, which requires exceptional talent and investment in research and engineering. Our teams are proud to put their work through the rigors of MITRE ATT&CK Evaluations year after year, demonstrating to the world the impact of their hard work.

Since 2022 Cortex XDR has led the industry in detection results in MITRE evaluations. We believe every year’s attack simulation provides unique insight into the threat actors of the world and the cybersecurity industry’s stance in defending against them. For that reason, we keep a public record of all MITRE ATT&CK Evaluation results in our interactive dashboard for easy comparison year over year.

Thanks to MITRE and all of our customers who run Cortex XDR to defend their endpoints. We’re honored to be your cybersecurity partner of choice.

Head over to our MITRE results page to learn more about our performance in this test, and contact us to schedule a demo when you are ready to see Cortex XDR in action.

The post Cortex XDR Delivers Unmatched 100% Detection in MITRE Evals 2024 appeared first on Palo Alto Networks Blog.

Go to Source
