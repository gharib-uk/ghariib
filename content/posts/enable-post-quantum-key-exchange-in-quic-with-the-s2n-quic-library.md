---
title: "Enable post-quantum key exchange in QUIC with the s2n-quic library"
date: 2025-01-05
categories: 
  - "cryptography"
  - "cryptography-library"
  - "encryption"
  - "security"
tags: 
  - "intermediate200"
  - "post-quantumquic"
  - "quic"
  - "quicprotocol"
  - "s2n-quic"
  - "securityidentitycompliance"
  - "securityblog"
---

January 25, 2023: AWS KMS, ACM, Secrets Manager TLS endpoints have been updated to only support NIST’s Round 3 picked KEM, Kyber. s2n-tls and s2n-quic have also been updated to only support Kyber. BIKE or other KEMs may still be added as the standardization proceeds. At Amazon Web Services (AWS) we prioritize security, performance, and \[…\]

> **January 25, 2023:** AWS KMS, ACM, Secrets Manager TLS endpoints have been updated to only support NIST’s Round 3 picked KEM, Kyber. s2n-tls and s2n-quic have also been updated to only support Kyber. BIKE or other KEMs may still be added as the standardization proceeds.

* * *

At Amazon Web Services (AWS) we prioritize security, performance, and strong encryption in our cloud services. In order to be prepared for quantum computer advancements, we’ve been investigating the use of quantum-safe algorithms for key exchange in the TLS protocol. In this blog post, we’ll first bring you up to speed on what we’ve been doing on the TLS front. Then, we’ll focus on the QUIC transport protocol and show how you can enable and experiment with the newly released post-quantum (PQ) key exchange by using our s2n-quic library. The s2n-quic library is an open-source implementation of the QUIC protocol.

## Why use PQ-hybrid key establishment in s2n-quic?

A large-scale quantum computer could break the current public key cryptography that is used to establish keys for secure communication connections. Although a large-scale quantum computer isn’t available today, traffic that is recorded now could be decrypted by one in the future. With such concerns in mind, the recent US Congress Quantum Computing Cybersecurity Preparedness Act and the White House National Security Memorandum set a goal of a timely and equitable transition of cryptographic systems to quantum-resistant cryptography.

At AWS, we are working to prepare for this future. Recently, AWS Key Management Service (AWS KMS), AWS Certificate Manager (ACM) and AWS Secrets Manager TLS endpoints started supporting post-quantum hybrid (PQ-hybrid) key establishment in TLS connections with three of the post-quantum key encapsulation mechanisms (KEMs) in the NIST Post-Quantum Cryptography (PQC) Project. The three post-quantum KEMs are Kyber (NIST’s Round 3 KEM selection), BIKE and SIKE (NIST’s Round 4 KEM candidates). All three of these AWS services’ support of post-quantum KEMs raises the security bar when making API requests to their endpoints over TLS.

PQ-hybrid key establishment in TLS is a feature that introduces post-quantum KEMs used in conjunction with classical Elliptic Curve Diffie-Hellman (ECDH) key exchange. The client and server still do an ECDH key exchange. Additionally, the server encapsulates a post-quantum shared secret to the client’s post-quantum KEM public key, which is advertised in the ClientHello message. This strategy combines the high assurance of a classical key exchange with the security of the proposed post-quantum key exchanges, to ensure that the handshakes are protected as long as the ECDH or the post-quantum shared secret cannot be broken.

After decapsulating the secret, the client and server have an ECDH and a post-quantum shared secret, which they concatenate and use to derive the symmetric keys that are used in the Authenticated Encryption with Additional Data (AEAD) cipher in TLS. These symmetric keys used by the AEAD cipher for data encryption will be secure against a quantum computer, which means that the TLS communication is secure against a quantum computer. The AWS implementation of TLS is s2n-tls, a streamlined open source implementation of TLS. The s2n-tls implementation already supports PQ-hybrid key exchange with ECDH and three NIST PQC Project KEMs (Kyber, BIKE, and SIKE) for TLS 1.2 and 1.3. The use of KEMs for TLS 1.2 is described in the draft-campagna-tls-bike-sike-hybrid IETF draft, and the use of KEMs for TLS 1.3 is described in the draft-ietf-tls-hybrid-design IETF draft.

> **Note**: The Kyber, BIKE, and SIKE implementations follow the algorithm specifications described in NIST PQ Project Round 3, which are expected to be updated as standardization proceeds.

## How PQ-hybrid key exchange works in s2n-quic

AWS recently announced s2n-quic, an open-source Rust implementation of the QUIC protocol. QUIC is an encrypted transport protocol that is designed for performance and is the foundation of HTTP/3. For tunnel establishment, QUIC uses TLS 1.3 carried over QUIC transport. To alleviate the _harvest-now-decrypt-later_ concerns for customers that use s2n-quic, in the next section we show you how to enable PQ-hybrid key establishment in s2n-quic. AWS services and software that use s2n-quic will automatically inherit the ability to support quantum-safe key exchanges in the future when post-quantum algorithms are standardized and are officially supported in s2n-quic.

