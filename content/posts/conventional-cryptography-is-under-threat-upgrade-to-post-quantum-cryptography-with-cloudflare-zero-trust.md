---
title: "Conventional cryptography is under threat. Upgrade to post-quantum cryptography with Cloudflare Zero Trust"
date: 2025-03-19
---

Quantum computers are actively being developed that will eventually have the ability to break the cryptography we rely on for securing modern communications. Recent breakthroughs in quantum computing have underscored the vulnerability of conventional cryptography to these attacks. Since 2017, Cloudflare has been at the forefront of developing, standardizing, and implementing post-quantum cryptography to withstand attacks by quantum computers. 

Our mission is simple: we want every Cloudflare customer to have a clear path to quantum safety. Cloudflare recognizes the urgency, so we’re committed to managing the complex process of upgrading cryptographic algorithms, so that you don’t have to worry about it. We're not just talking about doing it. Over 35% of the non-bot HTTPS traffic that touches Cloudflare today is post-quantum secure. 

The National Institute of Standards and Technology (NIST) also recognizes the urgency of this transition. On November 15, 2024, NIST made a landmark announcement by setting a timeline to phase out RSA and Elliptic Curve Cryptography (ECC), the conventional cryptographic algorithms that underpin nearly every part of the Internet today. According to NIST’s announcement, these algorithms will be deprecated by 2030 and completely disallowed by 2035.

At Cloudflare, we aren’t waiting until 2035 or even 2030. We believe privacy is a fundamental human right, and advanced cryptography should be accessible to everyone without compromise. No one should be required to pay extra for post-quantum security. That’s why any visitor accessing a website protected by Cloudflare today benefits from post-quantum cryptography, when using a major browser like Chrome, Edge, or Firefox. (And, we are excited to see a small percentage of (mobile) Safari traffic in our Radar data.) Well over a third of the human traffic passing through Cloudflare today already enjoys this enhanced security, and we expect this share to increase as more browsers and clients are upgraded to support post-quantum cryptography. 

While great strides have been made to protect human web traffic, not every application is a web application. And every organization has internal applications (both web and otherwise) that do not support post-quantum cryptography.  

How should organizations go about upgrading their sensitive corporate network traffic to support post-quantum cryptography?

That’s where today’s announcement comes in. We’re thrilled to announce the first phase of end-to-end quantum readiness of our Zero Trust platform**,** allowing customers to protect their corporate network traffic with post-quantum cryptography. **Organizations can tunnel their corporate network traffic though Cloudflare’s Zero Trust platform, protecting it against quantum adversaries without the hassle of individually upgrading each and every corporate application, system, or network connection.** 

More specifically, organizations can use our Zero Trust platform to route communications from end-user devices (via web browser or Cloudflare’s WARP device client) to secure applications connected with Cloudflare Tunnel, to gain end-to-end quantum safety, in the following use cases: 

- **Cloudflare’s clientless** **Access****:** Our clientless Zero Trust Network Access (ZTNA) solution verifies user identity and device context for every HTTPS request to corporate applications from a web browser. Clientless Access is now protected end-to-end with post-quantum cryptography.
    
- **Cloudflare’s** **WARP device client****:** By mid-2025, customers using the WARP device client will have all of their traffic (regardless of protocol) tunneled over a connection protected by post-quantum cryptography. The WARP client secures corporate devices by privately routing their traffic to Cloudflare's global network, where Gateway applies advanced web filtering and Access enforces policies for secure access to applications. 
    
- **Cloudflare** **Gateway**: Our Secure Web Gateway (SWG) — designed to inspect and filter TLS traffic in order to block threats and unauthorized communications — now supports TLS with post-quantum cryptography. 
    

In the remaining sections of this post, we’ll explore the threat that quantum computing poses and the challenges organizations face in transitioning to post-quantum cryptography. We’ll also dive into the technical details of how our Zero Trust platform supports post-quantum cryptography today and share some plans for the future.

### Why transition to post-quantum cryptography and why now? 

There are two key reasons to adopt post-quantum cryptography now:

#### 1\. The challenge of deprecating cryptography

History shows that updating or removing outdated cryptographic algorithms from live systems is extremely difficult. For example, although the MD5 hash function was deemed insecure in 2004 and long since deprecated, it was still in use with the RADIUS enterprise authentication protocol as recently as 2024. In July 2024, Cloudflare contributed to research revealing an attack on RADIUS that exploited its reliance on MD5. This example underscores the enormous challenge of updating legacy systems — this difficulty in achieving _crypto-agility_ — which will be just as demanding when it’s time to transition to post-quantum cryptography. So it makes sense to start this process now.

#### 2\. The “harvest now, decrypt later” threat

Even though quantum computers lack enough qubits to break conventional cryptography today, adversaries can harvest and store encrypted communications or steal datasets with the intent of decrypting them once quantum technology matures. If your encrypted data today could become a liability in 10 to 15 years, planning for a post-quantum future is essential. For this reason, we have already started working with some of the most innovative banks, ISPs, and governments around the world as they begin their journeys to quantum safety. 

The U.S. government is already addressing these risks. On January 16, 2025, the White House issued Executive Order 14144 on Strengthening and Promoting Innovation in the Nation’s Cybersecurity. This order requires government agencies to “_regularly update a list of product categories in which products that support post-quantum cryptography (PQC) are widely available…. Within 90 days of a product category being placed on the list … agencies shall take steps to include in any solicitations for products in that category a requirement that products support PQC._”

At Cloudflare, we’ve been researching, developing, and standardizing post-quantum cryptography since 2017. Our strategy is simple:

**Simply tunnel your traffic through Cloudflare’s quantum-safe connections to immediately protect against harvest-now-decrypt-later attacks, without the burden of upgrading every cryptographic library yourself.**

Let’s take a closer look at how the migration to post-quantum cryptography is taking shape at Cloudflare.

### A two-phase migration to post-quantum cryptography

At Cloudflare, we’ve largely focused on migrating the TLS (Transport Layer Security) 1.3 protocol to post-quantum cryptography.   TLS primarily secures the communications for web applications, but it is also widely used to secure email, messaging, VPN connections, DNS, and many other protocols.  This makes TLS an ideal protocol to focus on when migrating to post-quantum cryptography.

The migration involves updating two critical components of TLS 1.3: digital signatures used in certificates and key agreement mechanisms. We’ve made significant progress on key agreement, but the migration to post-quantum digital signatures is still in its early stages.

#### Phase 1: Migrating key agreement

Key agreement protocols enable two parties to securely establish a shared secret key that they can use to secure and encrypt their communications. Today, vendors have largely converged on transitioning TLS 1.3 to support a post-quantum key exchange protocol known as ML-KEM (Module-lattice based Key-Encapsulation Mechanism Standard). There are two main reasons for prioritizing migration of key agreement:

- **Performance:** ML-KEM performs well with the TLS 1.3 protocol, even for short-lived network connections.
    
- **Security**: Conventional cryptography is vulnerable to “harvest now, decrypt later” attacks. In this threat model, an adversary intercepts and stores encrypted communications today and later (in the future) uses a quantum computer to derive the secret key, compromising the communication. As of March 2025, well over a third of the human web traffic reaching the Cloudflare network is protected against these attacks by TLS 1.3 with hybrid ML-KEM key exchange.
    

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1Tgfy0HYHA5MM6JjaNP2Z1/b601d2938be3c52decf1f3cec7313c6e/image6.png)

_Post-quantum encrypted share of human HTTPS request traffic seen by Cloudflare per_ _Cloudflare Radar_ _from March 1, 2024 to March 1, 2025. (Captured on March 13, 2025.)_

Here’s how to check if your Chrome browser is using ML-KEM for key agreement when visiting a website: First, Inspect the page, then open the Security tab, and finally look for X25519MLKEM768 as shown here:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6EoD5jFMXJeWFeRtG9w6Uy/85aa13123d64f21ea93313f674d4378f/image1.png)

