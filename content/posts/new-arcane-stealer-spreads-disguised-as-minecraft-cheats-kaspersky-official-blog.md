---
title: "New Arcane stealer spreads disguised as Minecraft cheats | Kaspersky official blog"
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

Cybercriminals are spreading a stealer disguised as a "cheat manager" via YouTube videos to steal login credentials. Get the lowdown!

At the end of 2024, our experts discovered a new stealer called Arcane, which collects a wide range of data from infected devices. Now cybercriminals have taken it a step further by releasing ArcanaLoader — a downloader that claims to install cheats, cracks, and other “useful” gaming tools, but which actually infects devices with the Arcane stealer. Despite their lack of creativity in naming the loader, their distribution scheme is actually quite original.

Hopefully, you already know not to download random files from YouTube video descriptions. No? Then keep reading.

## How the Arcane stealer spreads

The malicious campaign distributing the Arcane stealer was active even before the malware itself appeared. In other words, cybercriminals were already spreading other malware, eventually replacing it with Arcane.

Here’s how it worked. First, links to password-protected archives containing malware were placed under YouTube videos advertising game cheats. These archives always included a _seemingly harmless_ BATCH file named **start.bat**. This file’s purpose was to launch PowerShell to download another password-protected archive containing two executable files: a miner and the VGS stealer. The VGS stealer was later replaced with Arcane. At first, the new stealer was distributed in the same way: YouTube video, first malicious archive, then second one, and bingo: Trojan on the victim’s device.

A few months later, the criminals upgraded their approach. Under the YouTube video they started linking to ArcanaLoader — a downloader with a graphical interface, supposedly needed to install cheats, cracks, and similar software. In reality, ArcanaLoader infected devices with the Arcane stealer.

![Inside the client — various cheat options for Minecraft](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/19043321/arcane-stealer-instead-of-cheats-for-minecraft-01.jpeg)

Inside the client — various cheat options for Minecraft

The operation didn’t end with ArcanaLoader. The attackers also set up a dedicated Discord server to embellish their scheme. Among other things, this server is used to recruit YouTubers willing to post links to ArcanaLoader in their video descriptions. The requirements for recruitment are minimal: at least 600 subscribers, over 1500 views, and at least two uploaded videos with links to the downloader. In exchange, participants are promised a new role on the server, the ability to post videos in the chat, instant addition of requested cheats to the downloader, and potential income for generating high traffic. Whether any of these unwitting malware distributors actually received payments is unknown.

![The ArcanaLoader Discord server has over 3000 members](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/19043438/arcane-stealer-instead-of-cheats-for-minecraft-02.png)

The ArcanaLoader Discord server has over 3000 members

All communication on the ArcanaLoader Discord server is in Russian, and our telemetry shows the highest number of victims are in Russia, Belarus, and Kazakhstan. We can conclude from this that Arcane primarily targets Russian-speaking gamers.

## How dangerous is the Arcane stealer?

A stealer is a type of malware that steals login credentials and other sensitive information, sending them to attackers. This information helps cybercriminals gain access to accounts in games, social networks, and more. Regarding Arcane, its capabilities are constantly evolving, with cybercriminals actively updating the stealer’s code. At the time of publication of this post, Arcane could steal the “golden classics”: usernames, passwords, and payment card details. The main sources of information for the stealer are browsers based on Chromium and Gecko engines, which is why we recommend against storing such confidential information in browsers. It’s better to use a trusted password manager.

The stealer has another method for extracting cookies from Chromium-based browsers, and stolen cookies can be used for various malicious purposes, including hijacking a YouTube channel. For how exactly this works, read the Securelist study.

In addition to browser data, Arcane steals configuration files, settings, and account information from the following applications:

- **VPN clients.** OpenVPN, Mullvad, NordVPN, IPVanish, Surfshark, Proton, HideMyName, PIA, CyberGhost, ExpressVPN.
- **Network clients and utilities.** Ngrok, PlayIt, Cyberduck, FileZilla, DynDns.
- ICQ, Tox, Skype, Pidgin, Signal, Element, Discord, Telegram, Jabber, Viber.
- **Email clients.**
- **Game clients and services.** Riot Client, Epic Games, Steam, Ubisoft Connect, Roblox, Battle.net, various Minecraft clients.
- **Cryptocurrency wallets.** Zcash, Armory, Bytecoin, Jaxx, Exodus, Ethereum, Electrum, Atomic, Guarda, Coinomi.

An impressive list, right? Arcane also steals various system information. The stealer tells attackers what version of the OS is installed, when it was installed, the Windows activation key, details of the infected system’s hardware, screenshots, running processes, and saved Wi-Fi passwords.

## How to protect yourself from Arcane

The attackers started by simply placing links to malicious archives under YouTube videos, and later set up their own Discord server and created a downloader with a graphical interface. Of course, all of this was done to give the scam false credibility, luring in potential victims. From this campaign, we can see that cybercriminal groups today are highly adaptable, quickly shifting their distribution strategies and methods.

- **Don’t download any files from YouTube video descriptions.** Experience shows that even trusted bloggers may sometimes unknowingly spread Trojans.
- **Protect your device** **with a reliable security solution** that will swiftly detect and neutralize the Arcane stealer (among others).
- **Don’t store usernames, passwords, and banking information in browsers****.** While convenient, this is a very risky way to store such critical data. Trust them to Kaspersky Password Manager — you just need a single master password, and we’ll take care of the rest.
- **Be suspicious of cheats, mods, and cracks.** Especially for Minecraft and Roblox — players of these games are targeted most often.

To learn more about other types of stealers and their capabilities, don’t miss these posts:

> - Beware of stealers disguised as… wedding invitations
> - Banshee: a stealer targeting macOS users
> - Mario Forever, malware too: a free game with a miner and Trojans inside
> - Hacking YouTube channels with stolen cookies
> - …and many others.

Subscribe to our blog and follow our Kaspersky Telegram channel to stay informed on the latest cybersecurity threats. Also, be sure to share this post with anyone who frequently plays games but may not be aware of the dangers.

Go to Source
