---
title: "coalesce Function in Splunk"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/v1-57-400x234.jpg)

The Splunk `coalesce` function returns the first non-null value among its arguments. It’s useful for normalizing data from different sources with varying field names.

For example, to unify multiple source IP fields into a single `src_ip` field:

```
| eval src_ip = coalesce(src_ip, sourceip, source_ip, sip, ip)
```

  
  

The post coalesce Function in Splunk appeared first on SOC Prime.

Go to Source
