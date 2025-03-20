---
title: "The Good, the Bad and the Ugly in Cybersecurity – Week 2"
date: 2025-01-12
tags: 
  - "technology"
  - "vpn"
---

US govt launches cyber trust program, spammers use neglected domains to evade detection, and Japan accuses MirrorFace of cyber espionage.

## The Good | Feds Launch Cyber Trust Program & OFAC Sanctions Flax Typhoon-Linked Company

The U.S. government took both defensive and offensive actions this week, starting with the launch of the Cyber Trust Mark and followed by sanctions on the company behind Flax Typhoon threat actors.

The White House first introduced the Cyber Trust Mark as a cybersecurity label for internet-connected devices. The label allows consumers to identify safe ‘smart’ products such as TVs, appliances, baby monitors, and fitness trackers. Devices that meet NIST’s criteria, including strong default passwords, regular software updates, and data protection measures, can display the mark.

![](https://www.sentinelone.com/wp-content/uploads/2025/01/cyber_trust_mark.jpg)

Consumers can also scan a QR code on the label to access detailed security tips, policies, and information about support duration. The program aims to address rising concerns that threat actors are exploiting insecure smart devices while encouraging companies to enhance their commitment to cybersecurity. Currently, the program involves makers like Amazon, Google, LG, Logitech, and Samsung with FCC-approved administrators overseeing the certification process.

The U.S. Treasury has sanctioned Integrity Tech, a company based in Beijing for its role in attacks led by Chinese state-sponsored threat actor Flax Typhoon. Using Integrity Tech’s infrastructure, Flax Typhoon targeted networks in the U.S. and Europe between 2022 and 2023, employing VPNs and remote desktop protocols to infiltrate systems.

This sanction is the follow-up to a court-ordered operation from last fall that worked to disrupt “Raptor Train”, a botnet responsible for infecting over 260,000 devices globally. Raptor Train was controlled by Flax Typhoon, used to execute DDoS and proxy attacks against both U.S. and Taiwanese military, government, IT, and telecommunication sectors.

Operating as a government contractor, Integrity Tech is allegedly tied to China’s Ministry of State Security with the goal of targeting critical U.S. and overseas infrastructure. These new sanctions by OFAC now freeze the contractors’ U.S. assets and prohibit any transactions with them.

## The Bad | Malware Spammers Exploit Disused Domains to Sidestep Email Security Measures

Spoofed email addresses in malspam campaigns continue to work for attackers who use them to bypass security mechanisms and trick victims into triggering the malware. Despite safeguards like DKIM, DMARC, and SPF designed to prevent attackers from spoofing well-known domains, attackers are getting around these by abusing neglected domains that lack DNS records, making them harder to detect.

Researchers have identified how these spam campaigns use disused domains to distribute phishing emails containing QR codes to malicious sites. In one campaign, active since December 2022, the spammers target users with tax-related lures and urge them to divulge personal and financial information.

Other phishing techniques observed include impersonating big brands like Amazon and Mastercard, redirecting users to fake login pages to steal credentials. Extortion emails, claiming access to compromising videos via malware, have also been noted to demand Bitcoin payments.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/phishing_emails_QRcodes.jpg)

<figcaption>

Instructions to use Alipay/WeChat to scan the QR code (Source: Infoblox)

</figcaption>

</figure>

The most recent of these malspam campaigns target industries like government and construction, using trusted platforms such as Canva and Dropbox to host phishing pages. These often deploy Cloudflare Turnstiles to evade detection by email security tools. The attackers have also launched SMS phishing schemes in the UAE, which revolve around the impersonation of law enforcement with fake payment requests.

Researchers report that generic top-level domains (gTLDs) like `.top` and `.xyz` are increasingly used for cybercrime and now account for 37% of malicious domains due to low registration fees and lax regulations.

Attackers are also employing tools like PhishWP, a malicious WordPress plugin, to create fake payment gateways aimed at harvesting sensitive user information in real-time. Evolving strategies like these emphasize the persistent challenges in combating phishing and domain abuse.

## The Ugly | China-Linked MirrorFace Leverages Backdoors in Long-Term Campaigns Against Japan

Japanese law enforcement have called out MirrorFace, a China-linked threat actor, for reportedly stealing information related to Japan’s national security and advanced technology via persistent attacks that started in 2019.

The National Police Agency (NPA) and National Center of Incident Readiness and Strategy for Cybersecurity (NCSC) accuse the threat actor of targeting Japanese organizations and individuals using tools like ANEL (_aka_ Uppercut),  NOOPDOOR (_aka_ HiddenFace), and LODEINFO.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/MirrorFace_operator_via_LODEINFO.jpg)

<figcaption>

Initial environment observation by the MirrorFace operator via LODEINFO (Source: WeLive Security)

</figcaption>

</figure>

ANEL is a backdoor that has come back since it fell out of use in 2018. The 32-bit HTTP-based implant is designed to capture screenshots, upload and download files, load executables, and run commands through `cmd.exe`. The latest version allows attackers to run a specific program with elevated privileges.

NOOPDOOR is a complex implant, supporting built-in functions and additional modules to both upload and download files while communicating with attacker-controlled servers in both active and passive modes.

According to the NPA and NCSC, MirrorFace operations can be broken down into three major campaigns:

- Campaign A (December 2019 to July 2023):  
    Targeted government agencies, think tanks, politicians, and the media using  
    spear phishing emails to deliver LODEINFO and LilimRAT malware.
- Campaign B (February to October 2023):  
    Focused on manufacturing, aerospace, semiconductor, and academic sectors by exploiting flaws to deploy Cobalt Strike and NOOPDOOR.
- Campaign C (June 2024 to present day):  
    Attacks on academia, think tanks, and politicians to distribute ANEL.

All of these campaigns have leveraged advanced TTPs, including the abuse of Visual Studio Code remote tunnels to enable remote system access, establish C2 connections, and evade network defenses. Other MirrorFace attacks observed since 2023 have included ones targeting India and Taiwan, highlighting the actor’s systematic approach to espionage in sensitive industries across Asia.

Go to Source
