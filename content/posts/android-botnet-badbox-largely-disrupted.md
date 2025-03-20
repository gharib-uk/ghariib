---
title: "Android botnet BadBox largely disrupted"
date: 2025-03-19
---

Removing 24 malicious apps from the Google Play store and silencing some servers almost halved a botnet known as BadBox.

The BadBox botnet focuses on Android devices, but not just phones. It also affects other devices like TV streaming boxes, tablets, and smart TVs.

The German BSI (Federal Office for Information Security) started the disruption campaign in December by blocking the malware on 30,000 devices. BadBox is referred to as a botnet, because one of its capabilities is to set up the affected device to act as a proxy, allowing other people to use the device’s internet bandwidth and hardware to route their own traffic.

This traffic can for example serve in DDoS attacks or as a platform to spread fake news and disinformation. But BadBox can also steal two-factor authentication (2FA) codes, install further malware, and perform ad fraud.

Unfortunately, the 30,000 devices cut off by the BSI were only the tip of the iceberg. Estimates say there may be as many as one million affected devices. These devices have not necessarily been infected by installing malicious apps. It’s been suggested that Chinese manufacturers hide firmware backdoors in their devices, BadBox being one of them.

The BSI said it found:

> “The BadBox malware was already installed on the respective devices when they were purchased.”

According to Satori Threat Intelligence researchers:

> “Devices connected to the BADBOX 2.0 operation included lower-price-point, “off brand”, uncertified tablets, connected TV (CTV) boxes, digital projectors, and more. The infected devices are Android Open Source Project devices, not Android TV OS devices or Play Protect certified Android devices.”

Off brand devices are devices which do not carry any specific brand name that you might recognize. They are often cheap and made by small manufacturers.

Following the botnet’s development after the German disruption, the researchers found new Command and Control (C2) servers which hosted a list of APKs targeting Android Open Source Project devices similar to those impacted by BadBox.

As part of the disruptions, the servers that were controlling the botnet have been sinkholed, which basically means that the traffic between those servers and the botnet clients gets redirected so it will no longer arrive at the intended destination.

## How to stay safe

This disruption will likely not be the end of the story. The botnet operators will adapt again and rebuild their infrastructure. Given their supply chain of compromised devices the botnet will resurface soon enough.

So here are a few things you can do:

- Check you don’t have the apps ‘Earn Extra Income’ and ‘Pregnancy Ovulation Calculator’, which had over 50,000 downloads each. You can recognize the malicious apps from the publisher name Seekiny Studio. If you find them on your device, remove them immediately.

- Protect your Android devices with an active security solution that can remove malicious apps and block malicious traffic.

- Google Play Protect automatically warns users and blocks apps known to exhibit BadBox 2.0-associated behavior at install time on Play Protect certified Android devices with Google Play Services. If a device isn’t Play Protect certified, carefully study its origin before purchasing it.

* * *

**We don’t just report on phone security—we provide it**

Cybersecurity risks should never spread beyond a headline. Keep threats off your mobile devices by downloading Malwarebytes for iOS, and Malwarebytes for Android today.

Go to Source
