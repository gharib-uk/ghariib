---
title: "Bridging the Gap Elevating Red Team Assessments with Application Security Testing"
date: Thu, 05 Dec 2024 14:00:00 +0000
draft: false
type: posts
categories: 
- Threat Intelligence
---
# Bridging the Gap Elevating Red Team Assessments with Application Security Testing

<br/>

<br/>
Written by: Ilyass El Hadi, Louis Dion-Marcil, Charles Prevost

* * *

Executive Summary
-----------------

Whether through a comprehensive [Red Team engagement](https://cloud.google.com/security/consulting/mandiant-red-team) or a targeted external assessment, incorporating application security (AppSec) expertise enables organizations to better simulate the tactics and techniques of modern adversaries. This includes:

-   **Leveraging minimal access for maximum impact:** There is no need for high privilege escalation. Red Team objectives can often be achieved with limited access, highlighting the importance of securing all internet-facing assets.
    
-   **Recognizing the potential of low-impact vulnerabilities through vulnerability chaining:** Low- and medium-impact vulnerabilities can be exploited in combination to achieve significant impact.
    
-   **Developing your own exploits:** Skilled adversaries or consultants will invest the time and resources to reverse-engineer and/or find zero-day vulnerabilities in the absence of public proof-of-concept exploits.
    
-   **Employing diverse skill sets:** Red Team members should include individuals with a wide range of expertise, including AppSec.
    
-   **Fostering collaboration:** Combining diverse skill sets can spark creativity and lead to more effective attack simulations.
    
-   **Integrating AppSec throughout the engagement:** Offensive application security contributions can benefit Red Teams at every stage of the project.
    

By embracing this approach, organizations can proactively defend against a constantly evolving threat landscape, ensuring a more robust and resilient security posture.

Introduction
------------

In today's rapidly evolving threat landscape, organizations find themselves engaged in an ongoing arms race against increasingly sophisticated cyber criminals and nation-state actors. To stay ahead of these adversaries, many organizations turn to Red Team assessments, simulating real-world attacks to expose vulnerabilities before they are exploited. However, many traditional Red Team assessments typically prioritize attacking network and infrastructure components, often overlooking a critical aspect of modern attack surfaces: web applications.

This gap hasn't gone unnoticed by cyber criminals. In recent years, industry reports consistently highlight the evolving trend of attackers exploiting public-facing application vulnerabilities as a primary entry point into organizations. This aligns with Mandiant's observations of common tactics used by threat actors, as observed in our [2024 M-Trends Report](https://services.google.com/fh/files/misc/m-trends-2024.pdf): “In intrusions where the initial intrusion vector was identified, 38% of intrusions started with an exploit. This is a six percentage point increase from 2022.”

The 2024 M-Trends Report also documents that 28.7% of Initial Compromise access is obtained through exploiting public-facing web applications ([MITRE T1190](https://attack.mitre.org/techniques/T1190/)).

![Initial Compromise statistics from the M-Trends report](https://storage.googleapis.com/gweb-cloudblog-publish/images/red-team-appsec-fig1.max-1000x1000.png)

Figure 1: Initial Compromise statistics from the M-Trends report

At Mandiant, we recognize this gap and are committed to closing it by integrating AppSec expertise into our Red Team assessments. This optional approach is offered to customers who wish to increase the coverage of their external perimeters to gain a deeper understanding of their security posture. While most of the infrastructure typically receive a considerable amount of security scrutiny, web applications and edge devices often lack the same level of consideration, making them prime targets for attackers.

This integrated approach is not limited to full-scope Red Team engagements. Organizations with varying maturity levels can also leverage application security expertise within the context of focused external perimeter assessments. These assessments provide a valuable and cost-effective way to gain insights into the security of internet-facing applications and systems, without the need for a Red Team exercise.

The Role of Application Security in Red Team Assessments
--------------------------------------------------------

The integration of AppSec specialists into Red Team assessments manifests in a unique staffing approach. The role of this specialist is to augment the Red Team's capabilities with the ever-evolving exploitation techniques used by adversaries to breach organizations from the external perimeter.

The AppSec specialist will often get involved as early as possible on an engagement, even during the scoping and early planning stages. They perform a meticulous review of the target perimeter, mapping out the various application inventory and identifying vulnerabilities within the various components of web applications and application programming interfaces (APIs) exposed to the internet.

While examination is underway, Red Team operators concurrently focus on other crucial aspects of the assessment, including infrastructure preparation, crafting convincing phishing campaigns, developing and refining tools, and creating effective payloads that will evade the target environment's controls and defense mechanisms.

Once an AppSec vulnerability of critical impact is discovered, the team will generally proceed to its exploitation, notifying our primary point of contact of our preliminary findings and validating the potential impacts of our discovery. It is important to note that a successful finding doesn't always result in a direct foothold in the target environment. The intelligence gathered through the extensive reconnaissance and perimeter review phase can be repurposed for various aspects of the Red Team mission. This could include:  

-   Identifying valuable reconnaissance targets or technologies to fine-tune a social engineering campaign
    
-   Further tailoring an attack payload
    
-   Establishing a temporary foothold that might lead to further exploitation
    
-   Hosting malicious payloads for later stages of the attack simulation
    

Once the external perimeter examination phase is complete, our Red Team operators will begin carrying out the remaining mission objectives, empowered with the AppSec team's insights and intelligence, including identified vulnerabilities and associated exploits. Even though the Red Team operators will perform most of the remaining activities at this point, the AppSec consultants will stay close to the engagement and often engage to further support internal exploitations efforts. For example, applications that are only accessible internally generally get a lot less scrutiny and are consequently assessed much less frequently than externally accessible assets. 

By incorporating AppSec expertise, we've achieved a significant increase of engagements where our Red Team successfully gained a significant advantage during a customer's external perimeter review, such as obtaining a foothold or gaining access to confidential information.  This overall approach translates to a more realistic and valuable assessment for our customers, ensuring comprehensive coverage of both network and application security risks. By uncovering and addressing vulnerabilities across the entire attack surface, Mandiant empowers organizations to proactively defend against a wide array of threats, strengthening their overall security posture.

Case Studies: Demonstrating the Impact of Application Security Support
----------------------------------------------------------------------

In this section, we focus on four of the multiple real-world scenarios where the support of Mandiant's AppSec Team has significantly enhanced the effectiveness of Red Team assessments. Each case study highlights the attack vectors, the narrative behind the attack, key takeaways from the experience, and the associated assumptions and misconceptions.

These case studies highlight the value of incorporating application security support in Red Team engagements, while also offering valuable learning opportunities that promote collaboration and knowledge sharing.

### Unlocking the Vault: Exposed API Key to Sensitive Internal Document Access

#### Context

A company in the energy sector engaged Mandiant to assess the efficiency of its cybersecurity team's abilities in detection, prevention, and response. Because the organization had grown significantly in the past years following multiple acquisitions, Mandiant suggested an increased focus on their external perimeter. This would allow the organization to measure the subsidiaries' external security posture, compared to the parent organization's.

#### Target of Interest

Following a thorough reconnaissance phase, the AppSec Team began examination of a mobile application developed by the customer for its business partners. Once the mobile application was decompiled, a hardcoded API key granting unauthorized access to an external API service was discovered. Leveraging the API key, authenticated reconnaissance on the API service was conducted, which led to the discovery of a significant vulnerability within the application's PDF generation feature: a full-read Server-Side Request Forgery (SSRF), enabled through HTML injection.

#### Vulnerability Identification

During the initial reconnaissance phase, the team observed that numerous internal systems' hostnames were publicly accessible through certificate transparency logs. With that in mind, the objective was to exploit the SSRF vulnerability to determine if any of these internal systems were reachable via the external API service. Eventually, one such host was identified: a commercial ASP.NET document management solution. Once the solution's name and version were identified, the AppSec Team searched for known vulnerabilities online. Among the findings was a recent CVE entry regarding insecure ViewState deserialization, which included details about the affected dynamic-link library (DLL) name.

#### Exploitation

With no public exploit proof-of-concepts available, the team searched for the DLL without success until the file was found in VirusTotal's public corpus. The DLL was then decompiled into C# code, revealing the vulnerable function, which provided all the necessary components for a successful exploitation. Next, the application security consultants leveraged the post-authentication SSRF vector to exploit the ViewState deserialization vulnerability, affecting the internal application. This attack chain led to a reliable foothold into the parent organization's internal network.

![HTML to PDF Server-Side Request Forgery to deserialization](https://storage.googleapis.com/gweb-cloudblog-publish/images/red-team-appsec-fig2.max-1000x1000.png)

Figure 2: HTML to PDF Server-Side Request Forgery to deserialization

#### Takeaways

The organization's demilitarized zone (DMZ) was now breached, and the remote access could be passed off to the Red Team operators. This enabled the operators to perform lateral movement into the network and achieve various predetermined objectives. However, the customer expressed high satisfaction with the demonstrated impact prior to lateral movement, especially since the application server housed numerous sensitive documents. This underscores a common misconception that exploiting the external perimeter must necessarily result in facilitating lateral movement within the internal network. Yet, the impact was evident even before lateral movement, simply by gaining access to the customer's sensitive data.

### Breaking Barriers: Blind XSS as a Gateway to Internal Networks

#### Context

A company operating in the technology industry engaged Mandiant for a Red Team assessment. This company, with a very mature security program, requested that no phishing be performed because they were already conducting numerous internal phishing and vishing exercises. They highlighted that all previous Red Team engagements had relied heavily on various social engineering methods, and the success rate was consistently low.

#### Target of Interest

During the external reconnaissance efforts, the AppSec Team identified multiple targets of interest, such as a custom-built customer relationship management (CRM) solution. Leveraging the Wayback Machine on the CRM hostname, a legacy endpoint was discovered, which appeared obsolete but still accessible without authentication. 

#### Vulnerability Identification

Despite not being accessible through the CRM's user interface, the endpoint contained a functional form to request support. The AppSec Team injected a blind cross-site scripting (XSS) payload into the form, which loaded an external JavaScript file containing post-exploitation code. When successful, this method allows an adversary to temporarily hijack the targeted user's browser tab, allowing attackers to perform actions on behalf of the user. Moments later, the team received a notification that the payload successfully executed within the context of a user browsing an internal customer support administration panel. 

The AppSec Team analyzed the exfiltrated Document Object Model (DOM) to further understand the payload's execution context and assess the data accessible within this internal application.The analysis revealed references to Apache Tapestry framework version 3, a framework initially released in 2004. Shortly after identifying the internal application's framework, Mandiant deployed a local Tapestry v3 instance to identify potential security pitfalls. Through code review, Mandiant discovered a zero-day deserialization vulnerability in the core framework, which led to remote code execution (RCE). Apache Software Foundation assigned CVE-2022-46366 for this RCE.

#### Exploitation

The zero-day, which affected the internal customer support application, was exploited by submitting an additional blind XSS payload. Crafted to trigger upon form submission, the payload autonomously executed in an employee's browser, exploiting the internal application's deserialization flaw. This led to a crucial foothold within the client's infrastructure, enabling the Red Team to progress with their lateral movement until all objectives were successfully accomplished.

![Remote code execution staged with blind cross-site scripting](https://storage.googleapis.com/gweb-cloudblog-publish/images/red-team-appsec-fig3.max-1000x1000.png)

Figure 3: Remote code execution staged with blind cross-site scripting

#### Takeaways

This real-world scenario highlights a common misconception that cross-site scripting holds minimal relevance in Red Team assessments. The significance and impact of this particular attack vector in this case study were evident: it acted as a gateway, breaching the external network and leveraging an employee's internal network position as a proxy to exploit the internal application. Mandiant had not previously identified XSS vulnerabilities on the external perimeter, which further highlights how the security posture of the external perimeter can be much more robust than that of the internal network.

### Logger Danger: From Log Files to Unauthorized Cloud Access

#### Context

An organization in the transportation sector engaged Mandiant to perform a Red Team assessment, with the goal of emulating an initial access broker (IAB) threat group, focused on breaching externally exposed systems and services. Those groups, who typically resell illegitimate access to compromised victims' environments, were previously identified as a significant threat to the organization by the Google Threat Intelligence (GTI) team while building a threat profile to help support assessment activities.

#### Target of Interest

Among hundreds of external applications identified during the reconnaissance phase, one stood out: a commercial Java-based supply chain management solution hosted in the cloud. This application brought additional attention upon discovery of an online forum post describing its installation procedures. Within the post, a link to an unlisted YouTube video was shared, offering detailed installation and administration guidance. Upon reviewing the video, the AppSec Team noted the URL for the application's trial installer, still accessible online despite not being referenced or indexed anywhere else.

Following installation and local deployment, an administration manual was available within the installation folder. This manual contained a section for a web-based performance monitor plugin that was deployed by default with the application, along with its default credentials. The plugin's functionality included logging performance metrics and stack traces locally in files upon encountering unhandled errors. Furthermore, the plugin's endpoint name was uniquely distinct, making it highly unlikely to be discovered with conventional directory brute-forcing methods.

#### Vulnerability Identification

The AppSec Team successfully logged into the organization's performance monitor plugin by using the default credentials sourced from the administration manual and resumed local testing to identify post-authentication vulnerabilities. Conducting code review in parallel with manual testing, a log management feature was identified, which allowed authenticated users to manipulate log filenames and directories. The team also observed they could induce errors through targeted, malformed HTTP requests. In conjunction with the log filename manipulation, it was possible to force arbitrary data to be stored at an arbitrary file location on the underlying server's file system.

#### Exploitation

The strategy involved intentionally triggering exceptions, which the performance monitor would then log in an attacker-defined Jakarta Server Pages (JSP) file within the web application's root directory. The AppSec Team crafted an exploit that injected arbitrary JSP code into an HTTP request's parameter, forcing the performance monitor to log errors into the attacker-controlled JSP file. Upon accessing the JSP log file, the injected code executed, enabling Mandiant to breach the customer's cloud environment and access thousands of sensitive logistics documents.

![Remote code execution through log file poisoning](https://storage.googleapis.com/gweb-cloudblog-publish/images/red-team-appsec-fig4.max-1000x1000.png)

Figure 4: Remote code execution through log file poisoning

#### Takeaways

A common assumption that breaches should lead to internal on-premises network access or to Active Directory compromise was challenged in this case study. While lateral movement was constrained by time, the primary objective was achieved: emulating an initial access broker. This involved breaching the cloud environment, where the client lacked visibility compared to its internal Active Directory network, and gaining access to business-critical crown jewels.

### Collaborative Intrusion: Webhooks to CI/CD Pipeline Access

#### Context

A company in the automotive sector engaged Mandiant to perform a Red Team assessment, with the goal of obtaining access to their continuous integration and continuous delivery/deployment (CI/CD) pipeline. Due to the sheer number of externally exposed systems, the AppSec Team was staffed to support the Red Team's reconnaissance and breaching efforts.

#### Target of Interest

Most of the interesting applications were redirecting to the customer's single-sign on (SSO) provider. However, one application had a different behavior. By querying the [Wayback Machine](http://web.archive.org/), the team uncovered an endpoint that did not redirect to the SSO. Instead, it presented a blank page with a unique favicon. With the goal of identifying the application's underlying technology, the favicon's hash was calculated and queried using Shodan. The results returned many other live applications sharing the same favicon. Interestingly, some of these applications operated independently of SSO, aiding the team in identifying the application's name and vendor.

#### Vulnerability Identification

Once the application's name was identified, the team visited the vendor's website and accessed their public API documentation. Among the API endpoints, one stood out—it could be directly accessed on the customer's application without redirection to the SSO. This API endpoint did not require authentication and only took an incremental numerical ID as its parameter's value. Upon querying, the response contained sensitive employee information, including email addresses and phone numbers. The team systematically iterated through the API endpoint, incrementing the ID parameter to compile a comprehensive list of employee email addresses and phone numbers. However, the Red Team refrained from leveraging this data, as another intriguing application was discovered. This application exposed a feature that could be manipulated into sending fully user-controlled emails from the company's no-reply@ email address.

Capitalizing on these vulnerabilities, the Red Team initiated a phishing campaign, successfully gaining a foothold in the customer's network before the AppSec Team could identify an external breach vector. As efforts continued on the internal post-exploitation, the application security consultants shifted their focus to support the Red Team's efforts within the internal network.

#### Exploitation

Digging into network shares, the Red Team found credentials of a developer for an enterprise source control application account. The AppSec Team sifted through reconnaissance data and flagged that the same source control application server was exposed externally. The credentials were successfully used to log in, as multi factor authentication was absent for this user. Within the GitHub interface, the team uncovered a pre-defined webhook linked to the company's internal Jenkins—an integration commonly employed for facilitating communication between source control systems and CI/CD pipelines. Leveraging this discovery, the team created a new webhook. When manually triggered by the team, this webhook would perform an SSRF to internal URLs. This eventually led to the exploitation of an unauthenticated Jenkins sandbox bypass vulnerability (CVE-2019-1003030), and ultimately in remote code execution, effectively compromising the organization's CI/CD pipeline.

![External perimeter breach via CI/CD SSRF](https://storage.googleapis.com/gweb-cloudblog-publish/images/red-team-appsec-fig5.max-1000x1000.png)

Figure 5: External perimeter breach via CI/CD SSRF

#### Takeaways

In this case study, the efficacy of collaboration between the Red Team and the AppSec Team was demonstrated. Leveraging insights gathered collectively, the teams devised a strategic plan to achieve the main objective set by the customer: accessing its CI/CD pipelines. Moreover, we challenged the misconception that singular critical vulnerabilities are indispensable for reaching objectives. Instead, we revealed the reality where achieving goals often requires innovative detours. In fact, a combination of vulnerabilities or misconfigurations, whether they are discovered by the AppSec Team or the Red Team, can be strategically chained together to accomplish the mission.

Conclusion
----------

As this blog post demonstrated, the integration of application security expertise into Red Team assessments yields significant benefits for organizations seeking to understand and strengthen their security posture. By proactively identifying and addressing vulnerabilities across the entire attack surface, including those commonly overlooked by traditional approaches, businesses can minimize the risk of breaches, protect critical assets, and hopefully avoid the financial and reputational damage associated with successful attacks.

This integrated approach is not limited to Red Team engagements. Organizations with varying maturity levels can also leverage application security expertise within the context of focused external perimeter assessments. These assessments provide a valuable and cost-effective way to gain insights into the security of internet-facing applications and systems, without the need for a Red Team exercise.

Whether through a comprehensive Red Team engagement or a targeted external assessment, incorporating application security expertise enables organizations to better simulate the tactics and techniques of modern adversaries.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/red-team-application-security-testing/)

<br/>
---
