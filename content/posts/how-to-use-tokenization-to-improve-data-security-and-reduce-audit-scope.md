---
title: "How to use tokenization to improve data security and reduce audit scope"
date: 2025-01-05
categories: 
  - "cryptography"
  - "encryption"
  - "security"
tags: 
  - "awsdatalake"
  - "awskms"
  - "awssecuritytokenservice"
  - "cardholderdata"
  - "compliance"
  - "pcidss"
  - "phi"
  - "pii"
  - "securityidentitycompliance"
  - "securityassurance"
  - "technicalhow-to"
  - "tokenization"
---

April 25, 2023: We’ve updated this blog post to include more security learning resources. Tokenization of sensitive data elements is a hot topic, but you may not know what to tokenize, or even how to determine if tokenization is right for your organization’s business needs. Industries subject to financial, data security, regulatory, or privacy compliance \[…\]

> **April 25, 2023:** We’ve updated this blog post to include more security learning resources.

* * *

Tokenization of sensitive data elements is a hot topic, but you may not know what to tokenize, or even how to determine if tokenization is right for your organization’s business needs. Industries subject to financial, data security, regulatory, or privacy compliance standards are increasingly looking for tokenization solutions to minimize distribution of sensitive data, reduce risk of exposure, improve security posture, and alleviate compliance obligations. This post provides guidance to determine your requirements for tokenization, with an emphasis on the compliance lens given our experience as PCI Qualified Security Assessors (PCI QSA).

## What is tokenization?

Tokenization is the process of replacing actual sensitive data elements with non-sensitive data elements that have no exploitable value for data security purposes. Security-sensitive applications use tokenization to replace sensitive data, such as personally identifiable information (PII) or protected health information (PHI), with tokens to reduce security risks.

De-tokenization returns the original data element for a provided token. Applications may require access to the original data, or an element of the original data, for decisions, analysis, or personalized messaging. To minimize the need to de-tokenize data and to reduce security exposure, tokens can retain attributes of the original data to enable processing and analysis using token values instead of the original data. Common characteristics tokens may retain from the original data are:

### Format attributes

| **Length** | for compatibility with storage and reports of applications written for the original data |
| --- | --- |
| **Character set** | for compatibility with display and data validation of existing applications |
| **Preserved character positions** | such as first 6 and last 4 for credit card PAN |

### Analytics attributes

| **Mapping consistency** | where the same data always results in the same token |
| --- | --- |
| **Sort order** |  |

Retaining functional attributes in tokens must be implemented in ways that do not defeat the security of the tokenization process. Using attribute preservation functions can possibly reduce the security of a specific tokenization implementation. Limiting the scope and access to tokens addresses limitations introduced when using attribute retention.

## Why tokenize? Common use cases

### I need to reduce my compliance scope

Tokens are generally not subject to compliance requirements if there is sufficient separation of the tokenization implementation and the applications using the tokens. Encrypted sensitive data may not reduce compliance obligations or scope. Such industry regulatory standards as PCI DSS 3.2.1 still consider systems that store, process, or transmit encrypted cardholder data as in-scope for assessment; whereas tokenized data may remove those systems from assessment scope. A common use case for PCI DSS compliance is replacing PAN with tokens in data sent to a service provider, which keeps the service provider from being subject to PCI DSS.

### I need to restrict sensitive data to only those with a “need-to-know”

Tokenization can be used to add a layer of explicit access controls to de-tokenization of individual data items, which can be used to implement and demonstrate least-privileged access to sensitive data. For instances where data may be co-mingled in a common repository such as a data lake, tokenization can help ensure that only those with the appropriate access can perform the de-tokenization process and reveal sensitive data.

### I need to avoid sharing sensitive data with my service providers

Replacing sensitive data with tokens before providing it to service providers who have no access to de-tokenize data can eliminate the risk of having sensitive data within service providers’ control, and avoid having compliance requirements apply to their environments. This is common for customers involved in the payment process, which provides tokenization services to merchants that tokenize the card holder data, and return back to their customers a token they can use to complete card purchase transactions.

### I need to simplify data lake security and compliance

A data lake centralized repository allows you to store all your structured and unstructured data at any scale, to be used later for not-yet-determined analysis. Having multiple sources and data stored in multiple structured and unstructured formats creates complications for demonstrating data protection controls for regulatory compliance. Ideally, sensitive data should not be ingested at all; however, that is not always feasible. Where ingestion of such data is necessary, tokenization at each data source can keep compliance-subject data out of data lakes, and help avoid compliance implications. Using tokens that retain data attributes, such as data-to-token consistency (idempotence) can support many of the analytical capabilities that make it useful to store data in the data lake.

