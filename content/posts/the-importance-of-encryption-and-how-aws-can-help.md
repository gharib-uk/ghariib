---
title: "The importance of encryption and how AWS can help"
date: 2025-03-19
categories: 
  - "aes"
  - "aws-cloudhsm"
  - "aws-key-management-service"
  - "aws-kms"
  - "aws-wickr"
  - "cryptography"
  - "encryption"
  - "foundational-100"
  - "key-management"
  - "openssl"
  - "s2n"
  - "security"
  - "security-blog"
  - "security-identity-compliance"
  - "tls"
---

February 12, 2025: This post was republished to include new services and features that have launched since the original publication date of June 11, 2020. Encryption is a critical component of a defense-in-depth security strategy that uses multiple defensive mechanisms to protect workloads, data, and assets. As organizations look to innovate while building trust with \[…\]

> **February 12, 2025:** This post was republished to include new services and features that have launched since the original publication date of June 11, 2020.

* * *

Encryption is a critical component of a defense-in-depth security strategy that uses multiple defensive mechanisms to protect workloads, data, and assets. As organizations look to innovate while building trust with customers, they need to meet critical compliance requirements and improve data security. Encryption, when used correctly, adds a layer of protection against unauthorized access that can help you strengthen data protection, adhere to regulations and standards, and enhance the security of communications.

## How and why does encryption work?

Encryption works by using an algorithm with a key to convert data into unreadable data (ciphertext) that can only become readable again with the right key. For example, a simple phrase like “Hello World!” may look like “1c28df2b595b4e30b7b07500963dc7c” when encrypted. There are several different types of encryption algorithms, all using different types of keys. A strong encryption algorithm relies on mathematical properties to produce ciphertext that can’t be decrypted using any practically available amount of computing power without also having the necessary key. Therefore, protecting and managing the keys becomes a critical part of any encryption solution.

## Encryption as part of your security strategy

An effective security strategy begins with stringent access control and continuous work to define the least privilege necessary for persons or systems accessing data. When using the AWS Cloud, you adopt the model of shared responsibility. You are responsible for managing your own access control policies. Encryption is a critical component of a defense-in-depth strategy because it can mitigate weaknesses in your primary access control mechanism. What if an access control mechanism fails and allows access to the raw data on disk or traveling along a network link? If the data is encrypted using a strong key, as long as the decryption key is not on the same system as your data, it is computationally infeasible for a bad actor to decrypt your data.

To show how infeasible this is, let’s consider the Advanced Encryption Standard (AES) with 256-bit keys (AES-256). It’s the strongest industry-adopted and government-approved algorithm for encrypting data. AES-256 is the technology we use to encrypt data in AWS, including Amazon Simple Storage Service (S3) server-side encryption. It would take at least a trillion years to break using current (and foreseeable future) computing technology. Current research suggests that even the future availability of quantum-based computing won’t sufficiently reduce the time it would take to break AES-256 encryption.

But what if you mistakenly create overly permissive access policies on your data? A well-designed encryption and key management system can also help prevent this from becoming an issue, because it separates access to the decryption key from access to your data.

## Requirements for an encryption solution

To get the most from an encryption solution, you need to think about two things:

1. **Protecting keys at rest**: Are the systems using encryption keys secured so the keys can never be used outside the system? In addition, do these systems implement encryption algorithms correctly to produce strong ciphertexts that cannot be decrypted without access to the right keys?
2. **Independent key management**: Is the authorization to use encryption independent from how access to the underlying data is controlled?

There are third-party solutions that you can bring to AWS to help meet these requirements. However, these systems can be difficult and expensive to operate at scale. AWS offers a range of options to simplify encryption and key management.

### Protecting keys at rest

When you use third-party key management solutions, it can be difficult to gauge the risk of your plaintext keys leaking and being used outside the solution. The keys have to be stored somewhere, and you can’t always know or audit all the ways those storage systems are secured from unauthorized access. The combination of technical complexity and the necessity of making the encryption usable without degrading performance or availability means that choosing and operating a key management solution can present difficult tradeoffs. The best practice to maximize key security is using a hardware security module (HSM). This is a specialized computing device that has several security controls built into it to help prevent encryption keys from leaving the device in a way that could allow an adversary to access and use those keys.

One such control in modern HSMs is tamper response, in which the device detects physical or logical attempts to access plaintext keys without authorization, and destroys the keys before the attack succeeds. Because you can’t install and operate your own hardware in AWS datacenters, AWS offers two services using HSMs with tamper response to protect customers’ keys: AWS Key Management Service (AWS KMS), which manages a fleet of HSMs on the customer’s behalf, and AWS CloudHSM, which gives customers the ability to manage their own HSMs. Each service can create keys on your behalf, or you can import keys from your on-premises systems to be used by each service.

