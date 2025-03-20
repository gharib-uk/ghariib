---
title: "Top 2022 AWS data protection service and cryptography tool launches"
date: 2025-01-05
categories: 
  - "cryptography"
  - "encryption"
  - "security"
tags: 
  - "acm"
  - "amazonmacie"
  - "awscertificatemanager"
  - "awscleanrooms"
  - "awskeymanagementsystem"
  - "awskms"
  - "awsnitrosystem"
  - "awsprivateca"
  - "awsprivatecertificateauthority"
  - "awssecretsmanager"
  - "opensource"
  - "securityidentitycompliance"
  - "securityblog"
  - "servicelaunches"
---

February 28, 2023: We updated this blog to include AWS Wickr. Given the pace of Amazon Web Services (AWS) innovation, it can be challenging to stay up to date on the latest AWS service and feature launches. AWS provides services and tools to help you protect your data, accounts, and workloads from unauthorized access. AWS \[…\]

> **February 28, 2023:** We updated this blog to include AWS Wickr.

* * *

Given the pace of Amazon Web Services (AWS) innovation, it can be challenging to stay up to date on the latest AWS service and feature launches. AWS provides services and tools to help you protect your data, accounts, and workloads from unauthorized access. AWS data protection services provide encryption capabilities, key management, and sensitive data discovery. Last year, we saw growth and evolution in AWS data protection services as we continue to give customers features and controls to help meet their needs. Protecting data in the AWS Cloud is a top priority because we know you trust us to help protect your most critical and sensitive asset: your data. This post will highlight some of the key AWS data protection launches in the last year that security professionals should be aware of.

AWS Key Management Service  
_Create and control keys to encrypt or digitally sign your data_

In April, AWS Key Management Service (AWS KMS) launched hash-based message authentication code (HMAC) APIs. This feature introduced the ability to create AWS KMS keys that can be used to generate and verify HMACs. HMACs are a powerful cryptographic building block that incorporate symmetric key material within a hash function to create a unique keyed message authentication code. HMACs provide a fast way to tokenize or sign data such as web API requests, credit card numbers, bank routing information, or personally identifiable information (PII). This technology is used to verify the integrity and authenticity of data and communications. HMACs are often a higher performing alternative to asymmetric cryptographic methods like RSA or elliptic curve cryptography (ECC) and should be used when both message senders and recipients can use AWS KMS.

At AWS re:Invent in November, AWS KMS introduced the External Key Store (XKS), a new feature for customers who want to protect their data with encryption keys that are stored in an external key management system under their control. This capability brings new flexibility for customers to encrypt or decrypt data with cryptographic keys, independent authorization, and audit in an external key management system outside of AWS. XKS can help you address your compliance needs where encryption keys for regulated workloads must be outside AWS and solely under your control. To provide customers with a broad range of external key manager options, AWS KMS developed the XKS specification with feedback from leading key management and hardware security module (HSM) manufacturers as well as service providers that can help customers deploy and integrate XKS into their AWS projects.

AWS Nitro System  
_A combination of dedicated hardware and a lightweight hypervisor enabling faster innovation and enhanced security_

In November, we published The Security Design of the AWS Nitro System whitepaper. The AWS Nitro System is a combination of purpose-built server designs, data processors, system management components, and specialized firmware that serves as the underlying virtualization technology that powers all Amazon Elastic Compute Cloud (Amazon EC2) instances launched since early 2018. This new whitepaper provides you with a detailed design document that covers the inner workings of the AWS Nitro System and how it is used to help secure your most critical workloads. The whitepaper discusses the security properties of the Nitro System, provides a deeper look into how it is designed to eliminate the possibility of AWS operator access to a customer’s EC2 instances, and describes its passive communications design and its change management process. Finally, the paper surveys important aspects of the overall system design of Amazon EC2 that provide mitigations against potential side-channel vulnerabilities that can exist in generic compute environments.

AWS Secrets Manager  
_Centrally manage the lifecycle of secrets_