### I want to allow sensitive data to be used for other purposes, such as analytics

Your organization may want to perform analytics on the sensitive data for other business purposes, such as marketing metrics, and reporting. By tokenizing the data, you can minimize the locations where sensitive data is allowed, and provide tokens to users and applications needing to conduct data analysis. This allows numerous applications and processes to access the token data and maintain security of the original sensitive data.

### I want to use tokenization for threat mitigation

Using tokenization can help you mitigate threats identified in your workload threat model, depending on where and how tokenization is implemented. At the point where the sensitive data is tokenized, the sensitive data element is replaced with a non-sensitive equivalent throughout the data lifecycle, and across the data flow. Some important questions to ask are:

- What are the in-scope compliance, regulatory, privacy, or security requirements for the data that will be tokenized?
- When does the sensitive data need to be tokenized in order to meet security and scope reduction objectives?
- What attack vector is being addressed for the sensitive data by tokenizing it?
- Where is the tokenized data being hosted? Is it in a trusted environment or an untrusted environment?

For additional information on threat modeling, see the AWS security blog post How to approach threat modeling.

## Tokenization or encryption consideration

Tokens can provide the ability to retain processing value of the data while still managing the data exposure risk and compliance scope. Encryption is the foundational mechanism for providing data confidentiality.

Encryption rarely results in cipher text with a similar format to the original data, and may prevent data analysis, or require consuming applications to adapt.

Your decision to use tokenization instead of encryption should be based on the following:

| **Reduction of compliance scope** | As discussed above, by properly utilizing tokenization to obfuscate sensitive data you may be able to reduce the scope of certain framework assessments such as PCI DSS 3.2.1. |
| --- | --- |
| **Format attributes** | Used for compatibility with existing software and processes. |
| **Analytics attributes** | Used to support planned data analysis and reporting. |
| **Elimination of encryption key management** | A tokenization solution has one essential API—create token—and one optional API—retrieve value from token. Managing access controls can be simpler than some non-AWS native general purpose cryptographic key use policies. In addition, the compromise of the encryption key compromises all data encrypted by that key, both past and future. The compromise of the token database compromises only existing tokens. |

### Where encryption may make more sense

Although scope reduction, data analytics, threat mitigation, and data masking for the protection of sensitive data make very powerful arguments for tokenization, we acknowledge there may be instances where encryption is the more appropriate solution. Ask yourself these questions to gain better clarity on which solution is right for your company’s use case.

| **Scalability** | If you require a solution that scales to large data volumes, and have the availability to leverage encryption solutions that require minimal key management overhead, such as AWS Key Management Services (AWS KMS), then encryption may be right for you. |
| --- | --- |
| **Data format** | If you need to secure data that is unstructured, then encryption may be the better option given the flexibility of encryption at various layers and formats. |
| **Data sharing with 3rd parties** | If you need to share sensitive data in its original format and value with a 3rd party, then encryption may be the appropriate solution to minimize external access to your token vault for de-tokenization processes. |

## What type of tokenization solution is right for your business?

When trying to decide which tokenization solution to use, your organization should first define your business requirements and use cases.

1. What are your own specific use cases for tokenized data, and what is your business goal? Identifying which use cases apply to your business and what the end state should be is important when determining the correct solution for your needs.
2. What type of data does your organization want to tokenize? Understanding what data elements you want to tokenize, and what that tokenized data will be used for may impact your decision about which type of solution to use.
3. Do the tokens need to be deterministic, the same data always producing the same token? Knowing how the data will be ingested or used by other applications and processes may rule out certain tokenization solutions.
4. Will tokens be used internally only, or will the tokens be shared across other business units and applications? Identifying a need for shared tokens may increase the risk of token exposure and, therefore, impact your decisions about which tokenization solution to use.
5. How long does a token need to be valid? You will need to identify a solution that can meet your use cases, internal security policies, and regulatory framework requirements.

### Choosing between self-managed tokenization or tokenization as a service

