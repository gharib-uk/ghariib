---
title: "Remote Working - Web Chats Threats and countermeasures"
date: 2020-05-18T02:59:00.000-07:00
draft: false
type: posts
categories: 
- chat,javascript,Remote Code Execution,remote working
---
# Remote Working - Web Chats Threats and countermeasures

<br/>

<br/>
### Introduction

With recent worldwide events, a sharply increasing number of companies are offering remote services to their customers. Even traditional businesses are implementing new features or pushing the migration of existing features to new needs of dematerialization of human-to-human relationships. 

Web chats are an example of such trends.

  

### Web chats

Rich messages web chats are a common feature implemented by companies to overcome the need of social distancing, while maintaining a close relationship with customers.

An example of rich messages web chat would be a graphical widget loaded by web site visitors to establish a chat session with a human operator, with the objective of sharing documents in a multimedia environment: users can share PDF files (e.g. personal documents, scans), video or audio files (e.g. vocal record of a formal declaration, acceptance of conditions and clauses for contracts, identity recognition) or even unpredicted formats, to deal with the abundance of multimedia files offered by end-user environments.

To do so, preview feature has a crucial role.

#### Scenario

As shown below, a typical scenario is a web server exposing chat capabilities, allowing human operators in a trusted network (e.g. a LAN) to interact with remote customers.

Usually, customers interact with the chat using a common browser over an untrusted network (the Internet, their own device); chat operators interact with customers using a browser or the backoffice component of the web chat, which commonly offers rich features, such as document viewing, session management, multiple channels interaction and capabilities to interact with customers in an "enhanced" manner.

  

This article will describe several attack vectors from potentially malicious remote customers against targeted chat operators and the software they use to interact with customers: the objective of described workflows is attacking the trusted internal corporate network.

  

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj3cXr-4yFOoNKG7uDkzur7Xvx1cjBqRznirKuNRssKVdF9ELVB8Vbm6kxhAmJSTfZy6r1Y1jaWvU6XpNwOgDDPsdt-2559ly-3XgSqcpDGjpNqQjZ7fTIc6P-qG5KTIC7GoHVfKm4BOWs2/s1600/Diagram1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj3cXr-4yFOoNKG7uDkzur7Xvx1cjBqRznirKuNRssKVdF9ELVB8Vbm6kxhAmJSTfZy6r1Y1jaWvU6XpNwOgDDPsdt-2559ly-3XgSqcpDGjpNqQjZ7fTIc6P-qG5KTIC7GoHVfKm4BOWs2/s1600/Diagram1.png)

  

#### Rich data exchange interaction

Backoffice chat components trie to recognize messages, files and URLs submitted by chat users with the aim of previewing them or offering operators with advanced tools to manipulate data.

Different recognition approaches are usually in place: parse file extensions, MIME type of uploaded files, and actually reading the real content of the file prior to supplying it to the operator.

If the recognition procedure does not thoroughly include a secure implementation of all the mentioned approaches, chat operators and internal resources may be prone to security threats.

  

### Threats

Web chats can accept several formats for files, sent by users (e.g. with drag & drop) or submitting URLs.

  

#### HTML payloads

The simplest and most known attack vector is abusing the HTML parser of web chats:

  

Customer enters <b>ciao!</b>

Operator sees **ciao!**

  

Customer enters <script>alert("XSS!")</script>

Operator sees:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7mPA450e6Ih0VbpFF-3yBQPawR8MSIP2oxWXSEDcXH9BV_DV4MfWcN1vYdGMgABlK0hYsLGtDJheoBbsng-G4EHOZzW4hlvzNDLrBisykXxlEcNJolH6QaUYygFPVoRsdUF1ol5VNrfKr/s640/xss.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7mPA450e6Ih0VbpFF-3yBQPawR8MSIP2oxWXSEDcXH9BV_DV4MfWcN1vYdGMgABlK0hYsLGtDJheoBbsng-G4EHOZzW4hlvzNDLrBisykXxlEcNJolH6QaUYygFPVoRsdUF1ol5VNrfKr/s1600/xss.png)

  

  

#### Malformed PDF file upload

User supplied PDF files can be opened by chat operators in embedded viewers in the backoffice component of the web chat or downloaded on their workstation, within the trusted corporate network.

Such files can be vehicle of arbitrary code (e.g. JavaScript or other active code), which can therefore be executed on the endpoint of chat operators, exploiting known vulnerabilities in the browser or in any other software used by operators to view the file.

  

