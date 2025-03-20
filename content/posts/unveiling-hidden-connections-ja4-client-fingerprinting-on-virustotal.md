---
title: "Unveiling Hidden Connections: JA4 Client Fingerprinting on VirusTotal"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

VirusTotal has incorporated a powerful new tool to fight against malware: **JA4 client fingerprinting**. This feature allows security researchers to track and identify malicious files based on the unique characteristics of their TLS client communications.

  

### JA4: A More Robust Successor to JA3

JA4, developed by FoxIO, represents a significant advancement over the older JA3 fingerprinting method. JA3's effectiveness had been hampered by the increasing use of TLS extension randomization in https clients, which made fingerprints less consistent. JA4 was specifically designed to be resilient to this randomization, resulting in more stable and reliable fingerprints.

  

## Unveiling the Secrets of the Client Hello

JA4 fingerprinting focuses on analyzing the TLS Client Hello packet, which is sent unencrypted from the client to the server at the start of a TLS connection. This packet contains a treasure trove of information that can uniquely identify the client application or its underlying TLS library. Some of the key elements extracted by JA4 include:  

- TLS Version: The version of TLS supported by the client.

- Cipher Suites: The list of cryptographic algorithms the client can use.

- TLS Extensions: Additional features and capabilities supported by the client.

- ALPN (Application-Layer Protocol Negotiation): The application-level protocol, such as HTTP/2 or HTTP/3, that the client wants to use after the TLS handshake.

## JA4 in Action: Pivoting and Hunting on VirusTotal

VirusTotal has integrated JA4 fingerprinting into its platform through the _behavior\_network_ file search modifier. This allows analysts to quickly discover relationships between files based on their JA4 fingerprints.  
  
To find the JA4 value, navigate to the "behavior" section of the desired sample and locate the TLS subsection. In addition to JA4, you might also find JA3 or JA3S there.  
  
Example Search: Let's say you've encountered a suspicious file that exhibits the JA4 fingerprint "t10d070600\_c50f5591e341\_1a3805c3aa63" during VirusTotal's behavioral analysis.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf2W0yOriDhlTfcdaYFcycXRTNf91eU6YLxbQyPU08nNGDRIZpmQjh9BCOOqPFWJXEcAcN2CDq_gXSFk-yRsva-tsxzAuRkstXT5ecjrED_pUiqByz0nXSID1S3LbCJxY3EzEbD50igejIw5NvZFSmGYzCPNQ?key=khJT4uVVpgoqOspYPp6ncg)

You can click on this JA4 to pivot using the search query behavior\_network:t10d070600\_c50f5591e341\_1a3805c3aa63 finding other files with the same fingerprint This search will pivot you to additional samples that share the same JA4 fingerprint, suggesting they might be related. This could indicate that these files are part of the same malware family or share a common developer or simply share a common TLS library.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfzj_7_qZ0Dt65xvK9_pQ0nF-NGwwYqRVakTFtUcOLqTNOOYHN9AoTSoibYwSjvpBQ9dknpEU5kbLM8Tq0ZoYuWMifvRrNqOM2ROvTnviakclIJx0f53Y6f4uHKteSFJ7P46rLJSgBIoPLl27_eqcBybvxOFw?key=khJT4uVVpgoqOspYPp6ncg)

### Wildcard Searches

To broaden your search, you can use wildcards within the JA4 hash. For instance, the search: behaviour\_network:t13d190900\_\*\_97f8aa674fd9  
  
Returns files that match the JA4\_A and JA4\_C components of the JA4 hash while allowing for variations in the middle section, which often corresponds to the cipher suite. This technique is useful for identifying files that might use different ciphers but share other JA4 characteristics.

  

### YARA Hunting Rules: Automating JA4-Based Detection

YARA hunting rules using the "vt" module can be written to automatically detect files based on their JA4 fingerprints. Here's an example of a YARA rule that targets a specific JA4 fingerprint:  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe4qx5VpKCugnjyBSgEaN-nMAwPAvCbLkg8myX81DWuSENowCWIPNa6WPcTMdD-__R598RYFkLS5UF_FBMf4xNdPmcJOGeEQIoJzGkH8oupGkpxIH2D29B9TEoMB5CiGIn6R36obiBxmdE_7RxJUXUD0Ba-?key=khJT4uVVpgoqOspYPp6ncg)

  
This rules will flag any file submitted to VirusTotal that exhibits the matching JA4 fingerprint. The first example only matches "t12d190800\_d83cc789557e\_7af1ed941c26" during behavioral analysis. The second rule will match a regular expression /t10d070600\_.\*\_1a3805c3aa63/, only matching JA4\_A and JA4\_C components, excluding the JA4\_B cipher suite. These fingerprints could be linked to known malware, a suspicious application, or any TLS client behavior that is considered risky by security analysts.

  

  

### A few Interesting JA4 Hash examples

| Description | JA4 | Example SHA256 |
| --- | --- | --- |
| Linux miner, trojan | t12d5908h1\_7bd0586cbef7\_046e095b7c4a | caed9b2d91f5802da4b1844068e7df971d50a11411ff2a792aedce96554539f9 |
| GoLang | t13d190900\_9dc949149365\_97f8aa674fd9 | 00b001f5d30e7a51bf9eced4e41267912353153dcc52605a737a6778aaecfbfb |
| SnakeLogger / Redline | t10d070600\_c50f5591e341\_1a3805c3aa63 | 03461c2a07431aed5ff68bbcf42d7ef82f32190b44ba140befd3f474614b5f3d |

  

  

### JA4: Elevating Threat Hunting on VirusTotal

VirusTotal's adoption of JA4 client fingerprinting will provide users with an invaluable tool for dissecting and tracking TLS client behaviors, leading to enhanced threat hunting, pivoting, and more robust malware identification.  
  
