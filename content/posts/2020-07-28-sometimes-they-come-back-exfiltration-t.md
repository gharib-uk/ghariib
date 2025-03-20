---
title: "Sometimes they come back exfiltration through MySQL and CVE-2020-11579"
date: Tue, 28 Jul 2020 16:18:14 +0200
draft: false
type: posts
categories: 
- 
---
# Sometimes they come back exfiltration through MySQL and CVE-2020-11579

<br/>

<br/>
Let’s jump straight to the strange behavior: up until [PHP 7.2.16](https://www.php.net/manual/en/mysqli.configuration.php) it was possible by default to exfiltrate local files via the [MySQL LOCAL INFILE feature](https://dev.mysql.com/doc/refman/8.0/en/load-data.html) through the connection to a malicious MySQL server. Considering that the previous PHP versions are still [the majority](https://w3techs.com/technologies/details/pl-php) in use, these exploits will remain useful for quite some time.

Like many other vulnerabilities, after reading about this _quite_\-unknown attack technique ([1](https://w00tsec.blogspot.com/2018/04/abusing-mysql-local-infile-to-read.html), [2](http://russiansecurity.expert/2016/04/20/mysql-connect-file-read/)), I could not wait to find a vulnerable software where to practice such unusual dynamic. The chance finally arrived after a network penetration test where [@smaury](https://twitter.com/smaury92) encountered [PHPKB](https://www.knowledgebase-script.com/), a knowledge-base software written in PHP which he felt might be interesting to review, and that was my trigger. 😏

#### [Source](https://www.shielder.com/blog/2020/07/sometimes-they-come-back-exfiltration-through-mysql-and-cve-2020-11579/)

<br/>
---
