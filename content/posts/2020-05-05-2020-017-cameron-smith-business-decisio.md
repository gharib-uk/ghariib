---
title: "2020-017-Cameron Smith business decisions and how it affects Security"
date: Tue, 05 May 2020 23:35:53 +0000
draft: false
type: posts
categories: 
- 
---
# 2020-017-Cameron Smith business decisions and how it affects Security

<br/>

<br/>
[**Cameron Smith** @Secnomancer](https://twitter.com/Secnomancer)

Layer8conference is virtual (https://layer8conference.com/layer-8-is-online-this-year/)

[https://csrc.nist.gov/publications/detail/sp/800-171/rev-1/final](https://csrc.nist.gov/publications/detail/sp/800-171/rev-1/final)

CMMC:[https://info.summit7systems.com/blog/cmmc](https://info.summit7systems.com/blog/cmmc)

[https://www.comptia.org/certifications/project](https://www.comptia.org/certifications/project) - Project+

Cameron’s Smith = [www.twitter.com/secnomancer](http://www.twitter.com/secnomancer)

Cybersmith.com - Up by 14 April

[Ask@thecybersmith.com](mailto:Ask@thecybersmith.com)

[Cameron@thecybersmith.com](mailto:Cameron@thecybersmith.com)

[https://en.wikipedia.org/wiki/Christopher\_Voss](https://en.wikipedia.org/wiki/Christopher_Voss) [https://www.amazon.com/Never-Split-Difference-Negotiating-Depended/dp/0062407805](https://www.amazon.com/Never-Split-Difference-Negotiating-Depended/dp/0062407805)

[https://www.masterclass.com/classes/chris-voss-teaches-the-art-of-negotiation](https://www.masterclass.com/classes/chris-voss-teaches-the-art-of-negotiation)

[https://www.masterclass.com/](https://www.masterclass.com/)

[https://www.autopsy.com/support/training/covid-19-free-autopsy-training/](https://www.autopsy.com/support/training/covid-19-free-autopsy-training/)

[https://www.youtube.com/playlist?list=PLg\_QXA4bGHpvsW-qeoi3\_yhiZg8zBzNwQ](https://www.youtube.com/playlist?list=PLg_QXA4bGHpvsW-qeoi3_yhiZg8zBzNwQ)

“There is nothing noble in being superior to your fellow man; true nobility is being superior to your former self.”― **Ernest Hemingway**  [**https://www.goodreads.com/quotes/76281-there-is-nothing-noble-in-being-superior-to-your-fellow**](https://www.goodreads.com/quotes/76281-there-is-nothing-noble-in-being-superior-to-your-fellow)
===============================================================================================================================================================================================================================================================================================================================================

**_Original B-Sides Talk Blurb_**

SITREP: A Consultant's Perspective from the Trenches of InfoSec In this session you will hear war stories and lessons learned consulting for hundreds of clients across dozens of verticals at every level, from bootstrapped startups with garage beginnings to Fortune 50 companies and everything in between. We will cover life on the front lines in InfoSec, ranging from individual contributions and staying relevant in a rapidly evolving field all the way to how bad most orgs are at InfoSec and what we can do as practitioners to help make them better.

### **Speaking Goal**

**_After my presentation is over, I want my audience to..._**

-   Feel better about where they are as an infosec practitioner
-   Understand that most of Cybersecurity is largely NOT about the latest hack or technique
-   Failing is OK as long as you learn from it

**_...so that ..._**

-   When they go back to their office / SOC / client engagements on Monday they focus on the things that matter to their organizations
-   Hopefully feel a little bit less that the work they are doing is boring, exhausting, unappreciated, or hopeless

### **Intro**

-   Security is a really crazy industry  
      
    
-   -   Like the wild west out here
    -   Constant threats
    -   Complacent or ignorant clients/dependents
    -   Resource and budget constraints
-   Security is really complex  
      
    
-   -   There are SO. MANY. MOVING. PIECES.
    -   There is a never ending stream of new information to learn and new threats to face
    -   Security always involves at LEAST 4 parts
    -   -   The practitioner - Hopefully you have backup!
        -   What you're protecting - Employer, Client, System, Application, Data, SOMETHING, etc
        -   What you're protecting it from - External TAs, Internal TAs, Incompetence, Apathy, Plain Ol' Vanilla Constraints, etc
        -   What you have to protect it with - Budgets, Time, Personnel, Training, Relationships, etc
-   Cybersecurity/Information Security is simultaneously an old and new/emergent discipline  
      
    
-   -   Cyber History
    -   -   Old
        -   -   Nevil Maskelyne / Guglielmo Marconi wireless telegraphy attack and Morse code insults - 1903
            -   Phreaking in the 1960s
            -   ARPANET Creeper - 1971
            -   Morris Worm - 1988
        -   New
        -   -   Gartner Coined term SOAR in 2017
            -   -   Yeah... It's barely 3 years old.
                -   Now you can literally find job openings with SOAR Engineering titles
            -   DevSecOps - Amazon presentation in 2015? Not even in grade school yet.
            -   Average enterprise is running 75 security tools in their environment (Cybersecurity almanac 2019)
    -   Most cybersecurity professionals over 30 do not have degrees in cybersecurity
    -   -   Many don't even have Computer Science or IT related degrees
        -   This is it's own problem
        -   -   Training cyber pros, Chris Sanders, cognitive crisis, etc.
            -   -   BDS ep 2019-021 and 2019-022
    -   Emergent disciplines are challenging by default
    -   You chose to play the game on hard mode for your first play through

**_Security really isn't as complicated as most people think_**

-   Occult Phenomenon
-   -   Things we don't understand we imagine to be far more complex
    -   Things we anticipate we imagine to be far worse than they are
-   Grass isn't greener
-   -   Most security departments aren't doing better than you are
    -   Maturity models aren't magic

**_Establish Credibility_**

-   I have been in A LOT of client environments in the last 12 years
-   Last time I checked, I have more than 350 discrete client engagements under my belt
-   -   I have worked with hundreds of internal, external, and hybrid IT and Security solutions
    -   I've met the same tired and beleaguered IT/Security personnel over and over again
    -   -   SSDD, very little actually changes from place to place
-   In that time, I've learned quite a bit about what makes security work
-   I've learned even more about what NOT to do
-   I want to share some of that with you today so you can see how organizations of all shapes and sizes can fail

### **Very Large Company Examples**

-   **Big Four Bank Example**
-   -   Situation
    -   -   Four Local Branches in Midwest
        -   Physical Security Assessment
        -   -   How got onto site as cash machine servicer was incredibly easy
    -   Problem
    -   -   Absolute trust of vendors/vendor compromise
    -   How do we as security practitioners fix it?
    -   -   Good internal relationships with functional area leaders
        -   Work closely with functional areas to left and to the right
        -   -   Who? Operations? HR? Purchasing?
            -   Every functional area and specifically the leadership
            -   Improved communications and availability
            -   8 and Up
            -   -   'Gotta git gud' at the soft stuff
-   **Top 50 Chain Restaurant Example**
-   -   Situation
    -   -   Doing Chip Reader refreshes across all ~600 locations for PCI Compliance during 2017 window
    -   Problem
    -   -   Poor project management on behalf of security team led to project failure
        -   A security problem became an IT problem
        -   Contractor to subcontractor to subcontractor added time and complexity
    -   How do we as security practitioners fix it?
    -   -   Security managers needs to be aware of how their projects impact others
        -   Managing up
        -   Security needs to be interdisciplinary

### **Government Examples**

-   **Police Department Example**
-   -   Situation
    -   -   City Administrator got Spear Phished
    -   Problem
    -   -   Spear phishing
        -   Poor logging
    -   How do we as security practitioners fix it?
    -   -   Look for the most basic problems and try to fix them
        -   Find or create solutions that provide basic capabilities
        -   Cannot prevent the lowest hanging fruit directly, so impact what you can change
        -   -   What you can actually do about phishing
            -   Getting people to do something that you want them to do
-   **Defense SubContractor Example**
-   -   Situation
    -   -   Working with MSP on security issues
        -   “Do we have a SIEM” email?
    -   Problem
    -   -   Company executives have never done due diligence
        -   Assumed that MSP had it under control
        -   MSP just did what they normally do and within letter of their contract
    -   How do we as security practitioners fix it?
    -   -   Security needs to be proactive

### **Small Company Examples**

-   **Light Manufacturer Example**
-   -   Situation
    -   -   Server not working, Ransomware
        -   Attackers pivoted through third party accountant access
    -   Problem
    -   -   Single Point of Failure (SPOF)
        -   Vendor Compromise
    -   How do we as security practitioners solve it?
    -   -   IT problems become security problems on long enough timeline
        -   Need to provide actual solutions to business problems
        -   Security CANNOT be decoupled from business needs
-   **Telecommunications Provider**
-   -   Situation
    -   -   Employee reports CEO was hacked
    -   Problem
    -   -   Employee panicked, emailed everyone
        -   Escalated way beyond what was necessary
    -   How do we as security practitioners solve it?
    -   -   Employee education - Boring answer
        -   What's actually under our control here?
        -   -   Clear processes for security incidents
            -   Clear communications channels for employees with IT and security groups
            -   Knowledge management
-   **Local NGO Example**
-   -   Situation
    -   -   Meeting with Executive Director regarding server failure
    -   Problem
    -   -   Mentions that she was sent security guidelines from global parent org
        -   Got so overwhelmed reading it she just closed it and kept working on something else
    -   How do we as security practitioners solve it?
    -   -   We have to make this information digestible and accessible
        -   We do NOT need to make already dense subject matter even more inaccessible
        -   When cannot mandate compliance, how do you achieve compliance
        -   -   More flies with honey than vinegar
            -   Build relationships - Layer 8 strikes again

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email bds.podcast@gmail.com**

#**Brakesec** Store!:[https://www.teepublic.com/user/bdspodcast](https://www.teepublic.com/user/bdspodcast)

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)  
  
**#Pandora**: [https://pandora.app.link/p9AvwdTpT3](https://pandora.app.link/p9AvwdTpT3 "https://pandora.app.link/p9AvwdTpT3")

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

#### [Source](http://brakeingsecurity.com/2020-017-cameron-smith-business-decisions-and-how-it-affects-security)

<br/>
---
