---
title: "Announcing Pwn2Own Berlin and Introducing an AI Category"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

_If you just want to read the contest rules, click_ _here__._

Willkommen, meine Damen und Herren, zu unserem ersten Wettbewerb in Berlin! That’s correct (if Google translate didn’t steer me wrong). While the Pwn2Own competition started in Vancouver in 2007, we always want to ensure we are reaching the right people with our choice of venue. Over the last few years, the OffensiveCon conference in Berlin has emerged as one of the best offensive-focused events of the year. And while CanSecWest has been a great host over the years, it became apparent that perhaps it was time to relocate our spring event to a new home. With that, we are happy to announce that the enterprise-focused Pwn2Own event will take place on May 15-17, 2025, at the OffensiveCon conference in Berlin, Germany. While this event is currently sold out, we do have tickets available for competitors, and we believe the conference will also open a few more tickets for the public, too. The conference sold out its first run of tickets in under six hours, so it should be a fantastic crowd of some of the best vulnerability researchers in the world.

We couldn’t go to a new venue without introducing a new category, and what technology has generated the most questions regarding its security posture? **Artificial Intelligence**. That’s why we are introducing the AI category with six targets in different frameworks. We’re going well beyond just prompt injections – you’ll need to execute arbitrary code to win this category. Last year, we introduced the **Cloud-Native/Container** category, and we were thrilled to see a successful Docker container escape at the contest. We’ll see if the AI category can make a similar splash or if it goes uncontested. I would bet there will be some participation – if I were a betting man.

The Cloud-Native/Container category returns as does the **Tesla** category. Tesla has been a great partner since 2019, and they continue to innovate and increase the security of their vehicles, and I’m sure they will take the learnings from the recent Pwn2Own Automotive forward to the Berlin event. For this event, we’re focused simply on impact and getting code execution in a target component on the vehicle. For some targets, that may mean you need to get code execution in multiple systems on the way. And no, the awards aren’t cumulative. For example, you may need to exploit the infotainment system on the way to the Autopilot, but you’ll only get the award for the Autopilot.

Of course, Pwn2Own wouldn’t be the same without our classic categories, like web browsers, OSes, and Enterprise Servers. Last year, the Master of Pwn was awarded to Manfred Paul, who successfully exploited all four web browsers at the contest. Altogether, we’ll again be offering more than $1,000,000 USD in cash and prizes at this year’s event. We’re looking forward to a new venue and an exciting event with some cutting-edge exploitation on display. Here is a full list of the categories for this year’s event:  

          \-- AI Category  
          \-- Web Browser Category  
          \-- Cloud-Native/Container Category  
          \-- Virtualization Category  
          \-- Enterprise Applications Category  
          \-- Server Category  
          \-- Local Escalation of Privilege Category  
          \-- Automotive Category

Of course, no Pwn2Own competition would be complete without us crowning a Master of Pwn (Meister von Pwn?). Since the order of the contest is decided by a random draw, contestants with an unlucky draw could still demonstrate fantastic research but receive less money since subsequent rounds go down in value. However, the points awarded for each unique, successful entry do _not_ go down. Someone could have a bad draw and still accumulate the most points. The person or team with the most points at the end of the contest will be crowned Master of Pwn, receive 65,000 ZDI reward points (instant Platinum status), a killer trophy, and a pretty snazzy jacket to boot.

Let's look at the details of the rules for this year's event.

**AI Category**

We’re excited to introduce this category as it goes beyond what other AI “hackathons” have done already. In the past, AI Hackathons have focused on using AI to develop vulnerabilities or other offensive frameworks. We're opening up the AI infrastructure that is used to run models for exploitation - vector databases, frameworks for running models, and development toolkits. An attempt in this category must be launched from the contestant’s laptop. For the NVIDIA Container Toolkit target, the attempt must be launched from within a crafted container image and execute arbitrary code on the host operating system.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/5ad6f0fe-970e-404d-a1ee-36c52ebb4b3b/AI-3.png?format=1000w)

_Back to top_

**Web Browser Category**

