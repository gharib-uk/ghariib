---
title: "<div>ATT&CK Goes to v11</div>"
date: 2025-01-02
categories: 
  - "security"
---

### ATT&CK Goes to v11: Structured Detections, Beta Sub-Techniques for Mobile, and ICS Joins the Band

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*JB9ySNw5GWctTPIxbjYlng.jpeg)

<figcaption>

These go to eleven

</figcaption>

</figure>

By Adam Pennington and Jason Ajmo

Right on cue, ATT&CK’s latest release is out, and this time we’ve gone to v11! If you’ve been following along with our roadmap there shouldn’t be any huge surprises in store, but we wanted to take a chance to go over our latest changes. The v11 set list includes detections now paired with related Data Sources: Data Components, a beta version of sub-techniques for ATT&CK for Mobile, ATT&CK for ICS on attack.mitre.org, as well as regular updates/additions across Techniques, Software, and Groups.

### ATT&CK for Enterprise Structured Detections

Over the past few years, transforming various actionable ATT&CK fields into managed objects has been a reoccurring theme. In v5 of ATT&CK, we converted mitigations into objects to enhance their value and usability — with this conversion, you can now identify a mitigation and pivot to various techniques it can potentially prevent. This has been a feature that many of you have leveraged to map ATT&CK to different control/risk frameworks. We previously converted data sources to objects for the v10 release, enabling similar pivoting and analysis opportunities.

In today’s v11 release we’ve taken a parallel approach for detections in Enterprise ATT&CK, taking the previously free text detections featured in Techniques, and have refined and merged them into descriptions that are connected to Data Sources. We have typically tried to match the detection text on a Technique to its Data Sources, but this makes the paring explicit. This will let you now see for each detection what you need to collect as inputs (Data Sources) paired with how you could analyze that data to identify a given Technique (detection). Below is an example of how Data Sources and Detections have changed for Steal or Forge Kerberos Tickets (T1558).

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*RNQMfeHnbQ_Hl9yY_v4E1g.png)

<figcaption>

Data sources and detections in ATT&CK v10 for Steal or Forge Kerberos Tickets (T1558)

</figcaption>

</figure>

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*L0AwG3Z0VkPk_RO6g-G1gg.png)

<figcaption>

Data sources and detections in ATT&CK v11 for Steal or Forge Kerberos Tickets (T1558)

</figcaption>

</figure>

Detections will also now be included on Data Source pages, associated with each Technique listed for a Data Component.

