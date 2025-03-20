---
title: "Know Your Tools"
date: Thu, 20 Mar 2025 16:45:40 +0000
draft: false
type: posts
categories: 
- Malware News
---
# Know Your Tools

<br/>

<br/>
In 1998, I was in a role where I was leading teams on-site to conduct vulnerability assessments for

[![](https://blogger.googleusercontent.com/img/a/AVvXsEhpRTQMXuF03TSXhbt1X7mgrjfMGPbaVY1_bawD6HFtrBOU64SYpjWrL-Z0ZFi1Em-gkMZ2GjkdsiJ9Zb4LiEX6NSZhvJQetzhymP4xcbDv1jml_v8soHzM8GkNRJLnVQMjIzNTlyE1PgVFTCNZP58ZYeOokcE6hdrme516vIoO4yvo6iEMKA)](https://blogger.googleusercontent.com/img/a/AVvXsEhpRTQMXuF03TSXhbt1X7mgrjfMGPbaVY1_bawD6HFtrBOU64SYpjWrL-Z0ZFi1Em-gkMZ2GjkdsiJ9Zb4LiEX6NSZhvJQetzhymP4xcbDv1jml_v8soHzM8GkNRJLnVQMjIzNTlyE1PgVFTCNZP58ZYeOokcE6hdrme516vIoO4yvo6iEMKA)

organizations. For the technical part of the assessments, we were using ISS's Internet Scanner product, which was a commercial scanner. Several years prior, while I was in graduate school, [the SATAN scanner](https://en.wikipedia.org/wiki/Security_Administrator_Tool_for_Analyzing_Networks) had been released, but it was open source, and you could look at the code and see what it was doing. This wasn't the case with Internet Scanner.

What we started to see, when we began looking closely, was that the commercial product was returning results that weren't...well...correct. One really huge example was the _AutoAdminLogon_ setting; you could set this value to "1", and the Administrator account name you chose would be included in another value, and the password would be included in a third value, _in plain text_. When the system was restarted, those credentials would be used to automatically login to the system.

Yep. _Plain text_.

Anyway, we ran a scan across an office within a larger organization, and the product returned 21 instances where the _AutoAdminLogon_ capability was enabled. However, the organization knew that only one had that functionality actually set; the other 20 had had it set at one point, but the capability had been disabled. On those 20 systems, the _AutoAdminLogon_ value was set to "0". We determined that the commercial product was checking for the existence of the _AutoAdminLogon_ value only, and not going beyond that...not checking to see if the value was set to "1", and not checking to see if the value that contained the plain text password actually existed. 

We found a wide range of other checks that were similarly incorrect, and others that were highly suspect. So, we started writing a tool to replace the commercial product, called _NTCAT_. This tool had various queries, all of which were based on research and included references to the Microsoft KnowledgeBase, so anyone running the tool could look it up, understand what was being queried, what responses meant, and could explain it to the customer.

[![](https://blogger.googleusercontent.com/img/a/AVvXsEhX8m56ITiY2rH-3rhLG1XEbo6VmYJZY6CMndJ1GbKcGzusdrqXFMLLJSOaR7Se8gFAfv2QTNjv7mvue2Zc95treevRzgzaaSReoYFmtv2idemBrggYxe4kStZwqPAqdY_wTv8vexgRv4LKU2QnofUyWkXV1Joi7Y0QVa59g_fKxQDLZXM5pw)](https://blogger.googleusercontent.com/img/a/AVvXsEhX8m56ITiY2rH-3rhLG1XEbo6VmYJZY6CMndJ1GbKcGzusdrqXFMLLJSOaR7Se8gFAfv2QTNjv7mvue2Zc95treevRzgzaaSReoYFmtv2idemBrggYxe4kStZwqPAqdY_wTv8vexgRv4LKU2QnofUyWkXV1Joi7Y0QVa59g_fKxQDLZXM5pw)

Later, when supporting CBP in a consulting role and waiting for my agency clearance, the Nessus scanner was a scanning tool that was popular at the time. One day, I heard a senior member of the CIRT talking to someone else who was awaiting their clearance, telling them that the Nessus scanner determined the version of the Windows operating system by firing off a series of specially crafted TCP packets at the target endpoint (perhaps via nmap), and then mapping the responses to a matrix. I listened, thinking that was terribly complicated. I found a system with Nessus installed, and started reading through the various modules, and found that Nessus determined the version of Windows by attempting to make an SMB connection to the target endpoint, and reading the Registry. If the scanner was run without the necessary privileges, a lot of the modules would not connect, and would simply fail. 

Jump forward to 2007 and 2008, and the IBM ISS X-Force ERS team was well into performing PCI forensic exams. At the time, we were one of seven companies on a list of certified organizations; merchants informed by their banks that they needed a forensic exam would go to the PCI web site, find the names of the companies listed, and invariably call through all seven companies to see who could offer the best (i.e., lowest) price, not realizing that, at the time, the price was set by Visa (this is prior to the PCI Council being stood up).

As our team was growing, and because we were required to meet very stringent timelines regarding providing information and reporting, [Chris Pogue](https://www.linkedin.com/in/christopher-pogue-msis-6148441/) and I settled on a particular commercial tool that most of our analysts were familiar with, and provided the documented procedures for them to move efficiently through the required processes, including file name, hash, path searches, and scans for credit card numbers.

During one particular engagement, someone from the merchant site informed us that JCB and Discover cards were processed by the merchant. This was important, because our PCI liaison needed to get points of contact at those brands, so we could share compromised credit card numbers (CCNs). We started doing our work, and found that we weren't getting any hits for those two card brands.

The first step was to confirm that, in fact, the merchant was processing those brands...we got the thumbs-up. Next, we went to the brands, got testing CCNs, and ran our process across those numbers...only to get zero hits. Nada. Nothing. Zippo. 

It turned out that the commercial suite we were using included an internal function called _IsValidCreditCard()_, and through extensive testing, and more than a few queries on the user forums, found out that the function did not recognize those two brands as valid. So, with some outside assistance, we wrote a function call to override the internal function call, and had everyone on our team add it to their systems. The new function ran a bit slower, but Chris and I were adamant with the team that long-running processes like credit card and AV scans should be run in the evening, not started at the beginning of your work. This way, you didn't tie up an image with a long running process when you could be doing actual work. 

In 2020, I was working at an IR consulting provider, and found that some of the team used [CyLR](https://github.com/orlikoski/CyLR); at the time, the middleware was [plaso](https://github.com/log2timeline/plaso), and the backend was Kibana. In March of that year, Microsoft released a fascinating blog post regarding [human-operated ransomware](https://www.microsoft.com/en-us/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/), in which they described the DoppelPaymer ransomware as using WMI persistence. Knowing that the team had encountered multiple ransomware engagements involving that particular variant, I asked if they'd seen any WMI persistence. They responded, "no". I asked, how did you determine that? They responded that they'd check the Kibana output for those engagements, and got no results.

The collection process for that toolset obtained a copy of the WMI repository, so I was curious as to why no results were observed. I then found out that, at least at the time, plaso did not have a parser for the WMI repository; as such, the data was collected, but not parsed...and the result of "no findings" in the backend was accepted without question. 

All of this is just to say that it's important to know and understand how your tools work. When I ran an internal SOC, the L3 DF analysts were able to dump memory from endpoints using the toolset we employed. Very often, they would do so in order to check for a particular IP address; however, most of them felt that running strings and searching for the IP address in question was sufficient. I had to work with them to get them to understand that (a) IP addresses, for the most part, are not stored in memory in ASCII/Unicode, and (b) different tools (Volatility, bulk\_extractor) look for different structures in order to identify IP addresses. So, if they were going to dump memory, running strings was neither a sufficient nor appropriate approach to looking for IP addresses. 

Know how your tools work, how they do what they do. Understand their capabilities and limitations. Sometimes you may encounter a situation or circumstance that you hadn't thought of previously, and you'll have to engage, ask questions, and intentionally engage in order to make a determination as the tools ability to address the issue.

> Introduction to Malware Binary Triage (IMBT) Course
> ---------------------------------------------------
> 
> Looking to level up your skills? Get **10% off** using coupon code: **MWNEWS10** for any flavor.
> 
> [Enroll Now and Save 10%: Coupon Code MWNEWS10](https://training.invokere.com/link/QHLuD5/MWNEWS10?url=https%3A%2F%2Ftraining.invokere.com)
> 
> _Note: Affiliate link – your enrollment helps support this platform at no extra cost to you._

Article Link: [Windows Incident Response: Know Your Tools](https://windowsir.blogspot.com/2025/03/know-your-tools.html)

1 post - 1 participant

[Read full topic](https://malware.news/t/know-your-tools/92331)

#### [Source](https://malware.news/t/know-your-tools/92331)

<br/>
---
