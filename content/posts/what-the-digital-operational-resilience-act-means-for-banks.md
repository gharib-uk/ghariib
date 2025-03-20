---
title: "<div>What the Digital Operational Resilience Act means for banks</div>"
date: 2025-01-17
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Developers play a critical role in ensuring banks remain competitive and compliant. One framework gaining significant attention is DORA. If you’re thinking of the DevOps Research and Assessment (DORA) metrics, this is something different. The Digital Operational Resilience Act is a new regulatory framework focused on safeguarding financial institutions against digital disruptions. For developers, understanding DORA regulations is not just a regulatory necessity; it’s an opportunity to drive innovation and enhance the overall stability of their organizations.

## What is DORA regulation?

The Digital Operational Resilience Act (DORA) is a legislative framework introduced by the European Union to strengthen the operational resilience of financial institutions. DORA aims to ensure that banks and other financial services providers can withstand, respond to, and recover from all types of information and communication technology (ICT) related disruptions and threats. DORA outlines specific requirements for risk management, incident reporting, testing, and the overall governance of digital operations.

## Core requirements of DORA

DORA introduces several critical requirements for financial institutions to ensure they can maintain operational continuity, including:

1. **Risk management:** Organizations must establish systems to identify, assess, and manage risks related to their digital operations. DORA fundamentally redefines the landscape of ICT risk management by placing accountability at the executive level. Detailed in Article 5, the management body of an organization is now entrusted with the ultimate responsibility for overseeing ICT risk management. This includes conducting regular risk assessments and implementing strategies to mitigate identified vulnerabilities.
    
2. **Regular testing:** Financial institutions are required to conduct systematic testing of their ICT systems to ensure they can handle potential disruptions effectively. This includes stress testing, scenario analysis, and recovery simulations to evaluate the resilience of their operations.
    
3. **Incident reporting:** Significant ICT-related incidents must be reported to regulators within specified timeframes. This requirement enhances oversight and allows regulators to coordinate responses across the financial sector, ensuring a unified approach to managing crises. The most recent Regulatory Technical Standards proposes time limits for reporting of the initial notification of four hours after classification and 24 hours after detection of the incident, 72 hours for reporting of the intermediate report, and one month for the reporting of the final report.
    
4. **Third-party risk management:** DORA also focuses on managing risks associated with outsourcing services to third-party providers. Organizations must ensure that their partners adhere to the same stringent standards, conducting due diligence and regular assessments of third-party performance. One of the biggest shifts for a bank is oftentimes centered around the establishment of exit strategies, detailed in Article 28.
    

Organizations need to prepare for scenarios where a third-party provider can no longer meet their operational needs or compliance obligations. This proactive approach ensures continuity and minimizes disruption in critical services. GitLab offers a distinct advantage in this area, as our platform is cloud-agnostic. This flexibility allows organizations to easily adapt their operations and transition between service providers as needed, simplifying the implementation of effective exit strategies.

For those who are interested in learning a bit more about the specifics listed above, the formal regulation documentation can be found here.

## Why DORA matters to developers

DORA is important for developers to understand for the following reasons:

1. **Enhanced security posture:** For developers, DORA emphasizes the importance of robust cybersecurity measures. As cyber threats continue to evolve, being part of an organization that prioritizes security means you’ll need to build applications with security in mind from the beginning, with a shift security left mindset. Compliance with DORA requires implementing best practices in secure coding, conducting regular vulnerability assessments, and ensuring that security controls are integrated into the software development lifecycle.
2. **Focus on resilience:** DORA requires banks to have clear strategies for operational resilience. Developers must now design systems that go beyond surface level functionality, building applications that can withstand failures and protect against disruptions. Having a clear understanding of DORA can guide you in architecting applications that can seamlessly handle disruptions, whether from a technical failure or an external threat.
3. **Collaboration and cross-functional teams:** Implementing DORA effectively requires a collaborative approach, which could pose a challenge in siloed banking structures. Developers will need to work closely with cybersecurity teams, risk management, and compliance officers.
4. **Agility in incident response:** DORA mandates that organizations report and respond to incidents efficiently. Developers must be equipped to quickly address vulnerabilities and deploy fixes.
5. **Continuous improvement culture:** DORA encourages a culture of continuous improvement and testing. This requires the adoption of practices like chaos engineering and regular stress testing of applications to ensure they can handle unexpected scenarios. Embracing these methodologies will not only help meet regulatory requirements but also improve the overall quality and reliability of the software that is built.

## GitLab's role in DORA compliance

GitLab is prepared to help financial institutions meet DORA’s stringent requirements. With security built into the earliest stages of deployment pipelines, GitLab is strategically positioned to equip organizations with software that is Secure by Design.