In February, AWS Secrets Manager added the ability to schedule secret rotations within specific time windows. Previously, Secrets Manager supported automated rotation of secrets within the last 24 hours of a specified rotation interval. This new feature added the ability to limit a given secret rotation to specific hours on specific days of a rotation interval. This helps you avoid having to choose between the convenience of managed rotations and the operational safety of application maintenance windows. In November, Secrets Manager also added the capability to rotate secrets as often as every four hours, while providing the same managed rotation experience.

In May, Secrets Manager started publishing secrets usage metrics to Amazon CloudWatch. With this feature, you have a streamlined way to view how many secrets you are using in Secrets Manager over time. You can also set alarms for an unexpected increase or decrease in number of secrets.

At the end of December, Secrets Manager added support for managed credential rotation for service-linked secrets. This feature helps eliminate the need for you to manage rotation Lambda functions and enables you to set up rotation without additional configuration. Amazon Relational Database Service (Amazon RDS) has integrated with this feature to streamline how you manage your master user password for your RDS database instances. Using this feature can improve your database’s security by preventing the RDS master user password from being visible during the database creation workflow. Amazon RDS fully manages the master user password’s lifecycle and stores it in Secrets Manager whenever your RDS database instances are created, modified, or restored. To learn more about how to use this feature, see Improve security of Amazon RDS master database credentials using AWS Secrets Manager.

AWS Private Certificate Authority  
_Create private certificates to identify resources and protect data_

In September, AWS Private Certificate Authority (AWS Private CA) launched as a standalone service. AWS Private CA was previously a feature of AWS Certificate Manager (ACM). One goal of this launch was to help customers differentiate between ACM and AWS Private CA. ACM and AWS Private CA have distinct roles in the process of creating and managing the digital certificates used to identify resources and secure network communications over the internet, in the cloud, and on private networks. This launch coincided with the launch of an updated console for AWS Private CA, which includes accessibility improvements to enhance screen reader support and additional tab key navigation for people with motor impairment.

In October, AWS Private CA introduced a short-lived certificate mode, a lower-cost mode of AWS Private CA that is designed for issuing short-lived certificates. With this new mode, public key infrastructure (PKI) administrators, builders, and developers can save money when issuing certificates where a validity period of 7 days or fewer is desired. To learn more about how to use this feature, see How to use AWS Private Certificate Authority short-lived certificate mode.

Additionally, AWS Private CA supported the launches of certificate-based authentication with Amazon AppStream 2.0 and Amazon WorkSpaces to remove the logon prompt for the Active Directory domain password. AppStream 2.0 and WorkSpaces certificate-based authentication integrates with AWS Private CA to automatically issue short-lived certificates when users sign in to their sessions. When you configure your private CA as a third-party root CA in Active Directory or as a subordinate to your Active Directory Certificate Services enterprise CA, AppStream 2.0 or WorkSpaces with AWS Private CA can enable rapid deployment of end-user certificates to seamlessly authenticate users. To learn more about how to use this feature, see How to use AWS Private Certificate Authority short-lived certificate mode.

AWS Certificate Manager  
_Provision and manage SSL/TLS certificates with AWS services and connected resources_

In early November, ACM launched the ability to request and use Elliptic Curve Digital Signature Algorithm (ECDSA) P-256 and P-384 TLS certificates to help secure your network traffic. You can use ACM to request ECDSA certificates and associate the certificates with AWS services like Application Load Balancer or Amazon CloudFront. Previously, you could only request certificates with an RSA 2048 key algorithm from ACM. Now, AWS customers who need to use TLS certificates with at least 120-bit security strength can use these ECDSA certificates to help meet their compliance needs. The ECDSA certificates have a higher security strength—128 bits for P-256 certificates and 192 bits for P-384 certificates—when compared to 112-bit RSA 2048 certificates that you can also issue from ACM. The smaller file footprint of ECDSA certificates makes them ideal for use cases with limited processing capacity, such as small Internet of Things (IoT) devices.

Amazon Macie  
_Discover and protect your sensitive data at scale_

