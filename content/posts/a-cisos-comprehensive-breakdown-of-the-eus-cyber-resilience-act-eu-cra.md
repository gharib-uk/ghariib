---
title: "<div>A CISO’s comprehensive breakdown of the EU’s Cyber Resilience Act (EU CRA)</div>"
date: 2025-02-06
---

Strong, wide-reaching regulation can bring safety to communities – but it can also bring uncertainty. The European Union’s Cyber Resilience Act (EU CRA, or just CRA for short) has proven no exception to this universal rule. Across the open source community and the wider tech landscape, people have been greeting the news with the whole spectrum of reactions: concern, anxiety, hope. 

But is there anything to fear? Does the CRA really change things in open source? And how should your teams be preparing for this legislation? In this article, I’ll dive into the CRA and explain its aims, requirements, and effects on the open source community, and give my cybersecurity recommendations to prepare for the CRA.

## What is the EU Cyber Resilience Act (EU CRA)?

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/ltBtIDvav6c?feature=oembed" title="What is the Cyber Resilience Act?" width="540"></iframe>

The CRA is a piece of European Union legislation that aims to make devices safer by implementing more rigorous cybersecurity, documentation, and vulnerability reporting requirements into the EU’s IT industry. The legislation would apply to developers, distributors, manufacturers and retailers of hardware, devices, software, applications, and other “products with digitally connected elements”. The CRA’s requirements, rules and regulations would cover those products throughout their life cycle.

> “_The_ _CRA_ _aims to safeguard consumers and businesses buying or using products or software with a digital component. The Act would see inadequate security features become a thing of the past with the introduction of mandatory cybersecurity requirements for manufacturers and retailers of such products, with this protection extending throughout the product life cycle.”_
> 
> _EU Cyber Resilience Act_

It promises:

- _harmonised rules when bringing to market products or software with a digital component;_
- _a framework of cybersecurity requirements governing the planning, design, development and maintenance of such products, with obligations to be met at every stage of the value chain;_
- _an obligation to provide duty of care for the entire lifecycle of such products._

If you want to read it for yourself first, you can read the entire CRA draft document, and if you’re just looking for the most pertinent points and a checklist of requirements and how they might apply to you, I’d recommend reading the Annexes to the CRA. 

Let’s take a look at the CRA’s main objectives, which will help to understand how the specific requirements fit in.

## What are the EU CRA’s main objectives? 

The CRA has four objectives:

1. Ensure that manufacturers improve the security of products with digital elements since the design and development phase and throughout the whole life cycle.
2. Ensure a coherent cybersecurity framework, facilitating compliance for hardware and software producers.
3. Enhance the transparency of security properties in products with digital elements.
4. Enable businesses and consumers to use products with digital elements securely.

For manufacturers and developers working with these regulated items, this means a few specific things:

- Manufacturers must make and keep devices secure before they’re built and after they’re sold.
- Hardware and software made public for sale must meet new EU compliance standards.
- Hardware and software must have clearer information and documentation about their components, design, and security. 
- Manufacturers and publishers are required to report critical vulnerabilities in their products.
- The list of hardware, software, and digital devices that will be subject to regulation is growing.

## What does the EU CRA mean for me?

This entirely depends on the CRA classification of the product, which has several classes for digital devices, or services you make. The most common ‘products with digital elements’ will fall outside of the critical classes I and II, which allows for self assessment, whereas the critical classes require third-party attestation and in some cases certification. Regardless of category, it means 4 big things:

1. There are rigorous new security standards your product will have to meet, in both its design and updates schedule, for its entire lifecycle.
2. There are a whole range of new requirements you’ll need to meet, including risk assessment, documentation access, conformity assessment, and vulnerability reporting.
3. The clock is ticking.
4. There could be serious penalties and fines for you if you don’t meet these new requirements. 

### New standards you need to meet for cybersecurity

At its core, the CRA establishes a new and more stringent set of ‘EU standards’ for software and digital products. These standards apply (in varying degrees) to developers, manufacturers, importers, and distributors.

These are the new security standards in brief:

- You need to make sure that whatever you’re building is as secure as it can be. It must have minimal attack surfaces. 
- Your device or product needs to be hardened. This means that: it has no unauthorised access; its data is encrypted or protected; and its data and commands can’t be intercepted or manipulated. It also means that your product keeps working, even under DoS attack; that it won’t interrupt other devices, even when attacked with exploits; and that it can provide security data by monitoring or logging changes in the device. 
- Your product needs to be able to receive security updates or rollbacks. This includes direct or remote updates, user notifications about updates, and the ability to roll back updates or reset the product to a factory/default state.

