---
title: "CrowdStrike incident takeaways: Revisiting vendor quality control and release standards to minimize outage exposure"
date: 2025-01-07
categories: 
  - "ci-cd"
  - "devops"
  - "devops-cloud"
  - "linux"
  - "open-source"
tags: 
  - "application-performance"
  - "application-security"
  - "crowdstrike"
  - "release-validation"
---

![Data privacy by design, CrowdStrike](https://dt-cdn.net/wp-content/uploads/2021/11/3d-render-abstract-graph-300x169.jpg)

A key learning from the outage caused by the faulty CrowdStrike “Rapid Response” update is how critical it is to understand your vendors’ quality control and release processes. Vendors should not only offer advanced technology to fulfill a business requirement but also demonstrate a strong commitment to reliability, security, and customer support.

A variety of events and circumstances can cause an outage. This blog will suggest five areas to consider and questions to ask when evaluating your existing vendors and their risk management strategies.

## 1\. Development and QA process

Assessing a vendor’s testing and quality assurance (QA) capabilities reveals their approach and commitment to validating changes and preventing new issues. Thorough testing reduces the risk of outages and vulnerabilities from untested updates, showcasing the vendor’s commitment to reliable and compatible solutions.

Vendors take different testing and QA approaches, ranging from simple crash testing to newer strategies such as canary and blue-green. These modern approaches move beyond simple software validation to include advanced testing-centric rollout strategies. Regardless of the approach, it is critical to understand the processes that a vendor uses to test and roll out new releases to ensure that potential issues are caught early.

Even in scenarios where vendors have modern QA release processes, organizations that rely on their software solutions can also benefit from internal testing before widespread deployment. Rather than releasing updates directly to production clients and servers, organizations can add confidence by testing new releases in sandbox environments and then do gradual rollouts. The goal is to identify issues vendors might have missed and reduce the risk of large-scale outages like CrowdStrike.

##### **Questions to ask a vendor:**

- How frequently do you release?
- What is your testing process?
- How do you roll out new features?

## 2\. Security and safe practices

Organizations need a holistic approach to third-party risk management to ensure that vulnerabilities in vendor applications don’t become a weak link in the overall security posture. This includes evaluating the resilience of third-party providers and ensuring that appropriate risk mitigation strategies are in place. Additionally, organizations can seek to contractually hold their vendors accountable to fix truly critical vulnerabilities in a timely manner.

Vetting a vendor’s application security processes ensures their practices meet high standards for protecting sensitive data and safeguarding against breaches. It verifies their compliance with regulatory requirements and assesses their ability to manage and mitigate security risks effectively. Regular audits and adherence to recognized standards further confirm their commitment to maintaining robust security measures.

At the same time, even the most robust security practices are not always foolproof. Hence, it is important to establish a “zero-trust” policy for exposures via third-party software. Organizations must continuously verify third-party software at runtime for exposures and inform the vendor as soon as possible. Additionally, organizations must contractually enforce remediation service-level agreements (SLAs) to ensure the vendor remediates vulnerabilities promptly.

##### **Questions to ask a vendor:**

- How do you identify, assess, and prioritize security risks?
- What is your incident response plan? How often do you test it?
- What industry-specific security standards or regulations do you adhere to?
- What forms of security testing do you perform (for example, code reviews, penetration testing)?
- How do you manage third-party dependencies and vulnerabilities?
- What measures are in place to ensure the security of open source components?

## 3\. Customer support and services

Evaluating a vendor’s support and customer service is crucial because when issues do occur, it is important to receive timely assistance from the vendor to mitigate the impact of issues and minimize any outage duration. Understanding their availability and responsiveness helps verify their support quality and ability to address and resolve problems effectively.

Vendors typically offer a wide range of support options and tiers, whether via chat, phone, or virtual. Organizations also publish response SLAs, which can vary by vendor and support level. It is critical to understand the support options and SLA commitments and ensure that the purchased levels align with business requirements.

As an added benefit, some vendors also offer experienced professional services teams that can potentially assist in remediating unexpected outages. Familiarizing yourself with these services and the options available in case their expertise is needed can help prepare for recovering from an incident like the CrowdStrike outage.

##### **Questions to ask a vendor:**

- What are the company’s support SLAs?
- What is the process for resolving customer issues?
- What is your average response time for critical and non-critical support requests?
- What on-demand professional services options do you have?

## 4\. Application monitoring

Modern applications consist of many services, all of which must function to deliver the desired end-user experiences. A comprehensive monitoring strategy will ensure that a vendor is aware of issues impacting customers and enable the vendor to proactively resolve problems before users are impacted.

Often, vendors use a range of monitoring tools, making it challenging to identify and resolve issues proactively. The challenge is magnified as companies modernize to microservices environments, which can add additional complexity. As a customer, you should understand how your vendors monitor their applications and report on uptime and SLAs.

##### **Questions to ask a vendor:**

- How do you currently monitor your application?
- Do you provide availability SLAs?
- What was the last issue that you experienced and how long did it take to resolve?
- What alerting and monitoring tools do you use, and how do they prevent or address issues?

## 5\. System resiliency and incident management

Assessing a vendor’s application resiliency strategy is crucial for ensuring quick recovery and minimal downtime during disruptions like CrowdStrike. An effective strategy confirms that the vendor has plans to maintain service continuity and protects your business from potential losses.

There are many ways to improve application availability, including local clustering, geographic clustering, and backup and recovery. While all approaches have their trade-offs, combining these technologies to provide layered recovery options is the best strategy. As a user, you must understand the availability and protection solutions used by your vendor to ensure that they have a plan for recovery in the case of an unexpected outage.

##### **Questions to ask a vendor:**

- How does your failover process work, and how do you ensure minimal downtime and data integrity?
- What are your RTO and RPO?
- How often do you test your disaster recovery plans?

## Ensuring resilience from a third-party CrowdStrike outage

Unfortunately, there is no guaranteed way to avoid being impacted if your vendor is affected by the CrowdStike outage. However, these five considerations will help teams better assess the risk of an outage by a given vendor.

Contact us to learn how the Dynatrace observability and security platform can become your system resiliency and incident management partner to help you avoid the effects of a third-party outage like CrowdStrike.

To learn more about the recent CrowdStrike update outage and explore more resources to help you maintain business resilience, check out the resource center, Business Resilience through CrowdStrike and Beyond.

Learn more

The post CrowdStrike incident takeaways: Revisiting vendor quality control and release standards to minimize outage exposure appeared first on Dynatrace news.

Go to Source