While browsers are the “traditional” Pwn2Own target, we’re continuously tweaking the targets in this category to ensure they remain relevant. We re-introduced renderer-only exploits a couple of years ago, and their reward remains at $60,000. However, if you have that Windows kernel privilege escalation or sandbox escape, that will earn you up to $100,000 or $150,000 respectively. The Windows-based targets will be running in a VMware Workstation virtual machine. Consequently, all browsers (except Safari) are eligible for a VMware escape add-on. If a contestant can compromise the browser in such a way that also executes code on the host operating system by escaping the VMware Workstation virtual machine, they will earn themselves an additional $80,000 and 8 more Master of Pwn points. Full exploits are still required for Apple Safari and Mozilla Firefox. Here’s a detailed look at the targets and available payouts:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/dee689f3-d1ab-442d-b3b5-1eb4a230a0b8/browsers-3.png?format=1000w)

_Back to top_

**Cloud-Native/Container Category**

We’re excited to have this category return for its sophomore season, and we’re hopeful even more contestants will target one of these container targets. For an attempt to be ruled a success against these three, the exploit must be launched from within the guest container/microVM and execute arbitrary code on the host operating system. The final target in this category is gRPC – a modern open-source high-performance Remote Procedure Call (RPC) framework that can run in any environment.  A success here must leverage a vulnerability in the gRPC code base to obtain arbitrary code execution. Here are the payouts for this category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/fafa4e06-b030-455c-b43a-86c6bc4d13d0/container-2.png?format=1000w)

_Back to top_

**Virtualization Category**

Some of the highlights for each contest can be found in the Virtualization Category, and we’re thrilled to see what this year’s event could bring with it. As usual, VMware is the main highlight of this category as we’ll have VMware ESXi alongside VMware Workstation as a target with awards of $150,000 and $80,000 respectively. Microsoft also returns as a target and leads the virtualization category with a $250,000 award for a successful Hyper-V Client guest-to-host escalation. Oracle VirtualBox rounds out this category with a prize of $40,000.

There’s an add-on bonus in this category as well. If a contestant can escape the guest OS, then escalate privileges on the host OS through a Windows kernel vulnerability (excluding VMware ESXi), they can earn an additional $50,000 and 5 more Master of Pwn points. That could push the payout on a Hyper-V bug to $300,000. Here’s a detailed look at the targets and available payouts in the Virtualization category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/bce8c7a9-3eda-4dda-95d4-fa74ed6396f6/virtualization.png?format=1000w)

_Back to top_

**Enterprise Applications Category**

Enterprise applications return as targets with Adobe Reader and various Office components on the target list once again. Attempts in this category must be launched from the target under test. For example, launching the target under test from the command line is not allowed. Prizes in this category run from $50,000 for a Reader exploit with a sandbox escape or a Reader exploit with a kernel privilege escalation and $100,000 for an Office 365 application. Word, Excel, and PowerPoint are all valid targets. Microsoft Office-based targets will have Protected View enabled where applicable. Adobe Reader will have Protected Mode enabled where applicable. Here’s a detailed view of the targets and payouts in the Enterprise Application category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/b5a1ad63-f71c-4f3b-b944-1c175b5a9283/entapps-3.png?format=1000w)

_Back to top_

**Server Category**

The Server Category for 2025 focuses solely on the server components we’re most interested in. These servers are often targeted by everyone from ransomware crews to nation/state actors, so we know there are exploits out there for them. The only question is whether we’ll see any of the competitors bring one of those exploits to Pwn2Own. SharePoint has been exploited in the wild, and in one case, part of that exploit chain was demonstrated at Pwn2Own. Microsoft Exchange has been a popular target for some time, and it returns as a target this year as well, with a payout of $200,000. This category is rounded out by Microsoft Windows RDP/RDS, which also has a payout of $200,000. Here’s a detailed look at the targets and payouts in the Server category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c7abaef2-38bf-4a1e-98b8-6f098bee53b8/servers.png?format=1000w)

_Back to top_

**Local Escalation of Privilege Category**

This category is a classic for Pwn2Own and focuses on attacks that originate from a standard user and result in executing code as a high-privileged user. A successful entry in this category must leverage a kernel vulnerability to escalate privileges. We’ve swapped Ubuntu Desktop for Red Hat Enterprise Linux for Workstations, while Apple macOS, and Microsoft Windows 11 return as targets in this category. Last year, an exploit demonstrated at Pwn2Own won the Pwnie Award for Best Privilege Escalation. It would be interesting if a Pwn2Own bug could go back-to-back. Here’s a detailed look at the targets and payouts in this category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1f0254a0-9d3d-41e5-87cd-ca2ff0cf4ed9/LPE.png?format=1000w)

