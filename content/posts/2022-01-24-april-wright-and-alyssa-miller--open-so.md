---
title: "April Wright and Alyssa Miller- Open Source sustainabilty"
date: Mon, 24 Jan 2022 18:08:12 +0000
draft: false
type: posts
---
# April Wright and Alyssa Miller- Open Source sustainabilty

<br/>

<br/>
Alyssa Milller (@AlyssaM\_InfoSec)  
April Wright (@Aprilwright)

0\. Open Source issues (quick discussion, because I value your opinions, and supply chain is important in the IoT world too.)  
Log4j and OSS software management and profitability  
Free as in beer, but you pay for the cup… (license costs $, not the software).   
“If you make money using our software, you must buy a license” - not an end-user license

Open source conference at Whitehouse:  
https://www.zdnet.com/article/log4j-after-white-house-meeting-google-calls-for-list-of-critical-open-source-projects/  
https://www.wsj.com/articles/white-house-convenes-open-source-security-summit-amid-log4j-risks-11642119406  
“For too long, the software community has taken comfort in the assumption that open source software is generally secure due to its transparency and the assumption that many eyes were watching to detect and resolve problems,” said Kent Walker, chief legal officer at Google in a blog post published after the meeting. “But in fact, while some projects do have many eyes on them, others have few or none at all.” 

  
Show was inspired by this Twitter conversation:

  
https://twitter.com/aprilwright/status/1461724712455782400?t=Fv2tmSTXrn-SSjPCka3gxg&s=19

https://twitter.com/AlyssaM\_InfoSec/status/1464661807751213056?t=CFy-hgcHo2a8NwowKYo0hg&s=19

IOT architecture (https://www.avsystem.com/blog/iot-ecosystem/)  
Open source IoT platforms: https://www.record-evolution.de/en/open-source-iot-platforms-making-innovation-count/

Cloud services - processing messages, register/de-register devices, pass messages to other devices/gateways  
Gateways -   
Devices -   
Mobile apps -  
SDKs -   
integrations

Cloud services DO go offline, point of failure:  
https://www.datacenterdynamics.com/en/news/aws-us-east-1-outage-brings-down-services-around-the-world/  
Connectivity and sharing mesh networks assumes you like your neighbors.  
Sidewalk Whitepaper: https://m.media-amazon.com/images/G/01/sidewalk/final\_privacy\_security\_whitepaper.pdf

network vulnerabilities: https://fractionalciso.com/why-you-should-not-be-using-xfinitywifi-hotspots/

  
 Stalking/privacy vs. tracking/surveillance

Fine GPS locations  
Nearby devices triangulate (via BLE, wifi, or 900mhz)  
We want to find our lost devices, but devices can be used for stalking  
https://www.autoevolution.com/news/police-claim-apple-has-unwillingly-created-the-most-convenient-stalking-device-179228.html

Just have an iPhone and you’ll be able to find a stalking device, just install a 100MB app (Ring, Alexa, etc) to detect all devices in the area, or use the right ecosystem to find these items (or know every possible device that could be used to track someone)

What do companies want with that information?

What is a ‘happy medium’ to allow you to find your dog, but not to track people?  
Device controls? Buzzers? (how loud can you make a noise in a small device?) Size issues, battery life, beaconing, self-identification (“Hi, I am a lost device…”)  
Is what Airtags doing enough to reduce the fear?  
Are we designing to edge cases? There are cheaper/easier ways to track someone (phones have a longer standby time than fetch/airtag/tile)  
How often do you lose your keys? Why is your dog not on a leash or properly trained?  
What will it take to make these kinds of devices more secure? 

https://spectrum.ieee.org/why-iot-sensors-need-standards  
Will it take privacy protections to motivate IoT devices to design a better IoT device? Or force standards to be followed, like https://www.ioxtalliance.org/get-ioxt-certified?  
Or NIST standards: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-213-draft.pdf  
https://csrc.nist.gov/publications/detail/sp/800-213a/final - detailed specs

  
Threat modeling, vulnerabilities in IoT networks and platforms

Does your Iot Platform give out SDKs for integrations or allowing 3rd party products or apps? https://www.iot-inspector.com/blog/advisory-multiple-issues-realtek-sdk-iot-supply-chain/

https://www.avsystem.com/blog/iot-ecosystem/  
Old and outdated libraries, like TCP vulnerabilities (RIPPLE20)

https://www.businessinsider.com/iot-security-privacy

https://www.eurofins-cybersecurity.com/news/security-problems-iot-devices/

  
https://arxiv.org/ftp/arxiv/papers/1302/1302.0939.pdf - Security and Privacy Issues in Wireless Mesh  
Networks: A Survey

https://krebsonsecurity.com/2021/09/apple-airtag-bug-enables-good-samaritan-attack/

https://www.amazon.com/gp/help/customer/display.html?nodeId=GZ4VSNFMBDHLRJUK  
Opt-out of Amazon sidewalk

Amazon Sidewalk discussion: https://www.silabs.com/support/training/amazon-sidewalk-development/amz-103-amazon-sidewalk-technology-architecture-and-infrastructure

Fetch:  
As one example, this week we announced Fetch, a compact, lightweight device that will clip to your pet’s collar and help ensure they’re safe. If your dog wanders outside a perimeter you’ve set using the Ring app, Fetch will let you know. In the future, expanding the Amazon Sidewalk network will provide customers with even more capabilities like real-time location information, helping you quickly reunite with your lost pet. For device makers, Fetch also serves as a reference design to demonstrate the potential that devices connected to a broad, reliable network can provide to their customers.  
https://www.aboutamazon.com/news/devices/introducing-amazon-sidewalk

#### [Source](http://brakeingsecurity.com/april-wright-and-alyssa-miller-open-source-sustainabilty)

<br/>
---
