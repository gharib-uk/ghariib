---
title: "The growing challenge of secrets management in DevOps and how automation can help"
date: 2025-01-07
categories: 
  - "ai"
  - "ci-cd"
  - "cloud"
  - "cloudnative"
  - "data"
  - "development"
  - "devops"
  - "devops-cloud"
  - "security"
tags: 
  - "devsecops"
  - "digital-transformation"
  - "expert-pov"
  - "ndc"
---

![](https://www.devopsonline.co.uk/wp-content/uploads/2024/11/AI-webpost-1.jpg)

The age of agile software development and the cloud has caused an explosion in enterprise secrets. From passwords, encryption keys and APIs, to tokens and certificates, digital secrets are what control access when data is transferred between applications. They’re essential to maintaining the security of that data, and in turn, the successful operation of digital enterprises.

## **A top emerging security challenge**

It’s no surprise, then, that secrets are extremely valuable to threat actors. Businesses need a robust secrets management strategy in place in order to not only effectively manage the respective secrets across their lifecycle, but also protect them from compromise. This is becoming a significant challenge within DevOps specifically, with Thales’ Data Threat Report finding that secrets management was identified as the top emerging DevOps security challenge by respondents.

The number of non-human entities – such as apps, APIs, containers and microservices – has also significantly increased in the past few years, adding to the complexity. Applications have become more agile and flexible, increasingly relying on APIs to draw on other sets of data and services. DevOps teams in particular, who are right at the centre of creating and managing these applications, therefore have their secret management challenges. With dozens of orchestrations, configuration management and other tools used every day, these operations rely on an array of automation and other scripts that require secrets to work. Compromise could lead to software supply chain attacks, impersonation, or worse.

From an operational perspective, expired or unmanaged secrets can also cause system outages. Figures from the likes of the ITIC have put the cost of IT downtime at a minimum of $5,000 per minute, much of which will have been caused by misconfiguration, expired certificates, or other such issues that having rigorous secrets management in place would eliminate.

## **Eliminate ‘security islands’**

The tight deadlines and high expectations of modern software development often mean productivity is prioritised over security, to ensure speedy delivery. Secrets might for instance be hardcoded into applications or configuration files, to allow swift access to other applications and work quickly. If those credentials have privileged access, they can be extremely powerful in the wrong hands.

Manually sharing secrets, or keeping them to a limited few also creates ‘security islands’, where only a small group in an organisation have specialised access no one else does. This can be a major brake on productivity – not to mention increasing the chances of employees seeking workarounds or turning to shadow methods to get what they need to get done. Centralising the verification and generation of new credentials and secrets reduces the amount of manual work needed, and allows developers’ workflows to continue more easily.

It’s important to say this is not the fault of developers, who are often under extreme time pressures to deliver new builds and applications and aren’t necessarily motivated to dwell too long on the security implications. Instead, it is the responsibility of IT leaders to establish the tools and environment needed to ensure developers can easily incorporate good secrets management practices into everything they do.

## **Keeping secrets out of source code**

Secrets need to be kept separate from the source code, and using a centralised vault will help with that – developers can use an API call instead to bring the necessary secret in when they need to. A management policy that can support dynamic secrets is even better because these are time-limited. As they automatically expire, even if they are somehow compromised they will be of no use in attacking a business.

This is where automation comes in. Secrets can be set to automatically generate and rotate at a predetermined frequency, alongside their storage and distribution. By removing the human element from the secrets management process, you eliminate the use of default, hardcoded, or even duplicate secrets. Any secrets management policy – including those like Zero Trust -determined by the organisation becomes far easier to govern and enforce.

## **A crucial part of DevSecOps**

While most cloud providers have at least one service that offers secrets management, such as AWS’ secret manager, or Azure’s Key Vault, in a multi-cloud world, organisations may find it more effective to use a cloud-neutral secrets management solution that can work across multiple cloud providers, as well as any on-premise locations they might have too.

Centralised secrets management additionally makes logging access, monitoring usage and sending alerts more feasible – not to mention being able to scale access management with the scale of applications as they grow. There are also numerous government regulations around the world that mandate specific controls for efficient secret storage and access, from GDPR to HIPAA – with secrets management tools helping to achieve compliance.  

Alongside measures like strong encryption, firewalls and regular patching and updating, a good secrets management platform complements these measures and therefore helps fortify overall cybersecurity resilience.

* * *

### **Get in touch**

For event sponsorship enquiries, please get in touch at **olliver.toke@31media.co.uk** or **calum.budge@31media.co.uk**  
For media enquiries, please get in touch with **vaishnavi.nashte@31media.co.uk**

<figure>

![Digital Transformation Awards 2025](https://www.devopsonline.co.uk/wp-content/uploads/2024/09/DTA_header.png)

<figcaption>

_**Enter the Digital Transformation Awards 2025 here.**_

</figcaption>

</figure>

The post The growing challenge of secrets management in DevOps and how automation can help first appeared on DevOps Online.

Go to Source
