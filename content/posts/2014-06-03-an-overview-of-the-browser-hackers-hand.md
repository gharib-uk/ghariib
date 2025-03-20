---
title: "An Overview of The Browser Hackers Handbook"
date: 2014-06-03T07:37:00.002-07:00
draft: false
type: posts
categories: 
- 
---
# An Overview of The Browser Hackers Handbook

<br/>

<br/>
Writing a book is definitely a big and time-consuming task. After one year me, Wade Alcorn and Cristian Frichot finally released the Browser Hacker's Handbook in March 2014. Looking backwards at our git repository (I know it's not ideal to track binary file changes such as MS Word with git..), I counted more than 2300 commits, starting  5 December 2012 drafting the ToC and finishing 30 March 2014 with the creation of the [https://browserhacker.com](https://browserhacker.com/) website.

  

Only after you write a book you can understand two things:

 - why your partner always deserves the first big THANKS

 - why you will not write another book for at least the next 5 years

  

Our adventure started when Wade and me were discussing about creating a BeEF training, to be presented at SysCan. Eventually we decided to switch to the book and maybe take care of the training later on (it's still in our overly long TODO list).

  

The Browser Hacker's Handbook (BHH) is the first book focused entirely on browser hacking from the attacker's perspective, which was something somehow missing so far (even Portswigger mentions that in the Web Application Hacker's Handbook 2nd edition). There are other books I personally recommend if you're interested in browser/web security, like [The Tangled Web](http://lcamtuf.coredump.cx/tangled/) from Michał Zalewski and [Web Application Obfuscation](http://www.amazon.com/Web-Application-Obfuscation-Evasion-Filters/dp/1597496049) from Mario Heiderich (friend and BHH technical editor, kudos!) et al. Both of the books are great and technically deep, but none of them focus exclusively on the browser ecosystem and how it can be attacked, so there you go BHH :-)

  

Something worth noting is that this is not a book about BeEF, which is mentioned multiple times but it's not the focus of the book. Most of the code (see [http://browserhacker.com/code/code\_index.html](http://browserhacker.com/code/code_index.html)) is pure JavaScript/Ruby, which you can use with your own browser hacking framework or something else different from BeEF. BeEF was mentioned throughout the book not only because Wade created it and I'm the lead core developer, but simply because there is no other open source tool at the moment that is mature enough (yeah, we're still in alpha..) and has the number of modules currently in BeEF. Many people around the world use BeEF professionally for social engineering and red-team assessments with success, demonstrating that BeEF is mature enough to be used during your own pentests. Even Jester modified it adding a bunch of 0days and other stuff, and was using it in the wild a while back: [http://jesterscourt.cc/2012/07/04/project-looking-glass/](http://jesterscourt.cc/2012/07/04/project-looking-glass/) 

  

What we decided to do was to create the first browser hacking methodology that comes handy in red-team and social engineering engagements. The methodology can be summarized with the diagram below:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9szgU7KrUPTqRJ5de1CqhUuWrSAwv2FYf4gCgfJWywchCmzQNBn6m47SXidxI2DSt2NMNUZnplC_fRYpDyOYNRrkERo2hh0Q_qtroqT1GjqX8bx-hTvN6jUgMaNxEnaUwedOsHTAfhEKI/s1600/662090+c01f001.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9szgU7KrUPTqRJ5de1CqhUuWrSAwv2FYf4gCgfJWywchCmzQNBn6m47SXidxI2DSt2NMNUZnplC_fRYpDyOYNRrkERo2hh0Q_qtroqT1GjqX8bx-hTvN6jUgMaNxEnaUwedOsHTAfhEKI/s1600/662090+c01f001.png)

As you can see there is an entire chapter dedicated to each of these categories, and to be honest it wasn't that easy to categorize some of the attacks to be in a chapter rather than another one. For instance, we discuss multiple RCEs in various web applications in Chapter 9 (and how you can exploit them cross-origin from the hooked browser), but also in Chapter 10 when discussing the BeEF Bind shellcode technique. SOHO router attacks and some social engineering attacks are other examples of attack categories that can overlap in multiple chapters.

  

**Initiating Control**

The book starts with introducing control initiation techniques, a mandatory step if you want to have the target browser execute your malicious code. From the multiple types of XSS including DOM-based and Universal-XSS types, to social engineering attacks involving baiting and phishing (btw, you can do template-based mass-mailing and phishing all with BeEF), finishing with classic Man-in-the-Middle scenarios like ARP Spoofing, DNS Poisoning, Wi-Fi related things and so on. After experimenting with source code and attacks discussed in this chapters, you should have a solid grasp about how to start your browser hacking journey.

  

**Retaining Control**

You initiated control with the browser executing some code, most likely JavaScript: now you need to retain that control as longer as possible. Some attacks might need only one or two seconds to complete, others might take several seconds or minutes depending on their configuration. Additionally, you want to have a dynamic communication channel (in other words, bidirectional) in order to have the hooked browser extruding data to your server, and the server pushing new code to be executed by the browser. Communication techniques such as XMLHttpRequest polling, WebSockets and DNS Tunneling (yes, bidirectional and purely in the browser without using plugins) are discussed, as well as some persistence techniques like Overlay IFrames, pop-unders, browser events and more advanced Man-in-the-Browser attacks. The chapter finishes presenting examples on evading detection and playing with obfuscation, which can generally be resumed as the following pseudo-code:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjt7iwgPL6NYadJXI8fNByYFoJy64FPlqP4xd2B5E432f5hU3PT9cpHlqFkox-yr_3-GE6FQopfUxJToYNbVIlOd0WWxGk5Q9-6JlYe7JyejwMWMLif349eqiYFz5orOaAg90tCwkMsDxtL/s1600/Screen+Shot+2014-05-20+at+6.28.42+PM.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjt7iwgPL6NYadJXI8fNByYFoJy64FPlqP4xd2B5E432f5hU3PT9cpHlqFkox-yr_3-GE6FQopfUxJToYNbVIlOd0WWxGk5Q9-6JlYe7JyejwMWMLif349eqiYFz5orOaAg90tCwkMsDxtL/s1600/Screen+Shot+2014-05-20+at+6.28.42+PM.png)

  

**Bypassing the SOP**

The Same Origin Policy is probably the most important, inconsistently implemented, broken and bypassed security control in nowadays browsers. The chapter goes through many different SOP implementations in Java, Adobe Reader/Flash, Silverlight and multiple browsers, presenting some well-known and less-known quirks, bugs and bypasses (some of them not even patched or by-design). This chapter also analyzes UI redressing attacks such as Clickjacking, Cursorjacking, Filejacking, drag&drop tricks and provides some real-world examples about how to steal browser history. Bypassing the SOP is an optional step, as well as the next attacking phases. While you need to initiate and retain control, you might not need to bypass the SOP or attack web applications if your goal is to trick the user to install a backdoored Chrome extensions for instance. Going through the book, especially in Chapter 9 and 10, you will discover the multitude of attacks you can still deliver against web applications and networks without the need for a SOP bypass.

  

**Attacking Users**

Humans are often referred to as the weakest link in information security. Is it our inherent desire to be ‘helpful’? Perhaps it’s our inexperience. Or, is it simply our (often) misplaced trust in each other? Either way, social engineering users is always fun. The less they know about computers in general, the better, as they tend to click OK or ALLOW on any kind of real or spoofed user prompt you can create. This chapter introduces how to capture multiple types of user input via hooking JavaScript events. 

  

For instance a nice and easy attack, in case the browser was hooked via a post-auth XSS, is to create an overlay IFrame that loads the same-origin resource being the login page of the hooked origin. This IFrame has also a JavaScript keylogger attached to it: the user thinks his session just expired, so he enters his credentials, which are captured and sent back to us. At the same time this attack achieves some form of persistence, as the communication channel is running in the background while the user browses in the foreground overlay IFrame. Many other social engineering attacks are discussed such as Signed Java Applets, Fake Software Updates, Malicious Extensions, HTAs and other tricks on Internet Explorer.

  

**Attacking Browsers**

This chapter deals with attacking the browser itself. Before attacking it you want to exactly fingerprint which browser type and version is hooked. Fingerprinting through HTTP headers, DOM properties, software bugs and other quirks are discussed. Combining these fingerprinting techniques all together you can be almost sure that even if someone is spoofing his browser type/version, you get an accurate fingerprinting result. Cookies, protocol handlers (aka schemes) and the SSL/TLS layers are also discussed, with some examples of attacks and bypasses. The chapters ends with some analysis of heap exploitation in Firefox and other examples of how you can get shells if you find a bug in the browser JavaScript interpreter or HTML parser.

  

**Attacking Extensions**

Browser extensions always run in a privileged context, with higher privileges than for instance the normal context where JavaScript is executed when you browse to [https://browserhacker.com](https://browserhacker.com/). For this reason, if the extension is bugged, or if you can trick the user to install your malicious extension, it's usually game over. Firefox and Chrome extensions are discussed, including fingerprinting, spoofing, cross-context scripting and various RCEs. Remember that at the time of writing the book and this blog post, Firefox extensions do not run in a sandbox, so an XSS in the extension leads to RCE. Chrome extensions (especially with manifest version 2, which is the default right now) are less vulnerable, especially thanks to the adoption of the Content Security Policy, but the malicious/backdoored extension social engineering trick is still a viable option. Luca and me discussed this a while ago in this post: [http://blog.nibblesec.org/2013/03/subverting-cloud-based-infrastructure.html](http://blog.nibblesec.org/2013/03/subverting-cloud-based-infrastructure.html)

  

**Attacking Plugins**

In the previous years (well, months) before Click-to-Play was implemented in the Java plugin and on Chrome/Firefox, 0days on Java, Adobe Reader/Flash, RealPlayer, VLC and others were the preferred and easier way to create botnets. The exploitation of those bugs in the wild was pretty crazy. So far Internet Explorer and Safari are the only major browsers that still do not implement Click-to-Play, so plugin attacks are still quite possible with those browsers, but not really an option on Chrome/Firefox (unless you have a Click-to-Play bypass in your 0day collection). Multiple bypasses are discussed in the chapter, as well as other tricks you can use with VLC, ActiveX and Java.

  

**Attacking Web Applications**

The main concept behind BHH is abusing existing functionality of the browser ecosystem to subvert the system. This includes launching traditional web application attacks from the hooked browser, which effectively becomes a beachhead and a pivot point for the internal network. Something (probably not that well known) is that without a SOP bypass you can still send cross-origin requests without generating a preflight request. This happens obviously with GET, HEAD, but also with POST with certain content types such as text/plain, application/x-www-form-urlencoded and multipart/form-data. Such behavior is enough to carry on attacks where:  
 - you need to "blindly" send the request: for example, you can exploit any XSRF, RCEs, DoS and so on cross-origin;  
 - you need to infer on request/response timings: for example, you can exploit cross-origin any kind of  SQL injection using time-based blind attack vectors.  
You can even detect and blindly exploit XSS cross-origin, damn! You can actually do so many things without a SOP bypass and working fully cross-origin, that you have to read the chapter to get a good grasp. You can even create a full HTTP/HTTPS proxy with just an XSS.

  

**Attacking Networks**

That last chapter of the book focus on attacking networks (mostly internal networks, but not limited to it), starting with various techniques to retrieve the internal IP address of the hooked browser, to ping sweeping, port scanning and fingerprinting. The main part of the chapter is devoted to IPC/IPX, Inter-protocol Communication and Exploitation. Basically you can communicate via HTTP with non-HTTP protocols like IRC, SIP, IMAP, and most ASCII protocols if two conditions are met:

 - the protocol implementation is tolerant to errors, meaning that it doesn't close the socket if you send garbage data;

 - the ability to encapsulate target protocol data into HTTP requests.

Generally speaking, when IPC works, you communicate with the target protocol sending a POST request with the body containing protocol commands. HTTP request headers are discarded as not-valid commands, but the POST body is actually executed.

  

In the book we exploit this behavior in order to send shellcode, the BeEF Bind shellcode originally written by Ty Miller for Windows and ported to Linux by Bart Leppens. BeEF Bind is a staging bind shellcode that acts like a minimal web server, returning the _Access-Control-Allow-Origin: \*_ HTTP response header and piping OS commands. In this way you can have the browser communicate via HTTP with the compromised box, and also a stealthier communication channel, as you can see in the diagram below:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivIrtWPIGaDZW_O1jDQEpvbxihykgEp22UFxvNP9Qq38eQs_aFEcdy-u5Fua-eZTBZh4M92y65VByH2DP6kBNaUAQUuL-sB83JwDEiywn7jDCLazB9FmH58ote9i_Kf-4Z9B66CynEgXqI/s1600/662090+c10f030.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivIrtWPIGaDZW_O1jDQEpvbxihykgEp22UFxvNP9Qq38eQs_aFEcdy-u5Fua-eZTBZh4M92y65VByH2DP6kBNaUAQUuL-sB83JwDEiywn7jDCLazB9FmH58ote9i_Kf-4Z9B66CynEgXqI/s1600/662090+c10f030.png)

  

So, this is the Browser Hacker's Handbook! Read it and experiment with the code at [https://browserhacker.com](https://browserhacker.com/) if you're interested in hacking browsers, web application security, or if you need to secure your (web-)infrastructure. Your browsers and intranets have never been more exposed! :-)

  
Cheers  
antisnatchor

#### [Source](http://blog.nibblesec.org/feeds/6317351427425189202/comments/default)

<br/>
---
