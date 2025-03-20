---
title: "Spotting the Adversary with Windows Event Log Monitoring Part II"
date: 2015-09-21T16:43:00.000-04:00
draft: false
type: posts
---
# Spotting the Adversary with Windows Event Log Monitoring Part II

<br/>

<br/>
Events to Monitor in a Windows Environment
------------------------------------------

Part II

  

Recently, on [part I](http://www.redblue.team/2015/09/spotting-adversary-with-windows-event.html) of Spotting the Adversary with Window Event Log Monitoring,  I walked you through the first half of [NSA's guide](https://www.nsa.gov/ia/_files/app/spotting_the_adversary_with_windows_event_log_monitoring.pdf) to spotting malicious behaviour using Windows event logs. In part II I will go through the second half of the guide:

9.  AppLocker
10.  System or Service Failures
11.  Windows Update Errors
12.  Kernel Driver Signing Errors
13.  Group Policy Errors
14.  Mobile Device Activities
15.  Printing Services
16.  Windows Firewall

### 9\. AppLocker

Windows 7/2008R2 introduced Microsoft's [AppLocker](https://technet.microsoft.com/en-us/library/ee424367\(v=ws.10\).aspx), which Microsoft describes as:  

> AppLocker is a new feature in Windows Server 2008 R2 and Windows 7 that advances the features and functionality of Software Restriction Policies. AppLocker contains new capabilities and extensions that allow you to create rules to allow or deny applications from running based on unique identities of files and to specify which users or groups can run those applications.

It is a great product for  restricting the authorized software on a user's machine or production server. As such, when AppLocker is installed, reports and alerts should be configured that notify you when an AppLocker violation occurs. Recommended alerts include:  

-   Alerts on AppLocker violations on production servers outside of change windows
-   Reports on all user AppLocker violations

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjsAjCorvh2FAooyRczVMyonGfWOGRq85UTbr3ESA_Qjd2joQe2o-YDMTKR6f_MugZxK0jaeeVljTa04rBg9Yq8qtW5TqOqwJznKPntQOPPa6RWgt6aqExRI9UDtmpMfP-H9dW-LMqnSOcw/s1600/4.1+AppLocker.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjsAjCorvh2FAooyRczVMyonGfWOGRq85UTbr3ESA_Qjd2joQe2o-YDMTKR6f_MugZxK0jaeeVljTa04rBg9Yq8qtW5TqOqwJznKPntQOPPa6RWgt6aqExRI9UDtmpMfP-H9dW-LMqnSOcw/s1600/4.1+AppLocker.png)

Table-1: AppLocker Blocked Events

The server alert,  of course, depends on the size of your environment and control over the machines. If this is unreasonable,  you may want to focus the alert on high-severity servers and create a report for everything else.  
  

### 10\. System or Service Failures

This is a difficult set of alerts/reports to properly utilize. A system or service failure should not happen on a regular occasion. That said, something on my personal security environment seems to fail on a bi-daily occasion. On the other hand, if a Windows service continues to fail repeatedly on the same machines, it may indicate that an attacker is targeting the service.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgdw-Hbikx8M6wiWpjSVwkssqQ_2u6cSOhST2o58N-NHqi4kmIu96Qo0Kf7klCa0ahOsQN3GRTTrgxDBQWQ5OGCIsJ8prpmLRllV4D34xtBZurrWzlcToDLk0b8hpPbWitL60Q4RKzzEMez/s1600/4.3+Windows+Service+Fails+or+Crashes.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgdw-Hbikx8M6wiWpjSVwkssqQ_2u6cSOhST2o58N-NHqi4kmIu96Qo0Kf7klCa0ahOsQN3GRTTrgxDBQWQ5OGCIsJ8prpmLRllV4D34xtBZurrWzlcToDLk0b8hpPbWitL60Q4RKzzEMez/s1600/4.3+Windows+Service+Fails+or+Crashes.png)

Table-2: System or Service Failures

