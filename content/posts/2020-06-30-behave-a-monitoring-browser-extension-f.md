---
title: "Behave A monitoring browser extension for pages acting as bad boi"
date: 2020-06-30T08:52:00.001-07:00
draft: false
type: posts
categories: 
- 
---
# Behave A monitoring browser extension for pages acting as bad boi

<br/>

<br/>
[![](https://user-images.githubusercontent.com/1196560/84408775-d7e64980-ac0c-11ea-87ed-38da5c38ffc6.png)](https://user-images.githubusercontent.com/1196560/84408775-d7e64980-ac0c-11ea-87ed-38da5c38ffc6.png)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Browsing: What Could Go Wrong?

There's so much literature about client side attacks, but most of the focus is usually about classical malware attacks, exploiting software vulnerabilities.

  

Malicious scripts happen to be executed every day by thousands of people and most of the times Malware/Virus/Malvertising try to exploit vulnerabilities or to lure the user to install software on his own machine with the intent of staying undetected as much as possible in order to do its criminal business. 

That's what AntiMalware/Virus/\[...\] are for.

  

It's the principle of minimum energy: usual malware wants comfortable, smooth, local execution. 

  

However, there's quite a number of alternative attacks on the client side, with minimal fingerprint that tend to drag less attention and that might go unnoticed on several environments.

  

Indeed there's a history of  [such alternative attacks](https://www.forcepoint.com/sites/default/files/resources/files/report-attacking-internal-network-en_0.pdf) as:

-   **[Local](https://web.archive.org/web/20060821065413/http://www.spidynamics.com/assets/documents/JSportscan.pdf) [Port](https://www.gnucitizen.org/blog/javascript-port-scanner/) [Scan](https://blog.jeremiahgrossman.com/2006/11/browser-port-scanning-without.html)**: _Impact_: Information Gathering which could be used to perform further client side attacks (Malware) or to have a better unique user profile (Advertising/RiskAnalysis).
-   **[Cross Protocol attacks](https://www.nccgroup.com/us/our-research/cross-protocol-request-forgery/)**: _Impact_: according to the protocol there might be an abuse of specific features. Such as SMTP abuse etc.
-   **[DNS](https://medium.com/@brannondorsey/attacking-private-networks-from-the-internet-with-dns-rebinding-ea7098a2d325) [rebinding](https://threatpost.com/unpatched-wi-fi-extender-remote-control/156990/):** _Impact: SOP bypass resulting in reading sensitive information of internal network servers._

  

which are not news at all. They are, indeed, quite old attacks that are still as reliable as difficult to completely "fix" by browser vendors because they abuse core features of the Web ecosystem.

  

### Behave! A Monitoring Extension for pages acting as "bad boi"

With those attacks in mind, we thought that, by taking advantage of the browser API at extension layer, a [browser extension](https://github.com/mindedsecurity/behave) might help monitoring HTML pages behavior.

That's [Behave!](https://github.com/mindedsecurity/behave)

Available as an extension for:

  

-   Firefox: [https://addons.mozilla.org/en-US/firefox/addon/behave/](https://addons.mozilla.org/en-US/firefox/addon/behave/)

-   Chrome: [https://chrome.google.com/webstore/detail/mppjbkhgconmemoeagfbgilblohhcica/](https://chrome.google.com/webstore/detail/mppjbkhgconmemoeagfbgilblohhcica/)

  
It monitors and warn if a web page performs any of following actions:  
  

-   Browser based Port Scan
-   Access to Private IPs
-   DNS Rebinding attacks to Private IPs

Here's Behave! pointing its finger to a malicious page hosted by _at.tack.er_ host performing access to local IPs:

  
  
  
[![image](https://user-images.githubusercontent.com/1196560/84412872-277a4480-ac10-11ea-8db2-0e8eec9adc21.png)](https://user-images.githubusercontent.com/1196560/84412872-277a4480-ac10-11ea-8db2-0e8eec9adc21.png)  

  

###   

### Behave! Future Plans 

There's a quite a bunch of stealth&malicious client side techniques that could be abused at several levels of security that might be monitored by Behave! in the future.

[Any Idea is welcome as usual.](https://github.com/mindedsecurity/behave/issues)

  

  

###

#### [Source](https://blog.mindedsecurity.com/feeds/5658850060110269195/comments/default)

<br/>
---