_Back to top_

**Automotive Category**

Since adding the Automotive Category in 2019, we’ve seen some amazing and creative research displayed – so much so that we expanded to holding a Pwn2Own Automotive event. Still, we’re happy to have Tesla return as a target for this event. As previously mentioned, we’ve streamlined the rules for this category this year, but that doesn’t mean it’s any easier to win. We’ll have both the 2024 Tesla Model 3 and 2025 Tesla Model Y bench-top units available as targets. We conduct all tests on the bench-top units as attempting the exploits on an actual vehicle could prove hazardous to bystanders and other vehicles in the area. Here are this year’s awards for the Automotive Category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/86bc96bf-e156-4f5c-a818-990bf7afdff2/tesla-4.png?format=1000w)

_Back to top_

**Conclusion**

The complete rules for Pwn2Own 2025 are found here. As always, we encourage entrants to read the rules thoroughly if they choose to participate. If you are thinking about participating but have specific configuration or rule-related questions, email us. Questions asked over X (nee Twitter), BlueSky, Instagram, or other means will not be answered. Registration is required to ensure we have sufficient resources on hand at the event. Please contact ZDI at pwn2own@trendmicro.com to begin the registration process. Registration for onsite participation closes at 5 p.m. Central European Time on May 8, 2025.

Be sure to stay tuned to this blog and follow us on Twitter, Mastodon, LinkedIn, or Bluesky for the latest information and updates about the contest. We look forward to seeing everyone in Germany, and we hope someone has a new car to drive home from this year’s Pwn2Own competition.

With special thanks to our Pwn2Own 2025 partner Tesla

©2025 Trend Micro Incorporated. All rights reserved. PWN2OWN, ZERO DAY INITIATIVE, ZDI, TREND ZERO DAY INITIATIVE, and Trend Micro are trademarks or registered trademarks of Trend Micro Incorporated. All other trademarks and trade names are the property of their respective owners.

_If you just want to read the contest rules, click_ _here__._

Willkommen, meine Damen und Herren, zu unserem ersten Wettbewerb in Berlin! That’s correct (if Google translate didn’t steer me wrong). While the Pwn2Own competition started in Vancouver in 2007, we always want to ensure we are reaching the right people with our choice of venue. Over the last few years, the OffensiveCon conference in Berlin has emerged as one of the best offensive-focused events of the year. And while CanSecWest has been a great host over the years, it became apparent that perhaps it was time to relocate our spring event to a new home. With that, we are happy to announce that the enterprise-focused Pwn2Own event will take place on May 15-17, 2025, at the OffensiveCon conference in Berlin, Germany. While this event is currently sold out, we do have tickets available for competitors, and we believe the conference will also open a few more tickets for the public, too. The conference sold out its first run of tickets in under six hours, so it should be a fantastic crowd of some of the best vulnerability researchers in the world.

We couldn’t go to a new venue without introducing a new category, and what technology has generated the most questions regarding its security posture? **Artificial Intelligence**. That’s why we are introducing the AI category with six targets in different frameworks. We’re going well beyond just prompt injections – you’ll need to execute arbitrary code to win this category. Last year, we introduced the **Cloud-Native/Container** category, and we were thrilled to see a successful Docker container escape at the contest. We’ll see if the AI category can make a similar splash or if it goes uncontested. I would bet there will be some participation – if I were a betting man.

The Cloud-Native/Container category returns as does the **Tesla** category. Tesla has been a great partner since 2019, and they continue to innovate and increase the security of their vehicles, and I’m sure they will take the learnings from the recent Pwn2Own Automotive forward to the Berlin event. For this event, we’re focused simply on impact and getting code execution in a target component on the vehicle. For some targets, that may mean you need to get code execution in multiple systems on the way. And no, the awards aren’t cumulative. For example, you may need to exploit the infotainment system on the way to the Autopilot, but you’ll only get the award for the Autopilot.