The s2n-quic implementation is written in the Rust programming language. It can use either s2n-tls (the TLS library for AWS) or rustls (the TLS library in Rust) to perform the TLS handshake. If you build s2n-quic with s2n-tls, then s2n-quic inherits the post-quantum support that is offered in s2n-tls. In turn, s2n-tls is built over other crypto libraries such as the AWS libcrypto (AWS-LC) or alternatively OpenSSL crypto library (libcrypto). AWS-LC is a general-purpose cryptographic library that is maintained by AWS, which will incorporate standardized post-quantum algorithms. Therefore, building s2n-tls with AWS-LC will provide s2n-tls with the post-quantum cryptographic algorithms for use in s2n-quic.

Such a model allows for AWS services and software that use s2n-quic to automatically inherit the standardized post-quantum options as they are implemented in s2n-tls and its underlying crypto libraries. There will be no need to tweak s2n-quic to support post-quantum TLS 1.3 handshakes. The whole stack of protocol implementations is architected in an agile manner without duplication of work.

In the following section, we show you how to run an experimental PQ build of s2n-quic that supports PQ-hybrid key exchange.

## Test PQ-hybrid key establishment in s2n-quic

The public s2n-quic GitHub repository includes an example that demonstrates how to build the library with PQ-hybrid key exchange support, along with a server and client to test. The PQ-hybrid key exchange feature test requires CMake in Linux or macOS. The experiments below were run in an Amazon Linux 2 instance with rustc, Cargo, Clang, and CMake installed. Connections that you establish with this experimental build of s2n-quic will support PQ-hybrid key exchange.

**To test PQ-hybrid key establishment**

1. Clone s2n-quic by using the following commands:
    
    git clone https://github.com/aws/s2n-quic  
    cd s2n-quic
    
2. Run the example post-quantum s2n-quic client and server in the post-quantum directory to confirm that they negotiate a PQ-hybrid key by using the following commands:
    
    cd examples/post-quantum  
    cargo run ‐‐bin pq\_server  
    cargo run ‐‐bin pq\_client
    
    > **Note:** Although these examples with the PQ-hybrid feature experimental build of s2n-quic are self-contained, if you want to manually change and build s2n-quic and s2n-tls to enable PQ-hybrid key exchange, you have to update the default\_tls13 policy in s2n-tls to point to security\_policy\_pq\_tls\_1\_0\_2021\_05\_26 in tls/s2n\_security\_policies.c. Then you rebuild s2n-tls and override the location that s2n-quic links to by setting the S2N\_TLS\_DIR, S2N\_TLS\_LIB\_DIR, and S2N\_TLS\_INCLUDE\_DIR environment variables at build time.
    
3. To confirm the PQ-hybrid key establishment, you capture the QUIC negotiation by using the following tcpdump command:
    
    sudo tcpdump -i lo port 4433 -w test.pcap
    
4. Open the capture by using a packet capture visualization application. First you look at the ClientHello message, as shown in the capture in Figure 1 taken from Wireshark.
    
    ![Figure 1: pq_client ClientHello in QUIC](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/07/20/image1-2-1024x583.png)
    
    Figure 1: pq\_client ClientHello in QUIC
    
    In the QUIC CRYPTO frame, you can see the TLS 1.3 cipher suites, and that the TLS version is 1.3 while the supported key exchange groups are classical ECDH (with identifiers 0x0017, 0x0018, 0x001d) and 0x2f39, 0x2f3a, 0x2f37…. 0x2f1f. The 0x2f… groups are the agreed upon identifiers (not standardized yet) for PQ-hybrid key exchange. You also see the PQ-hybrid X25519+Kyber512 (with identifier 12089 or 0x2f39) key share that is offered by the client. That key share includes 32 bytes for the Curve25519 ephemeral ECDH client public key, 800 bytes for the ephemeral Kyber512 public key, and 4 bytes for the identifier and the key share length.
    
    > **Note**: The post-quantum KEMs implementations at the time of this writing follow the NIST Round 3 Kyber, BIKE, and SIKE specifications. We expect these specifications to change as the NIST PQC Project proceeds with standardization. Post-quantum support in s2n-tls and s2n-quic will be experimental until NIST has selected and published standardized algorithms and identifiers. Pushing the change to the main branch now would mean that s2n-quic clients would be sending a PQ-hybrid key share that won’t be used until the servers on the internet start supporting it. The actual algorithms and their identifiers will still be integrated in future releases of s2n-tls and AWS-LC. Therefore, s2n-quic will still be able to negotiate the NIST and IETF standardized options. Meanwhile, we will continue to experiment with post-quantum QUIC and its potential challenges.
    
5. Next, take a look at the server-negotiated keys in the ServerHello message, as shown in Figure 2.
    
    ![Figure 2: pq_server ServerHello in QUIC](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/07/20/image2-2-1024x526.png)
    
    Figure 2: pq\_server ServerHello in QUIC
    

You can again see the TLS 1.3 cipher suite, the TLS version being 1.3, and the picked PQ-hybrid X25519+Kyber512 key share. The key share includes 4 bytes for the identifier and the key share length, 32 bytes for the Curve25519 ephemeral ECDH server public key, and 768 bytes for the Kyber512 ciphertext that encapsulates a post-quantum shared secret to the client’s ephemeral Kyber512 public key (included in its ClientHello message).