The keys in AWS KMS or AWS CloudHSM can be used to encrypt data directly, or to protect other keys that are distributed to applications that directly encrypt data. The technique of encrypting encryption keys is called envelope encryption, and it enables encryption and decryption to happen on the computer where the plaintext customer data exists, rather than sending the data to the HSM each time. For very large data sets (e.g., a database), it’s not practical to move gigabytes of data between the data set and the HSM for every read/write operation. Instead, envelope encryption allows a data encryption key to be distributed to the application when it’s needed. The “master” keys in the HSM are used to encrypt a copy of the data key so the application can store the encrypted key alongside the data encrypted under that key. Once the application encrypts the data, the plaintext copy of data key can be deleted from its memory. The only way for the data to be decrypted is if the encrypted data key, which is only a few hundred bytes in size, is sent back to the HSM and decrypted.

The process of envelope encryption is used in AWS services in which data is encrypted on a customer’s behalf (which is known as server-side encryption) to minimize performance degradation. If you want to encrypt data in your own applications (client-side encryption), you’re encouraged to use envelope encryption with AWS KMS or AWS CloudHSM. Both services offer client libraries and SDKs to add encryption functionality to their application code and use the cryptographic functionality of each service. The AWS Encryption SDK is an example of a tool that can be used anywhere, not just in applications running in AWS. To make it easier for customers to encrypt data in databases like Amazon DynamoDB, we built the AWS Database Encryption SDK. The AWS Database Encryption SDK is a set of software libraries that enable you to use client-side encryption in your database design, including record-level encryption of database items. Today, the AWS Database Encryption SDK supports Amazon DynamoDB with attribute-level encryption.

Because implementing encryption algorithms and HSMs is critical to get right, all vendors of HSMs should have their products validated by a trusted third party. HSMs in both AWS KMS and AWS CloudHSM are validated under the National Institute of Standards and Technology’s FIPS 140 program, the standard for evaluating cryptographic modules. This validates the secure design and implementation of cryptographic modules, including functions related to ports and interfaces, authentication mechanisms, physical security and tamper response, operational environments, cryptographic key management, and electromagnetic interference/electromagnetic compatibility (EMI/EMC). Encryption using a FIPS 140 level 3 validated cryptographic module is often a requirement for other security-related compliance schemes like FedRamp and HIPAA-HITECH in the U.S., or the international payment card industry standard (PCI-DSS).

### Independent key management

While AWS KMS and AWS CloudHSM can protect plaintext master keys on your behalf, you are still responsible for managing access controls to determine who can cause which encryption keys to be used under which conditions. One advantage of using AWS KMS is that the policy language you use to define access controls on keys is the same one you use to define access to all other AWS resources. Note that the language is the same, not the actual authorization controls. You need a mechanism for managing access to keys that is different from the one you use for managing access to your data. AWS KMS provides that mechanism by allowing you to assign one set of administrators who can only manage keys and a different set of administrators who can only manage access to the underlying encrypted data. Configuring your key management process in this way helps provide separation of duties you need to avoid accidentally escalating privilege to decrypt data to unauthorized users. For even further separation of control, AWS CloudHSM offers an independent policy mechanism to define access to keys.

In 2022, AWS KMS launched support for external key stores (XKS), a feature that allows you to store AWS KMS customer managed keys on an HSM that you operate on premises or at a location of your choice. At a high level, AWS KMS forwards requests for encryption and decryption to your HSM. Your key material never leaves your HSM. This can help you unblock use cases for a small portion of highly regulated workloads where encryption keys should be stored and used outside of an AWS data center. However, XKS forces a significant shift in the shared responsibility model—you now have responsibility for the durability, throughput, latency, and availability of your KMS key. If that key is lost or destroyed, you could permanently lose access to data, and if an XKS key becomes unavailable, all workloads in AWS that are dependent on that XKS key will be inaccessible.

Even with the ability to separate key management from data management, you can still verify that you have configured access to encryption keys correctly. AWS KMS is integrated with AWS CloudTrail so you can audit who used which keys, for which resources, and when. This provides granular vision into your encryption management processes, which is typically much more in-depth than on-premises audit mechanisms. Audit events from AWS CloudHSM can be sent to Amazon CloudWatch, the AWS service for monitoring and alarming third-party solutions you operate in AWS.

## Encrypting data at rest and in transit

