---
title: "All your iNotes emails are belong to me"
date: 2013-08-09T13:50:00.000-07:00
draft: false
type: posts
categories: 
- daath,IBM,inotes,lotus domino,xss
---
# All your iNotes emails are belong to me

<br/>

<br/>
This post describes a critical bypass of the **Active Content Filtering** (ACF) mechanism that is implemented in [IBM iNotes](http://en.wikipedia.org/wiki/IBM_Lotus_iNotes) to avoid the inclusion of malicious HTML tags as part of emails. The bug has been identified during a web application penetration test, and can be exploited to perform stored Cross-Site Scripting (XSS) attacks. The bypass has been successfully verified with **IBM iNotes 9** and an official [bulletin](http://www-01.ibm.com/support/docview.wss?uid=swg21645503) and fix have been released on August 1st, 2013.

  

### From zero to Domino admin in a matter of hours

  
Early this spring I have been asked to assess the security of the mail infrastructure owned by a big company here in Italy. Pentesting the Domino/Notes/iNotes ecosystem is nowadays a _piece of cake_ because of the large amount of publicly available documentation, advisories and tools.  
  
If you are interested in testing this kind of infrastructure, I would recommend the following resources.

  
First of all, **Marco Ivaldi**'s [script](http://www.exploit-db.com/exploits/3302/) can be used to automatically download all users' password hashes, together with details about every single account (e.g. name, surname, e-mail address, etc.). By simply accessing the **names.nsf** web resource, the tool extracts the desired information disclosed by the hidden attribute named **HTTPPassword**. The extracted hashes can be easily cracked using [John The Ripper](http://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats): **William Ghote** gave a great [talk](https://www.youtube.com/watch?v=vfUqZo1Hryg) at BSides Las Vegas 2012 detailing the Lotus Notes password cracking process.

  

Finally, [Penetration from application down to OS - Lotus Domino](http://dsecrg.com/files/pub/pdf/Penetration_from_application_down_to_OS_%28Lotus_Domino%29.pdf) by **Alexandr Polyakov** and [Lotus Domino: Penetration Through the Controller](http://www.youtube.com/watch?v=RS0RpEWcPfk) by **Alexey Sintsov** complete _the picture_ providing even more details on how to pentest Lotus Domino deployments.  
  
The links above are amazing resources that describe step by step how to easily hack into a mail infrastructure based on IBM solutions. As for my experience, a standard _attack pattern_ to breach the Domino/iNotes infrastructure and access every company's e-mail accounts can be schematized as follow:  
  

1.  Identify the location/path of the names.nsf web resource;
2.  Identify the user(s) with administrative privileges;
3.  Verify the user's password hash disclosure via the HTTPPassword hidden attribute;
4.  Get all the administrators' password hashes;
5.  Crack the so obtained hashes with John the Ripper;
6.  Log into the Domino Web Administrator application and have a drink.

  
The whole process took less than 30 hours and I can't hide that, at least for this time, this task was as easy as _cut and paste_ of known attacks against an outdated environment. As my pentest objectives were quickly accomplished, I decided to turn my job into a _security research_ session. Because of that, I dedicated the rest of the engagement to verifying the effectiveness of the aforementioned ACF mechanism.  
  

### Active Content Filtering (ACF) vulnerability details

  
The analysis of the filter started with injecting simple and well-known XSS attack vectors, in order to understand the underlying logic and spot potential defects. On the basis of my analysis - that must be considered an incomplete understanding of the filter's internals, based exclusively on black box observations - ACF tries to block malicious HTML tags by both commenting JavaScript code, specified by the **<script>** tag, and normalizing/filtering tag attributes that could lead to client-side code execution (e.g. by eliminating the **onXYZ** event handlers, such as **onerror** or **onmouseover**). During the engagement, I found that the filtering feature is not properly implemented and allows an attacker to inject arbitrary attributes. In details, what I found is that the ACF is not able to correctly sanitize the sequence of characters **src="<**. For the sake of clarity, the following attack payload:  
  

**<img src="< onerror=alert(1) src=x\>**

  
would be transformed in:  
  

**<img < onerror=alert(1) src=x\>**

  
resulting in the JavaScript alert method execution. Figure 1 shows how the above vector is incorrectly treated and used to set the **BodyHtml** variable - which contains the mail's HTML body message.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxiMO2DBuwTwx379x3YADsqQMpwIu2KXHkqHjPvtjJEybzTgpwsJ7b8czbLFztmjlZlSO4VnfujQvoi4Z5H2e8VIKc-jgkRq6b99vdr_oNwi6jAsBIjlrcpN_bj08DD52_pI_R1CcCS4DW/s1600/bypass.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgxiMO2DBuwTwx379x3YADsqQMpwIu2KXHkqHjPvtjJEybzTgpwsJ7b8czbLFztmjlZlSO4VnfujQvoi4Z5H2e8VIKc-jgkRq6b99vdr_oNwi6jAsBIjlrcpN_bj08DD52_pI_R1CcCS4DW/s1600/bypass.png)

Figure 1 - Bypass of the ACF mechanism and injection of JavaScript code.

  

### Conclusion

  

The ACF bypass can be effectively abused to perform stored XSS attacks against iNotes users. In a real-world attack scenario, the bug could not only be exploited to perform [Session Hijacking](http://en.wikipedia.org/wiki/Session_hijacking) but also combined with [Cross-Site Request Forgery](http://en.wikipedia.org/wiki/Cross-site_request_forgery) (CSRF) to add a new e-mails _forwarding rule_ to the victim's iNotes application, thus effectively **backdooring** the victim's mailbox.   
  
The following video demonstrates the execution of arbitrary JavaScript thanks to the described vulnerability. Moreover, it shows how the mail preview mechanism, if enabled, implies that the victim **is not required to open the message in order to trigger the execution of JavaScript code** - _greatly reducing_ the required user iteration: 

  
  

   

  
  

  

Finally, I would like to thank my fellow [Sandro Zaccarini](https://twitter.com/theguly) and [Leonardo Rizzi](https://twitter.com/l_rizzi) for providing me the infrastructure to properly investigate this issue, and **IBM Product Security Incident Response Team** (PSIRT) for their timely responses and professionalism.

#### [Source](http://blog.nibblesec.org/feeds/4496440186187363092/comments/default)

<br/>
---
