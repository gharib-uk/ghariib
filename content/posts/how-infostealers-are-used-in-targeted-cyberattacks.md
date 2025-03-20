---
title: "How infostealers are used in targeted cyberattacks"
date: 2025-01-03
categories: 
  - "cybersecurity"
  - "security"
---

Infostealer capabilities and how to protect your organization against this threat.

Although malicious programs that hunt for passwords, financial, and other sensitive data have been around for over 20 years, the word “infostealer” was coined only in the early 2010s. Recently, however, this relatively simple type of malware has been popping up in unexpected role — deployed as a springboard for major targeted hacks and cyberattacks. For example, the theft of the data of 500 million Ticketmaster customers and a ransomware attack on the Brazilian Ministry of Health were both traced to infostealers. The main challenge posed by infostealers is that they can’t be defeated solely at the infrastructure level and within a company’s perimeter. The non-work activities and personal devices of employees also need to be considered.

## Modern infostealers

Infostealers are programs indiscriminately installed on any accessible devices by threat actors looking to steal sensitive information of any kind. Their primary target is account passwords, crypto wallet credentials, credit card details, and browser cookies. The latter can be used to hijack a user session in an online service. In other words, if the victim is logged in to a work account in the browser, by copying cookies to another computer an attacker in some cases can gain access to it without even knowing the victim’s credentials.

Infostealers can also:

- Intercept email and chat messages
- Pilfer documents
- Steal images
- Take screenshots of the screen or windows of specific applications

And there are exotic specimens that apply optical character recognition to read text in JPG image files (pictures of passwords and financial data, for example). The infostealer sends all collected data to the C2 server, where it’s stored pending resale on the dark web.

Among recent years’ technical developments in the field of infostealers are: new methods of stealing data from protected browser storage, modular architecture for harvesting new types of data from already infected computers, and migration to a service model for distribution of this malware.

The cybercriminal market demands versatile infostealers, capable of data theft from dozens of browsers, crypto wallets, and popular applications, such as Steam and Telegram. The stealers must also be resistant to detection by security software, requiring developers to make frequent modifications to the malware, repackage it, equip it with anti-analysis and anti-debugging tools, and beef up its stealth. The “vendors” also often need to re-upload packaged malware to different hosting sites. This is necessary because old sources of malware are quickly blocked by infosec companies in cooperation with search engines and hosting providers.

Infostealers are mainly made for Windows and macOS systems — with the latter case being far from exotic but an up-and-coming segment in the cybercriminal market. There are stealers for Android, too.

Some common delivery channels for infostealers are spam and phishing, malicious advertising, and SEO poisoning. Besides campaigns involving infostealers kitted out with hacked software or game cheats, such malware may also be installed under the guise of a browser or antivirus update, as well as video conferencing applications. But in general, attackers monitor the zeitgeist and clothe their malware accordingly: this year, fake AI image generators were popular, and during the global CrowdStrike outage, there even appeared an infostealer masquerading as device recovery instructions.

## Infostealer ecosystem

A clear division of labor has taken root in the world of cybercrime. Some threat actors develop their own infostealers — plus the tools to manage them. Others get these programs onto victims’ devices using phishing and other techniques. Still others utilize stolen data. These three categories of criminals usually operate independently — not as one group, but they do have commercial relations with each other. The first of them increasingly offers infostealers under the malware-as-a-service (MaaS) model, often packaged with a handy cloud-based dashboard for customization.

The operators of actual attacks spread the malware but don’t use the stolen data themselves — instead putting large databases of harvested information up for sale on underground forums where other cybercriminals buy them and search for specific data they want using special tools. The same database can be purchased and repackaged many times: some buyers will extract gaming accounts, others look for bank card details or accounts in corporate systems. This latter type of data in particular has been gaining popularity since 2020 as threat actors have come to realize it provides a stealthy and effective way to penetrate an organization. Stolen accounts allow them to log in to a corporate system as a real user without exploiting any vulnerabilities or malware — thus arousing no suspicion.

The COVID-19 pandemic forced companies to make greater use of cloud services and allow remote access to their systems, causing the number of potentially vulnerable businesses to skyrocket. And more company employees are now using remote access from personal computers, where information security policies are less well-enforced (if at all). Thus, a home computer infected with an infostealer can ultimately lead to unwelcome guests in the corporate network.

Attackers who have obtained corporate credentials verify their validity and pass this filtered data to the operators of targeted cyberattacks.

## How to guard against infostealers

Securing every corporate computer and smartphone (EDR/EMM) is only the start. You need to also protect all employees’ **personal** devices against infostealers, and, in case of infection, mitigate the consequences. There are several ways to address this issue — some of which complement each other:

- Deny access to corporate systems from personal devices. The most drastic, inconvenient, and not-always-feasible solution. In any case, it doesn’t fix the problem entirely: for example, if your company uses public cloud services (email, file storage, CRM) for work tasks, a blanket ban will be impossible.
- Use group policies to disable browser synchronization on corporate computers so that passwords don’t end up on personal devices.
- Implement phishing-proof two-factor authentication at the corporate perimeter, in all important internal and public services.
- Make mandatory the installation of an Enterprise Mobility Management (EMM) solution on personal laptops and smartphones in order to monitor their security (check for up-to-date security solution databases, whether the solution is disabled, and whether the devices are password- and encryption-protected). A properly configured EMM system maintains strict separation of work and personal data on the employee’s device and doesn’t affect personal files and applications.
- Deploy an advanced identity management system (for the accounts of employees, devices, and software services) across your organization to help quickly locate and block accounts showing abnormal behavior; this will prevent, for example, employees from logging in to systems not needed for work or from suspicious locations.
- Get the latest dark-web threat intelligence with live reports on fresh leaks of your corporate data (including stolen accounts).

Go to Source
