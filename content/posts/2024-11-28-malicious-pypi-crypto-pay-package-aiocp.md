---
title: "Malicious PyPI crypto pay package aiocpa implants infostealer code"
date: Thu, 28 Nov 2024 11:02:38 GMT
draft: false
type: posts
categories: 
- Threat Research
---
# Malicious PyPI crypto pay package aiocpa implants infostealer code

<br/>

<br/>
[![Malicious PyPI crypto pay package aiocpa implants infostealer code](https://www.reversinglabs.com/hubfs/Blog/aiocpa-blog.webp)](https://www.reversinglabs.com/blog/malicious-pypi-crypto-pay-package-aiocpa-implants-infostealer-code)

Executive Summary
-----------------

ReversingLabs’ machine learning-based threat hunting system [detected malicious code](https://x.com/ReversingLabs/status/1859969249093025796) in a legitimate looking package, aiocpa, last week that was engineered to compromise crypto currency wallets. RL then reported the malicious package to the Python Package Index (PyPI) to be taken down, and the PyPI team then published [their own blog about the package](https://blog.pypi.org/posts/2024-11-25-aiocpa-attack-analysis/). Shortly after, researchers at Phylum reported on RL’s  [discovery of the package](https://thehackernews.com/2024/11/pypi-python-library-aiocpa-found.html). RL’s research team dug deeper because of the apparent uniqueness of the malicious campaign.   
  
Unlike the majority of the attacks targeting open source repositories like npm and PyPI, the malicious actors behind aiocpa were not impersonating or typosquatting legitimate looking packages. Instead, they published their own crypto client tool in order to steadily attract a user base that would later be compromised through a malicious version update. Using RL Spectra Assure to carry out differential analysis of two package versions, our team was able to determine how exactly these attackers carried out this distinctive campaign.   
  
Here’s how the RL research team discovered aiocpa, how attackers took a stealthier route to carry out the campaign — and how machine learning-based threat hunting can make or break a development team’s ability to spot a software supply chain attack before it arises.

#### [Source](https://www.reversinglabs.com/blog/malicious-pypi-crypto-pay-package-aiocpa-implants-infostealer-code)

<br/>
---