AWS services that handle customer data, encrypt data that is sent from one system to another—known as data in transit—provide options to encrypt data at rest. AWS services that offer encryption at rest using AWS KMS or AWS CloudHSM use AES-256. None of these services store plaintext encryption keys at rest—that’s a function that only AWS KMS and AWS CloudHSM may perform using their FIPS 140 level 3 validated HSMs. This architecture helps minimize the unauthorized use of keys.

When encrypting data in transit, AWS services use the Transport Layer Security (TLS) protocol to provide encryption between your application and the AWS service. Most commercial solutions use an open source project called OpenSSL for their TLS needs. OpenSSL has roughly 500,000 lines of code with at least 70,000 of those implementing TLS. The code base is large, complex, and difficult to audit. Moreover, when OpenSSL has bugs, the global developer community is challenged to not only fix and test the changes, but also to make sure that the resulting fixes themselves do not introduce new flaws.

AWS’s response to challenges with the TLS implementation in OpenSSL was to develop our own implementation of TLS, known as s2n, or signal to noise. We released s2n in June 2015, which we designed to be small and fast. The goal of s2n is to provide you with network encryption that is easier to understand and that is fully auditable. We released and licensed it under the Apache 2.0 license and hosted it on GitHub.

We also designed s2n to be analyzed using automated reasoning to test for safety and correctness using mathematical logic. Through this process, known as formal methods, we verify the correctness of the s2n code base every time we change the code. We also automated these mathematical proofs, which we regularly re-run to ensure the desired security properties are unchanged with new releases of the code. Automated mathematical proofs of correctness are an emerging trend in the security industry, and AWS uses this approach for a wide variety of our mission-critical software.

Similarly, in 2022, we released s2n-quic, an open-source Rust implementation of the QUIC protocol that was added to our set of AWS encryption open source libraries. QUIC is an encrypted transport protocol designed for performance and is the foundation of HTTP/3. It is specified in a set of IETF standards that were ratified in May 2021. Amazon CloudFront HTTP/3 support is built on top of s2n-quic, due to its emphasis on performance and efficiency. You can learn more about s2n-quic in this Security Blog post.

Implementing TLS requires using encryption keys and digital certificates that assert the ownership of those keys. AWS Certificate Manager and AWS Private Certificate Authority are two services that can simplify the issuance and rotation of digital certificates across your infrastructure that needs to offer TLS endpoints. Both services use a combination of AWS KMS and AWS CloudHSM to generate and/or protect the keys used in the digital certificates they issue.

## Encrypting data in use

You might also have use cases for protecting data that is actively being used by federated learning models or other applications. Cryptographic computing—a set of technologies that allow computations to be performed on encrypted data, so that sensitive data is not exposed—is a methodology for protecting data in use.

Consider the example of an insurance company that works with other companies to develop machine learning models for insurance fraud detection. You might need to use sensitive data about your customers as training data for your models, but you don’t want to share your customer data in plaintext form with the other companies. Cryptographic computing gives organizations a way to train models collaboratively without exposing plaintext data about their customers to each other, or to a cloud provider like AWS. You can read more about cryptographic computing in this AWS Security Blog post.

Today, you can see cryptographic computing at work in AWS Clean Rooms, a service that helps companies and their partners more easily and securely analyze and collaborate on their collective datasets—all without sharing or copying one another’s underlying data. AWS Clean Rooms has a feature called Cryptographic Computing for AWS Clean Rooms (C3R) that cryptographically protects your data even while it is being processed by an AWS Clean Rooms collaboration.

## The role of end-to-end encryption in secure communications

End-to-end encryption (E2EE) is a method of secure communication between two or more parties that combines encryption in transit and encryption at rest to protect data from unauthorized access, interception, or tampering. Decryption happens only on the parties you intend to communicate with, and no service providers in between. Every call, message, and file is encrypted with a unique private key and remains protected in transit. Unauthorized parties can’t access communication content, because they don’t have the private key required to decrypt the data.

AWS Wickr is an end-to-end encrypted messaging and collaboration service that protects one-to-one and group messaging, voice and video calling, file sharing, screen sharing, and location sharing with 256-bit encryption. With Wickr, each message gets a unique AES private encryption key and a unique Elliptic-curve Diffie–Hellman (ECDH) public key to negotiate the key exchange with recipients. Message content—including text, files, audio, or video—is encrypted on the sending device (your iPhone, for example) by using the message-specific AES key. This key is then exchanged by using the ECDH key exchange mechanism, so that only intended recipients can decrypt the message.

## Quantum computing and post-quantum cryptography