The rest of the handshake completes successfully by deriving symmetric keys from the X25519 and Kyber512 post-quantum shared secrets (as defined in the draft-ietf-tls-hybrid-design IETF draft) and encrypting the rest of the messages with Advanced Encryption Standard with Galois/Counter Mode (AES-GCM) by using these symmetric keys over QUIC.

## Benchmark

Now you can benchmark the post-quantum QUIC client and server by using netbench, a transport protocol benchmarking tool that is available in the s2n-quic repository.

**To benchmark the post-quantum QUIC client and server**

1. Go in the netbench directory and build it with the correct flags for the experimental post-quantum QUIC examples, by using the following commands:
    
    cd s2n-quic/netbench  
    RUSTFLAGS=”–cfg s2n\_quic\_unstable –cfg s2n\_quic\_enable\_pq\_tls” cargo build –release
    
2. Generate the netbench scenario by using the following commands:
    
    ./target/release/netbench-scenarios –request\_response.connections 10000 –request\_response.request\_size 1 –request\_response.response\_size 1
    
    In this example, you’re trying to create 10,000 sequential QUIC connections. The scenario opens a connection, sends a single byte, receives a single byte, closes it, and repeats 10,000 times.
    
3. Run the server by using the following command:
    
    ./target/release/netbench-driver-s2n-quic-server target/netbench/request\_response.json
    
4. Run the client by using the following command:
    
    SERVER\_0=localhost:4433 ./target/release/netbench-driver-s2n-quic-client target/netbench/request\_response.json
    
    The drivers read the request\_response.json to run the scenario. Then the driver is wrapped in a collector that outputs statistics to another JSON file. At the end of all of the 10,000 runs, the cli feature is used to generate the report.
    

Figure 3 shows the performance results for X25519, X25519+Kyber512, X25519+BIKE-1, and X25519+SIKEp434 key exchange. All connections used an ECDSA P256 server certificate for authentication.

![Figure 3: PQ-hybrid key exchange impact on QUIC connection rates](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/07/20/image3-2-1024x516.png)

Figure 3: PQ-hybrid key exchange impact on QUIC connection rates

The x-axis is time in seconds. The y-axis is the number of times send is called—which, for 1 byte per connection, practically means that the diagram shows the connection establishment rate (per second). The absolute performance numbers in these benchmarks are not important, because the results could change based on the netbench scenario parameters. The performance difference between PQ-hybrid key exchange algorithms is what this graph is highlighting.

You can see that the classical X25519 achieves higher connection rates, because it is the most efficient option (that offers no post-quantum protection). The performance of Kyber is competitive and achieves 8% fewer connections per second when used with X25519 in a PQ-hybrid key exchange. BIKE-1 is relatively efficient, but adds some extra latency and introduces two frames for the ClientHello, which leads to 37% fewer connections per second. SIKEp434, although it offers much smaller public keys and ciphertexts, is orders of magnitude slower, which means it offers 95% fewer connections per second. These results match previous results we have shared before and other research works, where the most efficient signature algorithms ended up with higher connection rates and lower connection failure probabilities due to overload.

## Conclusion

In this post, we showed how you can use s2n-quic in conjunction with s2n-tls to enable QUIC connections to negotiate encryption keys in a quantum-resistant manner. If you’re interested in learning more about s2n-quic, join us at AWS re:Inforce in July for the breakout session entitled **NIS304: Using s2n-quic: Bringing QUIC, the secure transport protocol, to AWS**.

As always, if you’re interested in using or contributing to s2n-quic, the source code and documentation are publicly available under the terms of the Apache Software License 2.0 from our s2n-quic GitHub repository. If you package or distribute s2n-quic or s2n-tls, or use it as part of a large multi-user service, you might be eligible for pre-notification of security issues. Contact s2n-pre-notification@amazon.com for more information. If you discover a potential security issue in s2n-quic or s2n-tls, we ask that you notify AWS Security by using our vulnerability reporting page.

If you have feedback about this post, submit comments in the Comments section below. If you have questions about this post, contact AWS Support.

**Want more AWS Security news? Follow us on Twitter.**

![Panos Kampanakis](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/07/21/Panos-Profile-pic.jpg)

### Panos Kampanakis

Panos has extensive experience with cyber security, applied cryptography, security automation, and vulnerability management. In his professional career, he has trained and presented on various security topics at technical events for numerous years. He has co-authored cybersecurity publications and participated in various security standards bodies to provide common interoperable protocols and languages for security information sharing, cryptography, and PKI. Currently, he works with engineers and industry standards partners to provide cryptographically secure tools, protocols, and standards.

![Cameron Bytheway](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/07/20/Cameron-Bytheway.jpg)

### Cameron Bytheway

Cameron is a Software Development Engineer at AWS, based in Salt Lake City, Utah. He leads and contributes to the s2n libraries of AWS, and enjoys using testing, fuzzing, simulations, and statical analysis to improve correctness of programs.

Go to Source