This indicates that your browser is using key-agreement protocol ML-KEM _in combination with_ conventional elliptic curve cryptography on curve X25519. This provides the protection of the tried-and-true conventional cryptography (X25519) alongside the new post-quantum key agreement (ML-KEM).

#### Phase 2: Migrating digital signatures

Digital signatures are used in TLS certificates to validate the authenticity of connections — allowing the client to be sure that it is really communicating with the server, and not with an adversary that is impersonating the server. 

Post-quantum digital signatures, however, are significantly larger, and thus slower, than their current counterparts. This performance impact has slowed their adoption, particularly because they slow down short-lived TLS connections. 

Fortunately, post-quantum signatures are not needed to prevent harvest-now-decrypt-later attacks. Instead, they primarily protect against attacks by an adversary that is actively using a quantum computer to tamper with a live TLS connection. We still have some time before quantum computers are able to do this, making the migration of digital signatures a lower priority.

Nevertheless, Cloudflare is actively involved in standardizing post-quantum signatures for TLS certificates. We are also experimenting with their deployment on long-lived TLS connections and exploring new approaches to achieve post-quantum authentication without sacrificing performance. Our goal is to ensure that post-quantum digital signatures are ready for widespread use when quantum computers are able to actively attack live TLS connections.

### Cloudflare Zero Trust + PQC: future-proofing security

The Cloudflare Zero Trust platform replaces legacy corporate security perimeters with Cloudflare's global network, making access to the Internet and to corporate resources faster and safer for teams around the world. Today, we’re thrilled to announce that Cloudflare's Zero Trust platform protects your data from quantum threats as it travels over the public Internet.  There are three key quantum-safe use cases supported by our Zero Trust platform in this first phase of quantum readiness.

#### Quantum-safe clientless Access

Clientless Cloudflare Access now protects an organization’s Internet traffic to internal web applications against quantum threats, even if the applications themselves have not yet migrated to post-quantum cryptography. ("Clientless access" is a method of accessing network resources without installing a dedicated client application on the user's device. Instead, users connect and access information through a web browser.)

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5mKiboLMsIEuNt1MaXlWsy/dad0956066e97db69401757b18e8ce5f/image4.png)

Here’s how it works today:

- **PQ connection via browser:** (Labeled (1) in the figure.) As long as the user's web browser supports post-quantum key agreement, the connection from the device to Cloudflare's network is secured via TLS 1.3 with post-quantum key agreement.
    
- **PQ within Cloudflare’s global network:** (Labeled (2) in the figure)  If the user and origin server are geographically distant, then the user’s traffic will enter Cloudflare’s global network in one geographic location (e.g. Frankfurt), and exit at another (e.g. San Francisco).  As this traffic moves from one datacenter to another inside Cloudflare’s global network, these hops through the network are secured via TLS 1.3 with post-quantum key agreement. 
    
- **PQ Cloudflare Tunnel:** (Labeled (3) in the figure) Customers establish a Cloudflare Tunnel from their datacenter or public cloud — where their corporate web application is hosted — to Cloudflare's network. This tunnel is secured using TLS 1.3 with post-quantum key agreement, safeguarding it from harvest-now-decrypt-later attacks.
    

Putting it together, clientless Access provides **end-to-end** quantum safety for accessing corporate HTTPS applications, without requiring customers to upgrade the security of corporate web applications.

#### Quantum-safe Zero Trust with Cloudflare’s WARP Client-to-Tunnel configuration (as a VPN replacement)

By mid-2025, organizations will be able to protect **any protocol**, not just HTTPS, by tunneling it through Cloudflare's Zero Trust platform with post quantum cryptography, thus providing quantum safety as traffic travels across the Internet from the end-user’s device to the corporate office/data center/cloud environment.

