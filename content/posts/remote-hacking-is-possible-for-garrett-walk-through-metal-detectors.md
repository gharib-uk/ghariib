---
title: "Remote hacking is possible for Garrett walk-through metal detectors."
date: 2025-01-08
categories: 
  - "cybercrime"
  - "cybermaniac"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security-awareness"
  - "threat-intelligence"
tags: 
  - "cyber-crime-news"
  - "cyberalerts"
  - "cybercrimenews"
  - "hacking"
  - "metaldetectors"
  - "remote"
  - "vulnerability"
---

A network component in Garrett Metal Detectors contains a number of security flaws that could allow remote attackers to bypass authentication requirements, tamper with metal detector configurations, and even execute arbitrary code.

In a disclosure released last week, Cisco Talos noted that an attacker could use this module to remotely monitor statistics on a metal detector, such as whether the alarm has been triggered or how many visitors passed through. “They could also alter device configurations, such as changing the sensitivity level, potentially posing a security risk to users of these metal detectors.”

Matt Wiseman, a security researcher at Talos, has been credited with discovering and reporting these vulnerabilities on August 17, 2021. A patch was released by the vendor on December 13, 2021.

- Garrett iC Module, which enables walk-through metal detectors like Garrett PD 6500i and Garrett MZ 6100 to communicate with a computer through the network, either wired or wirelessly, contains these flaws. Customers can control and monitor their devices in real time from a remote location.
- Below is a list of security vulnerabilities –
- CVE-2021-21901 (CVSS score: 9.8), CVE-2021-21903 (CVSS score: 9.8), CVE-2021-21905, and CVE-2021-21906 (CVSS scores: 8.2) – Stack-based buffer overflow vulnerabilities that can be triggered by sending malicious packets to the device
- Authentication bypass vulnerability stemming from a race condition that can be triggered by sending a sequence of requests (CVE-2021-21902) (CVSS: 7.5)
- CVE-2021-21904 (CVSS score: 9.1), CVE-2021-21907 (CVSS score: 4.9), CVE-2021-21908, and CVE-2021-21909 (CVSS scores: 6.5) – Directory traversal vulnerabilities that could be exploited by sending special commands

A successful exploitation of the aforementioned bugs in iC Module CMA version 5.0 could allow an attacker to hijack an authenticated user’s session, read, write, or delete arbitrary files on the device, and worse, execute remote code.

Users are strongly advised to update to the latest version of the firmware as soon as possible due to the severity of the security vulnerabilities.

The post Remote hacking is possible for Garrett walk-through metal detectors. appeared first on Cyber Crime Awareness Society.

Go to Source
