---
title: "SOC and Awe — How Autonomous Security Is Changing the Game"
date: 2025-03-19
---

![](https://www.paloaltonetworks.com/blog/wp-content/themes/panwblog2023/dist/images/audio-icon.svg)

{{interview\_audio\_title}}

_00:00_ _00:00_

Volume Slider 

10s 10s

10s 10s

Seek Slider 

> Given the volume and sophistication of threats, that old SOC model is starting to show real issues,

Clay Brothers warns in a recent eye-opening Threat Vector podcast. Brothers, a senior director at Palo Alto Networks Unit 42, pulls back the curtain on the future of security operations in his conversation with David Moulton, marketing director and host of the Threat Vector podcast.

Brothers doesn't mince words about today's security reality: traditional SOCs are drowning in alerts while threat actors keep getting faster, smarter and more destructive. With a front-row seat to security transformations across major industries, Brothers shares battle-tested insights on how organizations can revolutionize their security operations to match the escalating threat landscape before it's too late.

## **The Mandate for SOC Transformation**

Traditional SOCs are being pushed to their limits as threat actors increase their scale, sophistication and speed. According to Brothers, legacy SOC models typically rely on legacy SIEM tools collecting data from disparate parts of the environment, with statically signature-based detections triggering alerts that analysts manually triage, analyze and remediate.

Today, especially with AI, it's so easy to morph an attack to avoid a static detection,

Brothers says. This challenge is compounded by the fact that modern attacks rarely target just one area of infrastructure. According to the Palo Alto Networks Unit 42’s Global Incident Response Report, 2025, 84% of incidents attack multiple fronts – human, identity, network, cloud and more – with 70% involving three or more of these fronts.

<figure>

![Graph depicting how data silos make detecting attacks hard, team maintain detection content, and automation scales it. ](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-336238-1.png)

<figcaption>

Legacy SOCs with siloed teams, manual responses and automation as an afterthought.

</figcaption>

</figure>

![Graph showing how we must transform the SOC to be machine-led, human empowered. ](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-336238-2.png)

The need for transformation becomes even more critical when considering the increasingly destructive nature of attacks. Today's threats aren't just compromising sections of networks; they're locking up entire systems, exfiltrating sensitive data and potentially disrupting entire supply chains. Conventional, siloed, security approaches simply cannot keep pace with these sophisticated, multivector attacks.

## **Cloud-Native Detection — The Way Forward**

One of the most significant changes in threat detection and response strategies is the shift from manual, statically signature-based detections to cloud-native-based detection. Brothers emphasizes that the old approach – creating manual detections that trigger alerts when specific criteria are met – is becoming obsolete. From Brothers' perspective:

> Today it's all about cloud-native based detection.

He highlights how Cortex XSIAM leverages a team of 100+ threat researchers who constantly tune and add new detections that all customers can take advantage of through the cloud-based platform. This approach provides organizations opportunities:

- Benefit from continuously updated, expertly crafted detections.
- Reduce reliance on internal resources for detection engineering.
- Focus more time on response rather than detection creation.
- Adapt more quickly to emerging threat patterns.

While organizations will always have unique business requirements necessitating custom detections, relying on cloud-native capabilities and security experts significantly reduces this burden. Brothers explains:

> To me, that is the way forward – to be able to rely on experts to help supplement your team in actually detecting those needles in a haystack.

## **Leveraging AI and Automation for Enhanced Security**

AI and automation are no longer just buzzwords but essential components of modern SOC operations. Brothers shares that the Palo Alto Networks SOC has measured the time saved through automation and AI to be the equivalent of 65 full-time employees. This remarkable efficiency is achieved by identifying which analyst tasks consume the most time and applying AI or automation to either replace or supplement these activities.

<figure>

![Daily log and alert volume graph.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-336238-3.png)

<figcaption>

The Palo Alto Networks SOC team takes advantage of the machine learning in XSIAM to correlate and filter alerts, as well as make decisions about which alerts are important, bringing us down to about 75 alerts per day that actually create an alert in Cortex XSIAM for the SOC to handle.

</figcaption>

</figure>

The benefits extend beyond just time savings. By automating routine tasks and leveraging AI for initial analysis, SOC analysts can focus on more strategic and engaging work. "No analyst wants to spend their eight hours a day responding to alerts that end up being false positives," Brothers notes. This shift in focus not only improves security outcomes but also enhances job satisfaction and reduces the high turnover normally associated with SOC roles.

AI and machine learning provide the foundation for bringing disparate data together, stitching and normalizing it quickly, thus reducing what used to take months into hours or days. These technologies also excel at detecting anomalous activity that falls outside normal baselines for users, applications or services, catching sophisticated attacks that would bypass legacy detection methods.

## **Integrating Threat Intelligence for Proactive Security**

Unit 42's approach to integrating threat intelligence adds another critical dimension to modern SOC strategies. Brothers describes how Unit 42 uses their Threat Intelligence Knowledge Repository (TIKR) to collect and analyze data from millions of Palo Alto Networks firewalls, endpoints and cloud instances.

This vast data repository, combined with insights from incident responses, proactive services and MDR teams, enables Unit 42 to identify emerging trends and develop more effective detections. The organization also conducts independent threat research and dark web analysis to further enrich their understanding of the threat landscape. Brothers describes the impact:

> We are seeing and collecting all this data and being able to make sense of it, make decisions, understand trends, as well as our own independent threat research and deep and dark web analysis that we can leverage to be able to push data to our products and to our services so that we're better serving our customers.

This intelligence-driven approach allows SOCs to shift from purely reactive security to more proactive postures, identifying and mitigating threats before they can fully materialize. Threat hunting, anyone?

## **Measuring SOC Success — Beyond Traditional Metrics**

When it comes to measuring SOC effectiveness, Brothers emphasizes the need to move beyond simplistic metrics that can drive counterproductive behaviors. Historically, SOC leaders might have boasted about handling high volumes of alerts, but today's focus is on quality over quantity.

Traditional metrics, like mean time to detect, respond and close, remain important. But, Brothers cautions that these can be "gamified" if not properly managed. If analysts are evaluated primarily on how quickly they close tickets, they might rush their analysis or close incidents prematurely, potentially missing critical security issues. Brothers advises:

> It is so critical to make sure you are managing those metrics clearly, so you're not driving bad behavior.

Beyond these standard measurements, modern SOCs should consider efficiency options:

- Automation efficiency (tasks automated and FTE offset achieved)
- Time spent on proactive activities, like threat hunting
- Efficiency in creating and tuning detections
- Quality of response and thoroughness of analysis

These balanced metrics provide a more comprehensive view of SOC effectiveness while encouraging behaviors that enhance security outcomes.

## **Balancing People, Process and Technology**

SOC transformation requires careful alignment of people, process and technology. Brothers describes technology as "the heartbeat of the SOC," emphasizing the need for strong SIEM tools that leverage automation and AI to stitch data together for complete visibility. However, he cautions that "you can have the shiny new toy and still fail."

The human element remains critical. Having the right people with incident response experience and the skills to leverage new technologies effectively makes the difference between success and failure. Processes tie everything together, enabling the SOC to optimize the use of technology and ensuring consistent, effective responses to security incidents. In Brothers' assessment, "SOC transformation focuses a lot on enabling the SOC from a people and process perspective to leverage and optimize the use of the technology." Without all these pieces working in harmony, organizations risk missing incidents, responding inefficiently and potentially facing major business disruptions.

## **Building Resilience for the Future**

As organizations look to future-proof their security operations, Brothers envisions the "autonomous SOC" as the ultimate destination. This approach leverages AI, machine learning and automation to create a self-driving security operation that can match the increasing speed, sophistication and scale of modern threats.

**![Graphs comparing why we need to transition to analyst-assisted security operations. ](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-336238-4.png)**

"If you stay in that legacy environment, you have an increased likelihood and impact of a major security incident disrupting your business," Brothers warns. Moving toward an autonomous SOC not only better protects the organization but also creates a more engaging environment for security talent.

The concept of resilience (defined by Brothers as the ability to quickly bounce back from attacks) starts with prevention but extends to rapid detection and response when prevention fails. The SOC plays a critical role in this resilience by identifying patterns that inform prevention strategies, detecting and responding to threats that bypass preventive controls, and partnering with incident response teams to quickly contain and remediate successful attacks.

By embracing transformation now, organizations can position themselves to meet not only today's challenges but tomorrow's as well. As Clay Brothers concludes:

> This is where analysts want to go. This is the type of SOC they want to work in, and it's the right SOC to work in.

## **Learn More**

![Palo Alto Networks Unit 42 Global Incident Response Report, 2025.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/03/word-image-336238-5.jpeg)

Our latest Global Incident Response Report identifies three critical vulnerabilities:

- Siloed security systems prevented detection despite evidence existing in logs (75% of cases).
- Unmonitored cloud assets enabled 40% of cloud incidents.
- Excessive privileges facilitated lateral movement in 41% of attacks.

Download the 2025 Unit 42 Global Incident Response Report to discover how attackers are shifting tactics from theft to sabotage, and learn the critical security gaps your organization needs to address immediately.

The post SOC and Awe — How Autonomous Security Is Changing the Game appeared first on Palo Alto Networks Blog.

Go to Source