My recommendation is to focus on the absolute highest-severity systems at first, to take a report for the first couple of weeks of system/service failures, get a feel for your environment and then create alerts/reports depending on the stability of your environment because one malfunctioning system crashing every couple of hours to minutes can make your entire SIEM environment noisy and a factor to ignore instead of respond.  
  

### 11\. Windows Update Errors

Depending on how your environment's update policy is applied, an ad-hoc or scheduled report of Windows Update Errors should be created to be able to identify Microsoft Windows Update failures. Typically my recommendation is at least a report, and an alert for high-severity assets if constant diligence is a factor.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh1myedgLysxRRn1DlWWT5hW2aw-9Jip0gLGY6UObFbKylKh_qDnhDEoSLAkjf-T5Sd8WV_MvltqBBwj6Yga8AAArqEGgOXx-FKUfEOklBWoIyj_hzPiZwsjK0dSpZj4qorbiJ0L7ciFjFC/s1600/4.4+Windows+Update+Errors.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh1myedgLysxRRn1DlWWT5hW2aw-9Jip0gLGY6UObFbKylKh_qDnhDEoSLAkjf-T5Sd8WV_MvltqBBwj6Yga8AAArqEGgOXx-FKUfEOklBWoIyj_hzPiZwsjK0dSpZj4qorbiJ0L7ciFjFC/s1600/4.4+Windows+Update+Errors.png)

Table-3: Microsoft Windows Update Errors

### 12\. Kernel Driver Signing Errors

Microsoft introduced Kernel driver signing in Microsoft Vista 64-bit to improve defense against inserting malicious drivers or code into the kernel. Typically any indication of a protected driver violation may indicate malicious activity or a disk error and warrants investigation; however, much like the System or Service Failures, I recommend creating a report and monitoring your environment for a few weeks to ensure that there are no repeat offenders that will spam your SIEM alerting engine.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjC03rIcqBqK7QibXCj0KMV5PbANvCw2mWNZ-ZGKpf0D6j6m45c04DDAn4FrDDN6nwPwWSyK66dSf_CErqCgXAQZwg55MhQq82ME1YSXvL-JjvSuVtajLQeXiMb3Qz5SrkLZTi0fDO1F3CK/s1600/4.9+Kernel+Driver+Signing.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjC03rIcqBqK7QibXCj0KMV5PbANvCw2mWNZ-ZGKpf0D6j6m45c04DDAn4FrDDN6nwPwWSyK66dSf_CErqCgXAQZwg55MhQq82ME1YSXvL-JjvSuVtajLQeXiMb3Qz5SrkLZTi0fDO1F3CK/s1600/4.9+Kernel+Driver+Signing.png)

Table-4: Kernel Driver Signing Errors

### 13\. Group Policy Errors

Microsoft's built-in group policy functionality is an amazing way to ensure consistent security and configuration properties are applied to all Windows domain machines within your environment. The inability to apply a group-policy setting should be investigated immediately to determine the source. Depending on the size of your domain and frequency of GPO updates, I may recommend a report over an alert just because one failure across your domain could provide thousands of alerts instantly.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgr_sP1kbmACe_8PtgjYHxxKt5Ht6KHW0f654VhWsA4T9CrrhjIly3ezOCwOE8aQIhdAZuz_eZeQXJm0WC9aGobC26YmYyWDo9-YnvvoobMoNlHy-3b_1O8e5y_M7Q-DI3YMAK-H7SGfX84/s1600/4.10+Group+Policy+Errors.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgr_sP1kbmACe_8PtgjYHxxKt5Ht6KHW0f654VhWsA4T9CrrhjIly3ezOCwOE8aQIhdAZuz_eZeQXJm0WC9aGobC26YmYyWDo9-YnvvoobMoNlHy-3b_1O8e5y_M7Q-DI3YMAK-H7SGfX84/s1600/4.10+Group+Policy+Errors.png)

Table-5: Group Policy Errors

### 14\. Mobile Device Activities

