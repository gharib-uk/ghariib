---
title: "VulnCheck Initial Access Intelligence Update - July 2024"
date: 2024-08-01T00:00:00.000Z
draft: false
type: posts
categories: 
- 
---
# VulnCheck Initial Access Intelligence Update - July 2024

<br/>

<br/>
VulnCheck Initial Access Intelligence equips organizations and security teams with detection artifacts such as Suricata signatures, YARA rules, PCAPs, and private exploit PoCs to defend against initial access vulnerabilities that are either already being exploited or likely to be exploited soon.\\n\\nBefore we get into this months details, it's worth mentioned that go-exploit, VulnCheck's exploit framework, now supports scanless asset detection and version scanning, using the exact same code for active scanning. You can learn more about that \[here\](/blog/vulncheck-goes-scanless).\\n\\nIn July 2024, VulnCheck crossed 250+ Initial Access Intelligence (IAI) artifacts, developing artifacts for 14 CVEs, covering 13 different vendors and 10 different products.\\n\\n!\[Initial Access Intelligence - July 2024\](/blog/initial-access-intelligence-july-2024/vulncheck-initial-access-july.png){:width="100%"}\\n\\nTo provide better visibility into these updates, we’ve broken down July’s Initial Access Intelligence Artifacts by CVE. For each CVE, we provide a range of detection tools including: \\n\* Exploits\\n\* Version scanners\\n\* PCAPs\\n\* Suricata rules\\n\* Snort rules\\n\* YARA rules\\n\* Greynoise/Censys/Shodan queries\\n\\n## July 2024 Initial Access Artifacts\\n\\n| Artifact Name | Date Added | CVE | Exploit | Version Scanner | pcap | Suricata Rule | snortRule | yara |\\n| ----------- | ----------- | ----------- |----------- | ----------- | ----------- |----------- | ----------- | ----------- |\\n| Zyxel Customer-Provided Equipment Configuration Disclosure | 2024-07-04 | CVE-2023-28770 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n| Apache Superset Session Forgery | 2024-07-05 | CVE-2023-27524 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |\\n| GeoServer Remote Code Execution | 2024-07-05 | CVE-2024-36401 | ✅ | | ✅ | ✅ | ✅ | ✅ |\\n| Progress WhatsUp Gold Path Traversal | 2024-07-12 | CVE-2024-4885 | ✅ | | ✅ | ✅ | ✅ | ✅ |\\n| Zyxel CPE Diag Command Injection | 2024-07-12 | CVE-2024-40890 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n| Zyxel CPE Telnet Command Injection | 2024-07-12 | CVE-2024-40891 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n| Apache CloudStack Unsecured cluster API remote code execution | 2024-07-15 | CVE-2024-38346 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |\\n| Laravel Credential leak in log files | 2024-07-17 | CVE-2024-29291 | ✅ | | ✅ | ✅ | ✅ | ✅ |\\n| Zyxel Auth Bypass and pkg\_init\_cmd Command Injection | 2024-07-19 | CVE-2023-4473 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n| Magento XXE Information Disclosure | 2024-07-21 | CVE-2024-34102 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |\\n| H3C ERHMG2 Configuration/Password Leak | 2024-07-22 | CVE-2024-32238 | ✅ | | ✅ | ✅ | ✅ | |\\n| Elementor Essential Addons WordPress Plugin Authentication Bypass Remote Code Execution | 2024-07-25 | CVE-2023-32243 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n| Ghostscript Filesystem Format String RCE | 2024-07-30 | CVE-2024-29510 | ✅ | | | | | ✅ |\\n| AJ-Report unauthenticated path-traversal Java evaluation RCE | 2024-07-31 | CVE-2024-7314 | ✅ | ✅ | ✅ | ✅ | ✅ | |\\n\\n## Learn More About VulnCheck Initial Access Intelligence\\n\\nLearn more about how you can leverage Initial Access Intelligence detection artifacts to detect & respond to remote code execution (RCE) vulnerabilities here: https://docs.vulncheck.com/products/initial-access-intelligence/introduction\\n

#### [Source](https://vulncheck.com/blog/initial-access-intelligence-july-2024)

<br/>
---
