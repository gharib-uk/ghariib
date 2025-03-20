---
title: "Cybercriminals are distributing a miner disguised as a restriction-bypassing toolCybercriminals are distributing a miner disguised as a restriction-bypassing tool | Kaspersky official blog"
date: 2025-03-19
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

Scammers are blackmailing YouTube bloggers into promoting a miner disguised as a tool for bypassing restrictions and censorship.

Over the past six months, Windows Packet Divert drivers for intercepting and modifying network traffic on Windows systems have become popular in Russia. From August to January 2024, we noted that detections of these drivers almost doubled. The main reason? These drivers are being used in tools designed to bypass restrictions for accessing foreign resources.

This surge in popularity hasn’t gone unnoticed by cybercriminals. They’re actively distributing malware disguised as bypassing tools — and they’re doing it by blackmailing bloggers. So, every time you watch a video titled something like “How to bypass restrictions…”, be especially cautious — even the most reputable content creators might unknowingly be spreading stealers, miners, and other malware.

How cybercriminals exploit unsuspected users — and where bloggers fit into the picture — is what we’ll explore in this article.

## Hackers disguised as honest developers

There are plenty of software solutions designed to bypass restricted access to foreign platforms, but they all have one thing in common — they’re created by small-time developers. Such programs spread organically: an enthusiast writes some code, shares it with friends, makes a video about it, and _voilà_ — yesterday’s unknown programmer becomes a “people’s hero”. His GitHub repository is starred tens of thousands of times, and people thank him for restoring access to their favorite online resources. We recently wrote about one such case where cybercriminals boosted GitHub repositories containing malware.

There may be dozens or even hundreds of such enthusiasts — but who are they, and can they be trusted? These are key questions both current and potential users of these programs should be asking. A major red flag is when these developers recommend disabling antivirus protection. Disabling protection to voluntarily give a potential hacker access to your device? That’s a risky move.

Of course, behind the mask of a people’s hero might be a hacker looking for profit. An unprotected device is vulnerable to malware families like NJRat, XWorm, Phemedrone, and DCRat, which have been commonly spread alongside such bypassing software.

## Where do bloggers fit in?

We’ve identified an active miner distribution campaign that has claimed at least two thousand victims in Russia. One of the infection sources was a YouTube channel with 60,000 subscribers. The blogger uploaded several videos on bypassing restrictions, with a link to a malicious archive in the description. These videos accumulated over 400,000 views in total. Later, the channel owner deleted the link, leaving this note: _“Download the file here: (program does not work)”._ Originally, the link led to the fraudulent site _gitrok\[.\]com_, where the infected archive was hosted. According to the site’s counter, at the time of our study the bypassing tool had been downloaded at least 40,000 times.

Don’t rush to put all the blame on the bloggers — in this case, they were simply following the orders of cybercriminals, unaware of what was really going on. Here’s how it works. First, the criminals file a complaint against a video about such a restriction-bypassing tool, pretending to be the software’s developers. Then they contact the video creator and persuade them to upload a new video, this time containing a link to their malicious website — claiming that this is now the only official download page. Of course, the bloggers have no idea the site is distributing malware — specifically, an archive containing a miner. And for those who’ve already uploaded three or more videos on the topic, refusal is not an option. The hackers threaten to file multiple complaints, and if there are three or more, the channel would be deleted.

In addition, the criminals spread their malware and installation guides through other Telegram and YouTube channels. Most of these have been deleted — but there’s nothing to stop them from creating new ones.

## What about the miner?

The malware in question was a sample of SilentCryptoMiner, which we covered in October 2024. It’s a stealthy miner based on XMRig, another open-source mining tool. SilentCryptoMiner supports mining of multiple popular cryptocurrencies, including ETH, ETC, XMR, RTM, and others. The malware stops mining upon detecting certain processes, the list of which the criminals can provide remotely to evade detection. That makes it nearly impossible to detect without reliable protection.

For more about the malicious archive and how it persists in the system, check our post on Securelist.

## How to protect yourself from miners

- **Ensure that all personal devices have trusted protection** to safeguard against miners and other malware.
- **Avoid downloading programs from obscure or little-known sources.** Stick to official platforms, but remember — malware can creep into them too.
- **Keep in mind that even the most reputable bloggers can unknowingly spread malware**, including miners and stealers.

> Here are some relevant articles you can read to learn more about miners and their dangers:
> 
> Mario Forever, malware too: a free game with a miner and Trojans inside
> 
> XMRig Miner as a New Year’s gift
> 
> Prices down, miners up

Go to Source
