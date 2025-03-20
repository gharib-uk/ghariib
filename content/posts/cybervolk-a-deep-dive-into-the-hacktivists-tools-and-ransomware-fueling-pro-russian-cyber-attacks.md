---
title: "CyberVolk | A Deep Dive into the Hacktivists, Tools and Ransomware Fueling Pro-Russian Cyber Attacks"
date: 2025-01-08
tags: 
  - "attack"
  - "author"
  - "browser"
  - "bug"
  - "opjp"
---

A loose collective of mostly low-skilled actors, CyberVolk absorbs and adapts a wide array of destructive malware for use against political targets.

## Executive Summary

- CyberVolk/GLORIAMIST is a hacktivist collective originating in India with pro-Russia leanings. Between June and October 2024, CyberVolk claimed responsibility for multiple ransomware attacks.
- The main objective of CyberVolk and related groups is to leverage geopolitical issues to launch and justify attacks on public and government entities, primarily in the service of Russian government interests.
- SentinelLabs has observed a shared codebase used by CyberVolk, AzzaSec and DoubleFace’s ransomware. Additionally, CyberVolk has promoted other ransomware families like HexaLocker and Parano. These groups and the tools they leverage are all closely intertwined.
- These hacktivist groups are extremely dynamic and volatile. In-fighting, threats, and inflated political-posturing are common, leading to fragmentation and the rapid re-shaping of the hacktivist threat landscape.

## Introduction

CyberVolk is a politically motivated hacktivist collective which launched its own RaaS in June 2024. The group uses both DDoS and ransomware attacks in its efforts to undermine and disrupt the operations of those opposed to Russian interests.

The group has become an increasingly prominent player within the cybercrime ecosystem, adopting and repurposing existing commodity malware to advance its causes. Highly-skilled actors within the collective expand and revise such tools, effectively making them more sophisticated as they move through various hands.

The CyberVolk collective is a prime example of how readily threat actors can access and deploy dangerous ransomware builders such as AzzaSec, Diamond, LockBit, Chaos and others. This adaptability makes the group highly dynamic and increasingly challenging to track.

In this post, we provide an overview of a number of ransomware families within the context of CyberVolk’s operations, breaking down the increasingly blurred lines between tools and group affiliations. Understanding the shifting nature of dynamic hacktivist collectives like CyberVolk can help organizations prepare and fortify their defenses.

## The Origins of CyberVolk Ransomware

CyberVolk is a pro-India/pro-Russia “hacktivist” group that has been actively targeting entities in multiple countries. In its current form, it emerged during May 2024. Prior to this, the group was known by a few different names, including GLORIAMIST, GLORIAMIST India and Solntsevskaya Bratva. CyberVolk exploits current geopolitical issues, focused on launching and justifying its attacks on public and government entities.

![](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_1-1.jpg)

While the group claims alliances with other broad groups such as LAPSUS$, Anonymous, and the Moroccan Dragons, it has also been associated with NONAME057(16) and other RU-friendly, DDoS-focused, groups. However, CyberVolk has also embraced ransomware as a tool to further its cause, with self-branded ransomware payloads as well as alliances with associated ransomware families, namely Doubleface, HexaLocker, and the Parano family.

CyberVolk’s branded ransomware is derived from the AzzaSec (_aka_ AzzaSecurity) ransomware code. AzzaSec is a pro-Russia, anti-Israel, and anti-Ukraine hacktivist group that emerged in February of 2024. It has claimed alliances with some of the same groups linked to CyberVolk such as NONAME057(16). Initially conducting DDoS and defacement attacks, the group later expanded into extortion and ransomware.

<figure>

![June 2024 Release of AzzaSec Ransomware](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_30.jpg)

<figcaption>

June 2024 Release of AzzaSec Ransomware

</figcaption>

</figure>

In June, the source-code for “AzzaSec Ransom” was leaked and subsequently adopted and adapted by multiple groups aligned with AzzaSec’s mission. Prior to its disbandment in August of 2024, AzzaSec operated under a ransomware-as-a-service (RaaS) model and pushed out multiple iterations of its Windows-centric ransomware.

