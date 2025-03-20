---
title: "<div>MITRE ATT&CK Evaluations — Cortex XDR Among Elite in Endpoint Security</div>"
date: 2025-03-19
---

The endpoint security market is at a critical inflection point. While cyberthreats are evolving at an unprecedented pace, accelerated by artificial intelligence and automated attack tools, many traditional endpoint security solutions are struggling to keep up, leaving organizations vulnerable to sophisticated attacks. As we analyze the results from the December 2024 MITRE ATT&CK evaluation, a clear pattern emerges: Palo Alto Networks has established itself as one of the top three endpoint security solutions through years of demonstrated excellence and innovation.

## **The State of Endpoint Security — A Widening Gap**

As adversaries leverage AI and automated tools to develop increasingly sophisticated attacks, an uncomfortable truth is facing our industry: many endpoint security solutions, even those from established providers, are falling behind. Traditional approaches that worked five years ago are proving inadequate against today's advanced threats, and this capability gap continues to widen.

In professional sports, we often debate what makes a truly great team. Is it a single championship victory, or is it the ability to compete at the highest level year after year? The teams we remember aren't the ones who had a single great season, but those who maintained exceptional performance year after year. The answer comes down to consistent excellence – the ability to perform at the highest level against any opponent, regardless of their tactics or playbook. While cyberthreats are advancing at a pace that far exceeds anything seen in sports, this principle of consistent excellence and innovation against varying adversaries remains crucial to success.

## **The Evolution of MITRE ATT&CK Evaluations — Raising the Bar**

MITRE's evaluation framework has become increasingly comprehensive each year, adapting to match the rapid evolution of cyberthreats. While early evaluations focused solely on visibility and detection capabilities, the introduction of protection testing four years ago marked a crucial shift toward assessing real-world security effectiveness.

The latest 2024 evaluation represents the most challenging assessment yet, with two key changes to the test methodology:

1. **Multi-Platform Coverage –** The evaluation expanded beyond Windows-centric testing to include dedicated scenarios for macOS and Linux environments, reflecting the complex reality of today's enterprise environments.
2. **False Positive Testing –** For the first time, vendors were challenged with distinguishing between legitimate business activities and actual threats. By introducing legitimate business activity in the midst of the attack emulation, solution providers could no longer configure or tune their solutions to be aggressive in their detections or preventions, or they would suffer a high rate of false positives.

These changes significantly increased the complexity of the test, making it more representative of the real-world challenges defenders face in their own business environments. We believe these changes contributed to the decision for many vendors not to participate this year and resulted in just 19 vendors publishing their results vs. 29 the prior year.

<figure>

![31 participant logos from MITRE ATT&CK results.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-1.png)

<figcaption>

2023 (Turla) MITRE ATT&CK Enterprise Evaluation Enterprise Participants.

</figcaption>

</figure>

<figure>

![19 vendor logos who listed their MITRE ATT&CK results.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-2.png)

<figcaption>

2024 MITRE ATT&CK Enterprise Evaluation Enterprise Participants. Notably 10 fewer vendors opted to participate and publish their results this year.

</figcaption>

</figure>

## **Beyond the Headlines – Understanding MITRE Evaluation Results**

This year MTIRE published a new visual perspective on the evaluation results. The Cohort View shows the results for both the Detection and Protection scenarios in a view that consolidates the performance of all vendors.

<figure>

![Graph of percent of results for MITRE ATT&CK tactics.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-3.png)

<figcaption>

This graph from MITRE shows the “Cohort View” results for the 2024 MITRE ATT&CK Enterprise Evaluation for the Protection Scenario. This graph clearly indicates that the majority of solutions are failing to block the TTPs emulated in this evaluation regardless of the phase of the attack.

</figcaption>

</figure>

<figure>

![Graph of percent of results for MITRE ATT&CK tactics.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-4.png)

<figcaption>

This graph from MITRE shows the “Cohort View” results for the 2024 MITRE ATT&CK Enterprise Evaluation Detection Scenarios. This chart shows that a significant portion of the emulated TTPs in the detection scenarios were executed without generating an accurate, high-fidelity alert in many of the solutions under test.

</figcaption>

</figure>

For those that participated in this latest evaluation, MITRE’s Cohort graphs clearly indicate that many solutions are lagging behind in their ability to detect and prevent common MITRE ATT&CK tactics, techniques, and procedures (TTPs). Alarmingly, nearly all phases of the attack in the Protection scenario were prevented less than half of the time. Looking specifically at Exfiltration, we can see that over 83% of these techniques were not prevented. In fact, more than two thirds of the vendors tested this year missed the opportunity to block more than half of the attacks.

The above performance may come as a surprise to those who have been paying attention to these evaluations and reading the headlines and blogs from the solution providers who participated. It is important to point out that MITRE does not make any declarative statements on vendor performance or identify any rankings or ratings of solutions that participate in the evaluations. They just publish the data that reflects product performance observed during the evaluation. Interpretation is left up to the vendors. With that said, it's important that you understand how vendors are interpreting their results.