As an example, PDF files can contain dynamic JavaScript code, very similar to how XSS attacks work:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgmtuZVLfA14OSmh2BqrPfOjtG8JpCZCPdSrGJK6fZQOBvY1pNzABQUPVJjPOsfjvqS0fI8VmmjaCZOSlFdPSQZotzR2W80wWoQJyK6w3vQlv_mh4KSfc3dld5MOrYuVHhwZ0ysxr_ynEF/s640/pdf_dynamic.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgmtuZVLfA14OSmh2BqrPfOjtG8JpCZCPdSrGJK6fZQOBvY1pNzABQUPVJjPOsfjvqS0fI8VmmjaCZOSlFdPSQZotzR2W80wWoQJyK6w3vQlv_mh4KSfc3dld5MOrYuVHhwZ0ysxr_ynEF/s1600/pdf_dynamic.png)

  

  

Thus, malformed PDFs, especially if loaded with an outdated Adobe Acrobat version, can be an attack vector for further exploits (e.g. meterpreter payloads, malwares, droppers..) against the internal network, the browser, the operating system or other components in the trusted backoffice environment.

  

Note: any security concern for PDF files should be extended to Office documents (Word, Excel, Powerpoint), especially if older and/or not hardened versions of Microsoft Office are in use.

For example threats may include malwares embedded in Office documents or CSV Formula Injection attacks.

  

#### Malicious URLs (Abusing preview feature)

Web chats tries to parse URLs pasted by users' messages, with the aim of previewing their content.

If the parsing procedure is executed correctly, the PDF is previewed by an embedded viewer, and it can therefore lead to scenarios described above.

On the contrary, if the parsing procedure is not correctly executed, the preview mechanism (triggered by the presence of ".PDF" string in the URL) can lead to unexpected events.

  

For example, if the URL ends with ".pdf" string, the web chat may attempt to dynamically load _any_ preview module. As shown below, ".pdf" in the URL does not indicate a real PDF file, but a folder named ".pdf" on an arbitrary web server.

  

Content of the attacker's web site:

  

  

$ ls ./.pdf -1

minded (executable file)

minded.html (malicious HTML file)

minded.pdf (malicious PDF file)

$ 

  

  

Behaviour of the preview on chat operator's software:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOY1JdERJoEVExDtiaQAYbthq2M8Kvm16HBMhdqQdXSybzmBZ1v02pwD1H7Rygv0-L85K8XJnwmQzY3h-j8ICeDAtOVVudgYIzeg6RMlL0jmzwycKO0NrGwVUZqRryJr56rQej_CM4_zGi/s640/directory_listing1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiOY1JdERJoEVExDtiaQAYbthq2M8Kvm16HBMhdqQdXSybzmBZ1v02pwD1H7Rygv0-L85K8XJnwmQzY3h-j8ICeDAtOVVudgYIzeg6RMlL0jmzwycKO0NrGwVUZqRryJr56rQej_CM4_zGi/s1600/directory_listing1.png)

  

  

Several social engineering scenarios can be constructed over this behaviour, for example convincing the chat operator (whose job is trying to efficently interact with a customer) to download other files.

  

Semi-automatic scenarios, on the other hand, can include the execution of arbitrary code in HTML files, abusing the preview feature. For example, it would be possible to spawn a BeEF HTML hook against the browser in use by the chat operator:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhp8VDVtUw_BVcqfwqqU038-gSrubBe9tTIzu40oh7ZX3BBsDI0dtV9-nTEe_qoUZhRUFv12CxSYtNszEIVXfqL08OeXoCGU5P2phjpHAQC49NZMLAX5AEgesnEl3WIo5k9vWeL-bZowhou/s640/pdf_active_BEEF1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhp8VDVtUw_BVcqfwqqU038-gSrubBe9tTIzu40oh7ZX3BBsDI0dtV9-nTEe_qoUZhRUFv12CxSYtNszEIVXfqL08OeXoCGU5P2phjpHAQC49NZMLAX5AEgesnEl3WIo5k9vWeL-bZowhou/s1600/pdf_active_BEEF1.png)

  

  

