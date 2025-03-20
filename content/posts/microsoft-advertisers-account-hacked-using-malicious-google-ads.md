---
title: "Microsoft Advertisers Account Hacked Using Malicious Google Ads"
date: 2025-02-03
---

A new phishing campaign targeting Microsoft advertisers has been uncovered, leveraging malicious Google Ads to steal credentials.

This attack follows a similar campaign targeting Google Ads accounts, illustrating the ongoing threat of malvertising in the digital advertising ecosystem.

The attackers used Google’s sponsored search results to impersonate Microsoft Ads (formerly Bing Ads). These fraudulent ads appeared when users searched for “Microsoft Ads” on Google, redirecting them to phishing sites that mimicked legitimate Microsoft login pages.

Microsoft, which generated $12.2 billion in search and news advertising revenue in 2023, competes with Google for search engine traffic. 

However, the open nature of online advertising networks has enabled fraudulent actors to exploit this competition by evading Google’s security procedures.

## **Targeting Via Malicious Google Ads**

Threat actors employed advanced techniques such as redirection and cloaking to evade detection. When unwanted IP addresses—such as those from VPNs, bots, or security scanners—attempted to access the malicious ads, they were redirected to harmless “white pages.”

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc1pREYejNA84D8VwflZaESozDAzaKLil7XSO6xQjotLzJxpJTTZ9h_9cRqva7rauxjl-pSJFEg_At-4FfxDz9yj59SU1zloDlpHewqa-6pnCCF6_LY7INnQ7y_bvHp3ViB8qiDRQ?key=vPM7BxPVsWdlslqXeGjM9QEY)

<figcaption>

Google search for ‘microsoft ads’

</figcaption>

</figure>

Genuine users were subjected to a Cloudflare challenge to verify their authenticity before being redirected to phishing pages.  
  
The final phishing page mimicked Microsoft’s legitimate advertising platform (ads.microsoft.com) but was hosted on a fake domain (e.g., ads\[.\]mcrosoftt\[.\]com). 

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf68eePSyzmZMUSOPl0O9UmklSpWTNjeZ0NbKVRAjxTdIzTr7qm45E3AxBhV6PxwNXJW6J0VPDx7WxLZb5vIFQiWmKakSO1qofiAzuNYDRAFJK7OgUlQmVtIHtKQiuwK6oA18OEhg?key=vPM7BxPVsWdlslqXeGjM9QEY)

<figcaption>

Phishing Page

</figcaption>

</figure>

Victims were presented with a fake error message prompting them to reset their passwords. The phishing kit also attempted to bypass two-factor authentication (2FA), a common feature in modern phishing campaigns.

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfLJ2jF9iUZPa3Y6TvUbEom-wFFFaxPM9VagXMZD3ZSdj-bG5mmJhHG6Pf5SIWCuVOeCEjfRhcA3B6tv3OO8K4vFLxOdfV5u4Knv_ioyw8QIIfg7kciooQE9BgTXaJLSCJhYOAlnw?key=vPM7BxPVsWdlslqXeGjM9QEY)

<figcaption>

Steps of phishing

</figcaption>

</figure>

Users were met with a “rickroll,” an internet prank intended to make fun of visitors, if they went to the malicious domain directly instead of via the ad click. For the threat actors, this was an extra layer of obfuscation.

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdnlraM2cURq93-hTnqUP1rkb0DmZmGEp42BXcbD5m6-5ADXZR0u_HwFIGgEEHsIcHHxEiD1fnYl_VZgguljU_A1DS5zHQzyPx7xtdSBypV0TzKBCz19ZLN0tTcealGaMKmRJ9QHw?key=vPM7BxPVsWdlslqXeGjM9QEY)

<figcaption>

Rickroll redirect

</figcaption>

</figure>

According to MalwareBytes Report, this effort is a campaign of a larger scheme that has been targeting Microsoft accounts for a number of years. Further malicious domains have been identified by examining artifacts such favicon\[.\]ico files and URL patterns; some of these domains were hosted in Brazil or used Brazilian top-level domains (.com.br).

The scale of this operation suggests it may extend beyond Microsoft and Google accounts, potentially affecting other platforms like Facebook. This underscores the widespread vulnerabilities within online advertising ecosystems.

To mitigate risks from such attacks, users are advised to follow these best practices:

- **Verify URLs:** Always check for inconsistencies or misspellings in URLs before entering credentials.

- **Enable 2FA:** While two-factor authentication adds an extra layer of security, remain cautious about granting access requests.

- **Monitor Accounts:** Regularly review your advertising accounts for suspicious activity, such as unauthorized changes.

- **Report Suspicious Ads:** Reporting fraudulent ads helps protect other users and improves platform security.  
      
    Google has been notified about these malicious ads and is working to address the issue. 

This incident highlights the ongoing threat of phishing through online advertising. As tech companies strive to enhance security measures, user vigilance remains critical in combating these sophisticated attacks.

**``**`**Are you from SOC/DFIR Teams? – Analyse Malware Files & Links with ANY.RUN Sandox -> Start Now for Free.**`**``**

The post Microsoft Advertisers Account Hacked Using Malicious Google Ads appeared first on Cyber Security News.

Go to Source
