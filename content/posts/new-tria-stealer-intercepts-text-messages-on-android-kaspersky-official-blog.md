---
title: "New Tria stealer intercepts text messages on Android | Kaspersky official blog"
date: 2025-02-06
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

Attackers are distributing the Tria stealer under the guise of wedding invitations.

Getting married is certainly one of the most important events in anyone’s life. And in many cultures, it’s customary to invite hundreds of guests to the celebration — including some you barely know. Cybervillains take advantage of such traditions, using wedding invitations as bait to launch attacks on Android smartphone users.

Here’s what threat actors have come up with this time, and how to defeat it.

## How weddings and APKs are linked

You may already know about our global threat intelligence network — Kaspersky Security Network (KSN). In 2024, we spotted several suspicious and clearly malicious APK samples circulating in both Malaysia and Brunei. At the same time, social networks were buzzing with Android users of those same countries complaining about having their WhatsApp accounts hacked, or receiving suspicious APKs through WhatsApp or other messenger apps.

Connecting the dots, we deduced that cybercriminals were sending Android users in Brunei and Malaysia wedding invitations in the form of an APK, which victims were urged to install on their own devices themselves. In the message, the attacker begins by apologizing for inviting the recipient to such an important event through WhatsApp rather than in person, then suggests that the user find the time and place of the celebration in the attached file — which turned out to be the same malicious APK that we found in KSN.

![Examples of wedding invitations sent by attackers in the Indonesian language](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/02/03082820/tria-stealer-wedding-scam-01-1024x783.jpg)

Examples of wedding invitations sent by attackers in the Indonesian language

The scheme uses two versions of the same stealer (one appeared in March 2024, the other with added functionality in August), which we’ve called Tria — after the name of the user who appears to be responsible for supporting or even conducting the entire campaign.

## What the Tria stealer does

The malware primarily harvests data from text and email messages, but also reads call and message logs that it later sends to the C2 server through various Telegram bots. Naturally, the attackers don’t do this out of their love of reading other people’s correspondence. All stolen data is used to hack victims’ Telegram, WhatsApp, and other accounts, and then message their contacts asking for money. However, an even more unpleasant scenario is possible: attackers could gain access to the victim’s online banking accounts by requesting and intercepting OTP codes needed for login.

To disguise itself, the stealer employs social engineering tactics: hiding behind a gear icon, it mimics a system application to get the permissions it needs from the user. The malware needs ten permissions in total, including access to network activity and sending/reading text messages. For details on what other permissions Tria requests and how exactly the stealer works, see the full post on our Securelist blog.

It’s known at present that the attacks were limited to users in Malaysia and Brunei, and not targeted at any specific individuals; however, the cybervillains may decide to expand their reach going forward. And when it comes to the bogus invitation that leads to installing the APK, the scope isn’t limited to weddings — future attacks could exploit religious ceremonies, birthdays… you name it. So be vigilant, arm yourself with reliable protection, and read our tips on how to combat this stealer and other malware for Android.

## How to guard against the Tria stealer

The simple method of distribution makes it fairly easy to protect yourself against:

- **Never respond to strangers in messenger apps** — especially if they ask you to download and install something. Be wary of such messages even if they come from people in your contact list.
- **Never open APKs downloaded from untrusted sources.** If you need to install something on your smartphone, always use official app stores (though even these aren’t immune to malware) or developer websites.
- **Install Kaspersky for Android** **on your smartphone** to protect it from Tria.
- **Don’t grant apps more permissions than they need.** Be wary of new apps that are permission-hungry.
- **Harden your accounts in other messenger apps and social networks.** You can find in-depth guides to privacy settings at the Privacy Checker

At the end of any scam-themed post, we usually recommend setting up two-factor authentication (2FA) for all applications and services where it’s possible. However, in the fight against Tria, as well as many other Trojans, 2FA with OTP by text isn’t much help: this malware can intercept incoming messages, extract codes from them, and even delete such messages so you never notice anything.

As such, we advise using an authenticator app to generate 2FA codes. Kaspersky Password Manager is the perfect solution — it securely generates OTPs and reliably stores passwords and confidential documents, with the option to sync them across all your devices.

It’s worth noting that stealers are particularly fond of hijacking Telegram accounts. To avoid losing yours, we recommend setting up a Telegram cloud password this very instant, using Kaspersky Password Manager to create and store it. To find out how to configure 2FA, refer to our **What to do if your Telegram account is hacked** post.

Go to Source
