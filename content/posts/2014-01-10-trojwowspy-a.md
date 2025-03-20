---
title: "TrojWowSpy-A"
date: Fri, 10 Jan 2014 00:10:00 +0000
draft: false
type: posts
categories: 
- [object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object],[object Object]
---
# TrojWowSpy-A

<br/>

<br/>
Recently a malware who target World of Warcraft got identified.  
This threat is known as Disker, Mal/DllHook-A or Trojan.Siggen5.64266 and can steal player accounts even if they use a Battle.net Authenticator.  
Yes, this is another post about password stealer mawlare...  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhLMWTxLW7aG_9SFBQCQHzaY3zMXL5Aj7JTw8HOZMdI1Km4gYCogiMMOP3dtEAsdt8BDapAwLru_EtGCBSm1enK-khvvbbLyfh_BLEzw5ykhoN43IQHhdkJmu4peCXuCrm1YbNf7MCIYOM/s1600/10-01-2014+00-55-11.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhLMWTxLW7aG_9SFBQCQHzaY3zMXL5Aj7JTw8HOZMdI1Km4gYCogiMMOP3dtEAsdt8BDapAwLru_EtGCBSm1enK-khvvbbLyfh_BLEzw5ykhoN43IQHhdkJmu4peCXuCrm1YbNf7MCIYOM/s1600/10-01-2014+00-55-11.jpg)

 There is no option to retain password on the WoW client.  
  
The method used to spread this malware is by fake websites leading to malicious download.  
The Trojan is bundled with legit programs such as WowMatrix or Curse Client, used by players to manage their AddOns.  
  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDA-QxrB7idc_Ai6HEw1N7ukqmYgH1zpYR70XVBEvmhwlrQkB70F2gZTQBdxjWJrPkG6mu34YA96P0wbR-KdpquDPgw4ld1_eTFDjT6bbXVyCfudMu0m3ugY6dmhlrk2JGNlpJEffizZU/s1600/07-01-2014+03-20-13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDA-QxrB7idc_Ai6HEw1N7ukqmYgH1zpYR70XVBEvmhwlrQkB70F2gZTQBdxjWJrPkG6mu34YA96P0wbR-KdpquDPgw4ld1_eTFDjT6bbXVyCfudMu0m3ugY6dmhlrk2JGNlpJEffizZU/s1600/07-01-2014+03-20-13.png)

  
Malicious Wowmatrix installer. ([DCDD6986941B2B4E78A558CAB3ACF337](http://vxvault.siri-urz.net/ViriFiche.php?ID=25520))  
  
Fake sites:  
• dns: 1 ›› ip: 142.4.105.98 - adress: WWW.CURSE.PW  
• dns: 1 ›› ip: 142.4.105.98 - adress: WWW.WOWMATRIX.PW  
• dns: 1 ›› ip: 142.4.105.99 - adress: WWW.WOWMATRIX.PW.PW  
  
  
Blizzard released a statement due to this new threat:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFa6pVVN1PxU9Rj6IM1I8zVOMFzNvMHrrW62ssvn_m1tuvOKBlIXzZ__ehAhp0gO76qvLUmotY3CK9ZmMnF1H9MheILL_04EpzNLY1dukKrtzMlBdVZctjS5ypPlXjHmLAlxwv5g9b0nk/s1600/06-01-2014+22-32-01.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFa6pVVN1PxU9Rj6IM1I8zVOMFzNvMHrrW62ssvn_m1tuvOKBlIXzZ__ehAhp0gO76qvLUmotY3CK9ZmMnF1H9MheILL_04EpzNLY1dukKrtzMlBdVZctjS5ypPlXjHmLAlxwv5g9b0nk/s1600/06-01-2014+22-32-01.png)

  
I don't know how work the dll for the moment (at least a bit)  
My debugger got some stability issue when handling wow.exe but i will get back on this, the mechanism seem interesting (and they even use OutputDebugString!).  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihn_ZMAX809T6npAZ5Ubh_sTd0R0KCT3QHso0cUsvI40iwrdUEFcOphQXnLRvQeOFwLluCryt7O-pFKLR2pAAIwfdiCygzYIsLJ18St-pxSIx0HNhuLx6_acIuhovb3sRaNyP0bRLUVe4/s1600/07-01-2014+00-29-18.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihn_ZMAX809T6npAZ5Ubh_sTd0R0KCT3QHso0cUsvI40iwrdUEFcOphQXnLRvQeOFwLluCryt7O-pFKLR2pAAIwfdiCygzYIsLJ18St-pxSIx0HNhuLx6_acIuhovb3sRaNyP0bRLUVe4/s1600/07-01-2014+00-29-18.png)

  

