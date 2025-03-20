---
title: "UI Redressing Mayhem HttpOnly bypass PayPwn style"
date: 2012-12-19T03:15:00.000-08:00
draft: false
type: posts
categories: 
- 0day,daath,paypal,ui redressing
---
# UI Redressing Mayhem HttpOnly bypass PayPwn style

<br/>

<br/>
In the previous post, a new cross-domain extraction method - affecting the latest version of the Mozilla Firefox browser - has been presented. The _iframe-to-iframe technique_ was successfully used in a UI Redressing attack affecting LinkedIn. Today, I'm introducing an instance of the aforementioned method that involves a known Apache Web Server security issue, in order to steal session cookies that are protected by [HttpOnly](https://www.owasp.org/index.php/HttpOnly) flag, thus allowing the attacker to perform [Session Hijacking](https://www.owasp.org/index.php/Session_hijacking_attack) attacks. A new attack targeting PayPal systems will be also presented.  
  

### CVE-2012-0053: HttpOnly bypass and beyond

  
In January 2012 - even if the Apache defect was [known](http://stackoverflow.com/questions/2541418/how-to-delete-a-large-cookie-that-causes-apache-to-400) and exploited for a while - [Norman Hippert](http://www.the-wildcat.de/) disclosed [CVE-2012-0053](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0053) bug affecting the Apache Web Server. The software was not able to correctly restrict an [header field](http://people.apache.org/~trawick/2.0-CVE-2012-0053-r1234837.patch) information disclosure in case of overlong or malformed HTTP requests. The vulnerability could be effectively combined with a [Cross-Site Scripting](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29) attack to bypass the protection mechanism introduced by the HttpOnly flag and steal any session token stored as cookies value. Infact, an XSS vector could manipulate the [document.cookie](https://developer.mozilla.org/en-US/docs/DOM/document.cookie) object to set an overlong cookie field, and forward a malformed request to the affected Apache Web Server with the intention to trigger the error message and _extract_ the desiderated session cookies. The Apache bug can be abused in a series of attack scenarios such as the following:  

-   Bypassing HttpOnly flag with a XSS vulnerability on the same domain that is affected by the CVE-2012-0053;
-   Bypassing the limitation introduced by cookie [path](http://en.wikipedia.org/wiki/HTTP_cookie#Domain_and_Path) whereas the XSS vulnerability affects a web resources that resides _outside_ the defined path itself;
-   Bypassing HttpOnly flag if a XSS vulnerability is found on _any_ subdomains of the host that is affected by the Apache disclosure issue, if exploited in conjunction with a UI Redressing attack - that allows the cross-domain content extraction of the information included in the triggered Apache error message.

It should also be noted that the Apache Web Server is often used as a _reverse proxy_ configuration. As a result, any session object on any server-side technology, could be attacked with the described vectors.  
  

### Smashing PayPal for Fun but.. NO Profit

During my security research on UI Redressing attacks I found multiple PayPal subdomains (e.g. **https://b.stats.paypal.com**) affected by the Apache disclosure bug as detailed in Figure 1 and Figure 2.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEPcqhvKkSOIMhyphenhyphenaNj_v677NW05Ikppo1pj79-S3EMOHPuWQ63e9QYFF-MRZoJeeSqBoqkUOfQXKNUym8O78AsgC0jO1UdNuLrygFHtpVxluspTCSWTTg1DIna_33Ny3iYJOiGH9aCSqAT/s640/resp.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEPcqhvKkSOIMhyphenhyphenaNj_v677NW05Ikppo1pj79-S3EMOHPuWQ63e9QYFF-MRZoJeeSqBoqkUOfQXKNUym8O78AsgC0jO1UdNuLrygFHtpVxluspTCSWTTg1DIna_33Ny3iYJOiGH9aCSqAT/s1600/resp.png)

Figure 1 - HTTP request with the overlong cookie.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj7ecSPx_Bj6wm1wkkDQXYgiYJ_U0of6O43Y0TNmYjzlrh6MtC7Da-VoxPw6nScU8fqFcNoMnlK9Md0Cm5z9WH7RLcDy3D8feDdRc7yafAsIC9mLBKDVWqVuZlALRO6Jq6gf7IUPgwII-ZZ/s640/req.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj7ecSPx_Bj6wm1wkkDQXYgiYJ_U0of6O43Y0TNmYjzlrh6MtC7Da-VoxPw6nScU8fqFcNoMnlK9Md0Cm5z9WH7RLcDy3D8feDdRc7yafAsIC9mLBKDVWqVuZlALRO6Jq6gf7IUPgwII-ZZ/s1600/req.png)

Figure 2 - Apache error message with the disclosure of the malformed Cookie header.

  
Despite in the first instance the bug could appear as useless, I found that the PayPal application - www.paypal.com - delivers the session cookies defining the domain to **.paypal.com** (Figure 3 and Figure 4).  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjbdDd4qmLp4nHID2yWYvJzVuivr___O0zClfT_ZJiwVWBRLNFoNu24DmXpMUo3sIN41a29rubwKQxXgF-I6_voN8tAvCj4O6xJXL5MlcDpacXBtO5eNLiscfqKnUrl2heeRVq3HxLlcVQg/s640/sess_1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjbdDd4qmLp4nHID2yWYvJzVuivr___O0zClfT_ZJiwVWBRLNFoNu24DmXpMUo3sIN41a29rubwKQxXgF-I6_voN8tAvCj4O6xJXL5MlcDpacXBtO5eNLiscfqKnUrl2heeRVq3HxLlcVQg/s1600/sess_1.png)

Figure 3 - Post-authentication cookies delivery.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj2dlE5rn6N_r1BmhS17qmdY8rIrmzH5W4VDWfbWERPJ6uK45WiFSe9A65FnGKwUeZtX2ag8VDtD7bAb6ttbo-y0GGW9kWIa-gi4CnCA2Zqg8-BQrvVVMd5-v87WzfKGw4O_UGviadTYgTP/s640/sess_2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj2dlE5rn6N_r1BmhS17qmdY8rIrmzH5W4VDWfbWERPJ6uK45WiFSe9A65FnGKwUeZtX2ag8VDtD7bAb6ttbo-y0GGW9kWIa-gi4CnCA2Zqg8-BQrvVVMd5-v87WzfKGw4O_UGviadTYgTP/s1600/sess_2.png)

