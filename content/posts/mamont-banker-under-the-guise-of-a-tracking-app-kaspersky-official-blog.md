---
title: "Mamont banker under the guise of a tracking app | Kaspersky official blog"
date: 2025-01-03
categories: 
  - "cybersecurity"
  - "security"
---

Mamont banker is distributed under the guise of an application for tracking the delivery of goods offered at wholesale prices.

We’ve discovered a new scheme of distribution of the Mamont (Russian for mammoth) Trojan banker. Scammers promise to deliver a certain product at wholesale prices that may be considered interesting to small businesses as well as private buyers, and offer to install an Android application to track the package. However, instead of a tracking utility, the victim installs a Trojan that can steal banking credentials, push notifications, and other financial information.

## Scheme details

The attackers claim to sell various products at fairly attractive prices via number of websites. To make a purchase, the victim is asked to join a private Telegram messenger chat, where instructions for placing an order are posted. In essence, these instructions boil down to the fact that the victim needs to write a private message to the manager. The channel itself exists to make the scheme look more convincing: participants of this chat ask clarifying questions, receive answers, and comment on things. Probably, there are both other victims of the same scheme and bots that create the appearance of active trading in this chat.

The scheme is made more credible by the fact that the scammers don’t require any prepayment — the victim gets the impression that they’re not risking anything by placing an order. But some time after talking to the manager and placing an order, the victim receives a message that the order has been sent, and its delivery can be tracked using a special application. A link to the .apk file and the tracking number of the shipment are included. The message additionally emphasizes that to pay for the order after receiving it, you must enter a tracking number and wait while the order is loading (which can take more than 30 minutes).

The link leads to a malicious site that offers to download a tracker for the sent parcel. In fact, it’s not a tracker, but the Mamont banking malware for Android. When installed, the “tracker” requests permission to operate in the background, as well as work with push notifications, SMS and calls. The victim is required to enter a code, supposedly for tracking the parcel, and wait.

## What is this malware and why is it dangerous?

In fact, after the victim enters the received “track code”, which is apparently used as the victim’s identifier, the Trojan begins to intercept all push notifications received by the device (for example, confirmation codes for banking transactions) and forward them to the attackers’ server. At the same time, Mamont establishes a connection with the attackers’ server and waits for additional commands. Upon command, it can:

- change the application icon to a transparent one to hide it from the victim;
- forward all incoming SMS messages of the last three days to the attackers;
- open an interface for uploading a photo from the phone’s gallery to the attackers’ server;
- send an SMS to an arbitrary number.

In addition, the attackers can show the victim arbitrary text with boxes for entering additional information — this way they can manipulate the victim to submit additional credentials, or simply collect more information for further attacks using social engineering (for example, for threatening letters from regulators or law enforcement agencies). They probably steal photos from the gallery for the same purpose. This is especially dangerous if the victim is a small business owner: they often use their phone camera to quickly take photos of business information.

Our security solutions detect the malware distributed during this attack as Trojan-Banker.AndroidOS.Mamont.\*. A more detailed technical description of the malware, as well as indicators of compromise, can be found in the dedicated Securelist blog post.

## Targets of this scheme

This campaign is aimed exclusively at Russia-based users of Android smartphones. The attackers emphasize this and refuse to “deliver goods” anywhere else. However, cybercriminals’ tools often become freely available on the darknet, so it’s impossible to guarantee that users from other countries are immune to this threat.

## How to stay safe

We recommend following simple safety rules to avoid infecting your smartphone with this (or any other) malware. This is especially true if the phone is used not only for personal needs, but also for business. Here are these simple safety rules:

- be skeptical of especially-favorable offers of goods and services on the internet (if the price is significantly lower than the usual market price it means the seller’s benefiting in some other way);
- do not run .apk files obtained from unknown sources – they should be installed from official stores or from the official resource of a specific service;
- use a reliable security solution, which will prevent malware from being installed on your device and block malicious links.

Go to Source
