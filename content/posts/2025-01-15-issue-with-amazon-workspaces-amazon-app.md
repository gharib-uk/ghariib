---
title: "Issue with Amazon WorkSpaces Amazon AppStream 20 and Amazon DCV CVE-2025-0500 and CVE-2025-0501"
date: Wed, 15 Jan 2025 18:29:49 +0000
draft: false
type: posts
categories: 
- 
---
# Issue with Amazon WorkSpaces Amazon AppStream 20 and Amazon DCV CVE-2025-0500 and CVE-2025-0501

<br/>

<br/>
**Publication Date: 2025/01/15 10:30AM PST**

**Description:**

AWS identified two issues in specific versions of native clients for Amazon WorkSpaces, Amazon AppStream 2.0, and Amazon DCV. We have proactively communicated with customers regarding the end of support for these impacted versions.

**[CVE-2025-0500](https://www.cve.org/CVERecord?id=CVE-2025-0500):**

This issue applies to specific versions of native clients for Amazon WorkSpaces (when running Amazon DCV protocol), Amazon AppStream 2.0, and Amazon DCV, listed below. If leveraged, this issue could allow a bad actor to perform a man-in-the-middle attack, allowing them to access remote WorkSpaces, AppStream, or DCV sessions. We recommend customers upgrade to the versions with the fix to address this issue.

**Affected versions:**

-   Amazon WorkSpaces Windows client 5.20.0 or earlier, macOS client 5.20.0 or earlier, and Linux client 2024.1 or earlier.
-   Amazon AppStream 2.0 Windows client 1.1.1326 or earlier.
-   Amazon DCV Windows client 2023.1.8993 or earlier, macOS client 2023.1.6203 or earlier, and Linux client 2023.1.6203 or earlier for all supported Linux distributions.

**Resolution:**

This issue was fixed in specific versions of the Amazon WorkSpaces, Amazon AppStream 2.0, and Amazon DCV clients listed below. Upgrading to these or later versions remediates the issue.

**Versions with the fix:**

-   Amazon WorkSpaces Windows client 5.21.0 or later, macOS client 5.21.0 or later, and Linux client 2024.2 or later.
-   Amazon AppStream 2.0 Windows client 1.1.1332 or later.
-   Amazon DCV Windows client 2023.1.9127 or later, macOS client 2023.1.6703 or later, and Linux client 2023.1.6703 or later for all supported Linux distributions.

**[CVE-2025-0501](https://www.cve.org/CVERecord?id=CVE-2025-0501):**

The issue applies to specific versions of native clients for Amazon WorkSpaces (when running PCoIP protocol), listed below. If leveraged, this issue could allow a bad actor to perform a man-in-the-middle attack, allowing them to access remote WorkSpaces sessions. We recommend customers upgrade to the versions with the fix to address this issue.

**Affected versions:**

-   Amazon WorkSpaces Windows client 5.22.0 or earlier, macOS client 5.22.0 or earlier, Linux client 2024.5 or earlier, and Android client 5.0.0 or earlier.

**Resolution:**

This issue was fixed in specific versions of the Amazon WorkSpaces clients listed below. Upgrading to these or later versions remediates the issue.

**Versions with the fix:**

-   Amazon WorkSpaces Windows client 5.22.1 or later, macOS client 5.22.1 or later, Linux client 2024.6 or later, and Android client 5.0.1 or later.

**References:**

-   [CVE-2025-0500](https://www.cve.org/CVERecord?id=CVE-2025-0500)
-   [CVE-2025-0501](https://www.cve.org/CVERecord?id=CVE-2025-0501)

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2025-001/)

<br/>
---
