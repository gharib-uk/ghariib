---
title: "2021-046-Mick Douglas Log4j vulnerabilities egress mitigations- part2"
date: Thu, 23 Dec 2021 03:37:59 +0000
draft: false
type: posts
---
# 2021-046-Mick Douglas Log4j vulnerabilities egress mitigations- part2

<br/>

<br/>
Introduction

Overview of Log4j vuln (as of 16 December 2021)

Why is it a big deal? (impact/criticality/risk)

Talk about patching vs. mitigation

why wasn’t this given the same visibility in 2009? Because it’s Oracle or Java?

Good callout is building slides to brief org leadership, detections, and other educational tools.

Vuln fatigue (Java vulns in 2009 and pretty much forever cause us fatigue)

Are there other technologies like log4j that prop up the entire world, and we just don’t know?

Egress traffic (discussed at length on twitter, what problems it solve?)

[https://twitter.com/mubix/status/1470430085169745920](https://twitter.com/mubix/status/1470430085169745920)

Latest: [https://www.theregister.com/2021/12/14/apache\_log4j\_v2\_16\_jndi\_disabled\_default/](https://www.theregister.com/2021/12/14/apache_log4j_v2_16_jndi_disabled_default/) \- apache removed JDNI functionality

[https://www.reddit.com/r/blueteamsec/comments/rd38z9/log4j\_0day\_being\_exploited/](https://www.reddit.com/r/blueteamsec/comments/rd38z9/log4j_0day_being_exploited/) <- great aggregation

[https://www.techsolvency.com/story-so-far/cve-2021-44228-log4j-log4shell/](https://www.techsolvency.com/story-so-far/cve-2021-44228-log4j-log4shell/?fbclid=IwAR1ngTChJGz7Q5M-qbH6nwPBV4FByPDfJb98iLt_dbQO4EpZXVDwAxz0F8w)

[https://issues.apache.org/jira/plugins/servlet/mobile#issue/LOG4J2-313](https://issues.apache.org/jira/plugins/servlet/mobile#issue/LOG4J2-313)

Lots of discussion about “SBOM solving the issue”. @K8em0 weighs in [https://twitter.com/k8em0/status/1469437490691932164](https://twitter.com/k8em0/status/1469437490691932164)

[https://gist.github.com/SwitHak/b66db3a06c2955a9cb71a8718970c592](https://gist.github.com/SwitHak/b66db3a06c2955a9cb71a8718970c592) \-list of advisories for log4j

Mitigation: [https://twitter.com/brunoborges/status/1469186875608875011](https://twitter.com/brunoborges/status/1469186875608875011)  
[https://twitter.com/DannyThomas/status/1469709039911129088](https://twitter.com/DannyThomas/status/1469709039911129088) (holy hell, 2009?!?)

2009 in fact, #CVE-2009-1094, then a bypass was fixed in CVE-2018-3149: [https://bugzilla.redhat.com/show\_bug.cgi?id=1639834](https://t.co/5TR5g9vqo5). That's when the JDK was fully protected, but other implementations remained vulnerable  
  

[https://bugzilla.redhat.com/show\_bug.cgi?id=1639834](https://bugzilla.redhat.com/show_bug.cgi?id=1639834)

OpenJDK… 

[https://twitter.com/ThinkstCanary/status/1469439743905697797?s=20](https://twitter.com/ThinkstCanary/status/1469439743905697797?s=20) 

You can use a point & click canarytoken from https://canarytokens.org to help test for the #log4j  / #Log4Shell issue.

1) visit https://canarytokens.org;

2) choose the Log4shell token;

3) enter the email address you wish to be notified at;

4) copy/use the returned string...

Discussed in 2016 at Blackhat: [https://twitter.com/th3\_protoCOL/status/1469644923028656130](https://twitter.com/th3_protoCOL/status/1469644923028656130)

The [#Log4Shell](https://twitter.com/hashtag/Log4Shell?src=hashtag_click) attack vector was known since 2016…   
  
[https://twitter.com/bettersafetynet/status/1469470284977745932](https://twitter.com/bettersafetynet/status/1469470284977745932)

Just got off phone with a client. Log4j is in their network. Vendor claims patch will be available next release... which is multiple months from now. Here's what you do if you're in this situation. 1. Keep calm. There's no need to panic. 2. Carefully read this thread.

When dealing with attacks like this you should remember the acronym IMMA. 

I = Isolate 

M = Minimize 

M = Monitor 

A = Active Defense

[https://github.com/MarkBaggett/srum-dump](https://github.com/MarkBaggett/srum-dump)

“SRUM Dump extracts information from the System Resource Utilization Management Database and creates a Excel spreadsheet.

The SRUM is one of the best sources for applications that have run on your system in the last 30 days and is invaluable to your incident investigations!

To use the tool you will need a copy of the SRUM (located in c:\\windows\\system32\\sru\\srudb.dat, but locked by the OS).

This tool also requires a SRUM\_TEMPLATE that defines table and field names. You can optionally provide the SOFTWARE registry hive and the tool will tell you which wireless networks were in use by applications.

If you are looking for a version of this tool that creates CSV files instead of an Excel spreadsheet, dumps targeted tables or processes any ese then check out [ese2csv.](https://github.com/MarkBaggett/ese-analyst) ese2csv.exe is designed specifically for csv files with the CLI user in mind.”

[https://support.microsoft.com/en-us/office/digitally-sign-your-macro-project-956e9cc8-bbf6-4365-8bfa-98505ecd1c01](https://support.microsoft.com/en-us/office/digitally-sign-your-macro-project-956e9cc8-bbf6-4365-8bfa-98505ecd1c01)

#### [Source](http://brakeingsecurity.com/2021-046)

<br/>
---