Cloudflare’s Zero Trust platform is ideal for replacing traditional VPNs, and enabling Zero Trust architectures with modern authentication and authorization polices.  Cloudflare’s WARP client-to-tunnel is a popular network configuration for our Zero Trust platform: organizations deploy Cloudflare’s WARP device client on their end users’ devices, and then use Cloudflare Tunnel to connect to their corporate office, cloud, or data center environments.   

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5xovIIyVOO32xrXBs0ZFcf/110928926b86f12777f16518b1313875/image3.png)

 Here are the details:  

- **PQ connection via WARP client (coming in mid-2025):** (Labeled (1) in the figure) The WARP client uses the MASQUE protocol to connect from the device to Cloudflare’s global network. We are working to add support for establishing this MASQUE connection with TLS 1.3 with post-quantum key agreement, with a target completion date of mid-2025.  
    
- **PQ within Cloudflare’s global network:**  (Labeled (2) in the figure) As traffic moves from one datacenter to another inside Cloudflare’s global network, each hop it takes through Cloudflare’s network is already secured with TLS 1.3 with post-quantum key agreement.
    
- **PQ Cloudflare Tunnel:** (Labeled (3) in the figure) As mentioned above, Cloudflare Tunnel already supports post-quantum key agreement. 
    

Once the upcoming post-quantum enhancements to the WARP device client are complete, customers can encapsulate their traffic in quantum-safe tunnels, effectively mitigating the risk of harvest-now-decrypt-later attacks without any heavy lifting to individually upgrade their networks or applications.  And this provides comprehensive protection for any protocol that can be sent through these tunnels, not just for HTTPS!

#### Quantum-safe SWG (end-to-end PQC for access to third-party web applications)

A Secure Web Gateway (SWG) is used to secure access to third-party websites on the public Internet by intercepting and inspecting TLS traffic. 

Cloudflare Gateway is now a quantum-safe SWG for HTTPS traffic. As long as the third-party website that is being inspected supports post-quantum key agreement, then Cloudflare’s SWG also supports post-quantum key agreement. This holds regardless of the onramp that the customer uses to get to Cloudflare's network (i.e. web browser, WARP device client, WARP Connector, Magic WAN), and only requires the use of a browser that supports post-quantum key agreement.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6vnkEFkvKbhSAxp33GmRk7/c58d00a14767a03b2422af1c48a53ba9/image5.png)

Cloudflare Gateway's HTTPS SWG feature involves two post-quantum TLS connections, as follows:

- **PQ connection via browser:** (Labeled (1) in the figure)  A TLS connection is initiated from the user's browser to a data center in Cloudflare's network that performs the TLS inspection. As long as the user's web browser supports post-quantum key agreement, this connection is secured by TLS 1.3 with post-quantum key agreement.  
    
- **PQ connection to the origin server:** (Labeled (2) in the figure)  A TLS connection is initiated from a datacenter in Cloudflare's network to the origin server, which is typically controlled by a third party. The connection from Cloudflare’s SWG currently supports post-quantum key agreement, as long as the third party’s origin server also already supports post-quantum key agreement.  You can test this out today by using https://pq.cloudflareresearch.com/ as your third-party origin server. 
    

Put together, Cloudflare’s SWG is quantum-ready to support secure access to any third-party website that is quantum ready today or in the future. And this is true regardless of the onramp used to get end users' traffic into Cloudflare's global network!

### The post-quantum future: Cloudflare’s Zero Trust platform leads the way

Protecting our customers from emerging quantum threats isn't just a priority — it's our responsibility. Since 2017, Cloudflare has been pioneering post-quantum cryptography through research, standardization, and strategic implementation across our product ecosystem.

**Today marks a milestone:** We're launching the first phase of quantum-safe protection for our Zero Trust platform. Quantum-safe clientless Access and Secure Web Gateway are available immediately, with WARP client-to-tunnel network configurations coming by mid-2025. As we continue to advance the state of the art in post-quantum cryptography, our commitment to continuous innovation ensures that your organization stays ahead of tomorrow's threats.  Let us worry about crypto-agility so that you don’t have to.

To learn more about how Cloudflare’s built-in crypto-agility can future-proof your business, visit our Post-Quantum Cryptography webpage.

### Watch on Cloudflare TV

Go to Source
