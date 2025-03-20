---
title: "This Week in Security: IOCONTROL, (Location) Leaking Cars, and Passkeys"
date: 2025-01-03
categories: 
  - "security"
---

![](https://hackaday.com/wp-content/uploads/2016/01/darkarts.jpg?w=800)

Claroty’s TEAM82 has a report on a new malware strain, what they’re calling IOCONTROL. It’s a Linux malware strain aimed squarely at embedded devices. One of the first targets of this malware, surprisingly, is the Iraeli made Orpak gas station pumps. There’s a bit of history here, as IOCONTROL is believed to be used by CyberAv3ngers, a threat actor aligned with Iran. In 2023 a group aligned with Israel claimed to have compromised the majority of the gas stations in Iran. IOCONTROL seems to have been deployed as retribution.

There are a few particularly interesting aspects of this malware, and how TEAM82 went about analyzing it. The first is that they used unicorn to emulate the obscure ARM platform in question. This was quite an adventure, as they were running the malicious binary without the normal Linux OS under it, and had to re-implement system calls to make execution work. The actual configuration data was encrypted as the data section of the executable, presumably to avoid simple string matching detection and analysis.

Then to communicate with the upstream command and control infrastructure, the binary first used DNS-Over-HTTPS to resolve DNS addresses, and then used the MQTT message protocol for actual communications. Once in place, it has the normal suite of capabilities, like code execution, cleanup, lateral scanning, etc. An interesting speculation is that the level of control this malware had over these gas pumps, it was in a position to steal credit card information. This malware family isn’t limited to gas pumps, either, as it’s been spotted in IoT and SCADA devices from a whole host of vendors.

## Bit-unlocker

We have another attack against TPM backed Bitlocker full disk encryption. The idea here is that by default Bitlocker uses an encryption key provided by the system’s Trusted Platform Module (TPM). Unless the user intentionally turns on Bitlocker PIN, this key from the TPM is the only credential needed to decrypt the drive, and is automatically provided at boot time. We’ve covered one attack against Bitlocker, where the key is sniffed while it’s being transferred from the external TPM. The conclusion as of that coverage was that a firmware TPM saves you from this attack, since there’s no accessible bus to sniff data from.

Well. There’s another approach, as you might have guessed. Modern memory requires constant refreshing to not lose its value, but that doesn’t mean that it’s entirely lost immediately. That’s what \[Jack Crouse\] discovered, and put to work here. Using the reset pins on a motherboard, the system is reset and booted off a flash drive. That drive contains a very minimal EFI application that just reads system memory and dumps it to the flash drive. Because the memory is mostly intact, if you reset the machine at the right point during boot, the memory dump includes the disk encryption key, allowing for easy drive decryption. If nothing else, this should be your queue to add a PIN to your Bitlocker setup. This was also a talk given at 38c3, which is now available!

## Stars for Sale

GitHub stars are a useful way to determine the popularity of a project, and by extension how trustworthy that project is. At least, that’s the idea. Like any measure of popularity and trustworthiness, the GitHub Stars system has been gamed. Given how easy it is to create a GitHub account, and that giving out stars is a free action, it’s not surprising. The research suggested that between 3 and 4.5 million stars were fake, and GitHub has been quite responsive at removing the accounts and stars that are very likely to be inauthentic.

## The Downside to a Connected Car

In a tale that gets worse the more you think about it, it’s revealed that 800,000 Volkswagen electric vehicles were leaking their precise information history via an unsecured Amazon storage instance. This wasn’t explicitly referred to as an S3 bucket, but we’ll use the “bucket” term for ease of discussion. This was discovered via an unnamed whistleblower, so it’s unclear whether the bucket name was accidentally made public. Regardless, it was accessible without any authentication. The broader question is _why_ VW needs to keep these records on their drivers. It’s the downside to an always connected car.

## How’s the Passkey Doing?

\[Dan Goodin\] is no stranger to the pages of this column, and he has thoughts about Passkeys. This isn’t a vulnerability — the FIDO2 specification hasn’t been broken in some new and clever way. Passkeys are still a good, secure way to use a trusted device as an authentication source. The problem is, they’re sort of a pain to use. Say you’re using Google Chrome on an Apple device. A site prompts you to create a passkey. Is that passkey managed by Apple, or Google? The answer is, by Apple, unless you explicitly ask Chrome to manage it. And then, Chrome on Mac isn’t allowed to sync Passkeys to Chrome on an iPhone.

And those are essentially the two problems with Passkeys: Every vendor wants users to use their platform to store passkeys, and once stored it’s devilishly difficult to manage and move passkeys to another device/platform. The silver lining is that many password managers can act as a Passkey store, and handle syncing between devices. But then again, there’s not much difference between passwords and passkeys, when you use a password manager to handle them.

## Double-Click-Jack

And in related news, there’s a new approach to harvesting unintended clicks. Clickjacking is what happens when a site loads an advertisement at the top of the page, just as you’re trying to click on something, and your click gets hijacked to something else. Browsers have added protections to make truly malicious clickjacking harder to pull off. But Doubleclickjacking neatly sidesteps all of them. It’s simple: Launch another tab that claims to be a captcha, asking the user to double-click to prove they are human. Close the tab after a single click, and the second click goes to a different window. It’s clever and devious, and one more thing to watch out for.

<iframe loading="lazy" title="Salesforce Account Takeover via Doubleclick" width="640" height="480" src="https://www.youtube.com/embed/4rGvRRMrD18?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Bits and Bytes

The US Treasury has reported that it was breached, via the ironically named BeyondTrust remote support vendor. It’s reported that this was an APT affiliated with the Chinese government, though very few details are available.

The intersection of data scraping and AI writing has led to dangerously good targeted phishing emails. Part of the danger here is that so much of the legitimate emails that spam filters are trained on are also written by LLMs, and executives are so used to that style of message, phishing emails fit right in.

\[Mateusz Jurczyk\] has released part five of the Windows Registry deep dive over at Google Project Zero. This installment is all about how the data is actually encoded into the registry files, as well as how those files are loaded and verified. Good stuff.

Go to Source