The command & control server (used by the attacker / evil customer) would look similar to the following:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiRrxn9se6Ph9khh_JOiXg0tzUpMNglTZ0QoZgpM10kZGvvJLn-0rgOEXc6HHVLTUJO7OjaoHOGlJHEz5HoJgImiu7UHdw8VOGe_IkZhsv8mCeN96bjn9Swgx6eiyTq7TSddACXPZ_i0jOS/s1600/pdf_active_beef2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiRrxn9se6Ph9khh_JOiXg0tzUpMNglTZ0QoZgpM10kZGvvJLn-0rgOEXc6HHVLTUJO7OjaoHOGlJHEz5HoJgImiu7UHdw8VOGe_IkZhsv8mCeN96bjn9Swgx6eiyTq7TSddACXPZ_i0jOS/s1600/pdf_active_beef2.png)

  

  

Consequently, the attacker can use a large plethora of social engineering / hijacking techniques.

  

For example, spawning fake system messages / Java Applet load requests:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEid4D5AH6Udstu2qbmorw3WyRNOyMZmxuWAbXNY5XCQQzB03Qzs2obt0P6jxxhog2zrmL5sXpbiPHKSEnbaL9TQr3Omj2V1xMqLN3_HZ4Q7pk7FqgglZ-3j8Ilfm1fQ_6xJHBZfh7u_R1Qm/s640/arbitrary_content3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEid4D5AH6Udstu2qbmorw3WyRNOyMZmxuWAbXNY5XCQQzB03Qzs2obt0P6jxxhog2zrmL5sXpbiPHKSEnbaL9TQr3Omj2V1xMqLN3_HZ4Q7pk7FqgglZ-3j8Ilfm1fQ_6xJHBZfh7u_R1Qm/s1600/arbitrary_content3.png)

  

  

Or even spawning fake Clippy Office Assistants:

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjwg8qOW3FSeGV-Eno-9QQJVkFGukQnspZTYROC7l6h8aJqbUVOQ3l1pqaRlIe3kz89qrJwEwLuQA_px3OLqOgMBCDdQITjGtLesNigkldRl_HQQDCrHRH-LJrxBqO3Ldi-k8xLHeMS3PuD/s640/arbitrary_content.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjwg8qOW3FSeGV-Eno-9QQJVkFGukQnspZTYROC7l6h8aJqbUVOQ3l1pqaRlIe3kz89qrJwEwLuQA_px3OLqOgMBCDdQITjGtLesNigkldRl_HQQDCrHRH-LJrxBqO3Ldi-k8xLHeMS3PuD/s1600/arbitrary_content.png)

  

  

#### Embedded players

Web chats may also include MP3 players, which, depending on the library in use by the chat software, may be prone to vulnerabilities related to outdated software modules.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuRymM6eJk66tic92-NT9XtuNa2CW49-HgRRWMlJwMosVE2x1B8NrlCIy0DmNgWJN6HlbJvZE7_KTJR0okrSDLLlrezKPnr-rUhkxDEWXZCqvGOGnSL4tF8MmgP3vxqgV-TO8C1SUu63Yb/s1600/mp3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuRymM6eJk66tic92-NT9XtuNa2CW49-HgRRWMlJwMosVE2x1B8NrlCIy0DmNgWJN6HlbJvZE7_KTJR0okrSDLLlrezKPnr-rUhkxDEWXZCqvGOGnSL4tF8MmgP3vxqgV-TO8C1SUu63Yb/s1600/mp3.png)

  

  

### Mitigations

-   Validate any uploaded file according to a predefined list of expected file types:

> \- Extension
> 
> \- Content Type
> 
> \- Actual content of the file

-   Rescale / Resize with a 1:1 ratio any multimedia file, before allowing chat operators to open the file, in the attempt of removing any metadata
-   Properly hardening procedures should be applied to any software in use by chat operators:

> \- Update PDF viewer software to the latest available version
> 
> \- Apply proper security options (e.g. Enhanced security and protected mode in Adobe Acrobat) to harden PDF viewer software

-   Define a list of allowed file types for the preview feature, avoiding any other format: if chat operators are expected to receive only URLs, documents and images, define a list where only PDFs and JPGs/PNGs are allowed, while any other extension is _excluded_ from previewing components.

  

### References

https://acrobatusers.com/assets/collections/tutorials/legacy/tech\_corners/javascript\_corner/tips/2006/popup\_windows\_part2/AlertBoxExamples.pdf

https://www.adobe.com/devnet-docs/acrobatetk/tools/AppSec/enhanced.html

https://owasp.org/www-community/attacks/CSV\_Injection

https://beefproject.com/

#### [Source](https://blog.mindedsecurity.com/feeds/2672280608940115729/comments/default)

<br/>
---
