---
title: "Introducing VulnCheck NVD - Blog - VulnCheck"
date: 2024-03-14T00:00:00.000Z
draft: false
type: posts
categories: 
- 
---
# Introducing VulnCheck NVD - Blog - VulnCheck

<br/>

<br/>
!\[NVD Gap\](/blog/nvd-plus-plus/nvd-gap.png){:width="100%"}\\n\\nGiven the security community's ongoing concerns about the reliability, rate limits, and performance of NIST's National Vulnerability Database (NVD) 2.0 API, we recognized a growing need to address these challenges.\\n\\n## Top Challenges with NIST NVD:\\n\\n\*\*Reliability:\*\* Users often encounter issues with the NIST NVD API finding it slow and often unresponsive, leading to delays in accessing critical CVE data. This necessitates troubleshooting or waiting for resolution.\\n\\n::author-quote\\n---\\nauthor: Enterprise Security Architect\\nposition: \\n---\\nI've had to code delays into my scripts to handle timeouts and 503 errors.\\n::\\n\\n\*\*Rate Limits:\*\* The NIST NVD API imposes strict rate limits on queries, requiring users to wait between requests, which significantly extends downloads and processing times.\\n\\n!\[NIST NVD Rate Limits\](/blog/nvd-plus-plus/nvd-rate-limits.png){:width="100%"}\\n(Source: NIST NVD)\\n\\n::author-quote\\n---\\nauthor: Frustrated Security Researcher\\nposition: \\n---\\nThey require a 6-second delay between requests, causing unnecessary delays in my scripts.\\n::\\n\\n\*\*Lack of Recent Enrichment:\*\* The NVD’s pause of CVE data enrichment affects the incorporation of new CVEs into CPE, NIST CWE, and NIST CVSS scoring systems.\\n\\n## Addressing the Top Challenges with NIST NVD:\\n\\nTo address these issues, we are excited to introduce VulnCheck NVD++, a free community resource that aims to mitigate the limitations of the NIST NVD API.\\n\\n!\[NVD Gap\](/blog/nvd-plus-plus/vulncheck-nvd-2.png){:width="100%"}\\n\\n## How VulnCheck NVD++ Resolves The Limitations of the NIST NVD API?\\n\\nVulnCheck NVD++ solves for the challenges of using the NIST NVD API by providing reliable access to NVD 1.0 and 2.0 through NVD++ that is available at machine speeds as a community service. We are delivering this solution to offload some of the burden that NIST NVD faces, ensuring more accessible and timely access to this critical infrastructure.\\n\\n\*\*Enhanced Reliability:\*\* VulnCheck APIs are engineered for high availability, performance and stability, ensuring stable and reliable access to NIST NVD data. Users can retrieve NVD data without encountering reliability issues, alleviating the need for tuning and troubleshooting.\\n\\n\*\*Elimination of API Rate Limits:\*\* VulnCheck API removes the barriers of archaic rate limits, enabling users to access NVD data at their preferred speed without restrictions or introducing artificial delays.\\n\\n\*\*NVD Data Enrichment:\*\* While the NVD has paused CVE data enrichment, we are actively exploring solutions to help fill this enrichment gap and will have some exciting new community updates coming very soon... So stayed tuned!\\n\\nVulnCheck NVD++ provides the security community with a dependable alternative for maintaining a persistent connection to NIST NVD CVE data, empowering users with seamless and reliable access to this critical public resource.\\n\\nTo learn more about VulnCheck NVD++, please visit: https://vulncheck.com/nvd2\\n\\n\\n## About VulnCheck\\n\\nAre you interested in exploring threat actors? Do you want to track the vulnerabilities they are exploiting in the wild? If so, VulnCheck's \[Exploit & Vulnerability Intelligence\](https://vulncheck.com/product/exploit-intelligence) has broad threat actor coverage. Register and demo our data today.\\n

#### [Source](https://vulncheck.com/blog/nvd-plus-plus)

<br/>
---
