---
title: "2019-037-Lee Holmes Powershell logging and why theres an execution bypass"
date: Thu, 17 Oct 2019 05:02:56 +0000
draft: false
type: posts
---
# 2019-037-Lee Holmes Powershell logging and why theres an execution bypass

<br/>

<br/>
Derbycon9 talk - PowerShell Security Looking Back from the Inside - [https://www.youtube.com/watch?v=DYWPtt7qszY&list=PLNhlcxQZJSm\_ZDJBksg97I5q1XsdQcyN5&index=27&t=0s](https://www.youtube.com/watch?v=DYWPtt7qszY&list=PLNhlcxQZJSm_ZDJBksg97I5q1XsdQcyN5&index=27&t=0s)

Encarta - [https://en.wikipedia.org/wiki/Encarta](https://en.wikipedia.org/wiki/Encarta)

Scott Hanselman’s twitter thread about Encarta: https://twitter.com/shanselman/status/1158780839464849409

Congrats on the black badge :)

I like that you bring up execution policies. That it was never created to become a security control

-   I started alerting on it anyway at least from non-admin devices

[https://www.mssqltips.com/sqlservertip/2702/setting-the-powershell-execution-policy/](https://www.mssqltips.com/sqlservertip/2702/setting-the-powershell-execution-policy/) 

Want to learn Powershell? UnderTheWire wargame: [https://underthewire.tech/](https://underthewire.tech/)

Jeffrey Snover “The Cultural battle to remove Windows from Windows Server”: https://www.youtube.com/watch?v=3Uvq38XOark

You talk about “why would anyone want to remove powershell” as it came as a standalone download and part of the windows sdk. - I was taught when I was just getting into tech, that I should fear powershell and didn’t realize how powerful it could be as an admin because of it.

Powershell slime trail <3 (powershell transparency)

“You can’t force a powerful tool only to be used how you want it to be used, you can tilt the playing field on behalf of defenders”

If an attacker is going to use powershell, let’s make them regret it

Powershell has had quite an impact and history.

My own sorry logging/alerting attempts

You mentioned the amount of attacks listed in MITRE that use powershell, is that \*the\* recommended resource for blue teamers, are there any others?

Revoke-Obfuscation white paper (blackhat2017): [https://www.blackhat.com/docs/us-17/thursday/us-17-Bohannon-Revoke-Obfuscation-PowerShell-Obfuscation-Detection-And%20Evasion-Using-Science-wp.pdf](https://www.blackhat.com/docs/us-17/thursday/us-17-Bohannon-Revoke-Obfuscation-PowerShell-Obfuscation-Detection-And%20Evasion-Using-Science-wp.pdf)

[https://github.com/danielbohannon/Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation) 

[https://github.com/danielbohannon/Revoke-Obfuscation](https://github.com/danielbohannon/Revoke-Obfuscation)

[https://blog.trendmicro.com/trendlabs-security-intelligence/ransomware-now-uses-windows-powershell/](https://blog.trendmicro.com/trendlabs-security-intelligence/ransomware-now-uses-windows-powershell/) 

[https://www.trendmicro.com/vinfo/us/threat-encyclopedia/malware/TROJ\_POSHCODER.A](https://www.trendmicro.com/vinfo/us/threat-encyclopedia/malware/TROJ_POSHCODER.A) 

Ever thought of writing a powershell security sentric book? Bill Pollock was looking for someone to write a book for NoStarch…

Derbycon keynote with Lee Holmes and Jeffrey Snover - [http://www.irongeek.com/i.php?page=videos/derbycon6/101-key-note-jeffrey-snover-lee-holmes](http://www.irongeek.com/i.php?page=videos/derbycon6/101-key-note-jeffrey-snover-lee-holmes)

AMSI - Antimalware Scan Interface: https://docs.microsoft.com/en-us/windows/win32/amsi/antimalware-scan-interface-portal

[https://www.amazon.com/dp/B00ARN9MEK/ref=dp-kindle-redirect?\_encoding=UTF8&btkr=1](https://www.amazon.com/dp/B00ARN9MEK/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1) \-  Windows Powershell cookbook

Eric conrad: [https://www.ericconrad.com/2016/09/deepbluecli-powershell-module-for-hunt.html](https://www.ericconrad.com/2016/09/deepbluecli-powershell-module-for-hunt.html) 

[https://github.com/sans-blue-team/DeepBlueCLI](https://github.com/sans-blue-team/DeepBlueCLI) 

Daniel Bohannon - DevSec Defense - [https://www.youtube.com/watch?v=QJe8xikf-iE](https://www.youtube.com/watch?v=QJe8xikf-iE) 

[https://github.com/psconfeu/2018/tree/master/Daniel%20Bohannon/DevSec%20Defense](https://github.com/psconfeu/2018/tree/master/Daniel%20Bohannon/DevSec%20Defense) 

Constrained language mode: [https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/) 

Maslow’s security Hierarchy: [https://www.leeholmes.com/blog/2014/12/08/maslows-hierarchy-of-security-controls/](https://www.leeholmes.com/blog/2014/12/08/maslows-hierarchy-of-security-controls/) 

Just Enough Administration: https://docs.microsoft.com/en-us/previous-versions//dn896648(v=technet.10)?redirectedfrom=MSDN

[https://github.com/infosecn1nja/AD-Attack-Defense](https://github.com/infosecn1nja/AD-Attack-Defense) \- 

Also - DrawOnMyBadge.com - Super cool idea, loved the mona lisa

@Lee\_Holmes

@hackershealth

@log-md

@infosecCampout

@seasecEast

@brakesec

@bryanbrake

@boettcherpwned

@Infosystir

@packscott  

@dpcybuck

@megan\_roddie

@consultingCSO

#### [Source](http://brakeingsecurity.com/2019-037-lee-holmes-powershell-logging-and-why-theres-an-execution-bypass)

<br/>
---
