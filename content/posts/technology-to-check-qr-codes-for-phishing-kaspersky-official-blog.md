---
title: "Technology to check QR codes for phishing | Kaspersky official blog"
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

Kaspersky Secure Mail Gateway now checks links hidden in QR codes – including ones in PDF files.

In an attempt to bypass security solutions, attackers are increasingly hiding phishing and other malicious links inside QR codes. It’s for this reason that we’ve added a technology to Kaspersky Secure Mail Gateway that reads QR codes (including ones hidden inside PDF files), extracts links and checks them before they land in an employee’s inbox. We explain how it works.

![Example of a phishing QR code inside a PDF file](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/10101048/QR-phishing-protection-technology-1-pdf-inside.jpg)

Example of a phishing QR code inside a PDF file

## Why do attackers use QR codes?

Ever since even basic security tools learned to check phishing links effectively enough, attackers have been inventing ways to hide them from scanners. The most commonly employed trick is to post links on third-party services; that way, victims don’t receive an email directly from the attackers, but a notification from some legitimate site where a document with a malicious link is already placed. While such ploys work well on home users, with company employees the success rate is far lower. That’s because any self-respecting organization these days has equipped all its work computers with security software that catches redirects to phishing sites.

Therefore, attackers have turned their attention to QR codes. First, this technology obligingly transforms regular URLs into something incomprehensible to standard systems that check links for malicious intent. Second, QR codes are common enough for people to scan them without a second thought. And third and most important, people overwhelmingly scan QR codes with a phone or tablet that may not have a security solution with anti-phishing technology – especially if it’s a personal, not work, device.

Plus, in this case, less suspicion is raised by the prompt to enter work credentials, which are what the attackers basically want. On a computer, the user is likely to be signed in already, but accessing work systems from a personal device requires additional authentication, right?

![The goal of most phishing schemes is to extract work credentials](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/10101221/QR-phishing-protection-technology-2-ms-login.jpg)

The goal of most phishing schemes is to extract work credentials

## Why are QR codes most often hidden in PDF files?

Sure, a QR code can also be sent in the body of an email. But hardly anyone will follow a QR code without a few words explaining why they should, and this text can be analyzed and flagged as phishing. Besides, an image has certain characteristics – at least its dimensions – by which it can be identified.

![Phishing QR code in an image in the body of an email](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/10101704/QR-phishing-protection-technology-3-pdf-image.jpg)

A PDF file, on the other hand, is a kind of black box. The format is proprietary – you can’t peek inside without special tools. In addition, the cover email can contain minimal text, something like: “Important! All information in the PDF”

![Phishing email with a PDF file and minimal accompanying information](https://media.kasperskydaily.com/wp-content/uploads/sites/92/2025/03/10101749/QR-phishing-protection-technology-4-pdf-attachment.jpg)

Phishing email with a PDF file and minimal accompanying information

## How does our technology work?

Of course, a QR code in an email isn’t always a sign of phishing. For example, mobile application developers often furnish their PDF documents and mailings with direct links to app stores. In general, it’s a quick and easy way to send a link to a phone. That’s why we can’t mark each email with a QR-code as a suspicious. So our developers created a tool to extract URLs from QR codes for additional checking by anti-phishing modules and anti-spam heuristics.

Not only can the technology extract URLs from QR codes in images, but also check PDF files – extracting all links from all QR codes found inside them. If a link is recognized as phishing, the email is also flagged as phishing and processed in accordance with the Kaspersky Secure Mail Gateway settings. So the end user never even sees the dangerous QR code. The best outcome!

Go to Source