Mobile activities on a system are part of daily operations, so there is no need to report or alert directly on them. However, an abnormal number of disconnects or wireless association status messages may indicate a rogue wifi hotspot attempting to intercept your user's wireless traffic. Therefore, it may be important to log mobile activity traffic, and, if your SIEM is capable, to create a threshold or trending alert to notify you when there are an above-average number of mobile events.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFigRdhjnb2lZVy8uXzhd5ZRX-Lj0pmElDQYMs2WdDVLE6Pzjs4C_3nnzwPq378dCtH_ZooAjPee_5OZergmgV7foTTHmsp62DOSWgrrha19aYj_zrE_5lDblbqPJl7c4QoPPJVXV5Pil3/s1600/4.12+Mobile+Device+Activities.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFigRdhjnb2lZVy8uXzhd5ZRX-Lj0pmElDQYMs2WdDVLE6Pzjs4C_3nnzwPq378dCtH_ZooAjPee_5OZergmgV7foTTHmsp62DOSWgrrha19aYj_zrE_5lDblbqPJl7c4QoPPJVXV5Pil3/s1600/4.12+Mobile+Device+Activities.png)

Table-6: Mobile Device Activities

### 15\. Printing Services

Microsoft Print Service events are more of a good-to-have event useful for tracing who printed what in case of an internal data leak. That said, the massive amount of events that a print server can make tracking printing events prohibitive (I have seen print servers generate more events than an active directory server.) If possible, you way wish to offload printer events to a log server instead of a SIEM for historical service. A simple SNARE/ELK/Syslog-NG solution may work very well.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgJWUHA9Rzw-aQuFdf2FcJNDySf-rdxO7dPlJc5CpH47zsphmBX8dIO5CxzCWuZv__X6v9aIcF8qAfutbabBTEw-9BGl-hzX8C10K3070IGV21cy8kRLBswlPTRdGNyEUhLeglqVKB5w0oi/s1600/4.14+Printing+Services.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgJWUHA9Rzw-aQuFdf2FcJNDySf-rdxO7dPlJc5CpH47zsphmBX8dIO5CxzCWuZv__X6v9aIcF8qAfutbabBTEw-9BGl-hzX8C10K3070IGV21cy8kRLBswlPTRdGNyEUhLeglqVKB5w0oi/s1600/4.14+Printing+Services.png)

Table-7: Printing Services

### 16\. Windows Firewall

Windows Firewall modifications or service status outside of a change window/Windows update should not take place. As such, an alert indicating a firewall policy modification or status change is recommended for servers and high-severity assets. A report for user activities may be prudent depending on whether your users have administrative access or not (hopefully not!)  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQc4nrPtTnktWBl1-hpLlhkfkFdwklT1zPRl-NDiJR7AZRTy4ZHUQmXr2HuUwAR96qDH6l9zapcvkkd1wozwIUEmgJieSNgW_VqKESH-hGyqrdI7ol0hmE4j188kIA4hRtTaylERM9Xs_E/s1600/4.5+Windows+Firewall.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQc4nrPtTnktWBl1-hpLlhkfkFdwklT1zPRl-NDiJR7AZRTy4ZHUQmXr2HuUwAR96qDH6l9zapcvkkd1wozwIUEmgJieSNgW_VqKESH-hGyqrdI7ol0hmE4j188kIA4hRtTaylERM9Xs_E/s1600/4.5+Windows+Firewall.png)

Table-8: Windows Firewall

There you have it. The NSA's Spotting the Adversary with Windows Event Log Monitoring is an excellent starting guide for alerts/reports in a new SIEM environment. My only complaint is that they forgot Windows Task schedules, which are often used by malicious entities for privilege escalation, malicious code execution, etc. Otherwise, hopefully between the NSA's original guide and this breakdown you have a healthy set of alerts and reports to start with.

#### [Source](https://www.redblue.team/feeds/8848967115207695016/comments/default)

<br/>
---