- Selective Reporting – As seen above, this year’s Protection scenario proved particularly challenging for the industry. Thus many of the participants chose not to disclose their results in the Protection scenarios in their results’ publications.
    - In the 2024 Evaluation of the vendors that did not have false positives in the Protection scenario, no vendor had a higher prevention rate than Cortex XDR.
- Detection Modifiers – A topic that has been discussed since the inception of the MITRE evaluations is the Detection Modifiers that may accompany detections. There are two modifiers, Delayed Detections and Configuration Changes.
    - The Delayed Detection modifier is added to a detection when human action or intervention is required to augment an autonomously generated event to meet the documented Detection Criteria.
    - The Configuration Change modifier is added to a detection when the configuration of the vendor solution is changed after the start of the evaluation. This may be done to show that additional data can be collected and/or processed.

The Delayed Detection modifier was less frequent this year, but Configuration Changes continued to be used frequently in this year’s evaluation. It is important to note, vendors are not required to document the product changes that are made to achieve detections in the evaluation, and there is no commitment that these changes can or will be made in customer environments. If a vendor is claiming to have outstanding results, it's worth a quick check to see if they mention whether their results include detections achieved through configuration changes.

<figure>

![Graph of detections resulting from a configuration change: 2021-2023](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-5.png)

<figcaption>

This graph shows the cumulative number of Detections resulting from Configuration Changes per vendor 2021-2023. Note: Less is better.

</figcaption>

</figure>

<figure>

![Graph from detections resulting from a configuration change: 2024](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-6.png)

<figcaption>

Detections from Configuration Changes in the 2024 MITRE ATT&CK Enterprise Evaluation. Note: Less is better.

</figcaption>

</figure>

## **A New Approach to Endpoint Security**

While traditional solutions struggle with rapidly evolving threats, Palo Alto Networks has emerged as a transformative force in endpoint security. Our approach represents a fundamental shift from conventional endpoint protection, delivering the innovation needed to counter modern threats. Just as championship teams must excel against any opponent's strategy, we've proven our ability to detect and stop threats regardless of the attack technique or platform targeted.

## **Our Journey — From Newcomer to Leader**

<figure>

![Graphic of Cortex XDR 5 years of top endpoint security results. ](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-7.gif)

<figcaption>

Technique-Level Detections for the Top 10 Market Share Leaders over the last five years.

</figcaption>

</figure>

In just a few short years, we've not only caught up to traditional market leaders but surpassed them, as demonstrated:

- This year, we delivered an industry-first 100% Technique-Level Detection result in 2024 without any Configuration Changes or Delayed Detections.
- Our second year in a row of delivering 100% Analytic Detections without any Configuration Changes or Delayed Detections.
- Last year’s result, Cortex XDR was the only solution to block 100% of attacks in the Protection scenario.
- Achieving the best overall combined Detection and Protection result for the last four years since the Protection scenario was added to the evaluations.

<figure>

![Graphic technique level detections and substeps blocked: 2021-2023](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-8.png)

<figcaption>

A plot of both Technique-Level Detections (Y-axis) and Substeps Prevented (X-axis) from 2021 through 2023. Note this range of years is shown to include vendors that did not participate in 2024.

</figcaption>

</figure>

<figure>

![Graphic technique level detections and substeps blocked: 2021-2024](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/02/word-image-335141-9.png)

<figcaption>

A plot of both Technique-Level Detections (Y-axis) and Substeps Prevented (X-axis) from 2021 through 2024. Note that several vendors are no longer displayed as they opted out for 2024.

</figcaption>

</figure>

## **Why This Matters — The Speed of Innovation**

The pace of threat evolution isn't slowing down. AI-powered attacks are becoming more sophisticated and automated, while yesterday's endpoint security approaches struggle to adapt. Organizations need a solution that's not just keeping pace but staying ahead of emerging threats through continuous innovation and proven effectiveness.

For security leaders, our consistent performance in MITRE evaluations translates to several key benefits:

- Confidence in cross-platform protection across diverse enterprise environments.
- Reduced operational overhead from false positives.
- Validated security effectiveness against sophisticated attacks.
- Proven ability to detect and prevent threats across the entire attack chain.

## **Looking Forward — Setting the New Standard**

In professional sports, dynasties are remembered for their ability to win consistently, regardless of their opponent's tactics or strengths. Similarly, Palo Alto Networks has proven its ability to excel against any type of cyberthreat, across any platform, year after year. Our consistent excellence in MITRE evaluations isn't just about scoring well in tests; it's about demonstrating what modern endpoint security should look like in an age of AI-powered threats and sophisticated adversaries.

As the sophistication of cyberthreats continues to accelerate, the gap between traditional solutions and modern requirements will only widen. Organizations need a partner who's not just keeping up, but leading the way through continuous innovation. Through five years of increasingly challenging MITRE evaluations, we've proven that Palo Alto Networks isn't just the new player in endpoint security – we're the future of it.

The post MITRE ATT&CK Evaluations — Cortex XDR Among Elite in Endpoint Security appeared first on Palo Alto Networks Blog.

Go to Source
