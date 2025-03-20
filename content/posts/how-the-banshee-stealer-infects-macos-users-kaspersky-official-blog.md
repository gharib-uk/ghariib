---
title: "How the Banshee stealer infects macOS users | Kaspersky official blog"
date: 2025-02-01
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

The dangerous Banshee stealer for Macs has learned how to bypass built-in macOS security, and continues to evolve. How to protect yourself?

Many macOS users believe their operating system is immune to malware, so they don’t need to take extra security precautions. In reality, it’s far from the truth, and new threats keep popping up.

## Are there viruses for macOS?

Yes — and plenty of ’em. Here are some examples of Mac malware we’ve previously covered on Kaspersky Daily and Securelist:

- A crypto-wallet-stealing Trojan disguised as pirated versions of popular macOS apps.

![The Trojan's installation in macOS](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/01/30094153/banshee-stealer-targets-macos-users-1.jpg)

This Trojan’s malicious payload is stored in the “activator”. The cracked app won’t work until it’s launched.Source

- Another crypto-stealing Trojan, this one masquerading as a PDF document titled “Crypto-assets and their risks for financial stability”.
- A Trojan that used infected Macs to create a network of illegal proxy servers for routing malicious traffic.
- The Atomic stealer, distributed as a fake Safari update.

We could go on with this list of past threats, but let’s instead now focus on one of the latest attacks targeting macOS users, namely – the Banshee stealer…

## What the Banshee stealer does

Banshee is a fully-fledged infostealer. This is a type of malware that searches the infected device (in our case, a Mac) for valuable data and sends it to the criminals behind it. Banshee is primarily focused on stealing data related to cryptocurrency and blockchain.

Here’s what this malware does once it’s inside the system:

- Steals logins and passwords saved in various browsers: Google Chrome, Brave, Microsoft Edge, Vivaldi, Yandex Browser, and Opera.
- Steals information stored by browser extensions. The stealer targets over 50 extensions – most of which are related to crypto wallets, including Coinbase Wallet, MetaMask, Trust Wallet, Guarda, Exodus, and Nami.
- Steals 2FA tokens stored in the Authenticator.cc browser extension.
- Searches for and extracts data from cryptocurrency wallet applications, including Exodus, Electrum, Coinomi, Guarda, Wasabi, Atomic, and Ledger.
- Harvests system information and steals the macOS password by displaying a fake password entry window.

Banshee compiles all this data neatly into a ZIP archive, encrypts it with a simple XOR cipher, and sends it to the attackers’ command-and-control server.

In its latest versions, Banshee’s developers have added the ability to bypass the built-in macOS antivirus, XProtect. Interestingly, to evade detection, the malware uses the same algorithm that XProtect uses to protect itself, encrypting key segments of its code and decrypting them on the fly during execution.

## How the Banshee stealer spreads

The operators of Banshee primarily used GitHub to infect their victims. As bait, they uploaded cracked versions of expensive software such as Autodesk AutoCAD, Adobe Acrobat Pro, Adobe Premiere Pro, Capture One Pro, and Blackmagic Design DaVinci Resolve.

![Banshee stealer distribution on GitHub ](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/01/30094028/banshee-stealer-targets-macos-users-2.jpg)

The creators of Banshee used GitHub to spread the malware under the guise of pirated software. Source

The attackers often targeted both macOS and Windows users at the same time: Banshee was often paired with a Windows stealer called Lumma.

Another Banshee campaign, discovered after the stealer’s source code was leaked (more on that below), involved a phishing site offering macOS users to download “Telegram Local” – supposedly designed to protect against phishing and malware. Of course, the downloaded file was infected. Interestingly, users of other operating systems wouldn’t even see the malicious link.

![Banshee being spread through a phishing site](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/01/30093821/banshee-stealer-targets-macos-users-3.jpg)

A phishing site offers to download Banshee disguised as “Telegram Local”, but only to macOS users (left). Source

## The past and future of Banshee

Let’s now turn to Banshee’s history, which is really quite interesting. This malware first appeared in July 2024. Its developers marketed it as a malware-as-a-service (MaaS) subscription, charging $3000 per month.

Business must not have been great, as by mid-August they’d slashed the price by 50% – bringing the monthly subscription down to $1500.

![Discounted Banshee stealer announcement ](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/01/30093654/banshee-stealer-targets-macos-users-4.jpg)

A hacker site ad announcing a discount on Banshee: $1500 instead of $3000 per month. Source

At some point, the creators either changed their strategy, or decided to add an affiliate program to their portfolio. They began recruiting partners for joint campaigns. In these campaigns, Banshee’s creators provided the malware, and the partners executed the actual attack. The developers’ idea was to split the earnings 50/50.

However, something must have gone very wrong. In late November, Banshee’s source code was leaked and published on a hacker forum – thus ending the malware’s commercial life. The developers announced they were quitting the business – but not before attempting to sell the entire project for 1BTC, and then for $30,000 (most likely having learned of the leak).

Thus, for several months now, this serious stealer for macOS has been available to essentially anyone completely free of charge. Even worse, with the source code also available, cybercriminals can now create their own modified versions of Banshee.

And judging from the evidence, this is already happening. For example, the original versions of Banshee stopped working if the operating system was running in the Russian language. However, one of the latest versions has removed the language check, meaning Russian-speaking users are now also at risk.

## How to protect yourself from Banshee and other macOS threats

Here are some tips for macOS users to stay safe:

- Don’t install pirated software on your Mac. The risk of running into a Trojan by doing so is very high, and the consequences can be severe.
- This is especially true if you use the same Mac for cryptocurrency transactions. In this case, the potential financial damage could significantly exceed any savings you make on purchasing genuine software.
- In general, avoid installing unnecessary applications, and remember to uninstall programs you no longer use.
- Be cautious with browser extensions. They may seem harmless at first glance, but many extensions have full access to the contents of _all_ web pages, making them just as dangerous as full-fledged apps.
- And of course, be sure to install a reliable antivirus on your Mac. As we’ve seen, malware for macOS is a very real threat.

Finally, a word on Kaspersky security products. They can detect and block many Banshee variants with the verdict _Trojan-PSW.OSX.Banshee_. Some new versions resemble the AMOS stealer, so they can also be detected as _Trojan-PSW.OSX.Amos.gen_.

Go to Source
