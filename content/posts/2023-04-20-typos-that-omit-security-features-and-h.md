---
title: "Typos that omit security features and how to test for them"
date: Thu, 20 Apr 2023 07:00:08 +0000
draft: false
type: posts
categories: 
- 
---
# Typos that omit security features and how to test for them

<br/>

<br/>
During a security audit, I discovered an easy-to-miss typo that unintentionally failed to enable \_FORTIFY\_SOURCE, which helps detect memory corruption bugs in incorrectly used C functions. We searched, found, and fixed twenty C and C++ bugs on GitHub with this same pattern. Here is a list of some of them related \[…\]

#### [Source](https://blog.trailofbits.com/2023/04/20/typos-that-omit-security-features-and-how-to-test-for-them/)

<br/>
---
