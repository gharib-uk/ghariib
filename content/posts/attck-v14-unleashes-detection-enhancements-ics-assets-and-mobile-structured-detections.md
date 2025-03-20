---
title: "<div>ATT&CK v14 Unleashes Detection Enhancements, ICS Assets, and Mobile Structured Detections</div>"
date: 2025-01-02
categories: 
  - "security"
---

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*8nBv3HtBj0KyhQYkcfWBog.jpeg)

<figcaption>

Credit: https://flic.kr/p/dzyK9x CC BY-SA 2.0

</figcaption>

</figure>

ATT&CK has been brewing up something eerie for this Halloween — a release so hauntingly powerful that it will send a chill down the spine of even the most formidable adversaries. As v14 emerges from the depths, we’re proud to present a more robust and finely-tuned knowledge base. So, grab your flashlights and keep your wits about you as you navigate the latest changes, including enhanced detection guidance for many techniques, a (slightly) expanded scope on Enterprise and Mobile, Assets in ICS, and Mobile Structured Detections.

For the rest of our regular updates/additions across Techniques, Software, Groups and Campaigns take a look at our release notes, our detailed changelog, or our changelog.json.

#### **Detection Upgrade with Analytics**

In ATT&CK v13 we started adding “detection notes” and pseudocode analytics from CAR (Cyber Analytics Repository) directly into some detections. In v14 we’ve dramatically expanded the number of techniques with a new easy button and added a new source of analytics. One focus this release was Lateral Movement, which now features over 75 BZAR\-based analytics! BZAR (Bro/Zeek ATT&CK-based Analytics and Reporting) is a subset of CAR analytics that enable defenders to detect and analyze network traffic for signs of ATT&CK-based adversary behavior. Moving forward, we plan to continue working across tactics to enhance detection approaches.

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*X7qQq-oWIaajG6inaAkQaQ.png)

<figcaption>

Example BZAR-derived Analytic

</figcaption>

</figure>

Also new: enhanced relationships between detections, data sources, and mitigations. Improving techniques is a collaborative and iterative process, and we work with the community to identify new procedures and enhance data sources and mitigations. This release includes updated technique alignments to data sources and mitigations, better reflecting the most effective defensive measures for the impacted techniques.

Jump into the **#defensive\_attack** channel to be part of the action.

#### **Enterprise’s New(ish) Frontier**

Since its inception, ATT&CK has been dynamic, designed to catalog, categorize, and adapt to real-world adversary behaviors that primarily involve direct interaction with devices, systems, and networks. Over the past decade, this adaptability and focus has empowered defenders through consistent, threat-informed resources. As adversaries continually evolve their exploitation of human vulnerabilities, ATT&CK has expanded its scope with this release, encompassing more activities that are _adjacent_ to, yet lead to direct network interactions or impacts. The increased range incorporates deceptive practices and social engineering techniques that may not have a direct technical component, including Financial Theft (T1657: Financial Theft), Impersonation (T1656: Impersonation), and Spearphishing Voice (T1598.004: Phishing for Information: Spearphishing Voice).

Think some behaviors are still missing? Your input remains essential as we continue to expand ATT&CK’s horizons and refine content to match advancing adversary tactics. Email or Slack us what you’re seeing.

#### **Assets Join the ICS Arsenal**

We’ve been working on Asset refactoring for a while, and we’re thrilled to introduce the results of our initial efforts. v14 features 14 inaugural Assets, representing the primary functional components of the systems associated with the ICS domain. These Asset pages include in-depth definitions, meticulous mappings to techniques, and a list of related Assets. Our primary goals for Assets are to provide a common language for inter-sector communication, and to empower underrepresented sectors to leverage ATT&CK mappings, fostering meaningful communication about risks and threats. You can also now find Assets on the ATT&CK Navigator.

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*II7dLEhjK-F5qLgq0mrd9A.jpeg)

<figcaption>

The Data Gateway Asset

</figcaption>

</figure>

The Assets refactoring process involved an in-depth review of relevant CTI, researching and refining the resulting definitions based on industry standards, and analyzing how the device features map to ATT&CK Techniques. We look forward to leveraging the deep insights from our industry partners as we continue refining and expanding Assets.

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*Dbt2_PI3XGs6p9b8tKN0qQ.jpeg)

<figcaption>

A Partial List of Assets

</figcaption>

</figure>

If you’re interested in contributing, head over to the recently created **#****ics\_attack** channel.

#### **Reeling in Mobile Threats with Phishing & Structured Detections**

With Enterprise increasing its scope a bit, Mobile has also expanded its coverage to include Phishing (Phishing:T1660), which encompasses phishing attempts through vectors including SMS messaging (“smishing”), Quick Response (QR) codes (“quishing”), and phone calls (“vishing”). Mobile Phishing features a new mitigation (M1058: Antivirus/Antimalware), to enhance anti-virus and malware defenses. Also introduced with this release, Mobile structured detections. This allows you to explicitly see the required inputs (Data Sources) for each detection, along with how to analyze the data to identify a specific Technique (detection). Structured detections are part of the ongoing endeavor to bring Mobile to parity with Enterprise.

Next up? Refining existing mitigations and working with the Mobile security community to identify new content. Get involved at **#****mobile\_attack****.**

#### **Enhancing Your Website Navigation Experience**

![](https://cdn-images-1.medium.com/max/1024/1*Q34cYHgnQv31BQ8rjwk8EQ.png)

We’ve refined the navigation bar of the ATT&CK website, streamlining its structure and content to enhance the user experience and overall ease of navigation. Over time, our navigation bar accumulated a lot of ‘stuff’, and we hope this update strikes a balance between necessary links and user needs. The updated navigation bar features a single dynamic menu display, with access to secondary links (most previously featured on the primary bar) in associated dropdown menus:

![](https://cdn-images-1.medium.com/max/413/1*fGQ9N-X1qPC-iSKzuEjPBA.png)![](https://cdn-images-1.medium.com/max/277/1*24hEoSLm97cFjc9OfpAapQ.png)

Love it? Hate it? Let us know.

#### **Looking Forward**

We want to extend our deepest gratitude to the heroes of this release — our dedicated contributors. Your relentless commitment to enhancing collective defenses are the true magic behind ATT&CK. As 2023 draws to its end, let’s keep the collaboration alive, because together, we’ll continue to ward off the threats that go bump in the night. Stay vigilant, stay curious, and stay safe — and remember, with ATT&CK, every day is a day to keep adversaries at bay.

As always, connect with us on email, Twitter, or Slack.

©2023 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 22–00745–2.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=fa473603f86b)

* * *

ATT&CK v14 Unleashes Detection Enhancements, ICS Assets, and Mobile Structured Detections was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
