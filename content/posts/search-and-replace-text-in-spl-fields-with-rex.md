---
title: "Search and Replace Text in SPL Fields with rex"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/v1-100-400x234.jpg)

Sometimes when working with fields in SPL, it can be useful to search for and replace parts of text found in the field. Some reasons for doing this might be:  
– removing white space to reduce the size of the field  
– replacing field separators with characters that look nicer  
– rearranging values in a field in an order that is more appropriate (displaying names as first, last or last, first)

To replace text in a field, use the `rex` in sed mode using this syntax

```
| rex mode=sed field=<fieldname> "s/<whatyouwannachange>/<whatitshouldbeafterwards>/g"
```

  
  

The post Search and Replace Text in SPL Fields with rex appeared first on SOC Prime.

Go to Source
