---
title: "rare Сommand in Splunk"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/v1-84-400x234.jpg)

The `rare` command in Splunk helps you find the least common values in a specific field of your data. This is useful for spotting unusual or infrequent events. By default, the `rare` command in Splunk returns the 10 least common values for a specified field.

## Find Rare User Agents

To identify the least common user agents in your web access logs:

```
index=web sourcetype=access_logs | rare user_agent 
```

This returns 10 of the least common user agents.

## Identify Uncommon HTTP Status Codes

To detect rare HTTP status codes in your security logs:  
Here we set the limit of returns using `limit`

```
index=security sourcetype=security_logs | rare http_status_code limit=3
```

This returns the 3 least common HTTP status codes, helping to spot unusual responses.

  
  

The post rare Сommand in Splunk appeared first on SOC Prime.

Go to Source
