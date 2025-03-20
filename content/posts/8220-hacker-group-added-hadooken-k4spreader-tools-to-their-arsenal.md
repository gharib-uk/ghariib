---
title: "<div>8220 Hacker Group Added Hadooken & K4Spreader Tools To Their Arsenal</div>"
date: 2025-01-02
categories: 
  - "security"
---

The 8220 hacker group is known for targeting both Windows and Linux web servers by deploying “crypto-jacking” malware to exploit vulnerabilities.

This group reportedly has access to the source code of these OSs, which enhances their ability to carry out attacks via brute force and RCE.

Cybersecurity researchers at Sekoia recently discovered that the 8220 hacker group recently added two important tools to their arsenal, and the tools are “Hadooken” and “K4Spreader.”

8220 hacker group is a Chinese threat actor known since 2018 for targeting cloud environments to deploy cryptomining malware.

## **Hadooken & K4Spreader Tools**

On September 17, 2024, Sekoia’s Threat Detection & Research team identified a sophisticated cyber attack targeting both Windows and Linux systems through an Oracle WebLogic honeypot.

The attackers, believed to be the 8220 Gang, exploited two critical vulnerabilities:-

- CVE-2017-10271

- CVE-2020-14883

These vulnerabilities allowed remote code execution without authentication.

The infection chain involved deploying Python and Bash scripts to execute the K4Spreader malware, which subsequently delivered the Tsunami backdoor and a cryptominer.

For Windows systems, the attacker attempted to run a PowerShell script designed to install a cryptominer via a .NET-based loader called CCleaner64.exe.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEigt0-PKUvrhveXNksafA6ifzyrfEbu3upZzygodlulHkQTzIfCOQWfg3QXzwI7c3YUOdU3UozhnXBNAQqD0Qd60R_8Xli2nzvtTTXvR9oPcLBPuoLVcZC3Inf4nefgsQLpTkNNJaPraKV5qWG8764dGM7ERyK2wDuf96YYWI5xnLcB9NrOAXZyY1gYgGI_/s1536/Windows%20infection%20routine%20%28Source%20-%20Sekoia%29.webp)

<figcaption>

Windows infection routine (Source – Sekoia)

</figcaption>

</figure>

The Linux infection utilized scripts named “c” and “y” to deploy the Hadooken malware, disable cloud protection tools, and attempt lateral movement through SSH brute-force attacks.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgAAqAjF99goBmmW91CsdHvlJnHd96v8n8dt5wHfjz5w58Tos1S-b0v58WNi59_QvFPw9eLeGnz573YUrusH65wWXJlC0o8zyDDBiJwl7Z8yaRll4Ndkl565Zg1Zkc3r9DrIHa7sD_rJRG3eJnz8BgxWptpljiqZJd5seHdMJXFw5K4OY6GBJnr3iUBDD2D/s1536/Linux%20infection%20routine%20%28Source%20-%20Sekoia%29.webp)

<figcaption>

Linux infection routine (Source – Sekoia)

</figcaption>

</figure>

Both Windows and Linux infections resulted in the installation of PwnRig, a variant of the XMRig Monero cryptocurrency miner.

The attackers used a specific Monero wallet (“46E9UkTFqALXNh2mSbA7WGDoa2i6h4WVgUgPVdT9ZdtweLRvAhWmbvuY1dhEmfjHbsavKXo3eGf5ZRb4qJzFXLVHGYH4moQ”) and leveraged private mining pools with IP addresses like “51.222.111.116:80.”

The Tsunami malware is a Linux-based DDoS bot that used “Internet Relay Chat (IRC)” for C2, with servers at domains like “c4k-ircd\[.\]pwndns\[.\]pw” and “play\[.\]sck-dns\[.\]cc.”

This attack shared numerous similarities with a case reported by AquaSec on September 12, 2024, which includes the “common TTPs” and “IoCs,” strongly suggesting the involvement of the 8220 Gang.

The infection chain shares significant parallels with the “Hadooken” case, indicating a common attacker, likely the “8220 Gang.”

Both target “WebLogic servers” using similar scripts (“c” and “y”) and the “lwp-download binary” for initial access.

The malware deploys via these scripts, “c” installs the main payload (‘Hadooken’ or ‘K4Spreader’) and attempts “SSH propagation,” while “y” drops “central malware.”

Both attacks deploy “PwnRig cryptominer” and “Tsunami botnet,” establishing persistence through ‘crontab’ to download the “c” script from “hxxp://sck-dns\[.\]cc/c”.

The “PwnRig” payload uses identical hashes, proxies (run.on-demand\[.\]pw), and Monero wallet across cases.

While the analysis suggests ‘Hadooken’ and ‘K4Spreader’ are distinct but related to “Go-based” malware.⁤

Victim monitoring via the “Tsunami IRC” channel revealed “200-250” compromised machines, primarily on cloud services like “Oracle Cloud.” ⁤

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDyQS6oFpj27dLqPmJJom4A8qSzb3DP7Cw80F0kxW_LRFaRWV61QHOPSvK8ECryx1sR7HXW0hvBXRC0VDownx8RZHHWHaZTiCx1Mt0P9bGxU9_LM0c30ay3grLXlUK85HQTAj8aUtRiBZpnT8e98q41r_6fO8yGkwQocyGM4XK-f_imTV1AdFt4aXuSNIm/s1021/IPs%20infected%20by%20Tsunami%20%28Source%20-%20Sekoia%29.webp)

<figcaption>

IPs infected by Tsunami (Source – Sekoia)

</figcaption>

</figure>

Besides this, the “⁤Shodan scans” showed some affected IPs host vulnerable software like “Drupal” or “Apache Struts.” ⁤⁤

Geographically, victims cluster in Asia and South America, particularly Brazil seems to be impacted significantly due to “Portuguese function names” in the code of “K4Spreader.”

⁤⁤This victimology aligns with the Chinese 8220 Gang’s known activities with a Brazilian operator expansion.

Free Webinar on How to Protect Small Businesses Against Advanced Cyberthreats -> **Free Webinar**

The post 8220 Hacker Group Added Hadooken & K4Spreader Tools To Their Arsenal appeared first on Cyber Security News.

Go to Source
