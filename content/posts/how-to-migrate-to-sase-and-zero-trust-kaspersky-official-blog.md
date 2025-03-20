---
title: "How to migrate to SASE and zero trust | Kaspersky official blog"
date: 2025-01-29
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

SASE components: ZTNA, CASB CSWG, NGFW, SD-WAN, and how they improve network security

The traditional network security model — with a secure perimeter and encrypted channels for external access to that perimeter — is coming apart at the seams. Cloud services and remote working have challenged the very notion of “perimeter”, while the primary method of accessing the perimeter — VPN — has in recent years become a prime attack vector for intruders. Many high-profile hacks began by exploiting vulnerabilities in VPN solutions: CVE-2023-46805, CVE-2024-21887 and CVE-2024-21893 in Ivanti Connect Secure, and CVE-2023-4966 in Citrix solutions. By compromising a VPN server, which needs to be accessible online, intruders gain privileged access to an enterprise’s internal network and plenty of scope for covert attack development.

Server and enterprise applications are often configured to trust — and be accessible to — all intranet-based hosts, making it easier to find and exploit new vulnerabilities, and extract, encrypt, or destroy important data.

Often, VPN access is granted to company contractors too. If a contractor violates the information security requirements while being granted standard VPN access with extensive privileges in the corporate network, attackers can penetrate the network by compromising the contractor, and gain access to information through the latter’s accounts and privileges. And their activities can go unnoticed for a long time.

A radical solution to these network security issues requires a new approach in terms of network organization — one whereby each network connection is analyzed in detail, and participants’ credentials and access rights are checked. Any of them lacking explicit permission to work with a particular resource are denied access. This approach applies to both internal network services as well as public and cloud-based ones. Last year, cybersecurity agencies in the United States, Canada and New Zealand released joint guidance on how to migrate to this security model. It consists of the following tools and approaches.

## Zero trust

The zero trust model seeks to prevent unauthorized access to data and services through granular access control. Each request for access to a resource or microservice is analyzed separately, and the decision is based on a role-based access model and the principle of least privilege. During operation, every user, device, and application must undergo regular authentication and authorization — processes which are, of course, made invisible to the user by technical means. See our dedicated post for more about zero trust and its implementation.

## Secure service edge

Secure service edge (SSE) is a set of tools for securing applications and data regardless of users’ and their devices’ location. SSE helps implement zero trust, adapt to the realities of hybrid cloud infrastructure, protect SaaS applications, and simplify user verification. SSE components include zero trust network access (ZTNA), cloud secure web gateway (CSWG), cloud access security broker (CASB) and firewall-as-a-service (FWaaS).

## Zero trust network access

ZTNA provides secure remote access to a company’s data and services based on strictly defined access policies in line with zero trust principles. Even if intruders compromise an employee’s device, their ability to develop an attack is limited. For ZTNA, an agent application is deployed that checks the identity of the user or service, and access rights, then matches them with the policies and user-requested actions. Other factors that can be monitored are the security level of the client device (software versions, security solution database updates), the client’s location, and the like. The agent can also be used in multifactor authentication. Periodic reauthentication occurs during user sessions. If the user requires access to new resources and applications, the authentication and authorization process is repeated in full. However, depending on the solution settings, this may be transparent to the user.

## Cloud secure web gateway

CSWG protects both users and devices from online threats and helps enforce network policies. Features include filtering web connections by URL and content, controlling access to web services, and analyzing encrypted TLS/SSL connections. It’s also involved in user authentication and provides analytics on web application usage.

## Cloud access security broker

CASB helps enforce access policies for cloud SaaS applications — bridging them to their users, as well as manage data transferred between different cloud services. This makes it possible to detect threats targeting cloud services and unauthorized attempts to access cloud data, as well as to bring control of various SaaS applications under a single security policy.

## Firewall-as-a-service

Cloud-based FWaaS performs the functions of a traditional firewall — except that traffic analysis and filtering take place in the cloud instead of on a separate device in the company’s office. Besides the convenience of scalability, FWaaS makes it easier to protect a distributed infrastructure consisting of cloud and on-premises data centers, offices, and branches.

## Secure access service edge

Combining software-defined networks (SD-WAN) with full SSE functionality, SASE delivers the most effective integration of network control and security management. There are several advantages for companies in terms of not only security, but also cost efficiency:

- Reducing the cost of setting up a distributed network and combining different communication channels to increase speed and reliability
- Taking advantage of centralized network management, high visibility, and extensive analysis capabilities
- Lower administration costs due to automatic configuration and failure response
- All SSE functions (SWG, CASB, ZTNA, NGFW) can be integrated into the solution, giving defenders full visibility of all servers, services, users, ports, and protocols — plus automatic application of security policies when deploying new services or network segments
- Simplifying administration and policy enforcement with a centralized management interface

The SASE architecture allows all traffic to be routed dynamically and automatically, taking into account speed, reliability and security requirements. With information security requirements integrated deep into the network architecture, there is granular control over all network events — traffic is classified and inspected at multiple levels, including the application level. This delivers automatic access control as prescribed by zero trust, with granularity extending to a single application function and user rights in the current context.

The use of a single platform dramatically boosts monitoring performance and speeds up and improves incident response. SASE also simplifies updates and general management of network devices, which is another security benefit.

## Migration technicalities

Deploying the above solutions would help your company replace the traditional “perimeter behind firewall plus VPN” approach with a more secure, scalable, and cost-effective model, which factors in cloud solutions and employee mobility. At the same time, cybersecurity agencies that recommend this set of solutions warn that each case requires an in-depth analysis of a company’s requirements and current state of affairs, plus a risk analysis and step-by-step migration plan. When switching from VPN to SSE/SASE-based solutions, you must:

- Strictly limit access to the network control plane
- Separate and isolate the interface for managing the solution and the network
- Update the VPN solution and analyze its telemetry in detail to rule out the possibility of compromise
- Test the user authentication process and explore ways to simplify it, such as authentication in advance
- Use multifactor authentication
- Implement version control of the management configuration, and keep track of changes

Go to Source
