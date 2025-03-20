---
title: "<div>Introducing ATT&CK Campaigns</div>"
date: 2025-01-02
categories: 
  - "security"
---

![](https://cdn-images-1.medium.com/max/1024/1*_CSbbgPWimTXkEud2wxlIA.png)

### **Introducing Campaigns to MITRE ATT&CK**

**_By:_** _Amy Robertson, Jared Ondricek, and Matt Malone_

We’ve talked about building Campaigns into ATT&CK in our ATT&CK 2022 roadmap, at ATT&CKCon 3.0, and most recently on the SANS Threat Analysis Rundown but their release is now nigh! Our initial collection of Campaigns will be available starting with our ATT&CK v12 release on October 25, when you’ll be able to leverage the Campaigns structure for all of your ATT&CK use cases. Prior to the release, we’re taking the opportunity to walk you through our vision for Campaigns, give you a tour of Campaigns elements, and cover our longer-term Campaigns plans.

**The Campaigns Vision**

For our purposes in ATT&CK, we use “Campaigns” to describe a grouping of intrusion activity conducted over a specific period of time with common targets and objectives. A key aspect of Campaigns is that the activity may or may not be linked to a specific threat actor.

Our vision for Campaigns is to provide users with another way to view the evolution of malicious cyber operations. Threat actor activity in ATT&CK currently encompasses a broad set of behaviors that can inform a holistic picture of the adversary over time. But as adversaries evolve, their TTPs often change, and by introducing some structure with Campaigns, we hope to allow you to glean more actionable intelligence and context to inform your defense prioritization**.** Campaigns will enable you to identify trends, track significant changes in techniques used by various actors, and monitor the introduction of new capabilities (or exploited vulnerabilities). You’ll also be able to identify continued threat actor reliance on certain techniques regardless of the campaign objective and/or targets.

Campaigns will also allow us to more accurately categorize complex intrusion activity, including those involving multiple threats (such as Ransomware-as-a-Service operations) and parse out overlapping operations that have been given the same name. With the new structure, we’ll also be converting some of the Groups in ATT&CK to Campaigns. This will apply to Groups that meet our definition of a Campaign and only feature one cluster of activity (such as G0101/Frankenstein and G0014/Night Dragon).

As is our tradition of carefully integrating structural elements in ATT&CK, we’ll be incorporating a limited number of Campaigns into the v12 release. This initial collection of Campaigns will feature former Group entries that are more accurately categorized as Campaigns, a curated number of Campaigns linked to existing Groups, as well as unattributed Campaigns.

**Campaign Elements**

We structured Campaigns to visually align with Groups and Software pages, and the v12 release will feature an addition of a new “Campaigns” button on the main page tool bar for easy access.

<figure>

![](https://cdn-images-1.medium.com/max/649/1*rRkWQX7KklJV1lZ0H9pEhQ.png)

<figcaption>

_Figure 1: Example of the new ATT&CK tool bar with the “Campaigns” button._

</figcaption>

</figure>

The Campaigns homepage will include a Campaigns table featuring ID number, Name, and activity descriptions. The list of available Campaigns in the left column highlights the Campaigns added or converted to date. As we previously covered, we created some flexibility in terms of whether or not the activity was given a unique name — a limitation we currently face with Groups — by allowing a Campaign to be simply referenced by our own identifier (e.g., C0014) if it doesn’t already come with a name.

<figure>

![](https://cdn-images-1.medium.com/max/958/1*VpfouMBhClv5Zek8VGelyg.png)

<figcaption>

_Figure 2: A draft Campaign table, with unnamed activity referenced as C0014._

</figcaption>

</figure>

Each Campaign entry will feature a description of the intrusion activity, including details like known targeted countries and sectors where available, as well as any information that makes this Campaign particularly noteworthy.

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*4gW0dKk2KpkpJTTq2HvYtw.png)

<figcaption>

_Figure 3: A draft Campaign page for all of the Trekkies out there!_

</figcaption>

</figure>

Something we’ve been particularly mindful of is how to best capture the period of time related to a Campaign. We opted for the “First Seen” and “Last Seen” fields in the information box, with the corresponding reference citations, so users can see how a Campaign was scoped. For intrusion activity assessed to be ongoing at the time of report publication, we’ll add language to that affect in the Campaign description (e.g., “As of September 2022 security researchers assessed this activity was ongoing”) and update future versions of ATT&CK Campaign entries accordingly.

<figure>

![](https://cdn-images-1.medium.com/max/702/1*hFnZT2dOn2qoueCEwvBA9g.png)

<figcaption>

_Figure 4: An example Campaigns information box with time frame fields and citations._

</figcaption>

</figure>

As with Groups and Software, we’ve created a “Techniques Used” table to capture actor procedure examples observed during a Campaign, with a couple of significant differences.

1\. We’ll add as much detail as reporting allows regarding specific commands or steps taken by the actors, to help ATT&CK users identify corresponding detection and mitigation opportunities. We’ve found this concept to be more challenging for Group and Software pages, as those tend to aggregate a variety of reporting examples over time, resulting in more generic procedure example language.

2\. We’ll preface our Campaign procedure examples with the Campaign name or associated ID number, to separate it from techniques already found on a Group page. We realize the utility of this may not be immediately evident while looking at a Campaign page, but this allows for the procedure examples to stand out separately when a Campaign is associated with a Group (and, hopefully, allows for smoother integration in the future if an unattributed Campaign is later attributed to a Group).

<figure>

![](https://cdn-images-1.medium.com/max/790/1*Od0xrpCbicBcN1AeD8S18w.png)

<figcaption>

_Figure 5: Separate procedure examples as seen from a Group page, based on a fictional Campaign. The top line is the existing procedure example for T1566.001 for a Group, while the second line is specifically related to the associated Campaign._

</figcaption>

</figure>

**What does this mean for Groups and Software?**

We’ve made two key changes to Group and Software pages as they relate to Campaigns. As previously mentioned, techniques and corresponding procedure examples mapped to a Group-attributed Campaign will carry over to the associated Group page. We’ll also continue to map Campaign-specific procedure examples to Software pages.

We’ve added a Campaigns table to associated Group and Software pages, so ATT&CK users can easily reference Campaign ID numbers, Names (when applicable), and the Campaign description.

Group and Software pages will otherwise remain visually unchanged, and we’ll continue to update them separately as a collective list of all observed techniques. We want to preserve the functionality of ATT&CK Navigator Layers in that respect, for ATT&CK users who want to focus on all techniques used regardless of time or target.

**Introducing the Campaign STIX Object**

With the addition of Campaigns to ATT&CK, the ATT&CK Data Model (which can be found in our Usage document), has expanded to encompass these changes. The diagram below portrays how all the moving pieces work together, with the new additions of the Campaign object type and the Relationships connected to Campaigns. It’s important to note, there are no changes to objects that previously existed in ATT&CK. Software written to read earlier versions of ATT&CK should continue to work, albeit missing data that only appears in Campaigns.

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*jVbUes3GwidA_ionbx6hbw.png)

<figcaption>

Figure 6: Campaign STIX relationships

</figcaption>

</figure>

Now that you’ve seen our data model, we’d like to introduce you to the star of the show — the STIX Campaign object. As a part of the ATT&CK Data Model, it makes use of the same STIX extensions that can be found there, such as x\_mitre\_version. However, in addition to those previously documented fields, here is the breakdown of how ATT&CK utilizes each field that is unique to the Campaign object:  
  
Standard STIX fields:

- type: Follows the STIX specification
- name: The name used to identify the Campaign. If no name is given, then this field will contain an ATT&CK identifier in the form CXXXX
- description: Follows the STIX specification
- aliases: Used to hold associated Campaign names
- first\_seen (timestamp): The time frame that this Campaign was first seen. ATT&CK makes use of this field only to the level of granularity of month/year. The day and time part of this timestamp field should be ignored by parsers when displaying ATT&CK Campaign information
- last\_seen (timestamp): The time that this Campaign was last seen or reported. ATT&CK makes use of this field only to the level of granularity of month/year. The day and time part of this timestamp field should be ignored by parsers when displaying ATT&CK Campaign information
- objective: Not used by ATT&CK

Extensions of the STIX Spec:

- x\_mitre\_first\_seen\_citation (string): One to many citations for when the Campaign was first reported in the form “(Citation: <citation name>)” where <citation name> can be found as one of the source\_name of one of the external\_references.
- x\_mitre\_last\_seen\_citation (string): One to many citations for when the Campaign was last reported in the form “(Citation: <citation name>)” where <citation name> can be found as one of the source\_name of one of the external\_references.

As mentioned above, we have also added **three new STIX Relationships** that connect Campaigns to the rest of the ecosystem; namely that Campaigns can optionally be **attributed to a Group, use Software, or use Techniques**. The STIX Relationship objects themselves have no special modifications from the STIX standard and simply connect Campaigns to those previously existing objects. At a glance this seems straightforward enough, but there are some things to be aware of if you are parsing ATT&CK v12 STIX going forward.

When gathering data about Groups that have Campaigns attributed to them, it’s a bit more complex to parse out all the Techniques and Software that are used by the Group. For Campaigns associated with a Group, we won’t be creating relationships between techniques and Software in that Campaign and the Group, if you would like to view the inclusive list, you’ll need to combine technique sets and Software usage.

**Combining Technique Sets:** To get a comprehensive Group Technique view, you’ll need to combine the set of Techniques that are directly used by the Group with the set of Techniques used by all of their associated Campaigns.

<figure>

![](https://cdn-images-1.medium.com/max/638/1*1k6NxIMlGneRco1xvLe_1w.png)

<figcaption>

Figure 7: Example of Group Technique STIX inheritance

</figcaption>

</figure>

**Mapping Software Object Usage:** To holistically map out Software object usage, you’ll identify the Groups, the Group-attributed Campaigns, and the unattributed Campaigns using the Software, and combine them for the full picture.

For further technical details on how to handle retrieving all Techniques or Software that a Group uses starting with the v12 release and how it differs from the v11 and prior releases, please refer to the Relationships Microlibrary section of the GitHub Usage document.

**What to Expect Going Forward**

We’ll continue modifying and building out Campaigns, with the eventual goal of revisiting a major Group pages in ATT&CK and reconstructing earlier Campaigns to reflect how these actors have evolved over time. We’ll also shift focus from one-off or unattributed Campaigns to more complex Campaigns attributed to some of the more populated Group entries, such as the SolarWinds intrusion and G0016/APT29.

Campaigns will also serve a key role in tying together the various ATT&CK matrices — Enterprise (Cloud, Containers, macOS, and Linux), Mobile, and ICS, to further document how adversaries pivot across these domains using a variety of techniques to accomplish their objectives.

We greatly appreciate the community’s feedback on Campaigns to date, and as we continue to develop Campaigns, we welcome your input. Our Contributions page will be updated in the near future to include more detailed guidance and we look forward to connecting with you via email, Slack, or Twitter.

©2022 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 22–00744–13.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=6b15baa6cbb4)

* * *

Introducing ATT&CK Campaigns was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
