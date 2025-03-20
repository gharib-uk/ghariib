---
title: "USN-7363-1 PAM-PKCS11 vulnerabilities"
date: Thu, 20 Mar 2025 18:43:56 +0000
draft: false
type: posts
categories: 
- 
---
# USN-7363-1 PAM-PKCS11 vulnerabilities

<br/>

<br/>
Marcus Rückert and Matthias Gerstner discovered that PAM-PKCS#11 did not properly handle certain return codes when authentication was not possible. An attacker could possibly use this issue to bypass authentication. This issue only affected Ubuntu 24.04 LTS and Ubuntu 24.10. (CVE-2025-24531) It was discovered that PAM-PKCS#11 did not require a private key signature for authentication by default. An attacker could possibly use this issue to bypass authentication. (CVE-2025-24032)

#### [Source](https://ubuntu.com/security/notices/USN-7363-1)

<br/>
---
