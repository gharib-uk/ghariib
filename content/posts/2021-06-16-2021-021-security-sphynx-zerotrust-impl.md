---
title: "2021-021-Security Sphynx ZeroTrust implementation prep- part2"
date: Wed, 16 Jun 2021 01:52:26 +0000
draft: false
type: posts
categories: 
- 
---
# 2021-021-Security Sphynx ZeroTrust implementation prep- part2

<br/>

<br/>
EO from President Biden asked for a plan to create Zerotrust implementation in the next 90 days (well, 70ish days now… as of 23 May)

https://twitter.com/SecuritySphynx/status/1390475868032618496

@securitySphynx

“CIO: Zero Trust is the way…”

What is the optimal configuration (read: easiest) zero trust config?

Are there different ways to implement Zero Trust?\`

https://solutions.pyramidci.com/blog/posts/2021/february/the-swiss-cheese-approach/

https://tulsaworld.com/opinion/columnists/zero-trust-security-assume-that-everyone-and-everything-on-the-internet-is-out-to-get/article\_f6bdbfad-1aae-5063-8ac0-6a1faf5a244c.html

https://www.reddit.com/r/devops/comments/bqo6kp/open\_source\_or\_cheap\_zero\_trust\_beyondcorp/

https://opensource.com/article/17/6/4-easy-ways-work-toward-zero-trust-security-model

  
https://dodcio.defense.gov/Portals/0/Documents/Library/(U)ZT\_RA\_v1.1(U)\_Mar21.pdf

What is ZTA?

  
Who are your users?  
What Devices in use?  
Device attestation/health checks  
Applications exist?  
Connections exist?  
Not just into/out of the traditional LAN network - do you understand dependencies of applications and databases and how the traffic flows?  
Where is the data/traffic? coming from? Going to?  
When is this activity occurring and what is expected?  
WHY: Need to balance the access to technical resources in a rapidly evolving and dynamic business landscape that ceases to exist within the confines of normal security perimeters.  
Mobile workforce - how much work can you get done without ever getting on the VPN?  
Blockers  
Technical Debt  
IT Hygiene  
Zero Trust REQUIRES the pre-work of establishing baselines. You cannot detect abnormality in the absence of normality.  
Policy should exist to drive what the specifications of a baseline system, server, application, etc will be.  
Network traffic, endpoint performance, SIEM tuning, endpoint agent/software accountability  
ZTA is less useful if you're not doing basic patching, application updates, and allowing local admin on the system level).  
Legacy Systems:  
Not designed with this approach in mind, and often costly to modernize.  
Asset Management  
Where are your assets and how are they used? A “rough estimate” of endpoints is never good enough.  
What are you logging? What AREN’T you logging?  
User rights auditing  
Stale accounts, service accounts, HR Workflows for onboarding/offboarding  
Limitations of admin rights  
Local admin/password expiration issues for sales/travelling employees  
Human resources/talent  
Politics: Getting support/$$/Buy-in for retrofitting applications that are “working just fine” is a huge political/business hurdle.  
Where to go from here:  
SaaS/PaaS/etc offerings  
What can you move from traditional off-prem solutions to cloud-based services (more up to date, regularly reviewed for security vulnerabilities, offloading responsibility of maintenance, SSO capabilities)  
AAA requirements  
MFA is a MUST. No, it's not perfect, but it is one more layer in efficacy.  
Have discussions around REAL RBAC needs BEFORE implementing a solution. It is easier to expand permissions than it is to take them away. Resist the idea that the easy button of broad stroke permissions is always the right choice.  
Identify data owners, make them responsible for RBAC development with technical departments.  
Quantify risk associated with mishandled resources for crown jewels (see previous section on politics).  
Change control around permissions, access  
Security as an active participant in the development/acquisition of new products, software, services, or organizations Like remodeling a house, it is much easier to build security into the process than hire someone to retrofit it later..  
What auditing are you doing? Have you baselined behavior? Where are your logs going, and WHO IS RESPONSIBLE FOR REVIEWING THEM.  
Manage the Endpoint: Stop thinking about the perimeter as your weakest point. The endpoint is critical and increasingly vulnerable, mobile, out of traditional “control”. Real time, actionable data and capabilities are critical to remediation and progress.  
Asset Inventory (again)... Then…  
HIDS/Firewall  
Patch  
Applocker/Application Controls  
Lather, rinse, repeat.  
DLP Classification  
It’s hard, it’s time-consuming, and it requires a LOT of support for business unit owners.  
Capture metrics, then set KPIs and regular check ins to reduce MTTP/MTTR/MTTD

Would you like to know more?  
https://www.beyondtrust.com/blog/entry/why-zero-trust-is-an-unrealistic-security-model

#### [Source](http://brakeingsecurity.com/2021-021-security-sphynx-zerotrust-implementation-prep-part2)

<br/>
---
