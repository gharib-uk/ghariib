---
title: "New Tools for Addressing Supply Chain Attacks"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "supplychainattack"
---

In the recent codecov.io security incident, an attacker modified a shell script used by a common software development tool for code coverage. This modification did not take place at the original source code repository where it would have been visible to others, but after the code was packaged and placed on the web server from which it was served.

Prompted by this incident, we are now releasing new tools that provide information and detection of similar attacks against other projects. The scope is limited to attacks that modify the released code and do not touch the original source code. The following tools are now available:

- **dont\_curl\_and\_bash** – a list of projects that are installed directly off the Internet via a “curl | bash” manner along with possible mitigations
- **icetrust** – a tool that can be used to verify a downloaded artifact such as a shell script before it is executed via methods such as checksums and PGP signatures
- **release\_auditor** – a tool that can be used to check if a GitHub release was modified after initial publication (see our earlier blog post for additional information)

Additionally, we are also providing two examples of how a continuous monitoring dashboard can be setup to detect such attacks by project maintainers. The following are available (screenshots below):

- Simple one page dashboard (GitHub repo)
- Uptime / status-page like dashboard (GitHub repo)

![](https://wwws.nightwatchcybersecurity.com/wp-content/uploads/2021/05/screen-shot-2021-05-09-at-5.35.25-pm.png?w=1024)

![](https://wwws.nightwatchcybersecurity.com/wp-content/uploads/2021/05/screen-shot-2021-05-09-at-5.35.43-pm.png?w=803)

Go to Source
