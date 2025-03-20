---
title: "Elastic Beanstalk - XSS in Web Console"
date: Thu, 03 Jun 2021 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Elastic Beanstalk - XSS in Web Console

<br/>

<br/>
An adversary could gain access to IAM credentials in a victim's account, and make an API request to Elastic Beanstalk (even if they didn't have the proper IAM permissions). This request would be displayed in the management console in the Elastic Beanstalk section. Due to improper sanitization, an attacker could insert an XSS payload that would execute in a victim's browser.

#### [Source](https://www.cloudvulndb.org/aws-xss-console)

<br/>
---
