---
title: "How to Decrypt Manage Engine PMP Passwords for Fun and Domain Admin - a Red Teaming Tale"
date: Mon, 05 Sep 2022 10:00:30 +0000
draft: false
type: posts
categories: 
- 
---
# How to Decrypt Manage Engine PMP Passwords for Fun and Domain Admin - a Red Teaming Tale

<br/>

<br/>
TL;DR
-----

During a recent Red Teaming assessment we have found an internet-exposed instance of ManageEngine’s Password Manager Pro which was vulnerable to a pre-authentication Remote Code Execution ([CVE-2022-35405](https://www.manageengine.com/products/passwordmanagerpro/advisory/cve-2022-35405.html)). After gaining code execution we reverse engineered the password encryption/decryption routine to decrypt all the passwords and hack our way to become Domain Admin.

What’s a Red Teaming?
---------------------

Red Team(ing) is an abused word in the InfoSec world and it’s commonly used to define various things:

#### [Source](https://www.shielder.com/blog/2022/09/how-to-decrypt-manage-engine-pmp-passwords-for-fun-and-domain-admin-a-red-teaming-tale/)

<br/>
---