On top of that, you’ll also need to follow or provide: 

- New documentation requirements, with more communication around where this documentation can be accessed. 
- A clearer software supply chain and formal software bill of materials (SBOM). Your SBOM needs to be accessible and machine readable. 
- You’ll also need to do risk assessments of your products, and have a conformity assessment completed for your products. 
- You’ll also need a clear set of plans and processes to provide security updates for your product, and inform users and relevant EU authorities about vulnerabilities in your product.
- For a period of a maximum of 5 years (or the product lifespan, whichever is shorter) you’ll be required to recall or withdraw products that don’t meet conformity standards of the CRA. 

### New requirements in design, documentation, and conformity

There are four obligations set out by the CRA: 

1. Risk Assessment
2. Documentation
3. Conformity assessment
4. Vulnerability reporting

#### I. Risk Assessment

The manufacturer/developer must perform a cybersecurity risk assessment of their product. Briefly, this risk assessment determines:

- Can we deliver the product without any known exploitable vulnerabilities?
- Can we deliver the product with a ‘secure by default’ configuration?
- Does the product process as little data as possible?
- Does the product have the smallest attack surface possible?
- Does the product provide security updates and notify users of updates? 

If your device or product falls into the “Non-critical” category, then you can perform the assessment yourself. However, products in Class I and II have to be assessed by an independent third party.

#### II. Documentation

The CRA introduces new requirements for documentation and product information. In general, the manufacturer or developer would be required to provide:

- _A description of the design, development, and vulnerability handling process._
- _An assessment of cybersecurity risks._
- _A list of harmonised EU cybersecurity standards the product meets._
- _A signed EU Declaration of Conformity that the above essential requirements have been met._
- _A Software Bill of Materials (SBOM) documenting vulnerabilities and components in the product._

#### III. Conformity Assessment

Depending on the class that the device or product falls into, the product needs to undergo a conformity assessment to ensure that it meets all the requirements outlined in the CRA. 

| Class of product | Requirement |
| --- | --- |
| Non-critical products | The manufacturers/developers can perform a conformity assessment themselves. |
| Class I & Class II products | The assessment must be done by a “notified body” – i.e. an independent auditor certified by the EU. |

#### IV. Reporting and vulnerability handling

Under the CRA, software and hardware producers are required to report vulnerabilities to the European Union Agency for Cybersecurity (ENISA) within 24 hours. 

They also have a range of other requirements related to ongoing testing, updating and documentation of their product’s cybersecurity vulnerabilities. Developers must:

- Identify and document vulnerabilities that affect or are contained in your product. Your product should also have some clear address for users to report potential vulnerabilities. 
- Have an SBOM that covers its top-level dependencies at least. This SBOM should be accessible and machine readable.
- Ensure your devices can be securely updated against new vulnerabilities. Your updates must be free and sent out as soon as vulnerabilities are discovered, along with information to users about what actions they should take.
- Regularly test the product, and fix vulnerabilities immediately. Once a fix has been applied, you need to publicly disclose what the fixed vulnerability was (in line with the new coordinated public disclosure policy you need to have, under the CRA). You need to provide:
    - A description of the vulnerabilities and their severity.
    - Information allowing users to identify the product with digital elements affected.
    - The impacts of the vulnerabilities.
    - Information helping users to remediate the vulnerabilities.

### The clock is ticking

Time is running out to prepare for the CRA’s requirements. The CRA will enter into force on the 20th day following its publication in the Official Journal, in 2024 (which is expected at the beginning of May). Upon entry into force, manufacturers, importers and distributors of hardware and software products will have up to 36 months to adapt to the new requirements, with the exception of a more limited 21-month grace period in relation to the reporting obligation of manufacturers for incidents and vulnerabilities. 36 months sounds like a long time, but the more complex your product and operations the higher the demands will be. 

### There are new penalties and fines for non-compliance

If you’re found in breach of requirements, or don’t meet requirements, you’ll face monetary fines. This depends from country to country, as each Member State can decide its own fines and report them to the ENISA. However, current guidelines anywhere between 5 to 15 million euro or 1% to 2.5% of your worldwide annual turnover (whichever is highest) depending on the seriousness of your violation. For detailed information, see Chapter VII, Article 52 & 53, “Confidentiality and Penalties” of the CRA.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/aesdxS_JqFc?feature=oembed" title="Understand how the Cyber Resilience Act will impact device manufacturers" width="540"></iframe>

## What devices or products will the CRA affect?

The CRA is wide-reaching and its effects will vary depending on how your device or software is categorised. Devices and software are placed into three categories, based on their cybersecurity risk factor and their level of access authority or connection to sensitive infrastructure, networks, or systems. These are:

- Not critical: around 90% of all products according to the CRA. Examples are: smart speakers, hard drives, games and some high-level programming languages (Python and React)
- Critical Class I (lower risk): password managers; firewalls; microcontrollers; VPNs; management and configuration systems for networks or apps; web browsers; remote access software; Identity Management systems; and so on.
- Critical Class II (higher risk): Operating systems; microprocessors; industrial firewalls; CPUs; Container runtime systems; Public key infrastructure and digital certificate issuers; Hardware Security Models; and so on.

If you want a more exhaustive list of specific products and how they’re classified, you can read Annex 3 of the CRA.

At a glance, a list like that might make you think your product won’t fall into a critical class, but you should note that the CRA takes a very broad approach to vulnerabilities, stating that:

> _“Under certain conditions, all products with digital elements integrated in or connected to a larger electronic information system can serve as an attack vector for malicious actors.”_

This means that you’re not quite off the hook: even lower-class hardware or software can theoretically facilitate attacks on the larger devices or networks they connect to. 

As a result, even if you make less-critical products, you should be aware that you would be reasonably expected to design your product to meet the requirements of the CRA. This goes double for you if your product relies on other technologies, products or services that are otherwise safe or meet compliance, but through which a chain-of-vulnerability exploit could affect your users.

## What information must my product disclose or document?

As I mentioned above, the CRA will require you to document and disclose quite a large amount of information about your products or services. Most of this information needs to be made available on the product. Where this isn’t possible, you’ll need to include it in your product packaging or documentation. This could be in the form of new product stickers and warning labels, or new sections in your readme.txt or user manuals. 

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1492,h_1128/https://ubuntu.com/wp-content/uploads/1c51/CRA-documentation-checklist.png)

## What do I need to do to prepare for the EU CRA?

### Sit down with your CISO, security, and compliance teams and read it

Like with any CVE or cybersecurity vulnerability, you cannot prepare for what you don’t fully understand. Your first point of action should be to sit down with legal, compliance, and engineers to understand the final wording of the published CRA and identify areas where CRA regulation may apply. Don’t be afraid of overestimating your regulatory exposure: it’s better to over prepare and have extremely concrete documentation and cybersecurity principles, than it is to get caught out on a “safe” bet that “the wording isn’t exact enough to mean us”.

### Get documented

Documentation is vital. Make it clear to your customers where your software comes from, what it’s made of, and how safe it is (and how it’s kept safe). As a minimum, you need to have the following documented and available for the public and EU authorities:

- A description of the design, development, and vulnerability handling process
- An assessment of cybersecurity risks
- A list of harmonised EU cybersecurity standards the product meets
- A signed EU Declaration of Conformity that the above essential requirements have been met
- A Software Bill of Materials (SBOM) documenting vulnerabilities and components in the product

### Analyse and re-think your approach to cybersecurity

These days, security is non-negotiable. That’s good, because it means that most development organisations worth their salt will likely meet the basic cybersecurity requirements of the CRA just by meeting their current obligations, such as NIS2 or ISO27001.

However, now is the perfect time to take a hard look at your assessment, planning, and cybersecurity processes and design standards. Even if you’re not directly compelled by the CRA to up your cybersecurity standards, legislation like the CRA is a marker of a rising tide. The pressure is on for producers of all sorts of software, devices and products to demonstrate a safe, low-vulnerability foundation that users and buyers can trust implicitly. 

### Figure out a solid in-house process for reporting vulnerabilities

You need to have a clear process for reporting vulnerabilities as soon as you find them. This process should be baked into your internal teams and policies. You should have a team (or ideally, a core unit of specific team members with clear expectations for reporting) so that if the worst happens, you don’t forget to inform the EU CRA authority bodies, and run the risk of a fine. 

### Prepare for compliance and conformity assessment

Whether you’re being tested externally or doing your own assessment, you need a clear internal process and documentation for compliance and conformity assessment. You need to think about how your certification and documentation will be publicly accessible, and where you’ll advertise this accessible address (whether in your readme.txt, product info sheet, or on the device itself). 

In conclusion, the CRA is coming, and you need to be prepared. Depending on the Class your product falls into, there could be additional assessment, security, documentation, patching, compliance and reporting requirements on you and your teams. Find out how your digital product or service is categorised, reexamine your cybersecurity practices and design standards, and take a hard look at your internal processes to figure out how you can advertise your software supply chain and product information – as well as report new vulnerabilities – in an effective and timely manner. 

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_400/https://ubuntu.com/wp-content/uploads/af9c/CRA-Yocto-vs-Core-wp-banner-1.jpg)

Go to Source
