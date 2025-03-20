---
title: "Semgrep Rules for iOS Application Security (Swift)"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "ios-security"
  - "mapt"
  - "mobile-security"
  - "mstg"
  - "owasp"
  - "security"
  - "security-awareness"
  - "semgrep"
tags: 
  - "ios"
  - "swift"
---

  

![Swift_security](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6C9bCj_OW9A2TBw17WVTTFITXAYIqKMz_nzV4EYVXRRdmBxh0a5XEbnmJ6h85kj2CIDlI-G1yhYYul3YQmFZR0Vh5TqNBOxEWX5jlA5hl-00x0smkwZvR59bPsu91BWMaIOGyYfys88Q3WaiopDNn8VQ0KDZOq-TP0XuJ3LXuxH9gGHU0qht15AJPBcw/w200-h155/swift_sec2.png "Swift_security")

Nowadays, millions of people rely on iOS mobile applications for almost everything. As a result iOS devices manage a significant amount of data including sensitive ones, such as: _credentials_, _health data_, _payment data_ and so on. 

For these reasons ensuring the security of iOS applications is more critical than ever when developing iOS applications. Securely handling sensitive data and minimizing vulnerabilities are essential core concepts for developers when coding iOS applications, using **Swift** and **Objective-C** languages.

  

**Swift** is an open-source object-oriented language mostly used on Apple platforms, which  allows to write software for various environments and devices such as: phones, tablets, desktops, servers, etc.

To harden the attack surface of the **iOS applications** it is crucial to analyze and test the code both manually and using automatic tools throughout the **SDLC.** 

This can be achieved by combining: 

- **SCA** (_Software Composition Analysis_) is the process that aims to identify the versions of the open source dependencies used by the application, and then verify if there are known vulnerabilities related to them.
- **DAST** (_Dynamic Analysis Security Testing_) is the process of testing an application while it is running in order to detect security weaknesses and vulnerabilities present on it.
- **SAST** (_Static Analysis Security Testing_) is a testing process focused in the static analysis of the source code to find security flaws and marking their precise location.

#### SAST, Swift and Semgrep

**SAST** involves the comprehensive examination of an application's source code to identify and rectify security vulnerabilities, coding errors, and potential threats. 

This process aims to uncover weaknesses that could be exploited by malicious threat actors to compromise the integrity, confidentiality, or availability of the application and its associated data.

In particular, focusing on SAST tools for iOS applications, currently it is **_difficult_** to find an _efficient_ _automated tool_ able to analyze **adequately** **_Swift_** code-bases (especially regarding open-source SAST tools). In fact, in some cases performing source code analysis of iOS applications could be complicated and cumbersome. 

In the information security landscape various SAST tools are available (open and closed source software), in these recent years undoubtedly **_Semgrep_** (semantic grep) is the one that is gaining increasing fame.

**Semgrep** OSS is a fast, open-source, static analysis tool for searching code, finding bugs, and enforcing code standards at editor, commit, and CI time. It supports many different languages and package managers, and it comes with a number of other tools to help make static analysis intuitive, accessible and integrated into CI/CD pipelines. 

Semgrep OSS permit to perform **intra-file analysis**, and together with it also is provided a collection of pre-written rules and testing code excerpts in its public-free Registry. Additionally, any "semgrepper" user could create his custom rules for any of the supported languages and test/enhance them using the public-free Semgrep Playground.

This SAST tool could be useful during secure source code review activities, in particular are appreciable its efficiency, speed, ease of use/configuration and rule's customization. 

For instance, supposing you want detect all the occurrences of a peculiar vulnerability that has been found during manual code analysis. If this vulnerability is not recognized by the public Semgrep rules, it would be very helpful writing a custom rule to identify the other occurrences of the same vulnerable code pattern, especially when analyzing large volumes of code.

#### No Country for Old Rules

Due the complete absence of Semgrep rules regarding iOS applications security for Swift code (Objective-C is not yet supported), in late 2022 I decided to create and publish my first **Semgrep rule-set for Swift based mobile applications:** 

_**akabe1-semgrep-rules**_

with the aim to fill this gap, and of course to improve my knowledge on how Semgrep works and to speed-up source code analysis activities during my work at IMQ MindedSecurity.

With pleasure I noticed that the project has aroused interest in the security community, maybe because it was the first one for Swift.. 

Even better! The rules are also currently used by some security products that integrate code scanners to CI/CD pipelines.

For each Semgrep rule, as other tools, points for improvement can be identified during their usage. In fact, a Semgrep rule-set could necessitate periodic tuning in terms of efficiency and code coverage, then new rules could be added and existent ones could be improved. Furthermore the rules should evolve as the Semgrep syntax/features change and the target code evolve. 

For these reasons, I spent some spare time this year to add new rules and make some improvements to the existing ones for the Swift security rule-set. 

The newly released rules not only increase the coverage of iOS mobile apps security issues and improve the speed, but they also further reduce the percentage of false negatives/positives.

For the sake of completeness, it should be pointed out that currently writing Semgrep rules for Swift is not so easy. In fact, there are some obstacles to overcome during development and testing phases:

- the **Semgrep support for Swift language is still** **experimental**, this means that some specific pattern syntax necessary to write the Swift rules are not yet completely available and functioning
- the fact that almost all **iOS mobile apps have closed source code** reduces the opportunities to test extensively the Swift rules (there are few open-source Swift repos)