- **Robust risk management:** GitLab’s built-in tools enable organizations to identify, assess, and manage risk across their digital landscape. By utilizing features like issue tracking and merge requests, teams can collaboratively manage and document risks throughout the software development lifecycle. GitLab provides several tools that enable organizations to manage these requirements effectively:  
    \- **Audit logs and compliance dashboards:** GitLab's audit logs capture all activities within the platform, giving financial institutions a full history of changes made to code, configurations, and infrastructure. These logs allow compliance teams to review user actions and detect irregularities that could pose risks. Additionally, GitLab’s compliance dashboard provides real-time visibility into which projects comply with established policies, making it easier to manage large-scale governance.  
    \- **Custom compliance frameworks:** GitLab allows organizations to create custom compliance frameworks that are tailored to an organization's regulatory requirements and geographical regions. These frameworks ensure consistent enforcement of security and operational standards, meeting DORA’s systematic risk management objectives.
    
- **Comprehensive application security testing:** Security vulnerabilities pose significant regulatory, financial, and reputational risks. GitLab addresses these challenges by building security testing directly into its CI/CD pipelines, ensuring vulnerabilities are detected and mitigated before deployment. This approach leverages multiple testing methodologies:
    
    - Static Application Security Testing (SAST): Analyzes source code for security vulnerabilities.
    - Dynamic Application Security Testing (DAST): Tests running applications for security weaknesses.
    - Secret Detection: Prevents sensitive information from being exposed in code.
    - Fuzz Testing: Identifies potential security issues by testing with random inputs.
    
    GitLab’s security tools run automated tests that scan for vulnerabilities in code, containers, and third-party dependencies. These features help organizations meet the DORA requirement to continuously test IT systems, providing peace of mind that potential vulnerabilities are addressed before they become operational risks.
    
    ![GitLab features for DORA requirements in EU](https://images.ctfassets.net/r9o86ar0p03f/6eAeeWCc8nzFGm6GuAEFCR/fbd43b2bcefb3eab7ab561f96666c7af/image1.png)
    
- **Efficient incident reporting:** GitLab’s project management capabilities enable teams to effectively log and track significant ICT-related incidents. This centralized documentation, combined with continuous vulnerability scanning, facilitates timely reporting to regulators, enhances visibility, and supports compliance with DORA's incident reporting requirements. GitLab's incident management features streamline the workflow of remediation, making it easier for teams to identify, trace, and act on incidents as they arise.
    
    - Incident management tools: GitLab includes built-in tools for managing incidents, serving as a centralized record for teams to report, assess, and mitigate issues effectively. Users can create incident records, assign ownership, and document the investigation and resolution process. This centralization not only streamlines incident management but also enables teams to trace back and determine accountability for each incident. By facilitating clear ownership and structured workflows, GitLab positions organizations to effectively meet DORA’s requirements for effective incident response plans.
    - Real-time alerts and monitoring integrations: By integrating with monitoring tools such as Prometheus and Grafana, GitLab allows financial institutions to receive real-time alerts when issues arise. These alerts can trigger automated incident responses, helping teams address potential threats before they escalate, in line with DORA’s emphasis on quick reaction times.
- **Third-party risk management:** GitLab enables organizations to work closely with third-party providers, ensuring they adhere to the same rigorous standards required by the industry. The platform provides both technical controls and governance features to manage third-party risks:
    
    - Technical Controls
        
        - Dependency Scanning: Automatically detects vulnerabilities in third-party libraries and open-source components
        - Software Composition Analysis: Provides detailed inventory and security status of all external dependencies
        - Container Scanning: Identifies vulnerabilities in third-party container images
    - Governance Features
        
        - Policy Enforcement: Automatically enforce security policies for external code and components
        - Integration Controls: GitLab's API-first approach ensures secure and monitored integration with external systems
        - Audit Trails: Maintain comprehensive logs of all third-party component usage and changes
    
    These capabilities help organizations meet DORA's requirements for third-party risk management while maintaining operational efficiency.
    

The EU’s DORA regulations present new challenges for financial institutions, requiring them to enhance their governance, cybersecurity, and resilience frameworks. GitLab offers powerful features that address the key pillars of DORA, from incident management to cybersecurity testing and third-party risk management. By integrating GitLab into operational processes, financial institutions can streamline their compliance efforts, reduce risks, and ensure that they meet regulatory requirements with greater efficiency. GitLab provides a solid foundation for organizations seeking to stay ahead of the evolving regulatory landscape while maintaining strong security and operational resilience.

> #### Reach out to learn more about how GitLab can help meet your regulatory challenges.

## Read more

- GitLab supports banks in navigating regulatory challenges
- Meet regulatory standards with GitLab security and compliance
- How to ensure separation of duties and enforce compliance with GitLab

Developers play a critical role in ensuring banks remain competitive and compliant. One framework gaining significant attention is DORA. If you’re thinking of the DevOps Research and Assessment (DORA) metrics, this is something different. The Digital Operational Resilience Act is a new regulatory framework focused on safeguarding financial institutions against digital disruptions. For developers, understanding DORA regulations is not just a regulatory necessity; it’s an opportunity to drive innovation and enhance the overall stability of their organizations.

## What is DORA regulation?

The Digital Operational Resilience Act (DORA) is a legislative framework introduced by the European Union to strengthen the operational resilience of financial institutions. DORA aims to ensure that banks and other financial services providers can withstand, respond to, and recover from all types of information and communication technology (ICT) related disruptions and threats. DORA outlines specific requirements for risk management, incident reporting, testing, and the overall governance of digital operations.

## Core requirements of DORA

DORA introduces several critical requirements for financial institutions to ensure they can maintain operational continuity, including:

1. **Risk management:** Organizations must establish systems to identify, assess, and manage risks related to their digital operations. DORA fundamentally redefines the landscape of ICT risk management by placing accountability at the executive level. Detailed in Article 5, the management body of an organization is now entrusted with the ultimate responsibility for overseeing ICT risk management. This includes conducting regular risk assessments and implementing strategies to mitigate identified vulnerabilities.
    
2. **Regular testing:** Financial institutions are required to conduct systematic testing of their ICT systems to ensure they can handle potential disruptions effectively. This includes stress testing, scenario analysis, and recovery simulations to evaluate the resilience of their operations.
    
3. **Incident reporting:** Significant ICT-related incidents must be reported to regulators within specified timeframes. This requirement enhances oversight and allows regulators to coordinate responses across the financial sector, ensuring a unified approach to managing crises. The most recent Regulatory Technical Standards proposes time limits for reporting of the initial notification of four hours after classification and 24 hours after detection of the incident, 72 hours for reporting of the intermediate report, and one month for the reporting of the final report.
    
4. **Third-party risk management:** DORA also focuses on managing risks associated with outsourcing services to third-party providers. Organizations must ensure that their partners adhere to the same stringent standards, conducting due diligence and regular assessments of third-party performance. One of the biggest shifts for a bank is oftentimes centered around the establishment of exit strategies, detailed in Article 28.
    

Organizations need to prepare for scenarios where a third-party provider can no longer meet their operational needs or compliance obligations. This proactive approach ensures continuity and minimizes disruption in critical services. GitLab offers a distinct advantage in this area, as our platform is cloud-agnostic. This flexibility allows organizations to easily adapt their operations and transition between service providers as needed, simplifying the implementation of effective exit strategies.

For those who are interested in learning a bit more about the specifics listed above, the formal regulation documentation can be found here.

## Why DORA matters to developers

DORA is important for developers to understand for the following reasons:

1. **Enhanced security posture:** For developers, DORA emphasizes the importance of robust cybersecurity measures. As cyber threats continue to evolve, being part of an organization that prioritizes security means you’ll need to build applications with security in mind from the beginning, with a shift security left mindset. Compliance with DORA requires implementing best practices in secure coding, conducting regular vulnerability assessments, and ensuring that security controls are integrated into the software development lifecycle.
2. **Focus on resilience:** DORA requires banks to have clear strategies for operational resilience. Developers must now design systems that go beyond surface level functionality, building applications that can withstand failures and protect against disruptions. Having a clear understanding of DORA can guide you in architecting applications that can seamlessly handle disruptions, whether from a technical failure or an external threat.
3. **Collaboration and cross-functional teams:** Implementing DORA effectively requires a collaborative approach, which could pose a challenge in siloed banking structures. Developers will need to work closely with cybersecurity teams, risk management, and compliance officers.
4. **Agility in incident response:** DORA mandates that organizations report and respond to incidents efficiently. Developers must be equipped to quickly address vulnerabilities and deploy fixes.
5. **Continuous improvement culture:** DORA encourages a culture of continuous improvement and testing. This requires the adoption of practices like chaos engineering and regular stress testing of applications to ensure they can handle unexpected scenarios. Embracing these methodologies will not only help meet regulatory requirements but also improve the overall quality and reliability of the software that is built.

## GitLab's role in DORA compliance

GitLab is prepared to help financial institutions meet DORA’s stringent requirements. With security built into the earliest stages of deployment pipelines, GitLab is strategically positioned to equip organizations with software that is Secure by Design.

- **Robust risk management:** GitLab’s built-in tools enable organizations to identify, assess, and manage risk across their digital landscape. By utilizing features like issue tracking and merge requests, teams can collaboratively manage and document risks throughout the software development lifecycle. GitLab provides several tools that enable organizations to manage these requirements effectively:  
    \- **Audit logs and compliance dashboards:** GitLab's audit logs capture all activities within the platform, giving financial institutions a full history of changes made to code, configurations, and infrastructure. These logs allow compliance teams to review user actions and detect irregularities that could pose risks. Additionally, GitLab’s compliance dashboard provides real-time visibility into which projects comply with established policies, making it easier to manage large-scale governance.  
    \- **Custom compliance frameworks:** GitLab allows organizations to create custom compliance frameworks that are tailored to an organization's regulatory requirements and geographical regions. These frameworks ensure consistent enforcement of security and operational standards, meeting DORA’s systematic risk management objectives.
    
- **Comprehensive application security testing:** Security vulnerabilities pose significant regulatory, financial, and reputational risks. GitLab addresses these challenges by building security testing directly into its CI/CD pipelines, ensuring vulnerabilities are detected and mitigated before deployment. This approach leverages multiple testing methodologies:
    
    - Static Application Security Testing (SAST): Analyzes source code for security vulnerabilities.
    - Dynamic Application Security Testing (DAST): Tests running applications for security weaknesses.
    - Secret Detection: Prevents sensitive information from being exposed in code.
    - Fuzz Testing: Identifies potential security issues by testing with random inputs.
    
    GitLab’s security tools run automated tests that scan for vulnerabilities in code, containers, and third-party dependencies. These features help organizations meet the DORA requirement to continuously test IT systems, providing peace of mind that potential vulnerabilities are addressed before they become operational risks.
    
    ![GitLab features for DORA requirements in EU](https://images.ctfassets.net/r9o86ar0p03f/6eAeeWCc8nzFGm6GuAEFCR/fbd43b2bcefb3eab7ab561f96666c7af/image1.png)
    
- **Efficient incident reporting:** GitLab’s project management capabilities enable teams to effectively log and track significant ICT-related incidents. This centralized documentation, combined with continuous vulnerability scanning, facilitates timely reporting to regulators, enhances visibility, and supports compliance with DORA's incident reporting requirements. GitLab's incident management features streamline the workflow of remediation, making it easier for teams to identify, trace, and act on incidents as they arise.
    
    - Incident management tools: GitLab includes built-in tools for managing incidents, serving as a centralized record for teams to report, assess, and mitigate issues effectively. Users can create incident records, assign ownership, and document the investigation and resolution process. This centralization not only streamlines incident management but also enables teams to trace back and determine accountability for each incident. By facilitating clear ownership and structured workflows, GitLab positions organizations to effectively meet DORA’s requirements for effective incident response plans.
    - Real-time alerts and monitoring integrations: By integrating with monitoring tools such as Prometheus and Grafana, GitLab allows financial institutions to receive real-time alerts when issues arise. These alerts can trigger automated incident responses, helping teams address potential threats before they escalate, in line with DORA’s emphasis on quick reaction times.
- **Third-party risk management:** GitLab enables organizations to work closely with third-party providers, ensuring they adhere to the same rigorous standards required by the industry. The platform provides both technical controls and governance features to manage third-party risks:
    
    - Technical Controls
        
        - Dependency Scanning: Automatically detects vulnerabilities in third-party libraries and open-source components
        - Software Composition Analysis: Provides detailed inventory and security status of all external dependencies
        - Container Scanning: Identifies vulnerabilities in third-party container images
    - Governance Features
        
        - Policy Enforcement: Automatically enforce security policies for external code and components
        - Integration Controls: GitLab's API-first approach ensures secure and monitored integration with external systems
        - Audit Trails: Maintain comprehensive logs of all third-party component usage and changes
    
    These capabilities help organizations meet DORA's requirements for third-party risk management while maintaining operational efficiency.
    

The EU’s DORA regulations present new challenges for financial institutions, requiring them to enhance their governance, cybersecurity, and resilience frameworks. GitLab offers powerful features that address the key pillars of DORA, from incident management to cybersecurity testing and third-party risk management. By integrating GitLab into operational processes, financial institutions can streamline their compliance efforts, reduce risks, and ensure that they meet regulatory requirements with greater efficiency. GitLab provides a solid foundation for organizations seeking to stay ahead of the evolving regulatory landscape while maintaining strong security and operational resilience.

> #### Reach out to learn more about how GitLab can help meet your regulatory challenges.

## Read more

- GitLab supports banks in navigating regulatory challenges
- Meet regulatory standards with GitLab security and compliance
- How to ensure separation of duties and enforce compliance with GitLab

Go to Source
