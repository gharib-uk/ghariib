---
title: "2020-028-Shlomi Oberman RIPPLE20 supply chain security discussion software bill of materials"
date: Fri, 24 Jul 2020 03:54:59 +0000
draft: false
type: posts
categories: 
- 
---
# 2020-028-Shlomi Oberman RIPPLE20 supply chain security discussion software bill of materials

<br/>

<br/>
Whitepaper: [https://www.jsof-tech.com/ripple20/](https://www.jsof-tech.com/ripple20/)

\[blog\] Build your own custom TCP/IP stack: [https://www.saminiir.com/lets-code-tcp-ip-stack-1-ethernet-arp/](https://www.saminiir.com/lets-code-tcp-ip-stack-1-ethernet-arp/)

Another custom TCP/IP stack: [https://github.com/tass-belgium/picotcp](https://github.com/tass-belgium/picotcp)

RIPPLE 20 Whitepaper: [https://drive.google.com/file/d/1d3NNVCRPVFk0-V0HUO5CxWWVn9pYIvmF/view?usp=sharing](https://drive.google.com/file/d/1d3NNVCRPVFk0-V0HUO5CxWWVn9pYIvmF/view?usp=sharing) 

Agenda:

Part 1:

Background on the report

Why is it called RIPPLE20? What’s the RIPPLE about? 

Communications with Treck (and it’s Japanese counterpart)

Were you surprised about the reaction? Positive or negative?

Types of systems affected?

IoT

Embedded systems

SCADA

What precipitated the research?

What difficulties did you face in finding these vulns? Deadlines? 

What tools were used for analysis? (I think you mentioned Forescout --brbr)

What kind of extensibility are we talking about? TCP sizes? 

What did JSOF gain by doing this? 

What were the initial benefits of using the TCP/IP stack?

Speed? Size?  
Do these vulns affect other TCP/IP stacks? 

Did Treck give you access to source? Any specific requirements set by Treck? Any items that were off-limits? 

Updates since the report was released?

Are your vulns such that they can be detected online?

Part 2:

Supply chain issues

What should companies do when they don’t know what’s in their own tech stack?

[https://csrc.nist.gov/CSRC/media/Projects/Supply-Chain-Risk-Management/documents/briefings/Workshop-Brief-on-Cyber-Supply-Chain-Best-Practices.pdf](https://csrc.nist.gov/CSRC/media/Projects/Supply-Chain-Risk-Management/documents/briefings/Workshop-Brief-on-Cyber-Supply-Chain-Best-Practices.pdf)

Software bill of materials: [https://en.wikipedia.org/wiki/Software\_bill\_of\_materials](https://en.wikipedia.org/wiki/Software_bill_of_materials)

PicoTCP link above does not release all code, because they use binary blobs that make proper code review next to impossible

“Unfortunately we can't release all the code, a.o. because some parts depend on code or binaries that aren't GPL compatible, some parts were developed under a commercial contract, and some consist of very rough proof-of-concept code. If you want to know more about the availability under the commercial license, or the possibility of using our expert services for porting or driver development, feel free to contact us at picotcp@altran.com.”

BLoBs = [https://en.wikipedia.org/wiki/Proprietary\_device\_driver](https://en.wikipedia.org/wiki/Proprietary_device_driver)

Vendor Contact

How many organizations are affected by these vulnerabilities? 

Are some devices and systems more vulnerable than others?

 How many are you still investigating to see if they are affected?

What’s the initial email look like when you tell a company “you’re vulnerable to X”?

Who are you dealing with initially? What is your delivery when you’re routed to non-technical people?

How did you tailor your initial response when you learned of the position of the person?

Lessons Learned:  
What would you have done differently next time?

Any additional tooling that you’d have used?

BlackHat talk: 05 August

What should companies do to reduce or mitigate the chances of the types of vulnerabilities found by your org?

[https://cambridgewirelessblog.wordpress.com/2016/05/18/supply-chain-security-and-compliance-for-embedded-devices-iot/](https://cambridgewirelessblog.wordpress.com/2016/05/18/supply-chain-security-and-compliance-for-embedded-devices-iot/)

[https://blog.shi.com/solutions/embedded-hardware-supply-chain-attacks-embedded-system-attacks-how-to-stay-safe/](https://blog.shi.com/solutions/embedded-hardware-supply-chain-attacks-embedded-system-attacks-how-to-stay-safe/)

[http://www.intrinsic-id.com/wp-content/uploads/2018/02/2016-A-Platform-Solution-for-Secure-Supply-Chain-and-Chip-Cycle-Management-Computer-Volume-49-Issue-8-Aug.-2016-Joseph-P.-Skudlarek-Tom-Katsioulas-Michael-Chen-%E2%80%93-Mentor-Graphics..pdf](http://www.intrinsic-id.com/wp-content/uploads/2018/02/2016-A-Platform-Solution-for-Secure-Supply-Chain-and-Chip-Cycle-Management-Computer-Volume-49-Issue-8-Aug.-2016-Joseph-P.-Skudlarek-Tom-Katsioulas-Michael-Chen-%E2%80%93-Mentor-Graphics..pdf)

[https://www.supplychainservices.com/blog/major-security-risks-windows-embedded-users](https://www.supplychainservices.com/blog/major-security-risks-windows-embedded-users)

[https://www.bbc.com/news/business-32716802#:~:text=Japanese%20car%20giants%20Toyota%20and,March%202003%20and%20November%202007.](https://www.bbc.com/news/business-32716802#:~:text=Japanese%20car%20giants%20Toyota%20and,March%202003%20and%20November%202007.)

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

#### [Source](http://brakeingsecurity.com/2020-028-shlomi-oberman-ripple20-supply-chain-security-discussion-software-bill-of-materials)

<br/>
---
