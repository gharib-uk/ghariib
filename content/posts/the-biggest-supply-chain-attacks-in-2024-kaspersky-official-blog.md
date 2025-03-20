---
title: "The biggest supply chain attacks in 2024 | Kaspersky official blog"
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

Attacks on supply chains were one of the biggest threats in 2024. We discuss the most notable incidents of last year, and their consequences for the attacked.

A supply-chain attack can totally thwart all a targeted company’s efforts to protect its infrastructure. Preventing such attacks is extremely difficult because a significant portion of an attack occurs in infrastructure that’s not within the security team’s control. This makes supply-chain attacks one of the most dangerous threats in recent years, and today we’ll look at some of the biggest that took place in 2024.

## January 2024: malicious npm packages stole SSH keys from hundreds of developers on GitHub

The first major supply-chain attack in 2024 involved malicious npm packages uploaded to GitHub in early January. The main purpose of these modules, named warbeast2000 and kodiak2k, was to search infected systems for SSH keys and send them back to the criminals. Some versions of kodiak2k also included a script to launch Mimikatz, a tool used to extract passwords from memory.

In total, attackers managed to publish eight versions of warbeast2000, and over 30 versions of kodiak2k. By the time they were discovered and removed from the repository, the malicious packages had already been downloaded 412 and 1281 times, respectively — meaning potentially hundreds of developers were affected.

## February 2024: abandoned PyPI package used to distribute NovaSentinel infostealer

In February, a malicious update was discovered in the django-log-tracker package, which was hosted on the Python Package Index (PyPI). The latest legitimate version of this module was published in 2022, and since then it had been abandoned by its creators. It appears that the attackers managed to hijack the developer’s PyPI account and upload their own malicious version of the package.

The malicious update contained only two files with identical and very simple code; all the original module content was deleted. This code downloaded an EXE file from a certain URL and executed it.

This EXE file was an installer for the NovaSentinel stealer malware. NovaSentinel is designed to steal any valuable information it can find in the infected system, including saved browser passwords, cryptocurrency wallet keys, Wi-Fi passwords, session tokens from popular services, clipboard contents, and more.

## March 2024: backdoor implanted in popular Linux distributions using XZ Utils

In late March an incident was reported that could potentially have become the most dangerous supply-chain attack of 2024 with devastating consequences. As part of a sophisticated operation lasting two-and-a-half years, a GitHub user known as Jia Tan managed to gain control over the XZ Utils project — a set of compression utilities included in many popular Linux distributions.

With the project under his control, Jia Tan published two versions of the package (5.6.0 and 5.6.1), both containing the backdoor. As a result, the compromised liblzma library was included in test versions of several Linux distributions.

According to Igor Kuznetsov, head of Kaspersky’s Global Research & Analysis Team (GReAT), the CVE-2024-3094 vulnerability could have become the biggest ever attack on the Linux ecosystem. Had the vulnerability been introduced into stable distributions, we might have seen massive server compromises. Fortunately, CVE-2024-3094 was detected in test and rolling-release distributions, so most Linux users remained safe.

## April 2024: malicious Visual Studio projects spread malware on GitHub

In April, an attack targeting GitHub users was discovered in which attackers published malicious Visual Studio projects. To aid their attack, the attackers skillfully manipulated GitHub’s search algorithm. First, they used popular names and topics for their projects. Second, they created dozens of fake accounts to “star” their malicious projects, creating the illusion of popularity. And third, they automatically published frequent updates, making meaningless changes to a file included solely for this purpose. This made their projects appear fresh and up-to-date compared to available alternatives.

Inside these projects, malware resembling Keyzetsu Clipper was hidden. This malware intercepts and replaces cryptocurrency wallet addresses copied to the clipboard. As a result, crypto-transactions on the infected system are redirected to the attackers instead of the intended recipient.

## May 2024: backdoor discovered in the JAVS courtroom video recording software

In May, reports emerged about the trojanization of the JAVS (Justice AV Solutions) courtroom recording software. This system is widely used in judicial institutions and other law enforcement-related organizations, with around 10 000 installations worldwide.

A dropper was found inside the ffmpeg.exe file — included in the JAVS.Viewer8.Setup\_8.3.7.250-1.exe installer on the official JAVS website. This dropper executed a series of malicious scripts on infected systems, designed to bypass Windows security mechanisms, download additional modules, and collect login credentials.

## June 2024: tens of thousands of websites using Polyfill.io delivered malicious code

In late June, the cdn.polyfill.io domain began distributing malicious code to visitors of websites relying on the Polyfill.io service. Users were redirected to a Vietnamese-language sports betting site through a fake domain impersonating Google Analytics (www\[.\]googie-anaiytics\[.\]com).

