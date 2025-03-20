---
title: "GuardDuty detection bypass via cloudtrail"
date: Thu, 23 Apr 2020 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# GuardDuty detection bypass via cloudtrail

<br/>

<br/>
GuardDuty detected CloudTrail being outright disabled, but did not detect if an attacker with the necessary permissions filtered out all events from CloudTrail via PutEventSelectors, resulting in defenders having no logs to review. AWS fixed this issue by adding a GuardDuty detection that triggers if PutEventSelectors is used to disable all event types.

#### [Source](https://www.cloudvulndb.org/guardduty-cloudtrail-bypass)

<br/>
---
