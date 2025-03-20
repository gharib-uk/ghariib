---
title: "USN-7355-1 RestrictedPython vulnerabilities"
date: Tue, 18 Mar 2025 23:59:57 +0000
draft: false
type: posts
categories: 
- 
---
# USN-7355-1 RestrictedPython vulnerabilities

<br/>

<br/>
Nakul Choudhary and Robert Xiao discovered that RestrictedPython did not properly sanitize certain inputs. An attacker could possibly use this issue to execute arbitrary code. This issue only affected Ubuntu 20.04 LTS and Ubuntu 22.04 LTS. (CVE-2023-37271) Abhishek Govindarasu, Ankush Menat and Ward Theunisse discovered that RestrictedPython did not correctly handle certain format strings. An attacker could possibly use this issue to leak sensitive information. This issue only affected Ubuntu 20.04 LTS and Ubuntu 22.04 LTS. (CVE-2023-41039) It was discovered that RestrictedPython did not correctly restrict access to certain fields. An attacker could possibly use this issue to leak sensitive information. (CVE-2024-47532) It was discovered that RestrictedPython contained a type confusion vulnerability. An attacker could possibly use this issue to execute arbitrary code. This issue only affected Ubuntu 24.04 LTS and Ubuntu 24.10. (CVE-2025-22153)

#### [Source](https://ubuntu.com/security/notices/USN-7355-1)

<br/>
---
