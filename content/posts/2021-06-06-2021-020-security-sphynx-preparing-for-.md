---
title: "2021-020 Security Sphynx Preparing for ZeroTrust implementation - Part1"
date: Sun, 06 Jun 2021 18:19:29 +0000
draft: false
type: posts
categories: 
- 
---
# 2021-020 Security Sphynx Preparing for ZeroTrust implementation - Part1

<br/>

<br/>
Full show notes are available here: https://docs.google.com/document/d/14dCpXeQ520IcZC3m007zVPhlIPXKgfv0LkqVnbDx0fc/edit?usp=sharing

EO from President Biden asked for a plan to create Zerotrust implementation in the next 90 days (well, 70ish days now… as of 23 May)  
  
[https://twitter.com/SecuritySphynx/status/1390475868032618496](https://twitter.com/SecuritySphynx/status/1390475868032618496)

@securitySphynx

“CIO: Zero Trust is the way…”

What is the optimal configuration (read: easiest) zero trust config?

Are there different ways to implement Zero Trust?\`

[https://solutions.pyramidci.com/blog/posts/2021/february/the-swiss-cheese-approach/](https://solutions.pyramidci.com/blog/posts/2021/february/the-swiss-cheese-approach/)

[https://tulsaworld.com/opinion/columnists/zero-trust-security-assume-that-everyone-and-everything-on-the-internet-is-out-to-get/article\_f6bdbfad-1aae-5063-8ac0-6a1faf5a244c.html](https://tulsaworld.com/opinion/columnists/zero-trust-security-assume-that-everyone-and-everything-on-the-internet-is-out-to-get/article_f6bdbfad-1aae-5063-8ac0-6a1faf5a244c.html)

[https://www.reddit.com/r/devops/comments/bqo6kp/open\_source\_or\_cheap\_zero\_trust\_beyondcorp/](https://www.reddit.com/r/devops/comments/bqo6kp/open_source_or_cheap_zero_trust_beyondcorp/)

[https://opensource.com/article/17/6/4-easy-ways-work-toward-zero-trust-security-model](https://opensource.com/article/17/6/4-easy-ways-work-toward-zero-trust-security-model)

[https://dodcio.defense.gov/Portals/0/Documents/Library/(U)ZT\_RA\_v1.1(U)\_Mar21.pdf](https://dodcio.defense.gov/Portals/0/Documents/Library/\(U\)ZT_RA_v1.1\(U\)_Mar21.pdf)

1.  What is ZTA
2.  1.  **Who** are your users?
        1.  Devices in use? 
        2.  1.  Device attestation/health checks
        3.  Applications exist?
            1.  Not just into/out of the traditional LAN network - do you understand dependencies of applications and databases and how the traffic flows?
        4.  Connections exist?
    2.  **What** 
    3.  **Where** is the data/traffic? coming from? Going to?
    4.  **When** is this activity occurring and what is expected?
    5.  **WHY:** Need to balance the access to technical resources in a rapidly evolving and dynamic business landscape that ceases to exist within the confines of normal security perimeters.

Mobile workforce - how much work can you get done without ever getting on the VPN? 

1.  Blockers

-   **Technical Debt**

1.  1.  1.  IT Hygiene
        2.  1.  Zero Trust REQUIRES the pre-work of establishing baselines. You cannot detect abnormality in the absence of normality.
            2.  1.  Policy should exist to drive what the specifications of a baseline system, server, application, etc will be.
                2.  Network traffic, endpoint performance, SIEM tuning, endpoint agent/software accountability
            3.  ZTA is less useful if you're not doing basic patching, application updates, and allowing local admin on the system level).
            
            1.  Not designed with this approach in mind, and often costly to modernize.
        3.  Legacy Systems: 
            1.  Where are your assets and how are they used? A “rough estimate” of endpoints is never good enough.
            2.  What are you logging? What AREN’T you logging?
        4.  Asset Management
            1.  Stale accounts, service accounts, HR Workflows for onboarding/offboarding
            2.  Limitations of admin rights
            3.  Local admin/password expiration issues for sales/travelling employees
        5.  User rights auditing
        6.  Human resources/talent
    2.  **Politics:** Getting support/$$/Buy-in for retrofitting applications that are “working just fine” is a huge political/business hurdle.
    
    1.  **SaaS/PaaS/etc offerings**  
        What can you move from traditional off-prem solutions to cloud-based services (more up to date, regularly reviewed for security vulnerabilities, offloading responsibility of maintenance, SSO capabilities)
2.  **Where to go from here:**

-   **AAA requirements**

1.  1.  MFA is a MUST. No, it's not perfect, but it is one more layer in efficacy.
        1.  Identify data owners, make them responsible for RBAC development with technical departments.
        2.  Quantify risk associated with mishandled resources for crown jewels (see previous section on politics).
        3.  Change control around permissions, access
        4.  Security as an active participant in the development/acquisition of new products, software, services, or organizations Like remodeling a house, it is much easier to build security into the process than hire someone to retrofit it later..
    2.  Have discussions around REAL RBAC needs BEFORE implementing a solution. It is easier to expand permissions than it is to take them away.  Resist the idea that the _easy button_ of broad stroke permissions is always the right choice.
    3.  What auditing are you doing? Have you baselined behavior? Where are your logs going, and WHO IS RESPONSIBLE FOR REVIEWING THEM. 
    
    1.  Asset Inventory (again)... Then…
    2.  1.  HIDS/Firewall
        2.  Patch
        3.  Applocker/Application Controls
        4.  Lather, rinse, repeat. 
        
        1.  It’s hard, it’s time-consuming, and it requires a LOT of support for business unit owners.
    3.  DLP Classification
    4.  Capture metrics, then set KPIs and regular check ins to reduce MTTP/MTTR/MTTD
2.  **Manage the Endpoint:** Stop thinking about the perimeter as your weakest point. The endpoint is critical and increasingly vulnerable, mobile, out of traditional “control”.  Real time, actionable data and capabilities are critical to remediation and progress.

Would you like to know more?

[https://www.beyondtrust.com/blog/entry/why-zero-trust-is-an-unrealistic-security-model](https://www.beyondtrust.com/blog/entry/why-zero-trust-is-an-unrealistic-security-model)

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

**#AmazonMusic:** [https://brakesec.com/amazonmusic](https://brakesec.com/amazonmusic) 

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)  
  
**#Pandora**: [https://brakesec.com/pandora](https://brakesec.com/pandora) 

**#RSS**: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

**#Youtube Channel**:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

**#iTunes** Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

**#Google Play** Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

**#iHeartRadio** App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

**#SoundCloud**: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast by using our **#Paypal**: [https://brakesec.com/PaypalBDS](https://brakesec.com/PaypalBDS) OR our **#Patreon**

[https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

**#Twitter**: [@brakesec](https://twitter.com/brakesec) [@boettcherpwned](https://twitter.com/boettcherpwned) [@bryanbrake](https://twitter.com/bryanbrake) [@infosystir](https://twitter.com/infosystir)

**#Player.FM** : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

**#Stitcher** Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

**#TuneIn** Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

#### [Source](http://brakeingsecurity.com/2021-020-security-sphinx-preparing-for-zerotrust-implementation-part1)

<br/>
---