Quantum computing is a field of technology that uses quantum mechanics to solve complex problems faster than on classical computers. Quantum computers are able to solve certain types of problems faster by taking advantage of quantum mechanical effects, such as superposition and quantum interference. For cryptography, this has implications that affect traditional encryption mechanisms such as asymmetric key encryption, which is often used for protecting data in transit (TLS) or creating hash-based signatures to verify the integrity and authenticity of a message or file. Quantum computers, if they are performant and stable enough, could theoretically compromise the security of asymmetric key algorithms like RSA, Elliptic Curve Cryptography (ECC), or Diffie-Hellman key agreement schemes. Based on current research, symmetric key algorithms like AES are not considered to be at risk from a quantum computer, because the key length of 256 bits is already sufficient to compensate for a decrease in cryptographic key strength posed by quantum algorithms.

AWS gives customers the option of evaluating post-quantum algorithms alongside traditional algorithms, using hybrid schemes that make use of both classic cryptography and newer post-quantum cryptographic (PQC) algorithms that are designed to be resistant to quantum computer threats. AWS has taken the first step in deploying PQC by implementing ML-KEM, a module lattice-based key encapsulation mechanism, within AWS-LC, our open source FIPS-140-3 validated cryptographic library. AWS-LC is the core cryptographic library used throughout AWS. Specifically, AWS-LC is used in s2n-tls, our open source TLS implementation used across AWS services with HTTPS-based endpoints.

We have also deployed post-quantum s2n-tls with AWS KMS, AWS Certificate Manager (ACM), and AWS Secrets Manager TLS endpoints—bringing the benefits of post-quantum cryptography to customers who enable hybrid post-quantum TLS in their AWS SDK to connect to those services. AWS Transfer Family also supports post-quantum, hybrid SFTP file transfers. You can read more about our efforts to migrate more AWS managed service endpoints to PQC over TLS in this AWS Security blog post. You can also find information about the work of Amazon and AWS in cryptographic research and improvements on the Amazon Science Blog.

## Encrypt everything, everywhere

AWS provides customers the ability to encrypt everything, everywhere. Customers can encrypt data at rest, in transit, and in memory, with a few clicks in the AWS Management Console, or an AWS API call. Services like Amazon Simple Storage Service (Amazon S3) encrypt new objects by default, and also support the use of customer managed AWS KMS keys to give customers more control over their encryption keys. Importantly, AWS KMS uses techniques like envelope encryption and highly scalable key management infrastructure to enable AWS services like Amazon S3 or Amazon Elastic Block Store (Amazon EBS) to encrypt data with minimal performance impact to customer applications.

AWS is also consistently working to improve the performance and security of our customers’ data as it moves between networks or devices. As of June 2024, all AWS API endpoints support TLS 1.3 and require at least TLS 1.2 or higher. By using TLS 1.3, you can decrease your connection time by removing one network round trip for every connection request, and can benefit from some of the most modern and secure cryptographic cipher suites available today.

Customers who require memory encryption can use AWS Graviton, our custom-built family of processors based on ARM. AWS Graviton2, AWS Graviton3, and AWS Graviton3E support always-on memory encryption. The encryption keys are securely generated within the host system, do not leave the host system, and are destroyed when the host is rebooted or powered down. Memory encryption is also supported for other instance types; see the EC2 documentation for more details.

As part of our AWS Digital Sovereignty Pledge, we commit to continue to innovate and invest in additional controls for encryption features so that our customers can encrypt everything, everywhere with encryption keys managed inside or outside the AWS Cloud.

## Summary

At AWS, security is our top priority. We are committed to helping you control how your data is used, who has access to it, and how it is protected. By building and supporting encryption tools that work both on and off the cloud, we help you secure your data and enable compliance across your environment. We put security at the center of everything we do to make sure that you can protect your data using best-of-breed security technology in a cost-effective way.

If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, start a new thread on the AWS KMS forum or the AWS CloudHSM forum, or contact AWS Support.

![Ken Beer](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2018/11/20/author-bio-beer.jpeg) Ken Beer  
Ken is the Director of the AWS Key Management Service and Cryptography Libraries. Ken has worked in identity and access management, encryption, and key management for over 7 years at AWS. Before joining AWS, Ken was in charge of the network security business at Trend Micro. Before Trend Micro, he was at Tumbleweed Communications. Ken has spoken on a variety of security topics at events such as the RSA Conference, the DoD PKI User’s Forum, and AWS re:Invent.

![Zach Miller ](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/11/07/zachml.jpg) Zach Miller  
Zach is a Principal Security Specialist Solutions Architect at AWS. His background is in data protection and security architecture, focused on a variety of security domains, including applied cryptography and secrets management. Today, he focuses on helping enterprise AWS customers adopt and operationalize AWS security services to increase security effectiveness and reduce risk.

Go to Source
