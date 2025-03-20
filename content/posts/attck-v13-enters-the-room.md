---
title: "<div>ATT&CK v13 Enters the Room</div>"
date: 2025-01-02
categories: 
  - "security"
---

![](https://cdn-images-1.medium.com/max/1024/1*PkApzI8NMYPKdGGq2ZA1Vw.png)

### ATT&CK v13 Enters the Room: Pseudocode, Swifter Search, and Mobile Data Sources

It’s not like a regular Tuesday, it’s a lucky Tuesday — ATT&CK v13 has arrived. As we outlined in our Roadmap, we’re working toward enhanced tools for lower-resourced defenders, improving ATT&CK’s website usability, enhancing ICS and Mobile parity with Enterprise, and evolving overall content and structure this year. ATT&CK v13 is bringing some analytics pseudocode, Mobile-specific data sources, key website updates, ICS Asset refactoring, and more Cloud and Linux coverage. For the rest of our regular updates/additions across Techniques, Software, Groups and Campaigns take a look at our release notes, our (new) detailed changelog, or our (new) changelog.json.

### Defensive Easy Button: Pseudocode for Detection

This release features a new defensive ‘easy button’, with the addition of CAR pseudocode to a number of our data components. These pseudocode analytics add more context on what you should find and collect, by describing at a high level the steps involved in detecting certain types of behaviors. You can use these analytics as a blueprint for your custom detections, leaving you with more time to spend on the defensive activities of your choice.

![](https://cdn-images-1.medium.com/max/838/1*qbIfAT_TjlqGitKsI1z65Q.png)

Moving forward we’ll be revamping Mitigations and improving defenses tactic-by-tactic, by incorporating analytics from CAR and dynamic research into the data components falling under a given tactic. If you’d like to join our efforts, let us know or join us in the **#defensive\_attack** channel.

### ATT&CK’s Infrastructure: ATT&CK Search 2.0 and changelog.json

Two of our most frequently requested items are now here: faster search and machine-readable changelogs!

We’ve **all** experienced the patience-building opportunity associated with using the ATT&CK website search, so we’re thrilled to finally introduce a new and improved search! While it won’t be breaking the sound barrier anytime soon, it will save you some serious time when you’re trying to figure out which techniques cover Import Access Table. Your initial search with the enhanced version will be in the 5-second range, with following queries resolving near instantaneously. We’ll continue to adjust this important feature and appreciate all of you who stayed with us through the search bar trials. Let us know if you spot any new corner cases!

Another significant addition to this release is a machine-readable changelog. You’ll now be able to access and parse through the changelog, quickly identifying and integrating the updates. For more details check out the changelog.json format details in our GitHub. Our release notes format has also been improved, now documenting New, Major Version Changes, Minor Version Changes, and Patches for each of Techniques, Mitigations, Data Sources, Data Components, Software, Groups, and Campaigns. If you’re wondering what “Patches” are, it’s what we’re calling changes so minor (e.g., typos, URL fixes, grammar) no version update was necessary.

We don’t expect quite as much celebration for our ATT&CK Navigator updates, but new updates coming with ATT&CK v13 enable you to further customize your layer colors, scoring, image orientation and preset image sizing — for more details, check out the Navigator release notes (click on the ? in the top right of Navigator).

![](https://cdn-images-1.medium.com/max/138/1*1N1Gp76i9hL5TaCaMl7_Kw.png)

### Mobile: Data Sources are Live

On the Mobile front, you’ll now be able to access Mobile-specific Data Sources! Mobile has joined the filter list along with Enterprise and ICS, enabling you to toggle between the data sources for your chosen domain(s). In addition to the new Mobile-specific sources, the cross-domain mappings with Enterprise are now more accessible. The Mobile-specific and cross-mapped sources are also listed on the individual Data Source pages.

![](https://cdn-images-1.medium.com/max/480/1*3qu362KQAsQCKvDZuYZUIw.png)

Over the next few months, we’ll continue to add to our Mobile data sources, as well as architecting structured detections. If you’d like to contribute to this space or start a conversation, email or slack us (don’t call).

### ICS: Asset Refactoring for Enhanced Coverage

The ICS matrix features new techniques, a freshly cross-mapped campaign, and updates to Assets (the functional components of the systems in the ICS domain). Our Assets refactoring effort seeks to align how different industries describe assets, in order to better map device functionality to core dependencies and associate the Assets to the relevant techniques. Through this effort we’ve also been working to address gaps from underreported industries. We’ll continue to collaborate with the ICS community to better build out and describe assets and create these mappings. Our goal is to include Assets in the metadata box on technique pages to help inform defenders about a device’s susceptibility to techniques.

### Campaigns: Criminals, APTs and Cross-Mappings

Our Campaigns game is going strong, with v13 showcasing a blend of recent cyber intrusions and those previously captured in a Group page. A couple of our more contemporary entries include APT41’s compromise of U.S. state government networks (C0017), and an AvosLocker ransomware-as-a-service operation (C0018). Some of the older activity previously featured in Groups, include APT29’s Operation Ghost and the SolarWinds compromise. The star of this release, and one we’re particularly excited about, is the cross-domain Campaign entry, the 2016 Ukraine operation by Sandworm Team. Over the next several months, we’ll continue to focus on criminal group operations and expanding on the hybrid Campaigns that traverse domains.

### Cloud: Now with More Exec and LatMo Coverage

We assessed known gaps in the Execution and Lateral Movement tactics of the Cloud matrix and built out additions to address some of the disparities. These changes feature contributions from an ongoing partnership with the Cloud community to better represent behaviors in and against Cloud technologies, as well as reflecting how organizations are using Cloud in their operations. In the coming months, we’ll continue to expand coverage in these not-so-easy-to-capture cloud-related tactics, as well as evaluating where to develop more Exfiltration coverage.

Our end goal this year is to ensure that everyone can more effectively utilize ATT&CK for Cloud. If you have ideas or contributions to share, please email us or drift by the #**cloud\_attack** channel on the ATT&CK slack.

### Linux: Making the Penguin a Little More Secure

Our Linux team has spent the last few months going through contributions, coordinating with contributors, and navigating through open-source reporting for in-the-wild adversary behaviors. This release includes updated and new Linux-only (sub)techniques that will enhance the Linux defender’s toolset. We’ll continue building out Linux coverage in ATT&CK, as well as gaining a better understanding of the adversaries operating in this space. If you’d like to work with us or to join our very exclusive Linux channel **(#****linux\_attack**), we’d love to have a conversation.

### Next Up: v14 and ATT&CKcon

We know you’re still trying to catch your breath from all the v13 adjustments, but we’re still sprinting for v14! October’s release will feature upgraded coverage across domains, renovated mitigations, new cross-domain mappings, more pseudocodes, and Mobile structured detections.

We’ll also be releasing more details on ATT&CKCon 4.0 soon (October 24–25), so start getting ready with some light reading/watching of previous ATT&CKcon presentations or couch interviews.

As always, we look forward to connecting with you on email, Twitter, or Slack.

©2023 The MITRE Corporation. ALL RIGHTS RESERVED. Approved for public release. Distribution unlimited 22–00745–1

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=5cef174c32ff)

* * *

ATT&CK v13 Enters the Room was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
