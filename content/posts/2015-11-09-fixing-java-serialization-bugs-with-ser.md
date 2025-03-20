---
title: "Fixing Java Serialization Bugs with SerialKiller"
date: 2015-11-09T00:48:00.001-08:00
draft: false
type: posts
categories: 
- 0day,ikki,java,patch
---
# Fixing Java Serialization Bugs with SerialKiller

<br/>

<br/>
On Friday, [FoxGloveSecurity](http://foxglovesecurity.com/2015/11/06/what-do-weblogic-websphere-jboss-jenkins-opennms-and-your-application-have-in-common-this-vulnerability/) published a rather inaccurate and misleading blog post on five software vulnerabilities affecting WebLogic, WebSphere, JBoss, Jenkins and OpenNMS. By incorrectly attributing the vulnerability to the [Apache Commons Collection](https://commons.apache.org/proper/commons-collections/) library, the blog post generated misinformation on the root cause and possible fixes (e.g. [this news.softpedia article](http://news.softpedia.com/news/the-vulnerability-that-will-rock-the-entire-java-world-495840.shtml)).  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgiJ0f4fUV-OlHL7ivqCzAEmFAsMOSnbntbZGVmDYBax7hjcpN3k5t-UooACfxxo9syBX1-3Q73D6-y89cgVNb81xuP7MIHgabw7HPt_shQraOQk0byGY8aSXgrkFW6_QSDbREGUNzdUbk/s400/tweetthomas.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgiJ0f4fUV-OlHL7ivqCzAEmFAsMOSnbntbZGVmDYBax7hjcpN3k5t-UooACfxxo9syBX1-3Q73D6-y89cgVNb81xuP7MIHgabw7HPt_shQraOQk0byGY8aSXgrkFW6_QSDbREGUNzdUbk/s1600/tweetthomas.png)

  
If you're still unsure on what is the actual issue, Charles Miller published a short [blog post illustrating the problem](http://fishbowl.pastiche.org/2015/11/09/java_serialization_bug/).  
  
Probably thinking that the Apache project wasn't interested in fixing the bug, FoxGloveSecurity's post also contains working exploits for all products.  
  

> In fact, even though proof of concept code was released OVER 9 MONTHS AGO, none of the products mentioned in the title of this post have been patched, along with many more. In fact no patch is available for the Java library containing the vulnerability.

  
As it turned out, some [vendors were not aware](https://jenkins-ci.org/content/mitigating-unauthenticated-remote-code-execution-0-day-jenkins-cli) and others were [already working on a patch](https://twitter.com/matthias_kaiser/status/662739824642666497) in their products but haven't released it yet.  
  
Summing up, we're now dealing with five pre-authentication remote code execution vulnerabilities affecting major products. Luckily, the specific services affected by those vulnerabilities are generally not exposed over the Internet thus reducing the overall risk.  

  
Inspired by this story, I started thinking on how I could fix the problem in a systematic way. It didn't take me long to discover [this article](http://www.ibm.com/developerworks/library/se-lookahead/) on using method override to create a look-ahead deserialization filter. While the article explains a potential solution, it didn't provide an easy-to-use library that can be used to protect Java applications.  
  

### Introducing SerialKiller

[SerialKiller](https://github.com/ikkisoft/SerialKiller) is an easy-to-use look-ahead Java deserialization library to secure applications from untrusted input. You drop the jar in your classpath, use SerialKiller instead of the standard _java.io.ObjectInputStream_ and configure it to allow/block specific classes.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiiLh52rEsEfxJ3jPRqk87AlR_XZi0h9kxTou6KZ5l0WK085cG-sJ7Ysl89U15vDkp-oGH-TJFDil02cIjdd48DzyBRrd-CIYcdhmQSuGNklSmavQycpGmKwL04PbE9vNSkskJssqP21Hg/s640/skpayload.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiiLh52rEsEfxJ3jPRqk87AlR_XZi0h9kxTou6KZ5l0WK085cG-sJ7Ysl89U15vDkp-oGH-TJFDil02cIjdd48DzyBRrd-CIYcdhmQSuGNklSmavQycpGmKwL04PbE9vNSkskJssqP21Hg/s1600/skpayload.png)

  
The library, together with a simple tutorial, is available on Github:  
[https://github.com/ikkisoft/SerialKiller](https://github.com/ikkisoft/SerialKiller)  
  
At the moment, it supports the following features:  
  

-   **Hot-Reload** for the config file, so that you don't need to restart your application after changing SerialKiller's config
-   **Whitelisting**.  If you can quickly identify a list of trusted classes, this is the best way to secure your application. For instance, you could allow classes belonging to your application package only
-   **Blacklisting**. The default config file already includes a few known attack payloads (thanks to [YSoSerial](https://github.com/frohoff/ysoserial)). This can be used to block the exploits released by FoxGloveSecurity

  

If you want to contribute, [ping me on Twitter](http://twitter.com/_ikki) or [using Github](https://github.com/ikkisoft/SerialKiller).

#### [Source](http://blog.nibblesec.org/feeds/6458272097950349033/comments/default)

<br/>
---