<figure>

![AzzaSec “disbandment” announcement, August 2024](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_42.jpg)

<figcaption>

AzzaSec “disbandment” announcement, August 2024

</figcaption>

</figure>

## Post-AzzaSec | The Rise of CyberVolk’s RaaS Operations

The CyberVolk-branded RaaS was announced in late June 2024.

<figure>

![CyberVolk Ransomware Announcement, June 2024](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_20.jpg)

<figcaption>

CyberVolk Ransomware Announcement, June 2024

</figcaption>

</figure>

The CyberVolk-modified ransomware’s development is credited to `@ghostdoor_maldev` and is based on the earlier AzzaSec Ransomware code. The Windows-specific payloads are written in C++. Upon execution, the payloads drop bitmap images (files with extension `.bmp`) into `%temp%` folder, and display them, prior to any encryption occurring. The ransomware will terminate any running processes belonging to either Microsoft Management Console (MMC) or Task Manager.

<figure>

![CyberVolk Ransomware MMC processes and Task Manager Termination](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_8.jpg)

<figcaption>

CyberVolk Ransomware MMC processes and Task Manager Termination

</figcaption>

</figure>

Early versions of CyberVolk Ransomware used the AES algorithm for file encryption and the SHA512 algorithm for key generation. This was later updated to “ChaCha20-Poly1305 + AES + RSA + Quantum resistant algorithms” according to posts on the group’s Telegram.

<figure>

![CyberVolk wallpaper with countdown timer](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_26.jpg)

<figcaption>

CyberVolk wallpaper with countdown timer

</figcaption>

</figure>

The payment screen used by CyberVolk Ransomware displays the decryption timer along with payment details. CyberVolk ransomware supports BTC and USDT payments. Across all the functional samples we analyzed, the timeout value was set to 5 hours, with the ransom amount set to $1000.00 (equivalent in BTC or USDT). A ransom note named `CyberVolk_Readme.txt` containing the same contact details presented in the wallpaper and payment screens is also dropped to disk. Telegram contact details are provided as well.

<figure>