Do you want to manage the tokenization within your organization, or use Tokenization as a Service (TaaS) offered by a third-party service provider? Some advantages to managing the tokenization solution with your company employees and resources are the ability to direct and prioritize the work needed to implement and maintain the solution, customizing the solution to the application’s exact needs, and building the subject matter expertise to remove a dependency on a third party. The primary advantages of a TaaS solution are that it is already complete, and the security of both tokenization and access controls are well tested. Additionally, TaaS inherently demonstrates separation of duties, because privileged access to the tokenization environment is owned by the tokenization provider.

### Choosing a reversible tokenization solution

Do you have a business need to retrieve the original data from the token value? Reversible tokens can be valuable to avoid sharing sensitive data with internal or third-party service providers in payments and other financial services. Because the service providers are passed only tokens, they can avoid accepting additional security risk and compliance scope. If your company implements or allows de-tokenization, you will need to be able to demonstrate strict controls on the management and use of de-tokenization privilege. Eliminating the implementation of de-tokenization is the clearest way to demonstrate that downstream applications cannot have sensitive data. Given the security and compliance risks of converting tokenized data back into its original data format, this process should be highly monitored, and you should have appropriate alerting in place to detect each time this activity is performed.

### Operational considerations when deciding on a tokenization solution

While operational considerations are outside the scope of this post, they are important factors for choosing a solution. Throughput, latency, deployment architecture, resiliency, batch capability, and multi-regional support can impact the tokenization solution of choice. Integration mechanisms with identity and access control and logging architectures, for example, are important for compliance controls and evidence creation.

No matter which deployment model you choose, the tokenization solution needs to meet security standards, similar to encryption standards, and must prevent determining what the original data is from the token values.

## Conclusion

Using tokenization solutions to replace sensitive data offers many security and compliance benefits. These benefits include lowered security risk and smaller audit scope, resulting in lower compliance costs and a reduction in regulatory data handling requirements.

Your company may want to use sensitive data in new and innovative ways, such as developing personalized offerings that use predictive analysis and consumer usage trends and patterns, fraud monitoring and minimizing financial risk based on suspicious activity analysis, or developing business intelligence to improve strategic planning and business performance. If you implement a tokenization solution, your organization can alleviate some of the regulatory burden of protecting sensitive data while implementing solutions that use obfuscated data for analytics.

On the other hand, tokenization may also add complexity to your systems and applications, as well as adding additional costs to maintain those systems and applications. If you use a third-party tokenization solution, there is a possibility of being locked into that service provider due to the specific token schema they may use, and switching between providers may be costly. It can also be challenging to integrate tokenization into all applications that use the subject data.

In this post, we have described some considerations to help you determine if tokenization is right for you, what to consider when deciding which type of tokenization solution to use, and the benefits. disadvantages, and comparison of tokenization and encryption. When choosing a tokenization solution, it’s important for you to identify and understand all of your organizational requirements. This post is intended to generate questions your organization should answer to make the right decisions concerning tokenization.

You have many options available to tokenize your AWS workloads. After your organization has determined the type of tokenization solution to implement based on your own business requirements, explore the tokenization solution options available in AWS Marketplace. You can also build your own solution using AWS guides and blog posts. For further reading, see this blog post: Building a serverless tokenization solution to mask sensitive data.

If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, start a new thread on the Amazon Security Assurance Services or contact AWS Support.

**Want more AWS Security news? Follow us on Twitter.**

![Author](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/01/18/Tim-Winston-Author-2.jpg)

### Tim Winston

Tim is a Senior Assurance Consultant with AWS Security Assurance Services. He leverages more than 20 years’ experience as a security consultant and assessor to provide AWS customers with guidance on payment security and compliance. He is a co-author of the “Payment Card Industry Data Security Standard (PCI DSS) 3.2.1 on AWS”.

![Author](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/01/18/Kristine-Harper-Author.jpg)

### Kristine Harper

Kristine is a Senior Assurance Consultant and PCI DSS Qualified Security Assessor (QSA) with AWS Security Assurance Services. Her professional background includes security and compliance consulting with large fintech enterprises and government entities. In her free time, Kristine enjoys traveling, outdoor activities, spending time with family, and spoiling her pets.

![Author](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/01/18/Michael-Guzman-Author.jpg)

### Michael Guzman

Michael is an Assurance Consultant with AWS Security Assurance Services. Michael is a PCI QSA and HITRUST CCSFP, along with holding several AWS certifications. His background is in Financial Services IT Operations and Administrations, with over 20 years experience within that industry. In his spare time Michael enjoy’s spending time with his family, continuing to improve his golf skills and perfecting his Tri-Tip recipe.

Go to Source