Of course, Pwn2Own wouldn’t be the same without our classic categories, like web browsers, OSes, and Enterprise Servers. Last year, the Master of Pwn was awarded to Manfred Paul, who successfully exploited all four web browsers at the contest. Altogether, we’ll again be offering more than $1,000,000 USD in cash and prizes at this year’s event. We’re looking forward to a new venue and an exciting event with some cutting-edge exploitation on display. Here is a full list of the categories for this year’s event:  

          \-- AI Category  
          \-- Web Browser Category  
          \-- Cloud-Native/Container Category  
          \-- Virtualization Category  
          \-- Enterprise Applications Category  
          \-- Server Category  
          \-- Local Escalation of Privilege Category  
          \-- Automotive Category

Of course, no Pwn2Own competition would be complete without us crowning a Master of Pwn (Meister von Pwn?). Since the order of the contest is decided by a random draw, contestants with an unlucky draw could still demonstrate fantastic research but receive less money since subsequent rounds go down in value. However, the points awarded for each unique, successful entry do _not_ go down. Someone could have a bad draw and still accumulate the most points. The person or team with the most points at the end of the contest will be crowned Master of Pwn, receive 65,000 ZDI reward points (instant Platinum status), a killer trophy, and a pretty snazzy jacket to boot.

Let's look at the details of the rules for this year's event.

**AI Category**

We’re excited to introduce this category as it goes beyond what other AI “hackathons” have done already. In the past, AI Hackathons have focused on using AI to develop vulnerabilities or other offensive frameworks. We're opening up the AI infrastructure that is used to run models for exploitation - vector databases, frameworks for running models, and development toolkits. An attempt in this category must be launched from the contestant’s laptop. For the NVIDIA Container Toolkit target, the attempt must be launched from within a crafted container image and execute arbitrary code on the host operating system.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/5ad6f0fe-970e-404d-a1ee-36c52ebb4b3b/AI-3.png?format=1000w)

_Back to top_

**Web Browser Category**

While browsers are the “traditional” Pwn2Own target, we’re continuously tweaking the targets in this category to ensure they remain relevant. We re-introduced renderer-only exploits a couple of years ago, and their reward remains at $60,000. However, if you have that Windows kernel privilege escalation or sandbox escape, that will earn you up to $100,000 or $150,000 respectively. The Windows-based targets will be running in a VMware Workstation virtual machine. Consequently, all browsers (except Safari) are eligible for a VMware escape add-on. If a contestant can compromise the browser in such a way that also executes code on the host operating system by escaping the VMware Workstation virtual machine, they will earn themselves an additional $80,000 and 8 more Master of Pwn points. Full exploits are still required for Apple Safari and Mozilla Firefox. Here’s a detailed look at the targets and available payouts:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/dee689f3-d1ab-442d-b3b5-1eb4a230a0b8/browsers-3.png?format=1000w)

_Back to top_

**Cloud-Native/Container Category**

We’re excited to have this category return for its sophomore season, and we’re hopeful even more contestants will target one of these container targets. For an attempt to be ruled a success against these three, the exploit must be launched from within the guest container/microVM and execute arbitrary code on the host operating system. The final target in this category is gRPC – a modern open-source high-performance Remote Procedure Call (RPC) framework that can run in any environment.  A success here must leverage a vulnerability in the gRPC code base to obtain arbitrary code execution. Here are the payouts for this category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/fafa4e06-b030-455c-b43a-86c6bc4d13d0/container-2.png?format=1000w)

_Back to top_

**Virtualization Category**

Some of the highlights for each contest can be found in the Virtualization Category, and we’re thrilled to see what this year’s event could bring with it. As usual, VMware is the main highlight of this category as we’ll have VMware ESXi alongside VMware Workstation as a target with awards of $150,000 and $80,000 respectively. Microsoft also returns as a target and leads the virtualization category with a $250,000 award for a successful Hyper-V Client guest-to-host escalation. Oracle VirtualBox rounds out this category with a prize of $40,000.

There’s an add-on bonus in this category as well. If a contestant can escape the guest OS, then escalate privileges on the host OS through a Windows kernel vulnerability (excluding VMware ESXi), they can earn an additional $50,000 and 5 more Master of Pwn points. That could push the payout on a Hyper-V bug to $300,000. Here’s a detailed look at the targets and available payouts in the Virtualization category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/bce8c7a9-3eda-4dda-95d4-fa74ed6396f6/virtualization.png?format=1000w)

