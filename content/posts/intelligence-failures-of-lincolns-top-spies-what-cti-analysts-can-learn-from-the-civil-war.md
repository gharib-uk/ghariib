---
title: "Intelligence Failures of Lincoln’s Top Spies: What CTI Analysts Can Learn From the Civil War"
date: 2025-01-02
categories: 
  - "security"
---

_Guest Post by ATT&CKcon 3.0 Keynote Speaker, Selena Larson_

<figure>

![](https://cdn-images-1.medium.com/max/1024/1*Lt18htHq69IsEJ-3staO2w.jpeg)

<figcaption>

Allan Pinkerton (Alexander Gardner — Library of Congress)

</figcaption>

</figure>

At the onset of the Civil War, a man whose name would eventually become synonymous with famous American detectives was reportedly providing false reports to the Union’s top general. Allan Pinkerton, who once successfully smuggled Abraham Lincoln into Washington, D.C. to avoid a rumored assassination attempt before he was even sworn in as president, acted as General George McClellan’s top intelligence officer. He was considered one of the best spymasters in the United States, responsible for effectively founding the nation’s first secret service.

In this piece, we’ll dive into some major intelligence reporting failures that dogged the renowned spymaster, how effective and concise intelligence reporting can change the course of history, and how the MITRE ATT&CK framework can help streamline and effectively communicate actionable threat intelligence.

Pinkerton was a detective when he first got to know Lincoln, but quickly became an indispensable intelligencer for the Union, first in the nation’s capital and then on the battlefield, working as a Civil War spymaster in 1861–1862. He operated a large team of spies who conducted counterespionage operations throughout Washington and information gathering expeditions into enemy territory. Pinkerton’s successes and failures are many — he made many of his own tactical intelligence failures that cost at least one spy his life during the Civil War — but there is a lot modern day intelligence analysts can learn from him. Specifically, from his intelligence reports.

According to author Douglas Waller, author of _Lincoln’s Spies_, Pinkerton was not very good at validating or communicating information, or transforming it from data into intelligence. Throughout his time operating a secret service on behalf of the Union, he collected a lot of information. But that information was frequently poorly vetted, based on single sources, or received from biased narrators. And often, the information was ineffectively communicated, or outright falsified.

By dissecting the failures of the nation’s first intelligence service spymaster, modern day threat intelligence analysts can learn how and why effective intelligence communication and report writing can have major effects on an organization — and, in some cases, have the potential to change the course of history.

**HiPPO Bias**

One of the biggest failures plaguing Pinkerton’s reporting apparatus was his desire to please his boss. General McClellan was famously slow to take any offensive actions against the enemy, holding a deep fear of failure that paralyzed him into inaction.

McClellan reportedly believed the Confederate military to be much larger than it actually was, in part due to the “intelligence” provided to him by his top spy. In fact, the relationship between Pinkerton and McClellan was more like a self-licking ice cream cone. While stationed with McClellan in Washington, Virginia, and Maryland, Pinkerton worked his network of operators to collect information on enemy troop movement and the size of the Confederate army. Sometimes information proved to be correct; other times it was outright false. But in most cases, Pinkerton cherry-picked data that supported his boss’s beliefs of an opposing force either equal to or out-sizing the Union military, ignoring accurate information on the small size of the Confederate forces and further inflating already inflated estimates to appeal to McClellan’s beliefs.

> “Loyal to the point of sycophancy, Pinkerton never doubted the general’s ability as a commander. Instead of serving his country or his president as a true intelligence officer, he made his friend happy.” _Lincoln’s Spies_

Pinkerton was demonstrating Highest Paid Person’s Opinion (HiPPO) bias, or the idea that analysts collect and disseminate information in a way that favors or appeals to existing beliefs within an organization, typically driven by leadership.

> “Pinkerton admitted that he and McClellan had conspired to cook the books. In a later November 15 letter to the general, Pinkerton explained that his estimate of Confederate strength ‘was made large, as intimated to you at the time, so as to be sure to cover the entire number of the Enemy that our army was to meet.’ The controversial sentence appeared to show that before Pinkerton issued his October 4 report \[reporting double the total number of actual Confederate troops\], he and McClellan agreed to deliberately inflate the confederate numbers to be sure they included troops Pinkerton’s agents might not know about. ”

This can be a frequent issue for analysts tasked with certain objectives and directives, but it can also be detrimental to the organization’s decision making and ultimate success. For example, if leadership believes that Russian state-sponsored threats are the most important and likely the most targeted to their organization, defenders and analysts will be spending more key resources hunting for and defending against these threats, with the potential to miss or disregard tactics, techniques, and procedures (TTPs) associated with other relevant, but different, activity.

_Use data to build your case._ In her 2019 ATT&CKcon 2.0 keynote, Google’s Toni Gidwandi explained how the MITRE ATT&CK matrix can be a “powerful corrective” to HiPPO bias and enable security teams to understand what is happening in the landscape and how it translates to impacts on their organizations.

Beyond indicators of compromise (IOCs), ATT&CK allows defenders to visualize threat behaviors in a digestible way to show what TTPs are observed and impacting an organization versus what stakeholders expect or want to focus on.

Analysts can create mappings of MITRE ATT&CK to malware, malware families and techniques observed in their environment. Subsequently, analysts can craft search queries to help with threat hunting and detection efforts. For example, mapping and searching on specific execution techniques such as certutil or BITSAdmin which are being used to download follow-on payloads.

By identifying the most impactful behaviors, and possible gaps in defense, security teams can prioritize hunting, detection, and response based on observable threat behaviors rather than requests or knee-jerk reactions from stakeholders.

Ultimately, Pinkerton’s analysis failed his organization — his reporting coupled with McClellan’s ego and general aversion to taking decisive action may have cost the Union military successes early on in the Civil War.

**Reporting Is Not Letter Writing**

In addition to some reports containing easily disproved inaccuracies, Pinkerton and many of his staff typically wrote very long reports, with much of the key details hidden among flowery language, tens of pages deep. Effectively communicating actionable intelligence is a common issue with cyber threat intelligence dissemination, and it’s nice to know our predecessors experienced similar flaws.

> “He always wrote \[intelligence reports\] in the form of a letter, and they began with a flowery opening officers of the day commonly used, such as ‘I have the honor to report…’” _Lincoln’s Spies_

Pinkerton also reportedly doodled in the margins, drawing cartoon fingers to indicate what the most important parts of the reports were.

Succinctly and effectively communicating intelligence through written reports is difficult, but there are some key characteristics of good intelligence reporting that can help improve efficiency, streamline the writing process, and provide stakeholders with relevant data.

_Put the most important information first_. Frequently referred to as the stating Bottom Line Up Front (BLUF), immediately detailing the findings of your reporting and why they matter to an organization is crucial. This can be considered the “So What?” portion of the report. Most people — especially key stakeholders like executive audiences — will not read every word of an in-depth intelligence report. It is therefore important to ensure that in the short amount of time allotted for consuming reporting, they can read and understand the points that matter most.

_Be concise_. Pinkerton didn’t need flowery language and neither do you. I have said this before, but I firmly believe people should not require a thesaurus to read and understand threat intelligence reporting. The report should contain relevant information such as: What happened, why does it matter, and what can we do about it? Items such as anecdotes, extraneous clauses, and navel-gazing are generally unnecessary.

_Consider your audience_. Executives likely don’t need details of deconstructed malware. Security operations analysts likely don’t need geopolitical analysis of events occurring in places where the business does not operate. Threat intelligence analysts should always be aware of who is reading reports and why. Make sure you know the answer to: What decisions are being made based on this data? Gathering intelligence requirements and understanding how your audience is using intelligence throughout the organization can help shape and improve your reporting.

MITRE ATT&CK has become the universal framework for threat actor TTPs, and can be used to quickly distill and communicate threat intelligence. But where and how it’s used varies based on the audience receiving the information.

For example, in February 2022, intelligence agencies from the United States and United Kingdom published a joint advisory on a new malware called Cyclops Blink targeting small office/home office routers attributed to the Russian state actor Sandworm. The 10-page advisory was designed as an overview of the malware and related threats, documenting Sandworm’s historic and current activity and its relevance in the overall threat landscape. In this report, the MITRE ATT&CK mappings were presented at the end, to add additional insight and technical details to an otherwise fairly high-level, strategic report. However, in a companion malware analysis report published by the UK’s National Cyber Security Centre, the ATT&CK mappings were presented on page three of 20, demonstrating the framework can be used to summarize tactical threat intelligence.

Like any tool, where and how you use MITRE ATT&CK to document TTPs is crucial for an audience’s understanding of the threats.

**Always Evaluate OSINT**

While Pinkerton collected massive amounts of information and distributed it whole cloth to his superiors, there was little explanation given to where the information came from or its validity.

> “Rarely did Pinkerton include in his reports an evaluation of a source’s reliability beyond a general impression he had of it.” _Lincoln’s Spies_

Pinkerton was operating based on human intelligence, information collected by his operatives in the field. Much of it was gossip; some of it reached his ears by a convoluted game of telephone. A lot of it was reliable and accurate — some of it was not.

As intelligence analysts, understanding and evaluating the veracity of information is crucial to communicating and acting on it. Primary sources of intelligence — the data we collect on our networks — we typically understand to be reliable. But we also rely on open source intelligence (OSINT) to form a whole picture of adversary threat behaviors and an understanding of the threat landscape.

Unfortunately, there is a lot of bad information on the internet. The online claims of unconfirmed hacking campaigns during the ongoing war in Ukraine is an excellent example of information spreading far and wide without validation, and likely making it into intelligence briefings on the conflict.

There are multiple questions analysts should ask themselves when reviewing third-party data to support original research, for example:

- What is the visibility of the individual or organization?
- What evidence are their claims based on?
- Is this evidence available to me?
- Does this overlap with known threat activity?
- _Cui bono_? Or, who benefits and how?

There is always inherent bias in visibility; vendors or anti-virus companies will only have data from the organizations and geographies in which it is used. If the visibility is limited, it might not be an effective source for verifying or supporting existing hypotheses. Being able to independently validate or invalidate evidence provided in open-source artifacts with internal tools and resources can help further your own investigations or reporting. And finally, considering political, financial, economic, etc. motivations in external reporting can help identify potential biases in reporting and inform your assessments of a source’s reliability.

The MITRE ATT&CK framework exists in part to help answer these questions, especially for providing validated, authoritative third-party intelligence reporting.

**A Dictionary of Threats**

While the United States was fighting the bloodiest war in the nation’s history, an idea was blossoming among philologists in the United Kingdom. English speakers had colonized many parts of the world, with English customs and culture forcing itself into existing cultures and communities, with paltry existing resources standardizing vocabulary. Academics in the UK argued that there should be a single authoritative resource to define the English language, documenting and establishing the correct form of communications.

Formally proposed in 1857, what would become known as the Oxford English Dictionary would eventually achieve its goal of standardizing English words beginning in 1884. It was a massive undertaking that brought together academics, historians, and the English-speaking public to collect and define words. In fact, the dictionary could not have been written without considerable public assistance.

The MITRE ATT&CK framework has become the universal dictionary of TTPs, in large part due to contributions from analysts and researchers around the globe. According to MITRE, 155 people contributed to the framework in 2021. (In fact, this year my Proofpoint colleague Michael Raggi contributed an update to ATT&CK Technique T1221 to include a novel RTF template injection technique observed in use by multiple threat actors.)

The authoritative nature of the framework has allowed analysts to verify open-source reporting, and better understand the nature of threat actors. It has also allowed researchers to more effectively document and communicate threat behaviors, prioritize detections, and improve defense. By standardizing how we identify and classify threat behaviors, actionable intelligence can be more easily communicated to a variety of stakeholders.

Pinkerton did not have a reliable threat intelligence framework or dictionary off which to operate; indeed he was trailblazing the creation of a secret service that had never before existed. And while his early work helped pave the way for modern day spying and the development of the Secret Service, he and his team were far from perfect. But by examining the intelligence reporting failures documented by modern historians, threat intelligence analysts can be better prepared when they too one day may be called on to help change the course of history.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=35be8d12884)

* * *

Intelligence Failures of Lincoln’s Top Spies: What CTI Analysts Can Learn From the Civil War was originally published in MITRE ATT&CK® on Medium, where people are continuing the conversation by highlighting and responding to this story.

Go to Source