Network trafic after login in:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEisbG2Qmi8f_yil8q7MU7Zp_CR8j18HjZjuPBdSbcAr_z4xtTWNpAHeeql-Nf3p3XNdjNua1RDDuBoh4fiFMGFsGlqKf6f2CfffeMhPpMKWuV54jVjg1sYmcNOQAXDpyDHFAjtIEcO7wNI/s1600/06-01-2014+20-47-14.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEisbG2Qmi8f_yil8q7MU7Zp_CR8j18HjZjuPBdSbcAr_z4xtTWNpAHeeql-Nf3p3XNdjNua1RDDuBoh4fiFMGFsGlqKf6f2CfffeMhPpMKWuV54jVjg1sYmcNOQAXDpyDHFAjtIEcO7wNI/s1600/06-01-2014+20-47-14.png)

  
C&C (in Chinese):  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuiBBk6BiTw97ZzSRqoRPcKsdeW5H7AnnUjVGj9V5JzraSk5Tpd_LVLd7yj49LklivqZPd0lZIDOGqvNlGF2VWcdeUOJiLhYdpkuWUUsCEbFvpIsN0Hz-eKoKNLS8d8l4VHHMExqfZSXI/s1600/06-01-2014+12-05-39.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuiBBk6BiTw97ZzSRqoRPcKsdeW5H7AnnUjVGj9V5JzraSk5Tpd_LVLd7yj49LklivqZPd0lZIDOGqvNlGF2VWcdeUOJiLhYdpkuWUUsCEbFvpIsN0Hz-eKoKNLS8d8l4VHHMExqfZSXI/s1600/06-01-2014+12-05-39.png)

  
Compromised accounts:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVbTS7wMb3MAdqNEkOy3kf0QPXeK1iZ2QdU24xoV9vkT5mgfC9yoAC37fO-sREk0ZvKwd6gjLtKVH7onhbPxph48o2mpfhhaFKaeVM0Di1x4tBCZCG4RQ89uk_DjmTGITEQnFAZK3PjqQ/s1600/06-01-2014+04-12-28_b.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgVbTS7wMb3MAdqNEkOy3kf0QPXeK1iZ2QdU24xoV9vkT5mgfC9yoAC37fO-sREk0ZvKwd6gjLtKVH7onhbPxph48o2mpfhhaFKaeVM0Di1x4tBCZCG4RQ89uk_DjmTGITEQnFAZK3PjqQ/s1600/06-01-2014+04-12-28_b.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhZFLO3becbFuw2H6-vS98YVZVrALyYuBjMXcGYB2r8OqX8c17pHfsMueTUHu-FXowBxPeBzCJFwKFm7TLJSqZpyWoptePCTYTaV50oZZa0boaBw4gZI6zT0oMTfwkIySiLCsx1dcdEQ1k/s1600/06-01-2014+04-34-48_b.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhZFLO3becbFuw2H6-vS98YVZVrALyYuBjMXcGYB2r8OqX8c17pHfsMueTUHu-FXowBxPeBzCJFwKFm7TLJSqZpyWoptePCTYTaV50oZZa0boaBw4gZI6zT0oMTfwkIySiLCsx1dcdEQ1k/s1600/06-01-2014+04-34-48_b.png)

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhoTvdY77YOzmAC1oNGkfZF06mZc9527iUFxa4odbILT_2m0-uX5lMfe3zHrKELsyBYIAiK7WUSn1a9AYkiA3pTgA1xYNxL10QYrKmB4leUJTk4BzHpaYs39Wn5IiOfQDCxSzh-409rES4/s1600/06-01-2014+04-13-16_b.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhoTvdY77YOzmAC1oNGkfZF06mZc9527iUFxa4odbILT_2m0-uX5lMfe3zHrKELsyBYIAiK7WUSn1a9AYkiA3pTgA1xYNxL10QYrKmB4leUJTk4BzHpaYs39Wn5IiOfQDCxSzh-409rES4/s1600/06-01-2014+04-13-16_b.png)

  
That all for the moment :)

#### [Source](https://www.xylibox.com/2014/01/trojwowspy-a.html)

<br/>
---
