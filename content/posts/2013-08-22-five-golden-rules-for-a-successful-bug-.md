---
title: "Five Golden Rules For A Successful Bug Bounty Program"
date: 2013-08-21T22:47:00.000-07:00
draft: false
type: posts
categories: 
- bug bounty program,disclosure,ikki,vulnerability acquisition program
---
# Five Golden Rules For A Successful Bug Bounty Program

<br/>

<br/>
Bug bounty programs have become a popular complement to already existing security practices, like secure software development and security testing. In this space, there are successful examples with many bugs reported and copious rewards paid out. For vendors, users and researchers, this seems to be a mutually beneficial ecosystem.

  

True is that not all bug bounty initiatives have been so successful. In some cases, a great idea was poorly executed resulting in frustration and headache for both vendors and researchers.  Ineffective programs are mainly caused by _security immaturity_, as not all companies are ready and motivated enough to organize and maintain such initiatives.  Bug bounties are a great complement to other practices but cannot completely substitute professional penetration tests and source code analysis. Many organizations fail to understand that and jump on the bounties bandwagon without having mature security practices in place.

  

Talking with a bunch of friends during BlackHat/Defcon, we came up with a list of five golden rules to set your bug bounty program up for success. Although the list is not exhaustive, it was built by collecting opinions from numerous peers and should be a good representation of what security researchers expect.

  
**If you are a vendor considering to start a similar initiative, please read it carefully.**  
  
The Five Golden Rules:  
  
**1\. Build trust, with facts**  

Security testing is based on trust, between client and provider. Trust is important during testing, and especially crucial during disclosure time. As a vendor, make sure to provide as much clarity as you can. For duplicate bugs, increase your transparency by providing more details to the reporter (e.g. date/time of the initial disclosure, original bug ID, etc.). Also, fixing bugs and claiming that they are not relevant (thus non-eligible to rewards) is a perfect way to lose trust.

  
**2\. Fast turn around**  

Security researchers are happy to spend extra time on explaining bugs and providing workarounds, however they also expect to get notified (and rewarded) at the same reasonable speed. From reporting the bug to paying out rewards, you should have a fast turn around. Fast means days - not months. Even if you need more time to fix the bug, pay out immediately the reward and explain in detail the complexity of rolling out the patch. Decoupling internal development life cycles and bounties allows you to be flexible with external reporters while maintaining your standard company processes.

  
**3\. Get security experts**  

If you expect web security bugs, make sure to have web security experts around you. For memory corruption vulnerabilities, you need people able to understand root causes and to investigate application crashes. Either internally or through leveraging trusted parties, this aspect is crucial for your reputation. Many of us have experienced situations in which we had to explain basic vulnerabilities and how to replicate those issues. In several cases, the interlocutors were software engineers and not security folks: we simply talk different languages and use different tools.

  
**4\. Adequate rewards**  

Make sure that your monetary rewards are aligned with the market. What's adequate? Check [Mozilla, Facebook, Google, Etsy and many others](http://blog.nibblesec.org/2011/10/no-more-free-bugs-initiatives.html). If you don't have enough budget - just setup a wall of fame, send nice swags and be creative. For instance, you could decide to pay for specific classes of bugs or medium-high impact vulnerabilities only. Always paying at the low end of your rewards range, even for critical security bugs, it is just pathetic. Before starting, crunch some numbers by reviewing past penetration test reports performed by recognized consulting boutiques.

  
**5\. Non-eligible bugs**  

Clarify the scope of the program by providing concrete examples, eligible domains and types of bugs that are commonly rejected. Even so, you will have to reject submissions for a multitude of reasons: be as clear and transparent as possible. Spend a few minutes to explain the reason of rejection, especially when the researcher has over-estimated severity or not properly evaluated the issue.

  
Happy Bug Hunting, Happy Bug Squashing!

#### [Source](http://blog.nibblesec.org/feeds/7573499249583206464/comments/default)

<br/>
---
