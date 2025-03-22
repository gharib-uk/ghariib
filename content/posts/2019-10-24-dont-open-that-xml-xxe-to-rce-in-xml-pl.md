---
title: "Dont open that XML XXE to RCE in XML plugins for VS Code Eclipse Theia"
date: Thu, 24 Oct 2019 17:22:53 +0000
draft: false
type: posts
---
# Dont open that XML XXE to RCE in XML plugins for VS Code Eclipse Theia





TL;DR LSP4XML, the library used to parse XML files in VSCode-XML, Eclipse’s wildwebdeveloper, theia-xml and more, was affected by an XXE (CVE-2019-18213) which lead to RCE (CVE-2019-18212) exploitable by just opening a malicious XML file. Introduction 2019 seems to be XXE’s year: during the latest Penetration Tests we successfully exploited a fair amount

### TL;DR

LSP4XML, the library used to parse `XML` files in VSCode-XML, Eclipse’s wildwebdeveloper, theia-xml and more, was affected by an `XXE` ([CVE-2019-18213](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18213)) which lead to `RCE` ([CVE-2019-18212](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18212)) exploitable by just opening a malicious `XML` file.

Introduction
------------

2019 seems to be [XXE](https://www.owasp.org/index.php/XML_External_Entity_%28XXE%29_Processing)’s year: during the latest [Penetration Tests](https://www.shielder.com/services/application-security/) we successfully exploited a fair amount of `XXE`s, an example being [https://www.shielder.com/blog/exploit-apache-solr-through-opencms/](https://www.shielder.com/blog/exploit-apache-solr-through-opencms/).

![XXE, XXE everywhere meme](https://www.shielder.com/img/blog/xxe_everywhere.jpg)

It all started during a web application penetration test, while I was trying to exploit a `blind XXE` with [zi0black](https://twitter.com/zi0black). We started with a standard `XXE` payload with an external `DTD` pointing to our listening web-server; we knew the target server couldn’t perform `HTTP` requests to the internet, so we were expecting only a `DNS` interaction, but then we received two different `DNS` interactions and one `HTTP` request… What the Phrack?!

#### [Source](https://www.shielder.com/blog/2019/10/dont-open-that-xml-xxe-to-rce-in-xml-plugins-for-vs-code-eclipse-theia/)