![CyberVolk ransom note](https://www.sentinelone.com/wp-content/uploads/2024/11/cybervolk28.jpg)

<figcaption>

CyberVolk ransom note

</figcaption>

</figure>

The `.CyberVolk` extension is added to affected files:

<figure>

![CyberVolk-encrypted files](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_23.jpg)

<figcaption>

CyberVolk-encrypted files

</figcaption>

</figure>

The decryption timer is controlled via a `time.dat` file written to `%appdata%Roaming` directory. The timeout is set to 5 hours in CyberVolk samples analyzed by SentinelOne. This functionality is mirrored in the Invisible Ransom payloads as well, as they are based on the same codebase (AzzaSec Ransom).

<figure>

![Decryption timer in CyberVolk](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_24.jpg)

<figcaption>

Decryption timer in CyberVolk

</figcaption>

</figure>

The `time.dat` file stores the defined timeout value in seconds. Default builds of Invisible Ransom (or CyberVolk) will write `time.dat` with a value of **17967** seconds (roughly 5 hours).

CyberVolk has continued to utilize their branded ransomware payloads, more recently in attacks against entities in Japan, and hype up these attacks across their various channels (e.g., Telegram, X, Discord). These campaigns were part of “#OpJP” which extended through October. Victims of “#OpJP” include:

- The Japan Foundation
- Japan Oceanographic Data Center (JODC)
- The Japan Meteorological Agency (JMA)
- Tokyo Global Information System Centre

<figure>

![CyberVolk and #OpJP (Targeting Japan)](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_43.jpg)

<figcaption>

CyberVolk and #OpJP (Targeting Japan)

</figcaption>

</figure>

## CyberVolk Associates | Invisible/Doubleface Team Ransomware

Invisible or Doubleface ransomware is associated with “Doubleface Team” (_aka_ Double Alliance). This group emerged in August-September 2024 as a convergence of associates within CyberVolk, Doubleface, and Moroccan Black Cyber Army. CyberVolk began promoting Doubleface on September 22, 2024.

<figure>

![September 22 DoubleAlliance / CyberVolk posting](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_11.jpg)

<figcaption>

September 22 DoubleAlliance / CyberVolk posting

</figcaption>

</figure>

Invisible/Doubleface ransomware payloads function in an identical manner to CyberVolk-branded ransomware samples. This includes duplication of the 5-hour timeout setting and active wallpaper modification used for input of the decryption key and displaying payment details. Both ransomware families are derived from the same AzzaSec Ransomware code base.

<figure>

![Invisible/Doubleface ransomware wallpaper](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_4.jpg)

<figcaption>

Invisible/Doubleface ransomware wallpaper

</figcaption>

</figure>

### Invisible/Doubleface Source

The source code/builder for Invisible has been leaked publicly and distributed through various channels, mainly Telegram. As stated previously, both Invisible and CyberVolk’s payloads as of this writing are based on the same code, observed in AzzaSec Ransomware.

<figure>

![Invisible Ransom source files](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_17.jpg)

<figcaption>

Invisible Ransom source files

</figcaption>

</figure>

The archive contains the full Visual Studio project and source code for building new Invisible Ransom payloads, along with some already compiled samples/stubs.

The file `ransom.cpp` contains the main logic for the ransomware payload. It contains all the global variables including ransomware note template data, orchestrates all the encryption tasks, and handles any process termination or manipulation. The main `ransom.cpp` handles calls out to `Cryptographic.cpp` to process encryption tasks, including the implementation of AES/RSA (via `aes.cpp` and `rsa.cpp` respectively).

Similar to earlier AzzaSec ransomware examples, these payloads encrypt files via AES, then manage encryption keys and wrapping via RSA. The current implementation indicates AES-256 for file encryption, and RSA-2048 for wrapping the keys. Each file is encrypted via AES-256 (EncFile function in `Cryptographic.cpp`) with the AES key then encrypted via RSA-2048.

<figure>

![Encrypting files in Invisible Ransom](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_38.jpg)

<figcaption>

Encrypting files in Invisible Ransom

</figcaption>

</figure>

We can also see the implementation of the ransom countdown mechanism in `ransom.cpp`. As with CyberVolk Ransom, the default timeout period is 5 hours. This is initially set via the `timeLeft` variable in `ransom.cpp`

The timer writes a `time.dat` file to `%appdata%Roaming` as we saw with CyberVolk. The 5 hour time limit for these samples is read from this path. In theory, this file could be manipulated to alter the timeout.

<figure>

![Timer code in Invisible Ransom, referencing time.dat](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_21.jpg)

<figcaption>

Timer code in Invisible Ransom, referencing time.dat

</figcaption>

</figure>

The `ransom.cpp` source file also contains the template for the ransom notes, which are written to the same path from which the ransomware is executed as `InvisibleReadMe.txt`.

<figure>

![Invisible Ransom ransom note template in “ransom.cpp”](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_7cut.jpg)

<figcaption>

Invisible Ransom’s ransom note template in “ransom.cpp”

</figcaption>

</figure>

The termination of MMC processes and Task Manager is also present in the Invisible Ransom source (as we also see with CyberVolk samples)

## “Lock. Demand. Dominate” | HexaLocker Ransomware

HexaLocker is a ransomware family that first appeared in July 2024. HexaLocker is closely associated with LAPSUS$, reportedly having been originally developed by “ZZART3XX”, a proclaimed former associate of the group. HexaLocker payloads are written in Golang. As of this writing, we have only observed payloads targeting Windows systems.

On July 22, CyberVolk first referenced HexaLocker via one of Holy League’s posts. The Holy League is a loosely affiliated group of 70+ members born out of protest against Spain and associated arrests of members of NONAME057(16). HexaLocker’s posts and branding are all laden with their “Lock. Demand. Dominate” slogan.

In September 2024, HexaLocker launched a new Telegram channel after previous ones had been banned or removed. The updated channel contains details on their ongoing existence along with screenshots of the current HexaLocker panel.

<figure>

![HexaLocker Telegram message](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_44.jpg)

<figcaption>

HexaLocker Telegram message

</figcaption>

</figure>

Throughout October 2024, CyberVolk’s communication channels continued to echo messaging from HexaLocker. This includes HexaLocker’s solicitations for help in continuing to progress with HexaLocker. On October 2, 2024, HexaLocker posted an update asking for help but also teasing new features in the pipeline. These include:

- Stronger anti-debug/anti-analysis features
- Advanced obfuscations (crypting/packing)
- Inclusion of EDR/XDR/AV-Killer
- Permanent AMSI bypass
- Improved process-injection
- Remote Thread Hijacking
- UAC Bypass improvements
- Self-deletion

Interesting developments continued with HexaLocker throughout October 2024. On October 4th, the group posted an update with a video demo claiming that an “EDR Killer” and AMSI/WD Bypass features had been added. This was followed on October 6th with an update stating that UAC bypasses had been added to the product.

On October 20, 2024, the actor behind the HexaLocker Telegram channel (ZZART3XX) posted their intention to officially leave and denounce LAPSUS$ as well as any peripheral affiliations. This includes the maintenance of the HexaLocker project and all the relationships and alliances that revolve around it.

In this announcement, the author also indicated that they would be offering HexaLocker source code, including its web panel, along with “LAPSUS$ Ransomware” source for sale.

<figure>

![HexaLocker “shut down” posting via Telegram](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_31.jpg)

<figcaption>

HexaLocker “shut down” posting via Telegram

</figcaption>

</figure>

This was followed the next day on October 21 with offers to sell HexaLocker and the panel infrastructure, along with associated “LAPSUS$ Ransomware”. These messages were echoed across HexaLocker, Holy League, and LAPSUS$-aligned channels.

The overt promotion of HexaLocker within CyberVolk channels has ceased since the HexaLocker shutdown announcement.

## Parano Ransomware Within the CyberVolk Collective

In late October 2024, various identities in the CyberVolk community began promoting the release of Parano Ransomware v1. This appears to be another affiliation for the CyberVolk collective.

<figure>

![October 2024 Parano Ransomware Announcement by CyberVolk](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_15.jpg)

<figcaption>

October 2024 Parano Ransomware Announcement by CyberVolk

</figcaption>

</figure>

According to the announcement, Parano Ransomware comes with a $400.00 USD price tag per single payload/stub. The ransomware features strong anti-analysis and anti-debugging features, and uses a combination of AES-128 and RSA-4096 for key management.

The Parano malware family extends beyond ransomware, also including “Parano Checker” and “Parano Stealer”, a commodity, Python-based, infostealer. The malware attempts to gather and exfiltrate browser data, Discord details, crypto wallet information and keys, along with other related software and system diagnostic details. The data can be exfiltrated via Discord webhook to a channel or location of the attacker’s choosing.

<figure>

![Parano Stealer Announcement, October 2024](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_35.jpg)

<figcaption>

Parano Stealer Announcement, October 2024

</figcaption>

</figure>

Parano Checker is a reconnaissance and info-gathering tool capable of navigating targets and helping to isolate interesting ingress points or data repositories, or assist with dictionary/brute force attacks on certain interfaces (e.g., CPanel and WordPress login panels).

At the time of writing, we have not observed the executable variations of Parano (_aka_ Paraodeus Ransomware) in the wild. The author has shared a non-malicious Python-based demo of a “ScreenLocker”. As it stands, the nature of the relationship between CyberVolk and the Parano ecosystem is still evolving.

## Beyond Ransomware | CyberVolk Stealers and Webshells

In addition to ransomware and other disruptive attacks, CyberVolk develops and distributes infostealer malware and webshells. The group announced the new webshell in late October 2024.

<figure>

![CyberVolk Webshell announcement (Telegram)](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_33.jpg)

<figcaption>

CyberVolk Webshell announcement (Telegram)

</figcaption>

</figure>

The CyberVolk webshell is distributed as a standard PHP file with the actual malicious code base64-encoded inside its body.

<figure>

![CyberVolk Webshell decoded](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_27.jpg)

<figcaption>

CyberVolk Webshell decoded

</figcaption>

</figure>

The publicly released version of the CyberVolk webshell provides multiple functions. The current version of the webshell allows for files to be uploaded, renamed, and downloaded. Further, attackers can traverse directories on the target server and gather environmental details.

“CyberVolk Stealer” is distributed in the form of a Python script and is based on LBX-Grabber, an existing Python information stealer, which is in turn built into BLX-Stealer. Specifically, CyberVolk is derived from the LBX-Grabber component.

<figure>

![CyberVolk Stealer Python Script](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_41.jpg)

<figcaption>

CyberVolk Stealer Python Script

</figcaption>

</figure>

CyberVolk Stealer attempts to gather various types of data from the system, then exfiltrates the data via Discord. Version 1 of the stealer was announced in September 2024. Core features include robust gathering of browser, discord, game and crypto wallet data. The stealer targets multiple browsers and numerous wallets.

<figure>

![CyberVolk Stealer identifying sensitive data](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_6.jpg)

<figcaption>

CyberVolk Stealer identifying sensitive data

</figcaption>

</figure>

## Telegram Exit and Weaponized Bans

In early November 2024, CyberVolk, and numerous affiliated groups, vanished from Telegram as a result of a mass ban of hacktivist groups. The last significant posts to the CyberVolk Telegram channels were on November 3, 2024.

The group subsequently announced, via Twitter/X, that it would only be using the X platform going forward. The Twitter/X account for DoubleFace, `@DoubleFace_Team`, made a similar announcement around the same time.

<figure>

![Telegram ban updates on X](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_3.jpg)

<figcaption>

Telegram ban updates on X

</figcaption>

</figure>

Telegram channels belonging to CyberVolk and numerous allies appear to have been banned maliciously. We have observed specific actors claiming responsibility for attempting to close and/or extort channels belonging to AzzaSec and DoubleFace amongst others.

In September, some users began threatening to have rival hacktivist group channels including CyberVolk, APTZone, Doubleface, AzzaSec and others banned.

<figure>

![September 14, 2024 - “Matteo” targeting CyberVolk/APTZone with bans](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_12.jpg)

<figcaption>

September 14, 2024 – “Matteo” targeting CyberVolk/APTZone with bans

</figcaption>

</figure>

On November 16, 2024, RipperSec posted their ‘perspective’ on the banning of multiple hacktivist channels, indicating that some prior members of AzzaSec and Doubleface were attempting to exhort other channels under the threat of a Telegram ban.

<figure>

![November 13, 2024 RipperSec Telegram Post](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_19.jpg)

<figcaption>

November 13, 2024 RipperSec Telegram Post

</figcaption>

</figure>

This was highlighted in another post on November 13, 2024 in the “Hunt3r Kill3rs” Telegram channel. In this post, an actor referring to AzzaSec claimed “they didn’t close nobody’s channels, all the channels got closed by me”. The post went on to advise others receiving extortion threats to ignore them. “I’m the only one that is closing the hacktivist community’s channels and groups”, the actor claimed.

<figure>

![November 13, 2024 posting in Hunt3r Kill3rs Telegram Channel](https://www.sentinelone.com/wp-content/uploads/2024/11/CyberVolk_2.jpg)

<figcaption>

November 13, 2024 posting in Hunt3r Kill3rs Telegram Channel

</figcaption>

</figure>

This weaponization of the Telegram Terms-of-Service is increasing in parallel with threat actor groups moving off Telegram to seek more clandestine and secure communication channels in response to increased scrutiny and declining trust in the platform after the arrest of Telegram founder Pavel Durov.

## Conclusion

The number of ransomware families associated with the CyberVolk hacktivist group highlights the ability of this group to rapidly pivot, building upon existing tools to suit their needs and further their causes. Though primarily composed of lower-skilled threat actors that reach for and use whatever works for them, we continue to see how quickly the group is able to move and adapt.

The reuse of tools like AzzaSec Ransom, Diamond RW, and even more established ones like LockBit and Chaos, demonstrates just how dynamic these affiliations and alliances between hacktivist groups can be. Not only are such groups touting new tools within short time frames only to abandon them and pivot to something else later, the number of hacktivist groups is also growing. Infighting and tensions amongst them are also fuel for the rates of growth and change – as alliances crumble or shift, the threat environment stays highly volatile and dynamic, making it more difficult for cyber defenders to track their activities consistently.

As groups like CyberVolk leverage openly-available commodity tools with high potential for causing damage, they continue to add more layers of complexity, expanding and revising the tools as they are passed around within the collective. Ransomware operations will only get muddier and increase how much cybersecurity teams will need to monitor in order to stay up to date on the happenings within the cybercrime ecosystem.

## Indicators of Compromise

**CyberVolk**  
0ce59e479ec6eacd3a44ed3de2dc572676e5b2dd  
16bf55122bbb6073cc1d77ce23e2a8e6052f9ec1 | Stealer  
3bf6a90017bf22083ab735ecf3f8589a3f220e53 | Ransom note  
5d8bed459f55a37e2fcb801d04de337a01c5d623 | Ransom note  
813c510fb2463ecc6dff7795ef96744ca82544b3 | Webshell  
b958fb7241cc9675b8dd967b02df6a6ad92de52d  
c70d2350cbac3d0abeb896adcce2fcf243943633 | Webshell  
eae366ee4a7c19a87bc5ab9360f4333907a6a387  
f5d0c94b2be91342dc01ecf2f89e7e6f21a74b90

**AzzaSec**  
5499da31260a4aa75eea46c1d4aa6559074749a8 | Ransomware (src)

**Invisible / Doubleface Team**  
197d5c9c5cbf53ed3e78d53a008b6ad665fa3e4c  
1f325950a7a8e1a2050e954f33d2c3774510bd6e  
59e293623e4fb828a29fb982d5ac9a4f993abc3b  
d35da3f4e36eebf36a130bc7e0182fc4c35cf551 | Archive

**HexaLocker**  
11288fb54c6f2ed4d8cddfb004c754e5e9c35ad5  
4c24fdf504af452fb7245db33bfc1dc4f72c04a8  
4f8e1e7bfd484cf312fdc77e8687086f6f6007c  
7ba4eb7842730bbc82fc129a3f3d4a239ac436c2  
84dacf9da57d9d69c2ca711831895bf185834b8c  
904f46ef4c66ccf844bf31d37c11298fb7f65157  
93334882ff3c03c42b1179d9db0c165c99145369  
a4b7ef2ca1d5fda318505cac6757b5313b47eeac  
b6a9c5692b76f2defc1c170bdce0e41d91d706db  
be581fc1f430dd6855effd9e54429c5c5fcb9f8c | Ransom note  
cd8f934fa7ba7817bb62f0e4b968b3f124355b60

**PDBs**

```
C:/Users/zzart/Desktop/HexaLocker RaaS/crypter.go
C:/Users/zzart/Desktop/MalwareDeveloppement/HexaLocker RaaS/decrypt_key.go
C:/Users/zzart/Desktop/MalwareDeveloppement/HexaLocker RaaS/crypterV1.go

```

**DNS**  
darkslategray-baboon-853641\[.\]hostingersite\[.\]com

**LAPSUS$/HexaLocker**  
481830db2daf40607748bd9624e970781e7f4408  
e1d8993ef4bbc8d2aa331262e5422d91865acc4f

**Parano**  
3ce26f45f5da58ab75b4d1cecc78c3bbe275f708 | Stealer (archive)  
4581b30e6f5946a570963cd76dc79beaa8bcf1c3 | ScreenLocker  
91abb7fadf847f3810bbe0734e3c31d5dc7bce6d | ScreenLocker  
b5d8e690a75f07e7d3e18fcc5b86bfe2362a3300 | ScreenLocker

Go to Source