These problems, as can be guessed, make the development of efficient and effective Swift security rules for Semgrep a quite slow and effort-intensive process. As a result, there's always room for further improvements to my Swift rule set.

#### Project Details

The "_**akabe1-semgrep-rules**_" project is a collection of **Semgrep** **rules** for **Swift** language - there's also some rule for Java - that are based on my experiences in the security field.

The Swift rule-set cover many of the checklist of the **OWASP** **Mobile Security Testing Guide** **for iOS** and has been developed by studying the Apple documentation, analyzing Swift source code, and sometimes writing purposely vulnerable Swift code by myself.

For developers and testers working with **Swift-based iOS applications**, these Semgrep rules could be a useful resource to include into SDLC processes, in order to detect in Swift code-bases:

- Vulnerabilities
- Coding bad-practices
- Security misconfiguration issues 

The rules cover a wide range of topics for iOS apps security, including:

- Biometric Authentication issues
- Certificate Pinning issues
- External XML Entities (XXE) issues
- SQL Injection issues
- Cryptographic issues
- Log Injection issues
- NoSQL Injection issues
- WebView issues
- Insecure Storage issues
- Keychain Settings issues
- Critical Device Feature issues
- Hardcoded Sensitive Data issues

  

Below is reported a summary table:

| N° | Security Issue | Rule Name | Rule Description | OWASP MASTG Category |
| --- | --- | --- | --- | --- |
| 1 | Biometric Authentication | improper\_biometric\_auth | Find insecure biometric authentication mechanisms | MASVS-AUTH |
| 2 | Certificate Pinning | afnetworking\_pinning | Find security misconfigurations for certificate pinnijng via AFNetworking libraries | MASVS-NETWORK |
| 3 | Certificate Pinning | alamofire\_pinning | Find security misconfigurations for certificate pinnijng via Alamofire libraries | MASVS-NETWORK |
| 4 | Certificate Pinning | trustkit\_pinning | Find security misconfigurations for certificate pinnijng via Trustkit libraries | MASVS-NETWORK |
| 5 | XXE | xxe | Detect if the resolution of XML external entities is enabled | MASVS-CODE |
| 6 | SQL Injection | sqli\_query | Detect SQL queries built insecurely | MASVS-CODE |
| 7 | Hardcoded Secrets | hardcoded\_secret | Detect secrets hardcoded into Swift source code | MASVS-STORAGE |
| 8 | Log Injection | log\_inj | Detect if logs are written using untrusted input | MASVS-CODE |
| 9 | NoSQL Injection | nosql\_inj | Detect NoSQL queries built insecurely | MASVS-CODE |
| 10 | WebView Misconfiguration | improper\_wkwebview | Detect security misconfigurations on WKWebview | MASVS-PLATFORM |
| 11 | WebView Deprecation | insecure\_webview | Find usages of insecure/deprecated webviews (UIWebview and SFSafariViewController) | MASVS-PLATFORM |
| 12 | Insecure Storage | insecure\_storage | Find usages of cleartext storage mechanisms | MASVS-STORAGE |
| 13 | Insecure Storage | none\_file\_protection\_part1 | Find occurrences of some specific insufficient data protection classes | MASVS-STORAGE |
| 14 | Insecure Storage | none\_file\_protection\_part2 | Find occurrences of some specific insufficient data protection classes | MASVS-STORAGE |
| 15 | Insecure Storage | weak\_file\_protection\_part1 | Find occurrences of some specific weak data protection classes | MASVS-STORAGE |
| 16 | Insecure Storage | weak\_file\_protection\_part2 | Find occurrences of some specific weak data protection classes | MASVS-STORAGE |
| 17 | Keychain Settings | exportable\_keychain | Find exportable configurations of iOS Keychain | MASVS-STORAGE |
| 18 | Keychain Settings | weak\_keychain | Find weak configurations of iOS Keychain | MASVS-STORAGE |
| 19 | Critical Device Features | critical\_device\_features | Find the usages of critical device features (SMS/Mail sending, phone calls) | MASVS-PLATFORM |
| 20 | Broken Cryptography | broken\_commoncrypto | Find usages of insecure cryptography | MASVS-CRYPTO |
| 21 | Broken Cryptography | broken\_crypto\_idz | Find usages of insecure cryptography | MASVS-CRYPTO |
| 22 | Broken Cryptography | broken\_cryptoswift | Find usages of insecure cryptography | MASVS-CRYPTO |
| 23 | Broken Cryptography | broken\_rncrypt | Find usages of insecure cryptography | MASVS-CRYPTO |
| 24 | Broken Cryptography | broken\_swiftcrypto\_sodium | Find usages of insecure cryptography | MASVS-CRYPTO |

  

#### Conclusions

The secure source code analysis plays a pivotal role in bolstering the security of software development, including iOS mobile applications. 

By conducting thorough static code analysis, developers/testers can identify and remediate security vulnerabilities, promote compliance with industry standards and best practices, and enhance the overall resilience of their applications against emerging threats. 

Nevertheless the manual analysis performed by security professional people remains indispensable, the introduction of SAST automated instruments with tailored rule-sets facilitates and speeds-up the secure source code reviews. 

As the mobile landscape continues to evolve, integrating secure source code analysis into the development workflow remains essential for safeguarding user data, maintaining user trust, and mitigating the risk of security breaches and cyber attacks.

  

Go to Source