Figure 4 - Cookies delivered to the personal.paypal.com subdomain.

  
The highlighted security issues could be abused to attack authenticated PayPal users, implementing the mentioned UI Redressing attacks combined with the cookie disclosure bug. But.. I had a problem: I had **no** XSS vulnerability on any PayPal web application - not that there're not! I was able to circumnavigate the limitation identifying another vulnerability on a different PayPal subdomain, that allowed me to define a _monster cookie_ with a single HTTP request. As first, please note the following URL:

-   **https://history.paypal.com/helpcenter/script/pphc\_rating.js.jsp?locale=&\_dyncharset=UTF-8&countrycode=CA&cmd=\_AAAAKKKKKKAAAAKKKKKK\[..very long string here...\]KKKKKKAAAAKKKKKKAAAAKKKKKK&serverInstance=9002&no\_strip=** 

As detailed in Figure 5, the navigation of the above URL involves the setting of the cookie named **navcmd** and then a bit of _client-side black magic_ defines two new cookie fields named **s\_sess** and **s\_pers** (Figure 6) that complete the desiderated malformed HTTP request.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgneCM6ueq_Kx4VkYMF05MthXgH-bFNMG4nC7Uyzf5nLtJW7YEMt_WTJs2dnuXqT07dlDW0enITujodiDY8uDwkYxyjd81ImSe3yw3GGG9Mogd7NuVj14jYmiy_Yjq1pjAlHdc2EM_Q9UDL/s640/monster_1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgneCM6ueq_Kx4VkYMF05MthXgH-bFNMG4nC7Uyzf5nLtJW7YEMt_WTJs2dnuXqT07dlDW0enITujodiDY8uDwkYxyjd81ImSe3yw3GGG9Mogd7NuVj14jYmiy_Yjq1pjAlHdc2EM_Q9UDL/s1600/monster_1.png)

Figure 5 - Cookie defined with attacker-controlled input.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiTj69WA5bSfhSCqFgulDhOD0IjsZ6go2TVlylzO0YTnF6DP-aaKC3Nhkpj7IHVF8wvUsXrEF_NfpOMQGZ8zbTGjbbvleTDcuoOcAIt3QIcUgs5Li_5Bkm_9bPjSgsXIueR3SpwyMAPLpsT/s640/monster_2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiTj69WA5bSfhSCqFgulDhOD0IjsZ6go2TVlylzO0YTnF6DP-aaKC3Nhkpj7IHVF8wvUsXrEF_NfpOMQGZ8zbTGjbbvleTDcuoOcAIt3QIcUgs5Li_5Bkm_9bPjSgsXIueR3SpwyMAPLpsT/s1600/monster_2.png)

Figure 6 - Final monster cookie.

### 7350PayPwn

The exploitation is now trivial. The following are the logical steps implemented by the Proof of Concept exploit:  

1.  The exploit triggers the victim to open an _under pop_ (Figure 7) web page that generates the monster cookie - with **domain=.paypal.com** - involving the **history.paypal.com** application;
2.  The **https://b.stats.paypal.com** is then framed thus inducing the forward of a malformed HTTP request that triggers the disclosure of the Cookie header, containing the PayPal account's session cookies;
4.  The malicious page allows the victim to play the d&d game with the extraction of the secret session cookies.

  

  
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBsx_D_5qwSQcHo0H30-OX_QcFnoCdFJb_miETE72i3WTtP1p1uqCWFy0THA3qe2xhmo_HRbHAFcUNIC3x7UzUnCA_p6PoY6CDwon7cRPSOI7yp0MFeN3Jn8jJ71FWkji9g6w2KBACRkJB/s640/pop_under.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBsx_D_5qwSQcHo0H30-OX_QcFnoCdFJb_miETE72i3WTtP1p1uqCWFy0THA3qe2xhmo_HRbHAFcUNIC3x7UzUnCA_p6PoY6CDwon7cRPSOI7yp0MFeN3Jn8jJ71FWkji9g6w2KBACRkJB/s1600/pop_under.png)

Figure 7 - Pop-under page with the navigation of the monster cookie's generation URL.

  
The attacker now holds the cookies that can be used to perform a Session Hijacking attack against the victim's PayPal account. A working Proof of Concept has been developed and can be download [here](https://github.com/daath1/nibblesec/tree/master/ui_redressing_mayhem/paypal). The following is a video that illustrates the described attack:

#### [Source](http://blog.nibblesec.org/feeds/2138231835866578844/comments/default)

<br/>
---
