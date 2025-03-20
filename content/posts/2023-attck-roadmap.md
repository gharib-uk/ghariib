---
title: "<div>2023 ATT&CK Roadmap</div>"
date: 2025-01-02
categories: 
  - "security"
---

#### A Roadmap of 2023’s key efforts: From ICS Assets to more Linux and ATT&CKcon 4.0.

![](https://cdn-images-1.medium.com/max/1024/1*pZsmRwpPNtBygO-4zxDVLQ.png)

It’s 2023 and we’re all a little older, including ATT&CK, which will be celebrating its 8th (!) release anniversary in a few short months. Last year we matured, expanded, deconflicted, and renovated the knowledge base, persevering through challenges to meet our 2022 goals. Some of our most notable efforts including unveiling Enterprise structured detections, publishing Mobile sub-techniques, introducing ICS detections, transitioning ATT&CK for ICS to the mothership (aka attack.mitre.org), launching Campaigns, and hosting ATT&CKcon 3.0.

The ATT&CK team also trekked around the U.S. and the globe to talk about ATT&CK for defenders, threat hunters, and cyber threat intelligence analysts. We held riveting conversations about purple teaming, discussed best practices for adversary emulation, and connected over common misuses of ATT&CK. You inspired us with your stories, insights and expertise, and we’re thrilled to continue collaborating with you in 2023.

### **2023 Roadmap**

Our focus areas in 2023 will be targeted growth and integration. We’ll be maintaining framework stability as we build out content and structure, while expanding and increasing the scope of some of ATT&CK’s current platforms. We’ll be looking for places where we can add defensive “easy buttons” to ATT&CK and other ways we can improve for lower-resourced defenders. Relatedly, we’ll also be working to enhance the usability, accessibility, and functionality of the ATT&CK website and Navigator. These updates and more will be mainly centered on our April and October releases.

**Linux | April & October 2023**

We made significant progress in updating the macOS platform last year, and while we’ll continue evolving that content, we’ve officially transitioned the spotlight to Linux for 2023. April’s release will feature Linux contributions (thank you, contributors!) that focus on modifications to parent technique scope, including new sub-techniques and updated procedures.

For the October release, we’re targeting an expanded representation of Linux within ATT&CK. We’re looking to not only better account for activity within on-premise Linux servers, but some of the broader Linux-based (and not always x86) spaces adversaries have been abusing. This will be a substantial effort given how under reported Linux activity is, and the Linux security community’s input is essential for us to improve this platform. We’re working to build out opportunities to connect both online and offline and would like to hear how you’d like to collaborate. In the interim, whether you have specific insights to share, or would just like to talk about ATT&CK for Linux in general, email us and join the #**linux\_attack** channel on the ATT&CK slack.

**Defensive Coverage | October 2023**

When we added explicit pairing of detections to data sources in ATT&CK v11, it was intended to let you identify the inputs you need to collect (Data Sources), combined with how to analyze that data to identify a given Technique (detection). We’ll be leveling up this year, exploring and including in ATT&CK more specifics on what you as defenders can be collecting related to detections and how. This quest will result in an more directly usable guidance for defenders, as well as a more in-depth look at data collection, analyzation, and identification of a given technique.

We will also be assessing ATT&CK mitigations for gaps and potential improvements this year. We converted mitigations into objects in v5 to increase their usability, and our goal has always been to continue to evolve and improve that knowledge base. Over the next several months, we’ll be researching new preventions and crafting out additional ways to prevent a given technique from succeeding. If you’d like to contribute to either of these quests, let us know or join us in the **#defensive\_attack** channel in our Slack.

**ICS | April & October 2023**

This will be ICS’s first full year on the ATT&CK site, and we’ll be making additions across the matrix, including more cross-domain mappings (e.g., ICS + Enterprise). We’ll be sharing more details about this and our approach to leveraging ATT&CK for holistic ICS defense in an upcoming blog post.

Over the next several months, we’ll be focusing on addressing overlaps and integration with other domains (primarily between Enterprise and ICS, although Mobile could be included) and revamping ICS Assets. This effort will focus on evaluating Assets in various industries, identifying their interrelations, and determining how they fit into the ATT&CK structure.

If you’d like to share contributions or have other inputs for ICS, connect with us!

**Mobile | April & October 2023**

We released Mobile sub-techniques last summer, and we’ll continue expanding on those, as well as building out contributions, and collaborating on enhancing multi-domain techniques. Mobile-specific Data Source objects are another goal this year, and they’ll mirror the concept of Data Source objects that Enterprise and ICS currently leverage, informing a more detailed and defined data collection strategy. The Mobile Data Sources will eventually be featured on both the overall Data Sources list as well as the individual Data Source pages. For October, we plan on charting out structured detections, to enable you to enhance your Mobile detections approach.

A core focus for the domain overall is bolstering our collaboration with the mobile security community. Along with the rest of ATT&CK, Mobile’s matrix is mostly crowdsourced, and we rely on the deep expertise from the mobile community to validate our content and help us to mature this knowledge base. If you’d like to contribute to this space or start a conversation, reach out to partner with us.

**Campaigns | April & October 2023**

We don’t typically highlight all the updates we’ll be making to ATT&CK Techniques, Software, or Groups, but as the newest object on the block, we figured you might be curious about our plans for Campaigns this year. Over the next few months, we’ll be extracting significant Campaigns from several APT groups in ATT&CK and adding them to the knowledge base. Closer to October, we’ll pivot to building out campaigns conducted by criminal groups, including ransomware operations. If you’d like to contribute or just share your thoughts on Campaigns, let’s have a conversation!

**Cloud | April & October 2023**

Since initially releasing ATT&CK for Cloud in October 2019 (v6), we have continually worked to expand and refine how these platforms fit within the broader ATT&CK for Enterprise. Cloud introduces many new challenges for defenders, as well as potential opportunities to rethink how we describe adversary behaviors. For example, how does our understanding of tactics such as Execution, Lateral Movement, and Exfiltration change when considering cloud environments?

Throughout 2023 we will work to adapt these definitions with the goal of helping everyone (cloud expert or not) better understand and utilize ATT&CK for Cloud. As always, if you have specific insights or relevant ideas to share, please email us and join the #**cloud\_attack** channel on the ATT&CK slack.

**ATT&CKcon 4.0 | October 24–25, 2023**

Whether you’re a sophisticated ATT&CKer, a hobbyist, or you’re just getting started, we’d love to connect with you at ATT&CKcon 4.0! We’ll be hosting 4.0 in-person and virtually from McLean, VA October 24–25 this year. Watch our Twitter and LinkedIn for announcements on our CFP when it opens in the next few months, followed by our lineup of speakers in late summer. If your organization is interested in sponsoring ATT&CKcon, drop us a line at attackcon@mitre.org.

### **Here’s to (all) of You**

We received an exceptional number of contributions across domains and platforms last year and are incredibly grateful for the support. ATT&CK will always be community-driven and we’d like to thank each and every one of you that took the time to submit contributions, share research and new use cases, and join us in innumerable ATT&CK discussions.

We’re looking forward to another great year of collaboration and can’t wait to connect with you in-person or via email, Twitter, or Slack!

©2023 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 22–00744–16.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=452fab541673)

* * *

2023 ATT&CK Roadmap was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
