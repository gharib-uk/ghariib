---
title: "All about the xz-utils backdoor"
date: Fri, 29 Mar 2024 00:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# All about the xz-utils backdoor
https://www.kali.org/blog/about-the-xz-backdoor/images/xz-utils.jpg
<br/>

<br/>
As of 5:00 pm ET on March 29, 2024 the following information is accurate. Should there be updates to this situation, they will be edited onto this blog post.

The [xz-utils package](https://pkg.kali.org/pkg/xz-utils), starting from versions 5.6.0 to 5.6.1, was found to [contain a backdoor (CVE-2024-3094)](https://nvd.nist.gov/vuln/detail/CVE-2024-3094). This backdoor could potentially allow a malicious actor to compromise sshd authentication, granting unauthorized access to the entire system remotely.

With a library this widely used, the severity of this vulnerability poses a threat to the entire Linux ecosystem. Luckily, this issue was caught quickly so the impact was significantly less than it could have been. It has already been patched in Debian, and therefore, Kali Linux.

The impact of this vulnerability affected Kali between March 26th to March 29th, during which time [xz-utils 5.6.0-0.2](https://pkg.kali.org/news/578094/xz-utils-560-02-imported-into-kali-rolling/) was available. If you updated your Kali installation on or after March 26th, but before March 29th, it is crucial to apply the latest updates today to address this issue. However, if you did not update your Kali installation before the 26th, you are not affected by this backdoor vulnerability.

[![](https://www.kali.org/blog/about-the-xz-backdoor/images/pkg-kali-xz-utils.png)](https://www.kali.org/blog/about-the-xz-backdoor/images/pkg-kali-xz-utils.png)

Should you wish to check if you have the vulnerable version installed, we can perform the following command:

```
kali@kali:~$ apt-cache policy liblzma5
liblzma5:
 Installed: 5.4.5-0.3
 Candidate: 5.6.1+really5.4.5-1
 Version table:
    5.6.1+really5.4.5-1 500
       500 http://kali.download/kali kali-rolling/main amd64 Packages
*** 5.4.5-0.3 100
       100 /var/lib/dpkg/status
```

If we see the version `5.6.0-0.2` next to **Installed:** then we _must_ upgrade to the latest version, `5.6.1+really5.4.5-1`. We can do this with the following commands:

```
kali@kali:~$ sudo apt update && sudo apt install -y --only-upgrade liblzma5
...
kali@kali:~$
```

More information can be found at [Help Net Security](https://www.helpnetsecurity.com/2024/03/29/cve-2024-3094-linux-backdoor/) for a summarized post on the details of the vulnerability, [Openwall](https://www.openwall.com/lists/oss-security/2024/03/29/4) for the initial disclosure, and [NIST’s NVD](https://nvd.nist.gov/vuln/detail/CVE-2024-3094) entry for this vulnerability.

#### [Source](https://www.kali.org/blog/about-the-xz-backdoor/)

<br/>
---
