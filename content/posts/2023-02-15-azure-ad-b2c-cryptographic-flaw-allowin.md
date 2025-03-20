---
title: "Azure AD B2C cryptographic flaw allowing account compromise"
date: Wed, 15 Feb 2023 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Azure AD B2C cryptographic flaw allowing account compromise

<br/>

<br/>
Azure Active Directory B2C service (AD B2C) mistakenly implemented RSA key authentication using the public part of the key pair instead of the private one. This cryptographic flaw could have allowed an unauthenticated attacker to craft an OAuth refresh token for any AD B2C user account if they knew their public key. Moreover, every AD B2C user's public key was recoverable through an unrelated vulnerability (though asymmetric cryptography should not rely on public key secrecy regardless). An attacker could redeem this refresh token for a session token, thereby gaining access to the victim account as if they had logged in through a legitimate login flow.

#### [Source](https://www.cloudvulndb.org/azure-b2c-crypto-flaw)

<br/>
---