Amazon Macie introduced two major features at AWS re:Invent. The first is a new capability that allows for one-click, temporary retrieval of up to 10 samples of sensitive data found in Amazon Simple Storage Service (Amazon S3). With this new capability, you can more readily view and understand which contents of an S3 object were identified as sensitive, so you can review, validate, and quickly take action as needed without having to review every object that a Macie job returned. Sensitive data samples captured with this new capability are encrypted by using customer-managed AWS KMS keys and are temporarily viewable within the Amazon Macie console after retrieval.

Additionally, Amazon Macie introduced automated sensitive data discovery, a new feature that provides continual, cost-efficient, organization-wide visibility into where sensitive data resides across your Amazon S3 estate. With this capability, Macie automatically samples and analyzes objects across your S3 buckets, inspecting them for sensitive data such as personally identifiable information (PII) and financial data; builds an interactive data map of where your sensitive data in S3 resides across accounts; and provides a sensitivity score for each bucket. Macie uses multiple automated techniques, including resource clustering by attributes such as bucket name, file types, and prefixes, to minimize the data scanning needed to uncover sensitive data in your S3 buckets. This helps you continuously identify and remediate data security risks without manual configuration and lowers the cost to monitor for and respond to data security risks.

AWS Wickr  
_Protect enterprise communications with end-to-end-encryption_

At AWS re:Invent in November, we announced the general availability of AWS Wickr. AWS Wickr is an end-to-end encrypted service that facilitates one-to-one and group messaging, voice and video calling, file sharing, screen sharing, and location sharing. With AWS Wickr, businesses and public sector organizations can collaborate more securely, while retaining data to help meet requirements such as e-discovery and Freedom of Information Act (FOIA) requests. You can quickly create and manage AWS Wickr networks through the AWS Management Console, and securely automate workflows using Wickr bots.

### Support for new open source encryption libraries

In February, we announced the availability of s2n-quic, an open source Rust implementation of the QUIC protocol, in our AWS encryption open source libraries. QUIC is a transport layer network protocol used by many web services to provide lower latencies than classic TCP. AWS has long supported open source encryption libraries for network protocols; in 2015 we introduced s2n-tls as a library for implementing TLS over HTTP. The name s2n is short for _signal to noise_ and is a nod to the act of encryption—disguising meaningful signals, like your critical data, as seemingly random noise. Similar to s2n-tls, s2n-quic is designed to be small and fast, with simplicity as a priority. It is written in Rust, so it has some of the benefits of that programming language, such as performance, threads, and memory safety.

### Cryptographic computing for AWS Clean Rooms (preview)

At re:Invent, we also announced AWS Clean Rooms, currently in preview, which includes a cryptographic computing feature that allows you to run a subset of queries on encrypted data. Clean rooms help customers and their partners to match, analyze, and collaborate on their combined datasets—without sharing or revealing underlying data. If you have data handling policies that require encryption of sensitive data, you can pre-encrypt your data by using a common collaboration-specific encryption key so that data is encrypted even when queries are run. With cryptographic computing, data that is used in collaborative computations remains encrypted at rest, in transit, and in use (while being processed).

If you’re looking for more opportunities to learn about AWS security services, read our AWS re:Invent 2022 Security recap post or watch the Security, Identity, and Compliance playlist.

### Looking ahead in 2023

With AWS, you control your data by using powerful AWS services and tools to determine where your data is stored, how it is secured, and who has access to it. In 2023, we will further the AWS Digital Sovereignty Pledge, our commitment to offering AWS customers the most advanced set of sovereignty controls and features available in the cloud.

You can join us at our security learning conference, AWS re:Inforce 2023, in Anaheim, CA, June 13–14, for the latest advancements in AWS security, compliance, identity, and privacy solutions.

Stay updated on launches by subscribing to the AWS What’s New RSS feed and reading the AWS Security Blog.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.

**Want more AWS Security news? Follow us on Twitter.**

![Author](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2019/07/08/Marta-Taggart-author.jpg)

### Marta Taggart

Marta is a Seattle-native and Senior Product Marketing Manager in AWS Security Product Marketing, where she focuses on data protection services. Outside of work you’ll find her trying to convince Jack, her rescue dog, not to chase squirrels and crows (with limited success).

Go to Source
