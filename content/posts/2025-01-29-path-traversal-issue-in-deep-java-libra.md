---
title: "Path traversal issue in Deep Java Library - CVE-2025-0851"
date: Wed, 29 Jan 2025 21:31:19 +0000
draft: false
type: posts
---
# Path traversal issue in Deep Java Library - CVE-2025-0851





Publication Date:&nbsp;2025/01/29 1:30 PM PST AWS identified CVE-2025-0851, a path traversal issue in ZipUtils.unzip and TarUtils.untar in Deep Java Library (DJL) on all platforms that allows a bad actor to write files to arbitrary locations. If leveraged, an actor could gain SSH access by injecting an SSH key into

**Publication Date: 2025/01/29 1:30 PM PST  
**

AWS identified [CVE-2025-0851](https://www.cve.org/CVERecord?id=CVE-2025-0851), a path traversal issue in ZipUtils.unzip and TarUtils.untar in Deep Java Library (DJL) on all platforms that allows a bad actor to write files to arbitrary locations. If leveraged, an actor could gain SSH access by injecting an SSH key into the authorized\_keys file, or upload HTML files to leverage cross-site scripting issues. We can confirm that this issue has not been leveraged. A fix for this issue has been released and we recommend the users of [DJL](https://github.com/deepjavalibrary/djl) upgrade to version 0.31.1 or later.

**Affected versions:** 0.1.0 - 0.31.0

**Resolution**

The patches are included in DJL 0.31.1.

**Reference**

-   [CVE-2025-0851](https://www.cve.org/CVERecord?id=CVE-2025-0851)
-   [GHSA-6h2x-4gjf-jc5w](https://github.com/deepjavalibrary/djl/security/advisories/GHSA-jcrp-x7w3-ffmg)

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2025-003/)

