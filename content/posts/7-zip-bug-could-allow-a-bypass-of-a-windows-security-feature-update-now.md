---
title: "7-Zip bug could allow a bypass of a Windows security feature. Update now"
date: 2025-01-22
---

A patch is available for a vulnerability in 7-Zip that could have allowed attackers to bypass the Mark-of-the-Web (MotW) security feature in Windows.

The MotW is an attribute added to files by Windows when they have been sourced from an untrusted location, like the internet or a restricted zone. The MotW is what triggers warnings that opening or running such files could lead to potentially dangerous behavior, including installing malware on their devices. 7-Zip added support for MotW in June 2022.

The MotW also makes sure that Office documents that are marked with the MotW will be opened in Protected View, which automatically enables read-only mode and means that all macros will be disabled until the user allows them.

<figure>

![Security warning in file properties](https://www.malwarebytes.com/wp-content/uploads/sites/2/2022/02/Unblock_VBA_macros.png?w=379)

<figcaption>

MotW security warning in file properties

</figcaption>

</figure>

For years, attackers were able to bypass the MotW by putting their malicious files in archives. This worked because the MotW is in fact another file that is attached to the main file as an Alternate Data Stream (ADS), and over the years we have seen many vulnerabilities in archivers where the ADS didn’t pass on the individual files when the archive was decompressed.

The same is true this time. Only the attacker will have to prepare an especially crafted nested archive. A nested archive means there is an open archive inside another open archive. Exploitation of the vulnerability also requires user interaction, meaning the target will have to visit a malicious page or open a malicious file.

If you’re a Windows user, check whether you are using version 7-Zip 24.09 or later. If you’re not, then they’ll need to update.

7-Zip does not have an auto-update function, so you will have to download the version that is suitable for your system from the 7-Zip downloads page.

## Other security measures

There are some general safety tips to keep in mind when you’re handling archived files on a regular basis:

- Keep track of how and where you obtained the archive.

- Always be careful when opening archived files that you downloaded from the internet.

- Make sure you are using an updated anti-malware solution that is capable of scanning inside archives, and you have that setting enabled.

<figure>

![Malwarebytes scan within archives option enabled](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/scan-archives.png)

<figcaption>

Malwarebytes scan within archives option enabled

</figcaption>

</figure>

- Keep track of who accesses archived files and when. This can help identify unauthorized access attempts and help monitor unwanted changes.

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

Go to Source
