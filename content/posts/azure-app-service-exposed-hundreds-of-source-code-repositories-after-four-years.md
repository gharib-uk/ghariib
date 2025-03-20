---
title: "Azure App Service Exposed Hundreds of Source Code Repositories after four years."
date: 2025-01-08
categories: 
  - "cybercrime"
  - "cybermaniac"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "threat-intelligence"
tags: 
  - "azure"
  - "ccas"
  - "cyber-crime-news"
  - "cybercrimeawareness"
  - "flaw"
  - "microsoft"
  - "tech"
  - "vulnerability"
---

A security flaw has been discovered in Microsoft’s Azure App Service that exposed source code for customer applications written in Java, Node, PHP, Python, and Ruby for at least four years since September 2017.

According to Wiz researchers, the vulnerability, codenamed “Not Legit,” was first reported to the tech giant on October 7, 2021, and mitigations were implemented to fix the bug in November. Microsoft said “a limited subset of customers” are at risk. “Customers who deployed code to App Service Linux via Local Git after files had already been created in the application are the only impacted customers.”

Azure App Service (aka Azure Web Apps) is a platform for building and hosting web applications on the cloud. Source code and artifacts can be deployed to the service using a local Git repository, or via GitHub and Bit bucket repositories. When the Local Git method is used to deploy to Azure App Service, the Git repository is created within a publicly accessible directory **(home/site/wwwroot)** Microsoft does add a “web. config” file to the repository’s .git folder to restrict public access, but these configuration files are only used by C# or ASP.NET applications that rely on Microsoft’s IIS web servers, leaving out PHP, Ruby, Python, and Node apps that run on different web servers like Apache, Nginx, or Flask.

Shir Tamari, the Wiz researcher, said that a malicious actor simply had to fetch the ‘/.git’ directory from the target application and retrieve its source code. Malicious actors are continually searching the internet for exposed Git folders from which they can obtain secrets and intellectual property. Additionally, the leaked source code can often be used for more sophisticated dangerous attacks.

The post Azure App Service Exposed Hundreds of Source Code Repositories after four years. appeared first on Cyber Crime Awareness Society.

Go to Source