Polyfill.io was originally created by the Financial Times to ensure that websites remain compatible with older or less common browsers. However, in 2024, it was sold to Chinese CDN provider Funnull, along with its domain and GitHub account — and this is where the trouble began.

Over the years, Polyfill.io became very popular. Even at the time of the incident, more than 100 000 websites worldwide — including many high-profile ones — were still using polyfills, even though they’re no longer needed. Following the attack, the original creator of Polyfill.io advised users to stop using the service. However, the script is currently still present on tens of thousands of websites.

## July 2024: trojanized jQuery version found on npm, GitHub, and jsDelivr

In July, a trojanized version of jQuery — the popular JavaScript library used to simplify interaction with the HTML Document Object Model (DOM) — was discovered. Over the course of several months, the attackers managed to publish dozens of infected packages to the npm registry. The trojanized jQuery was also found on other platforms, including GitHub, and even jsDelivr n — a CDN service for delivering JavaScript code.

Despite being compromised, the trojanized versions of jQuery remained fully functional. The main difference from the original library was the inclusion of malicious code designed to capture all user data entered into forms on infected pages and then send it to an attacker-controlled address.

## August 2024: infected plug-in for the multi-protocol messenger Pidgin

At the end of August, one of the plug-ins published on the official Pidgin messenger page was found distributing DarkGate — a multi-functional malware that gives attackers remote access to infected systems where they can install additional malware.

Pidgin is an open-source “all-in-one” messenger, allowing users to communicate across multiple messaging systems and protocols without installing separate applications. Although Pidgin’s peak popularity has long passed, it remains widely used among tech enthusiasts and open-source software advocates.

The infected ss-otr (ScreenShareOTR) plug-in was designed for screen sharing over the Off-The-Record (OTR) protocol — a cryptographic protocol for secure instant messaging. This means the attackers specifically targeted users who prioritize privacy and secure communication.

## September 2024: hijacking deleted projects on PyPI

In September, researchers published a study exploring the theoretical possibility of hijacking deleted PyPI projects — or rather, their names. The issue arises because after a package is deleted, nothing prevents anyone from creating a new project with the same name. As a result, developers who request updates for the deleted package end up downloading a fake, malicious version instead.

PyPI is aware of this risk, and issues a warning when you try to delete a project:

![PyPI warning when deleting a project](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/02/04125039/supply-chain-attacks-in-2024-9.jpg)

When a project is deleted, PyPI alerts its current owner about the potential consequences. Source

In total, the researchers found over 22 000 PyPI projects vulnerable to this attack. Moreover, they discovered that the threat is not just theoretical — this attack method was already observed “in the wild”.

To protect some of the most obvious high-risk targets, the researchers registered the names of certain popular deleted projects under a secure account they created.

## October 2024: malicious script in the LottieFiles Lottie-Player

In late October, a supply-chain attack targeted the LottieFiles Lottie-Player, a JSON-based library for playing lightweight animations used in mobile and web applications. The attackers simultaneously published multiple versions of Lottie-Player (2.0.5, 2.0.6, and 2.0.7) containing malicious code. As a result, a cryptodrainer appeared on sites thar used this library.

At least one major crypto-theft has been confirmed, with the victim losing nearly 10 bitcoins (over US$700 000 at the time of the incident).

## November 2024: JarkaStealer found in the PyPI repository

In November, our experts from the Global Research and Analysis Team (GReAT) discovered two malicious packages in the PyPI repository: claudeai-eng and gptplus. These packages had been available on PyPI for over a year — downloaded over 1700 times by users across 30+ countries.

The packages posed as libraries for interacting with popular AI chatbots. However, in reality, claudeai-eng and gptplus only imitated their declared functions using a demo version of ChatGPT. Their real purpose was to install the JarkaStealer malware.

As you might guess from the name, this is an infostealer. It steals passwords and saves browser data, extracts session tokens from popular apps (Telegram, Discord, Steam), gathers system information, and takes screenshots.

## December 2024: infected Ultralytics YOLO11 AI model in PyPI

In December, another AI-themed supply-chain attack was carried out via the PyPI repository. This time, the attack targeted the popular package, Ultralytics YOLO11 (You Only Look Once) — an advanced AI model for real-time object recognition in video streams.

Users who installed the Ultralytics YOLO11 library, whether directly or as a dependency, also unknowingly installed the cryptominer XMRig Miner.

## How to protect against supply-chain attacks

For detailed recommendations on preventing supply-chain attacks, check out our dedicated guide. Here are the main tips:

- Always carefully review any code used in your projects.
- Maintain a Software Bill of Materials (SBOM) to track dependencies and components.
- Monitor suspicious activity in your corporate network using an XDR-class security solution.
- If your internal security team lacks sufficient resources, consider using an external service for timely threat detection and response.

Go to Source
