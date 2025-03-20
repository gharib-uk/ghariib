---
title: "New Variant Of XWorm Delivered Via Windows Script File"
date: 2025-01-02
categories: 
  - "security"
---

XWorm refers to a type of malware that has been analyzed for its obfuscation techniques and potential impacts on systems.

While this malware is known for its ability to disguise itself and evade detection which makes it a significant threat in the cybersecurity landscape.

NetSkope researchers recently identified a new variant of XWorm that is delivered via Windows script file.

⁤XWorm is a versatile malware tool that was discovered in “2022,” and since then it has evolved to version 5.6 as recently uncovered by “Netskope Threat Labs.” ⁤

## **XWorm Delivered Via Windows Script**

This “.NET-based” threat initiates its infection chain via a “Windows Script File” (‘WSF’), which downloads and executes an obfuscated “PowerShell script” from “paste\[.\]ee.” ⁤⁤

Analyse Any Suspicious Links Using ANY.RUN’s New Safe Browsing Tool: **Try for Free**

The script creates multiple files (“VsLabs.vbs,” “VsEnhance.bat,” and “VsLabsData.ps1”) in “C:ProgramDataMusicVisuals” and establishes persistence through a scheduled task named “MicroSoftVisualsUpdater.” ⁤

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjuqVvsilUIrxgUqYrJqd4IWqABk1V9J7i1-hfZ3IaZtcl81W8ptaCpupJ4OumQl3deKHjC_Dcuduu14pFphhw2HyRaxuH5QU4MegnKEuj4bEqpM0kSG6Ig1PMoJ7T0cLCI6tvEX72Tl1LxRmWGHjSD0Vh30uAJByczpjpOpHXCvE0sw9mKbS_lF4f_qE8Z/s1127/Scheduled%20task%20as%20persistence%20by%20XWorm%20%28Source%20-%20NetSkope%29.webp)

<figcaption>

Scheduled task as persistence by XWorm (Source – NetSkope)

</figcaption>

</figure>

While besides this, ⁤XWorm employs evasive techniques like “reflective code loading of a DLL loader” (‘NewPE2’) and “process injection into legitimate processes” like ‘RegSvcs.exe.’ ⁤

It communicates with its “command and control (‘C2’) server” via “TCP sockets,” using “AES-ECB encryption” with a modified “MD5 hash” as the key.

Here the new features in v5.6 include the ability to remove plugins and a “Pong” command for response time reporting.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMYUgnQMVVgdCA6fYcXhyphenhyphenz4SXwwSuZgd_82Hnof9EcFSwGSD3S1QuFPZR3zYEEHGdIyTzEVFTD3zJSNZNBc6z4TlDKMNBd-i9ElneeH2-L5VYdDPYgUvroDuPkAv6Z-VN6ptHfMhFrJhJ2rUhtJr4UFijt1xh4lhHwC3zFnqSLCFEkNdM_4Ylz409GeQ8h/s1488/XWorm%20execution%20flow%20%28Source%20-%20NetSkope%29.webp)

<figcaption>

XWorm execution flow (Source – NetSkope)

</figcaption>

</figure>

The malware conducts extensive system reconnaissance by collecting data on “hardware,” “software,” and “user privileges.”

Not only that even it also notifies the attackers through ‘Telegram’ upon “successful infection.”

These sophisticated techniques enable “XWorm” to “access sensitive information,” “gain remote access,” and deploy “additional malware” while evading detection.

XWorm employs multiple attack vectors and can modify the host files on infected systems to redirect the DNS requests for malicious purposes.

The malware launches “DDoS” attacks by sending repetitive “POST requests” to target “IP addresses” and “ports.”

XWorm captures ‘screenshots’ using the “CopyFromScreen” function and stores them as “JPEG” images in memory before transmission.

It executes a wide range of commands like “system manipulation” (‘shutdown,’ ‘restart,’ ‘logoff’), “file operations,” and “remote code execution” via PowerShell.

The malware can download and execute additional payloads like “send HTTP requests,” and “persistently install plugins.”

XWorm utilizes a well-defined message format for communication in its back-channel with the C2 server, and often adds the ‘system information’ of the victim as well.

Another feature is ‘process monitoring,’ where certain operations are conducted stealthily by hiding some activities from the user.

This diverse toolkit enables the actors to have a extensive access and control over the systems that have been compromised which makes “XWorm” a significant threat in today’s cybersecurity ecosystem.

Free Webinar on How to Protect Small Businesses Against Advanced Cyberthreats -> **Free Webinar**

The post New Variant Of XWorm Delivered Via Windows Script File appeared first on Cyber Security News.

Go to Source
