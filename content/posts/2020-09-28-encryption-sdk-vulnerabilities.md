---
title: "Encryption SDK vulnerabilities"
date: Mon, 28 Sep 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Encryption SDK vulnerabilities

<br/>

<br/>
AWS KMS and all versions of AWS Encryption SDKs prior to version 2.0.0 were susceptible to information leakage (an attacker could create ciphertexts that would leak the user’s AWS account ID, encryption context, user agent, and IP address upon decryption), ciphertext forgery (an attacker could create ciphertexts that were accepted by other users) and lack of robustness (an attacker could create ciphertexts that decrypt to different plaintexts for different users).

#### [Source](https://www.cloudvulndb.org/encryption-sdk-issues)

<br/>
---
