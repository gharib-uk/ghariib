---
title: "Issue with Temporary elevated access management TEAM - CVE-2025-1969"
date: Tue, 04 Mar 2025 19:05:32 +0000
draft: false
type: posts
---
# Issue with Temporary elevated access management TEAM - CVE-2025-1969





Publication Date:&nbsp;2025/03/04 10:30 AM PST Description Improper&nbsp;request input validation in Temporary Elevated Access Management (TEAM) for AWS IAM Identity Center allows a user to modify a valid request and spoof an approval in TEAM. We recommend customers upgrade TEAM to the latest release, version 1.2.2. Affected versions: &lt;1.2.2 

**Publication Date: 2025/03/04 10:30 AM PST  
**

**Description**

Improper request input validation in Temporary Elevated Access Management (TEAM) for AWS IAM Identity Center allows a user to modify a valid request and spoof an approval in TEAM. We recommend customers upgrade TEAM to the latest release, version [1.2.2](https://github.com/aws-samples/iam-identity-center-team/releases/tag/v1.2.2).

**Affected versions:** <1.2.2  

**Resolution**

A fix has been released in version [1.2.2](https://github.com/aws-samples/iam-identity-center-team/releases/tag/v1.2.2).

Please refer to the "[Update TEAM solution](https://aws-samples.github.io/iam-identity-center-team/docs/deployment/update.html#if-upgrading-to-v111-custom-domain)" documentation for instructions on upgrading.

**References**

-   [GHSA-x9xv-r58p-qh86](https://github.com/aws-samples/iam-identity-center-team/security/advisories/GHSA-x9xv-r58p-qh86)
-   [CVE-2025-1969](https://www.cve.org/CVERecord?id=CVE-2025-1969)

**Acknowledgement**

We would like to thank Redshift Cyber Security for collaborating on this issue through the coordinated vulnerability disclosure process.

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2025-004/)