![](https://cdn-images-1.medium.com/max/1024/1*p-dY4rNMtsq51NoNmZr_zQ.png)

As with everything else in ATT&CK, these new detections also appear in our STIX as a part of the “detects” relationship added in our last ATT&CK release in its “description” field. For more information about ATT&CK’s STIX representation, including the data source objects and relationships added in ATT&CK v10, you can check out our STIX usage document.

![](https://cdn-images-1.medium.com/max/793/1*j5LUee-vgAzhPIGAqP5A8g.png)

### Mobile Sub-Techniques Beta

In 2020, we added Sub-Techniques to ATT&CK for Enterprise. In the time since, they’ve been well-received and solved some of the growth issues we were having in our biggest matrix. As ATT&CK’s Mobile Lead Jason Ajmo recently talked about in the ATT&CK Blog, we’re now bringing this improvement to ATT&CK for Mobile as a beta release. The content on the main ATT&CK site now contains the Sub-Techniques beta, and the current, stable Mobile content can be accessed at https://attack.mitre.org/versions/v10/matrices/mobile/. We plan on making ATT&CK for Mobile with Sub-Techniques final this summer, after we’ve given the community time to check out the content, get ready for it, and send us any feedback they have to attack@mitre.org. Until that time, the main STIX representation of ATT&CK for Mobile will remain the v10 pre-Sub-Techniques version.

#### How can I move to the beta ATT&CK for Mobile with sub-techniques?

First, you’ll need to support some changes to Mobile ATT&CK’s technique structure necessary to support sub-techniques. If you’re already using or have moved to versions of ATT&CK for Enterprise with sub-techniques, the structural changes and the process of moving are identical. As with ATT&CK for Enterprise, we’ve expanded Mobile technique IDs to identify corresponding sub-techniques: T\[technique\].\[sub-technique\]. In Mobile’s STIX representation of ATT&CK we’ve added the “x\_mitre\_is\_subtechnique = true” to “attack-pattern” objects that represent a sub-technique, and “subtechnique-of” relationships between techniques and sub-techniques. Both are already contained in our STIX documentation. You can find a STIX representation of ATT&CK that includes the v11 Mobile Beta here.

Next, if you want to get a head start and remap your content from a previous version of Mobile ATT&CK, to this beta release. As we did when we released Sub-Techniques for ATT&CK for Enterprise, we’re providing a translation table or “crosswalk” from previous release Mobile technique IDs to beta ones to help with the transition. The JSON file shows what happened to each technique in the beta release. The top-level technique ID represents each technique from the v10 release, and the structure underneath shows what changed with the v11 beta release, if anything.

Thanks to the excellent feedback from the community, we identified four key types of changes:

1. Remains Technique
2. Became a Sub-Technique
3. One or More Techniques Became New Technique
4. Deprecated

Each of these types of changes is represented in the “change-type” field in the JSON. Some of these changes are simpler to implement than others. We recognize this, and in the following steps, we incorporate the four types of changes into tips on how to move from our previous release to ATT&CK with sub-techniques.

![](https://cdn-images-1.medium.com/max/1024/1*rwDx0DyZCuFfyC7CatO32Q.png)

#### Step 1: Start with the easy to remap techniques first and automate

For “Remains Technique”, “Became a Sub-Technique”, or “One or More Techniques Became New Technique” change types you can replace the previous technique ID with the new technique ID.

In some cases, technique names have changed, or tactics have been removed, so it’s also worth checking the “explanation” in the JSON.

**Remains Technique**

![](https://cdn-images-1.medium.com/max/1024/1*mN9-SYnxJp7bwPwRb4bSEg.png)

The first thing that’s easy to remap — the techniques that aren’t changing and don’t need to be remapped. Anything labeled “Remains Technique” is still a technique with an unchanged technique ID like T1398 in the above example.

**Became a Sub-Technique**

![](https://cdn-images-1.medium.com/max/785/1*U4O_kLo_D6DYrVSlaIysbA.png)

Next in the “easy to remap category” are the technique to sub-technique transitions, labeled “Became a Sub-Technique”. These techniques were converted into the sub-technique of another technique. In this example, Modify System Partition (T1400) became Hijack Execution Flow: System Runtime API Hijacking (T1625.001).

Finally, there are a few techniques that merged with other techniques.

**One or More Techniques Became New Technique**

![](https://cdn-images-1.medium.com/max/1012/1*q0nG7sRGlRByQewj4lpBhA.png)

For techniques labeled “One or More Techniques Became New Technique” a new technique was created covering the scope and content of one or more previous techniques. For example, Network Traffic Capture or Redirection (T1410) and a few other techniques merged together to create Adversary-in-the-Middle (T1638).

For any of these “easy” types of changes anything represented by the previous ATT&CK technique ID should be transitioned to the new technique or sub-technique ID. The ATT&CK STIX objects represent this type of change as a revoked object which leaves behind a pointer to what they were revoked by. In the case of T1400, that means it was revoked by T1625.001.

In all of these cases, it’s enough to take what’s listed as the top-level key and replace it with what’s listed in the nested “id” key.

#### Step 2: Look at the deprecated techniques to see what changed

This is where some manual effort will take place. Deprecated techniques are not as straightforward.

**Deprecated**

![](https://cdn-images-1.medium.com/max/959/1*WpysMkjdhA09mrYwIEAzfA.png)

For techniques labeled as “Deprecated”, we removed them from ATT&CK without replacing them. They were deprecated because we felt they did not fit into ATT&CK or due to a lack of observed in the wild use. For example, Remotely Wipe Data Without Authorization (T1469) was removed because we hadn’t been able to find evidence of any adversary using it in the wild.

#### Step 3: Review the techniques that have new sub-techniques to see if the new granularity changes how you’d map

If you want to take full advantage of sub-techniques, there’s one more step. Many “Remains Technique” techniques now have new sub-techniques you can take advantage of.  
  
One great example of an existing technique that now has new sub-techniques is Application Discovery (T1418). Its name was updated to Software Discovery, and its content was broken out into a new sub-technique: Security Software Discovery (T1418.001).

The new sub-techniques add more detail and taking advantage of them will require some manual analysis. The good news is that the additional granularity will allow you to represent different types of software discovery that can happen at a more detailed level. These types of remaps can be done over time, because if you keep something mapped to Software Discovery, then it’s still correct. You can map new stuff to the sub-techniques and come back to the old ones to make them more precise as you have time and resources.

TL;DR, if you do just Step 1 while mapping things that are deprecated to NULL, then it will still be correct. If you do Step 2, then you’ll have pretty much everything you mapped before now also mapped to the new Mobile ATT&CK. If you complete Step 3, then you’ll get the newfound power of sub-techniques!

### ATT&CK for ICS Joins attack.mitre.org

ATT&CK for ICS launched at the beginning of 2020 on a MediaWiki site similar to how attack.mitre.org used to appear. Being on a separate site has let it develop and mature independently while we’ve added it to one ATT&CK resource at a time. Today we’ve added ATT&CK for ICS to our most visible resource, the ATT&CK website (attack.mitre.org).

What’s changed? First off, ATT&CK for ICS will no longer have that nostalgic ATT&CK Wiki look and feel, and links to ATT&CK for ICS will need to be updated. Second, we’ve merged the Groups and Software from ICS, adding ICS techniques to Group and Software pages that existed on both sites, and updating descriptions to include both.

![](https://cdn-images-1.medium.com/max/1024/1*vfPGgsdQL7OofPV8jDkl6w.png)

Finally, we’ve merged Data Sources and Data Components in from ATT&CK for ICS. Since there’s quite a bit of overlap between ICS and Enterprise Data Sources we’ve added a filter that allows you to see just Enterprise, just ICS, and all Data Sources and Components on both the overall Data Sources list and individual Data Source pages.

![](https://cdn-images-1.medium.com/max/780/1*vKMoYC9fQVqFbJKRcnpu_Q.png)

What hasn’t changed? ATT&CK for ICS’s content hasn’t changed and its STIX representation remains in the same place. We will also be keeping the previous website in place until October 2022 to avoid breaking your deep links. We will have increasingly dire warnings on each page reminding people to update their links before it is eventually deprecated. In the future, content will only be updated on attack.mitre.org and not the MediaWiki site.

### What’s Left in 2022?

We’ve just released our 2022 roadmap and continue to work across the framework. In v12 we plan on adding a new object related to groups in ATT&CK, Campaigns. Check out the slides from Matt Malone’s talk from ATT&CKcon 3.0, our recent roadmap blog post, or stay tuned for more details coming soon about their implementation.

We continue our work on improving the macOS platform and plan to focus on improvements to Linux between now and October. Please reach out to us via attack@mitre.org or via our Slack if you’d like to contribute knowledge of what adversaries have been up to on either platform.

As always, if you have feedback, comments, contributions, or just want to ask questions, connect with us on email, Twitter, or Slack.

©2022 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 22–00744–2.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=599a9112a025)

* * *

ATT&CK Goes to v11 was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