Happy Hunting.  

VirusTotal has incorporated a powerful new tool to fight against malware: **JA4 client fingerprinting**. This feature allows security researchers to track and identify malicious files based on the unique characteristics of their TLS client communications.

  

### JA4: A More Robust Successor to JA3

JA4, developed by FoxIO, represents a significant advancement over the older JA3 fingerprinting method. JA3's effectiveness had been hampered by the increasing use of TLS extension randomization in https clients, which made fingerprints less consistent. JA4 was specifically designed to be resilient to this randomization, resulting in more stable and reliable fingerprints.

  

## Unveiling the Secrets of the Client Hello

JA4 fingerprinting focuses on analyzing the TLS Client Hello packet, which is sent unencrypted from the client to the server at the start of a TLS connection. This packet contains a treasure trove of information that can uniquely identify the client application or its underlying TLS library. Some of the key elements extracted by JA4 include:  

- TLS Version: The version of TLS supported by the client.

- Cipher Suites: The list of cryptographic algorithms the client can use.

- TLS Extensions: Additional features and capabilities supported by the client.

- ALPN (Application-Layer Protocol Negotiation): The application-level protocol, such as HTTP/2 or HTTP/3, that the client wants to use after the TLS handshake.

## JA4 in Action: Pivoting and Hunting on VirusTotal

VirusTotal has integrated JA4 fingerprinting into its platform through the _behavior\_network_ file search modifier. This allows analysts to quickly discover relationships between files based on their JA4 fingerprints.  
  
To find the JA4 value, navigate to the "behavior" section of the desired sample and locate the TLS subsection. In addition to JA4, you might also find JA3 or JA3S there.  
  
Example Search: Let's say you've encountered a suspicious file that exhibits the JA4 fingerprint "t10d070600\_c50f5591e341\_1a3805c3aa63" during VirusTotal's behavioral analysis.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf2W0yOriDhlTfcdaYFcycXRTNf91eU6YLxbQyPU08nNGDRIZpmQjh9BCOOqPFWJXEcAcN2CDq_gXSFk-yRsva-tsxzAuRkstXT5ecjrED_pUiqByz0nXSID1S3LbCJxY3EzEbD50igejIw5NvZFSmGYzCPNQ?key=khJT4uVVpgoqOspYPp6ncg)

You can click on this JA4 to pivot using the search query behavior\_network:t10d070600\_c50f5591e341\_1a3805c3aa63 finding other files with the same fingerprint This search will pivot you to additional samples that share the same JA4 fingerprint, suggesting they might be related. This could indicate that these files are part of the same malware family or share a common developer or simply share a common TLS library.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfzj_7_qZ0Dt65xvK9_pQ0nF-NGwwYqRVakTFtUcOLqTNOOYHN9AoTSoibYwSjvpBQ9dknpEU5kbLM8Tq0ZoYuWMifvRrNqOM2ROvTnviakclIJx0f53Y6f4uHKteSFJ7P46rLJSgBIoPLl27_eqcBybvxOFw?key=khJT4uVVpgoqOspYPp6ncg)

### Wildcard Searches

To broaden your search, you can use wildcards within the JA4 hash. For instance, the search: behaviour\_network:t13d190900\_\*\_97f8aa674fd9  
  
Returns files that match the JA4\_A and JA4\_C components of the JA4 hash while allowing for variations in the middle section, which often corresponds to the cipher suite. This technique is useful for identifying files that might use different ciphers but share other JA4 characteristics.

  

### YARA Hunting Rules: Automating JA4-Based Detection

YARA hunting rules using the "vt" module can be written to automatically detect files based on their JA4 fingerprints. Here's an example of a YARA rule that targets a specific JA4 fingerprint:  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe4qx5VpKCugnjyBSgEaN-nMAwPAvCbLkg8myX81DWuSENowCWIPNa6WPcTMdD-__R598RYFkLS5UF_FBMf4xNdPmcJOGeEQIoJzGkH8oupGkpxIH2D29B9TEoMB5CiGIn6R36obiBxmdE_7RxJUXUD0Ba-?key=khJT4uVVpgoqOspYPp6ncg)

  
This rules will flag any file submitted to VirusTotal that exhibits the matching JA4 fingerprint. The first example only matches "t12d190800\_d83cc789557e\_7af1ed941c26" during behavioral analysis. The second rule will match a regular expression /t10d070600\_.\*\_1a3805c3aa63/, only matching JA4\_A and JA4\_C components, excluding the JA4\_B cipher suite. These fingerprints could be linked to known malware, a suspicious application, or any TLS client behavior that is considered risky by security analysts.

  

  

### A few Interesting JA4 Hash examples

| Description | JA4 | Example SHA256 |
| --- | --- | --- |
| Linux miner, trojan | t12d5908h1\_7bd0586cbef7\_046e095b7c4a | caed9b2d91f5802da4b1844068e7df971d50a11411ff2a792aedce96554539f9 |
| GoLang | t13d190900\_9dc949149365\_97f8aa674fd9 | 00b001f5d30e7a51bf9eced4e41267912353153dcc52605a737a6778aaecfbfb |
| SnakeLogger / Redline | t10d070600\_c50f5591e341\_1a3805c3aa63 | 03461c2a07431aed5ff68bbcf42d7ef82f32190b44ba140befd3f474614b5f3d |

  

  

### JA4: Elevating Threat Hunting on VirusTotal

VirusTotal's adoption of JA4 client fingerprinting will provide users with an invaluable tool for dissecting and tracking TLS client behaviors, leading to enhanced threat hunting, pivoting, and more robust malware identification.  
  
Happy Hunting.  

Go to Source
