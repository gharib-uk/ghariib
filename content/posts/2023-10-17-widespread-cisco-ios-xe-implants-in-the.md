---
title: "Widespread Cisco IOS XE Implants in the Wild - Blog - VulnCheck"
date: 2023-10-17T00:00:00.000Z
draft: false
type: posts
categories: 
- 
---
# Widespread Cisco IOS XE Implants in the Wild - Blog - VulnCheck

<br/>

<br/>
::check-list\\n---\\ntitle: Key Takeaways\\nico: mdi:check-bold\\nlist:\\n - CVE-2023-20198 appears to have been widely exploited to install implants on Cisco IOS XE systems.\\n - VulnCheck performed an internet scan and found thousands of implanted hosts.\\n - VulnCheck released a scanner to detect the implant on affected devices.\\n---\\n::\\n\\nOn October 16, 2023 Cisco \[disclosed\](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-iosxe-webui-privesc-j22SaA4z) an authentication bypass, \[CVE-2023-20198\](https://nvd.nist.gov/vuln/detail/CVE-2023-20198), affecting Cisco IOS XE. The disclosure reported that the vulnerability had been exploited in the wild to help install implants on affected switches and routers. Additionally, Cisco shared a simple technique to determine if an IOS XE device had an active implant on it. The implant responds with an 18-character hexadecimal string when a specific HTTP POST is sent to the system:\\n\\n\`\`\`sh\\n$ curl -X POST http://192.168.1.1/webui/logoutconfirm.html?logon\_hash=1\\n1a80b7389ccd0a5dab\\n\`\`\`\\n\\nCisco buried the lede by not mentioning \*thousands\* of internet-facing IOS XE systems have been implanted. \*\*VulnCheck scanned internet-facing Cisco IOS XE web interfaces and found thousands of implanted hosts\*\*. This is a bad situation, as privileged access on the IOS XE likely allows attackers to monitor network traffic, pivot into protected networks, and perform any number of man-in-the-middle attacks.\\n\\nVulnCheck has \[released\](https://github.com/vulncheck-oss/cisco-ios-xe-implant-scanner) the scanner used to find implanted systems on the internet. \\n\\n\`\`\`\\n$ ./implant-scanner -rhost 192.168.1.1 -rport 80 -a -v -c | grep “implant-id”\\ntime=2023-10-17T05:32:29.522-04:00 level=SUCCESS msg=Found implant-id=1a80b7389ccd0a5dab rhost=2192.168.1.1 rport=80 ssl=false\\n\`\`\`\\n\\nIf your organization uses an IOS XE system, it's imperative that you determine if your systems have been compromised and take appropriate action once implants have been discovered. While a patch is not yet available, you can protect your organization by disabling the web interface and removing all management interfaces from the internet immediately.\\n\\nFor additional guidance, read Cisco PSIRT’s \[advisory\](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-iosxe-webui-privesc-j22SaA4z). Additionally, Cisco Talos wrote an informative blog about discovery of the \[issue\](https://blog.talosintelligence.com/active-exploitation-of-cisco-ios-xe-software/).\\n

#### [Source](https://vulncheck.com/blog/cisco-implants)

<br/>
---
