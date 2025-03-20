---
title: "“Can you try a game I made?” Fake game sites lead to information stealers"
date: 2025-01-03
categories: 
  - "security"
---

_The background and the IOCs for this blog were gathered by an Expert helper on our forums and Malwarebytes researchers. Our thanks go out to them._

A new, malicious campaign is making the rounds online and it starts simple: Unwitting targets receive a direct message (DM) on a Discord server asking about their interest in beta testing a new videogame (targets can also receive a text message or an email). Often, the message comes from the “developer” themselves, as asking whether you can try a game that they personally made is a common method to lure victims.

If interested, the victim will receive a download link and a password for the archive containing the promised installer.

The archives are offered for download on various locations like Dropbox, Catbox, and often on the Discord content delivery network (CDN), by using compromised accounts which add extra credibility.

What the target will actually download and install is in reality an information stealing Trojan.

There are several variations going around. Some use NSIS installers, but we have also seen MSI installers. There are also various information stealers being spread through these channels like the Nova Stealer, Ageo Stealer, or the Hexon Stealer.

The Nova Stealer and the Ageo Stealer are a Malware-as-a-Service (MaaS) stealer where criminals rent out the malware and the infrastructure to other criminals. It specializes in stealing credentials stored in most browsers, session cookie theft for platforms like Discord and Steam, and information theft related to cryptocurrency wallets.

Part of the Nova Stealer’s infrastructure is a Discord webhook which allows the criminals to have the server send data to the client whenever a certain event occurs. So they don’t have to check regularly for information, they will be alerted as soon as it gets in.

The Hexon stealer is relatively new, but we know it is based on Stealit Stealer code and capable of exfiltrating Discord tokens, 2FA backup codes, browser cookies, autofill data, saved passwords, credit card details, and even cryptocurrency wallet information.

One of the main interests for the stealers seem to be Discord credentials which can be used to expand the network of compromised accounts. This also helps them because some of the stolen information includes friends accounts of the victims. By compromising an increasing number of Discord accounts, criminals can fool other Discord users into believing that their everyday friends and contacts are speaking with them, emotionally manipulating those users into falling for even more scams and malware campaigns.

But the end goal to this scam, and most others, is monetary gain. So keep an eye on your digital and flat currency if you’ve fallen for one of these scams.

## How to recognize the fake game sites

There is one very active campaign that uses a standard template for the website. This makes it easier for the cybercriminals to change name and location, but also for us to recognize them.

<figure>

![Standard layout of the fake game websites](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/example_site.jpg?w=1024)

<figcaption>

Example of the templated fake website

</figcaption>

</figure>

The websites are hosted by various companies that are very unresponsive to take down requests and usually protected by Cloudflare which adds an extra layer of difficulty for researchers looking to get the sites taken down. At which point they will easily set up a new one.

Another campaign uses blogspot to host their malware. These sites on blogspot have a different, but also standard, design.

<figure>

![fake game site on blogspot](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/blogspot_example.jpg)

<figcaption>

blogspot template

</figcaption>

</figure>

Other effective measures to stay safe from these threats include:

- Having an up-to-date and active anti-malware solution on your computer.

- Verifying invitations from “friends” through a different channel, such as texting them directly or contacting them on another social media platform. Remember, their current account may have been compromised.

- Remembering to not act upon unsolicited messages and emails, especially when they want you to download and install something.

## IOCs

**Download sites:**

dualcorps\[.\]fr

leyamor\[.\]com

crystalsiege\[.\]com

crystalsiege\[.\]online

dungeonofdestiny\[.\]pages.dev

mazenugame\[.\]blogspot.com

mazenugames.\[\]blogspot.com

yemozagame\[.\]blogspot.com

domenubeta\[.\]blogspot.com

domenugame\[.\]blogspot.com

The known download sites will be blocked by Malwarebytes/ThreatDown products which will also detect the information stealers.

**We don’t just report on threats – we help protect your social media**

Cybersecurity risks should never spread beyond a headline. Protect your social media accounts by using Malwarebytes Identity Theft Protection.

Go to Source
