---
title: "Defending against Java Deserialization Vulnerabilities"
date: 2016-10-01T21:54:00.002-07:00
draft: false
type: posts
categories: 
- 1day,exploit,ikki,java
---
# Defending against Java Deserialization Vulnerabilities

<br/>

<br/>
During a recent [OWASP Meetup in San Francisco](http://www.meetup.com/Bay-Area-OWASP/events/233591691/), I gave a presentation on Java Deserialization vulnerabilities focused on defense techniques for identifying and fixing this class of bugs.  
  
  

**[Download the slides deck: "Defending against Java Deserialization Vulnerabilities"](https://www.slideshare.net/ikkisoft/defending-against-java-deserialization-vulnerabilities " Defending against Java Deserialization Vulnerabilities")**

  
While most of the content is based on the work of several Java Security aficionados ([@cschneider4711](https://twitter.com/cschneider4711), [@e\_rnst](https://twitter.com/e_rnst), [@matthias\_kaiser](https://twitter.com/matthias_kaiser),  [@pwntester](https://twitter.com/pwntester), [@frohoff](https://twitter.com/frohoff) and many others), this presentation contains a couple of new things:  
  

-   Technical details (and exploit) of a serialization bug via JSF view state affecting Sun Java Web Console
-   New features introduced in [SerialKiller](https://github.com/ikkisoft/SerialKiller) 

  

### Sun Java Web Console serialized object injection via JSF view state

Since it appears that there're no publicly disclosed details on Java serialization vulnerabilities triggered via JSF ViewState, I thought it would be a good idea to illustrate a bug I discovered in 2010. From slides 12 to 17, you can read more about this issue affecting Sun Java Web Console (which was the default web admin console for Solaris). I've also released an exploit [(download here)](https://www.ikkisoft.com/stuff/SJWC_DoS.java) that uses Hashtable collisions to trigger DoS. RCE is also possible via Apache Common Collections.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhM0xMbWQOV3YGWoqevm6qATMxUUll_lInSMcTYcaIYVz0baquButX7sHGw9PQ6qcibM74dEPkHBSZcgsc7BbY6gwuScQ_T4Rzr0IBm0PYl2O4stbSFwkYylOXCkEJ7UviLk6ja2ZnRbVA/s400/exploitDoS_screenshot+%2528cut%2529.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhM0xMbWQOV3YGWoqevm6qATMxUUll_lInSMcTYcaIYVz0baquButX7sHGw9PQ6qcibM74dEPkHBSZcgsc7BbY6gwuScQ_T4Rzr0IBm0PYl2O4stbSFwkYylOXCkEJ7UviLk6ja2ZnRbVA/s1600/exploitDoS_screenshot+%2528cut%2529.png)

  

Interestingly enough, old versions of _javax.faces.ViewState_ (client-side and with no signature) can be abused in multiple ways:

-   XSS and other UI redressing attacks. See Black Hat 2010's presentation ["Beware of Serialized GUI Objects Bearing Data"](https://www.blackhat.com/presentations/bh-dc-10/Byrne_David/BlackHat-DC-2010-Byrne-SGUI-slides.pdf) for more details
-   Java serialization bugs, as demonstrated in this [exploit](https://www.ikkisoft.com/stuff/SJWC_DoS.java)
-   [Expression Language injection](https://www.mindedsecurity.com/fileshare/ExpressionLanguageInjection.pdf) since some of the elements of the ViewState tree are actually loaded by a subclasses of ELContext

###   

### SerialKiller v0.4

I've released a new version of [SerialKiller](https://github.com/ikkisoft/SerialKiller) with new features and improvements:

-   Basic _logging support_, using Java's native logging
-   _Profiling mode_. While look-ahead whitelisting provides a robust protection to modern applications, it requires complete enumeration of all Java classes exchanged by the application. With this feature, it is possible to setup SK in "non-blocking" mode in order to enumerate all classes within client-server requests. A step-by-step tutorial on how to whitelist classes is available in the [documentation page](http://a%20step-by-step%20example%20for%20whitelisting%20classes/)
-   _Signatures parity_ with [Ysoserial](https://github.com/frohoff/ysoserial). I've created default blacklisting signatures for all exploits (as of 09/07) included in this popular payloads generator tool

#### [Source](http://blog.nibblesec.org/feeds/6889376318217384666/comments/default)

<br/>
---