_Back to top_

**Enterprise Applications Category**

Enterprise applications return as targets with Adobe Reader and various Office components on the target list once again. Attempts in this category must be launched from the target under test. For example, launching the target under test from the command line is not allowed. Prizes in this category run from $50,000 for a Reader exploit with a sandbox escape or a Reader exploit with a kernel privilege escalation and $100,000 for an Office 365 application. Word, Excel, and PowerPoint are all valid targets. Microsoft Office-based targets will have Protected View enabled where applicable. Adobe Reader will have Protected Mode enabled where applicable. Here’s a detailed view of the targets and payouts in the Enterprise Application category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/b5a1ad63-f71c-4f3b-b944-1c175b5a9283/entapps-3.png?format=1000w)

_Back to top_

**Server Category**

The Server Category for 2025 focuses solely on the server components we’re most interested in. These servers are often targeted by everyone from ransomware crews to nation/state actors, so we know there are exploits out there for them. The only question is whether we’ll see any of the competitors bring one of those exploits to Pwn2Own. SharePoint has been exploited in the wild, and in one case, part of that exploit chain was demonstrated at Pwn2Own. Microsoft Exchange has been a popular target for some time, and it returns as a target this year as well, with a payout of $200,000. This category is rounded out by Microsoft Windows RDP/RDS, which also has a payout of $200,000. Here’s a detailed look at the targets and payouts in the Server category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c7abaef2-38bf-4a1e-98b8-6f098bee53b8/servers.png?format=1000w)

_Back to top_

**Local Escalation of Privilege Category**

This category is a classic for Pwn2Own and focuses on attacks that originate from a standard user and result in executing code as a high-privileged user. A successful entry in this category must leverage a kernel vulnerability to escalate privileges. We’ve swapped Ubuntu Desktop for Red Hat Enterprise Linux for Workstations, while Apple macOS, and Microsoft Windows 11 return as targets in this category. Last year, an exploit demonstrated at Pwn2Own won the Pwnie Award for Best Privilege Escalation. It would be interesting if a Pwn2Own bug could go back-to-back. Here’s a detailed look at the targets and payouts in this category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1f0254a0-9d3d-41e5-87cd-ca2ff0cf4ed9/LPE.png?format=1000w)

_Back to top_

**Automotive Category**

Since adding the Automotive Category in 2019, we’ve seen some amazing and creative research displayed – so much so that we expanded to holding a Pwn2Own Automotive event. Still, we’re happy to have Tesla return as a target for this event. As previously mentioned, we’ve streamlined the rules for this category this year, but that doesn’t mean it’s any easier to win. We’ll have both the 2024 Tesla Model 3 and 2025 Tesla Model Y bench-top units available as targets. We conduct all tests on the bench-top units as attempting the exploits on an actual vehicle could prove hazardous to bystanders and other vehicles in the area. Here are this year’s awards for the Automotive Category:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/86bc96bf-e156-4f5c-a818-990bf7afdff2/tesla-4.png?format=1000w)

_Back to top_

**Conclusion**

The complete rules for Pwn2Own 2025 are found here. As always, we encourage entrants to read the rules thoroughly if they choose to participate. If you are thinking about participating but have specific configuration or rule-related questions, email us. Questions asked over X (nee Twitter), BlueSky, Instagram, or other means will not be answered. Registration is required to ensure we have sufficient resources on hand at the event. Please contact ZDI at pwn2own@trendmicro.com to begin the registration process. Registration for onsite participation closes at 5 p.m. Central European Time on May 8, 2025.

Be sure to stay tuned to this blog and follow us on Twitter, Mastodon, LinkedIn, or Bluesky for the latest information and updates about the contest. We look forward to seeing everyone in Germany, and we hope someone has a new car to drive home from this year’s Pwn2Own competition.

With special thanks to our Pwn2Own 2025 partner Tesla

©2025 Trend Micro Incorporated. All rights reserved. PWN2OWN, ZERO DAY INITIATIVE, ZDI, TREND ZERO DAY INITIATIVE, and Trend Micro are trademarks or registered trademarks of Trend Micro Incorporated. All other trademarks and trade names are the property of their respective owners.

Go to Source
