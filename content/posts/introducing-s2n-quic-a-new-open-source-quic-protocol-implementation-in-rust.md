---
title: "Introducing s2n-quic, a new open-source QUIC protocol implementation in Rust"
date: 2025-01-05
categories: 
  - "cryptography"
  - "encryption"
  - "security"
tags: 
  - "cryptographiclibrary"
  - "quic"
  - "quicprotocol"
  - "s2n"
  - "s2n-quic"
  - "securityidentitycompliance"
  - "securityblog"
---

At Amazon Web Services (AWS), security, high performance, and strong encryption for everyone are top priorities for all our services. With these priorities in mind, less than a year after QUIC ratification in the Internet Engineering Task Force (IETF), we are introducing support for the QUIC protocol which can boost performance for web applications that \[…\]

![](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/02/17/S2N-QUIC-Blog-Announcement-Logo.png)At Amazon Web Services (AWS), security, high performance, and strong encryption for everyone are top priorities for all our services. With these priorities in mind, less than a year after QUIC ratification in the Internet Engineering Task Force (IETF), we are introducing support for the QUIC protocol which can boost performance for web applications that currently use Transport Layer Security (TLS) over Transmission Control Protocol (TCP). We are pleased to announce the availability of s2n-quic, an open-source Rust implementation of the QUIC protocol added to our set of AWS encryption open-source libraries.

## What is QUIC?

QUIC is an encrypted transport protocol designed for performance and is the foundation of HTTP/3. It is specified in a set of IETF standards ratified in May 2021. QUIC protects its UDP datagrams by using encryption and authentication keys established in a TLS 1.3 handshake carried over QUIC transport. It is designed to improve upon TCP by providing improved first-byte latency and handling of multiple streams, and solving issues such as head-of-line blocking, mobility, and data loss detection. This enables web applications to perform faster, especially over poor networks. Other potential uses include latency-sensitive connections and UDP connections currently using DTLS, which now can run faster.

## Renaming s2n

AWS has long supported open-source encryption libraries; in 2015 we introduced s2n as a TLS library. The name s2n is short for _signal to noise_, and is a nod to the almost magical act of encryption—disguising meaningful signals, like your critical data, as seemingly random noise.

Now that AWS introduces our new QUIC open-source library, we are renaming s2n to s2n-tls. s2n-tls is an efficient TLS library built over other crypto libraries like OpenSSL libcrypto or AWS libcrypto (AWS-LC). AWS-LC is a general-purpose cryptographic library maintained by AWS which originated from the Google project BoringSSL. The s2n family of AWS encryption open-source libraries now consists of s2n-tls, s2n-quic, and s2n-bignum. s2n-bignum is a collection of bignum arithmetic routines maintained by AWS designed for crypto applications.

## s2n-quic details

Similar to s2n-tls, s2n-quic is designed to be small and fast, with simplicity as a priority. It is written in Rust, so it reaps some of its benefits such as performance, thread and memory-safety. s2n-quic depends either on s2n-tls or rustls for the TLS 1.3 handshake.

The main advantages of s2n-quic are:

- Simple API. For example, a QUIC echo server-example can be built with just a few API calls.
- Highly configurable. s2n-quic is configured with code through providers that allow an application to granularly control functionality. You can see an example of the server’s simple config in the QUIC echo server-example.
- Extensive testing. Fuzzing (libFuzzer, American Fuzzy Fop (AFL), and honggfuzz), corpus replay unit testing of derived corpus files, testing of concrete and symbolic execution engines with bolero, and extensive integration and unit testing are used to validate the correctness of our implementation.
- Thorough interoperability testing for every code change. There are multiple public QUIC implementations; s2n-quic is continuously tested to interoperate with many of them.
- Verified correctness, post-quantum hybrid key exchange, and maturity for the TLS handshake when built with s2n-tls.
- Thorough compliance coverage tracking of normative language in relevant standards.

Some important features in s2n-quic that can improve performance and connection management include CUBIC congestion controller support, packet pacing, Generic Segmentation Offload (GSO) support, Path MTU Discovery, and unique connection identifiers detached from the address.

AWS is continuing to invest in encryption optimization techniques, UDP performance improvement technologies, and formal code verification with the AWS Automated Reasoning Group to further enhance the library.

Like s2n-tls, which has already been introduced in various AWS services, AWS services that need to make use of the benefits of QUIC will begin integrating s2n-quic. QUIC is a standardized protocol which, when introduced in a service like web content delivery, can improve user experience or application performance. AWS still plans to continue support for existing protocols like TLS, so existing applications will remain interoperable. Amazon CloudFront is scheduled to be the first AWS service to integrate s2n-quic with its support for HTTP/3 in 2022.

## Conclusion

If you are interested in using or contributing to s2n-quic source code or documentation, they are publicly available under the terms of the Apache Software License 2.0 from our s2n-quic GitHub repository.

If you package or distribute s2n-quic or s2n-tls, or use it as part of a large multi-user service, you may be eligible for pre-notification of security issues. Please contact s2n-pre-notification@amazon.com.

If you discover a potential security issue in s2n-quic or s2n-tls, we ask that you notify AWS Security by using our vulnerability reporting page.

Stay tuned for more topics on s2n-quic like quantum-resistance, performance analyses, uses, and other technical details.

If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.

**Want more AWS Security news? Follow us on Twitter.**

![Author](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/02/17/Panos-Kampanakis-Author.jpg)

### Panos Kampanakis

Panos has extensive experience on cybersecurity, applied cryptography, security automation, and vulnerability management. He has trained and presented on various security topics at technical events for numerous years, and also co-authored Cisco Press books, papers, standards, and research publications. He has participated in various security standards bodies to provide common interoperable protocols and languages for security information sharing, cryptography, and PKI. In his current role, Panos works with engineers and industry standards partners to provide cryptographically secure tools, protocols, and standards.

Go to Source
