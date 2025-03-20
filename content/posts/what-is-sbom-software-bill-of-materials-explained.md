---
title: "What is SBOM? Software bill of materials explained"
date: 2025-01-02
categories: 
  - "general"
---

In the wake of EU drafted legislation and US executive orders, a software bill of materials (SBOM) has gone from a nice-to-have to a fundamental piece of software documentation. In this article, we’ll examine what an SBOM is, what information it must include, and the approaches that developers and manufacturers alike should consider as they start building their SBOM.

# What is a software bill of materials?

A software bill of materials has lots of definitions with common core elements. 

In the United States, the National Telecommunications and Information Administration (NTIA) defines a software bill of materials as “a complete, formally structured list of components, libraries, and modules that are required to build (i.e. compile and link) a given piece of software and the supply chain relationships between them. These components can be open source or proprietary, free or paid, and widely available or restricted access”. Over in the EU, the Cyber Resilience Act defines SBOMs as “a formal record containing details and supply chain relationships of components included in the software elements of a product with digital elements.” 

Related video: What is the Cyber Resilience Act (CRA)? | Open Source Cybersecurity 

The White House takes a simpler approach, likening SBOMs to a list of ingredients on food packaging. It calls SBOMs “a formal record containing the details and supply chain relationships of various components used in building software.” 

So, in short, an SBOM boils down to a detailed and accessible list of all the components that make up your software and where they come from.

# Why is an SBOM important? 

The White House’s food packaging comparison is a perfect analogy of why SBOMs are important: it lets you know what’s in the food and whether it’ll make you sick. Just like how food packaging can show you what allergens are in a product, where they’re from, and inform you whether the food is safe to eat, an SBOM tells you where software (and its constituent parts) comes from and how safe it is to use.

SBOMs are useful for everyone who uses your software: buyers, developers, procurement, regulators, end users and consumers. This is because an SBOM:

- Tells users and developers about an application’s code and contents
- Tells users and developers where an application and its source code come from
- Reveals the safety, dependencies, and vulnerability of an application’s software 
- Highlights potential chain-of-supply attack vulnerabilities and CVEs
- Reveals potential compliance issues over software sourcing
- Exposes where components might be unsupported, incompatible, or outdated
- Helps developers to figure out where core components of software could be replaced, improved, refactored, or upgraded for better software
- Helps to manage updates, licences, implementations, mitigations, migrations, and upgrades
- Helps with risk assessment in further software development efforts
- Makes it faster and easier to meet conformity and compliance requirements of cybersecurity regulations such as the Cyber Resilience Act
- Helps manufacturers ensure their devices do not contain vulnerable components developed by third parties

# The benefits of an SBOM

Let’s break down what the benefits of an SBOM are for the biggest groups of software and device users.

### For software developers

An SBOM maps out a software supply chain and comprehensively lists what a software stack is made of; this directly lessens the difficulty, time, and effort needed to maintain, develop, and fix that software. As a result, SBOMs accelerate code review time and reduce review difficulty, as well as reduce maintenance work and unplanned downtime. 

SBOMs also make the software product easier to improve. It helps developers to understand and decrease in the complexity and ‘bloat’ of software and its source code, reveal critical software dependencies, and avoid components becoming unsupported or reaching their end-of-life 

### For companies buying software or devices

SBOMs help companies navigate their software purchasing decisions better, make business faster, and improve the security of their software products. Thanks to SBOMs, it’s easier to perform due diligence, as one can easily determine if software or its components are blacklisted, unsecure, or compromised. With this one document, companies can tell if components are at risk of EoL or vulnerabilities, and are better equipped to determine whether a piece of software will be able perform as advertized. Finally, SBOMs improve the cost and financial operations of purchasers: it’s easier to assess expected costs of software and make informed comparisons between products, as well as understand licence requirements and avoid hidden or unexpected licence costs

### For manufacturers and suppliers

SBOMs help manufacturers gain better transparency and security. WIth an SBOM, manufacturers can ascertain the security of the firmware or software they install on their devices, Identify redundancies and optimizations in devices and device software, and more easily meet compliance and regulatory requirements for device security. This all translates to safer software and devices, better cost-savings and code optimization, and increased consumer confidence.

# What should an SBOM include?

An SBOM can come in different levels of depth and detail.

A base SBOM outlines the core tools, frameworks, libraries, and modules, and other components that make up the software. This approach should tell the user the basics and top-level information about the software and components that you’ve used to build your  device or software. This would include:

- The device’s firmware
- In-house or proprietary components and code
- Open source software and source code
- Third-party and other software 
- Libraries
- Frameworks
- Modules

For more mature organizations and software, an SBOM can be more advanced. This version goes beyond a base SBOM to include detailed information about each individual module. This could include:

- All pertinent information about the component and its application or use in your product
- The name of the component
- The authors or creators of the component
- The suppliers of the component
- The version or version string of the component being used in your product
- The Hash of the component 
- The component’s unique identifier (ie its universally unique identifier (UUID) or globally unique identifier)
- The relationships, dependencies, and connections between components
- Time stamps and dates related to the components updates, adoptions, versioning, and so forth
- The licence information of the component (if applicable)
- The pedigree or the component (if available)
- The provenance of the component (if available)
- Unknown areas, where developers acknowledge grey areas in the software (eg unknown components, dependencies, or vulnerabilities)

The NTIA has an extensive guide to establishing and building a comprehensive SBOM on its website. 

# How to distribute your SBOM

The most important thing is that an SBOM is as accessible as possible. You can make your SBOM available to the public and regulators on one or more of the following:

- Product webpage
- Organization, developer, or manufacturer website 
- On the device itself
    - In a machine-readable format
    - On a user-accessible storage device
    - On the product label
- In the device’s supplementary materials
    - In the product user guide
    - In digital files made available to the user 
- As a Readme file in the product’s distribution, 
- on disk next to the binary 
- On a centralized website where the product is sold, listed, or hosted
- On a trusted third-party website where the product is sold, listed, or hosted
- On a pointer from device (for example, a MUD) 

# When should you update your SBOM?

Because of patches, updates, migrations, upgrades, and alterations, software changes all the time. As your software changes, your SBOM should alos be updated to reflect the latest snapshot and details of your software supply chain. 

As a rule, your SBOM should be updated when you have a new release of your product. This includes upgrades, new versions, modifications or patches, or entirely new releases. You should also update your SBOM if information in the components or of your software supply chain changes and these changes affect the current public version of your product – for example, the name, owner, author, publisher, or other such core information of the component is altered. 

Similarly, your SBOM should be updated when there are dependency changes in your software. These can seem small, but chain-of-supply attacks are increasingly risky and so reporting these sub-component dependencies is vital.

And finally, you should update your SBOM with missing, unknown or grey areas in your software supply chain. If there are unknowns or question marks in your software, it could have serious implications for vulnerabilities that your downstream users must be aware of.

# Approaches and best practices

There are a few considerations you need to take when planning for, creating, and maintaining your SBOM.

**Distribution/publication methods** – where will your SBOM be hosted? Will it be physically available, or hosted digitally online, or electronically on the device or source code itself? For consumer peace of mind it’s advised to use multiple distribution channels to inform users of your SBOM and future changes to this vital documentation. Remember that you’ll need a clear process that ensures consistency across all of your chosen distribution channels – your SBOM needs to be up to date everywhere it’s accessed. More publication methods means more work, so an automated distribution of a centralized version of your SBOM would be advised.

**Cadence**  – as a rule, your SBOM should be updated with every major release – but what will you do when there are minor releases, daily releases, or debug builds? Depending on the complexity of your SBOM and the number of distribution methods you use, more frequent SBOM updates could be labour intensive or time consuming, and so you should be thinking about cadence, especially when you have a frequent release schedule.

**Discoverability** – how do you know when components of your software supply chain have changed? How long does it take to discover a component has changed? Your team needs robust methods of tracking changes in your software supply chain and alerting the relevant parties when an SBOM update needs to be issued. 

**Provenance (or chain of custody)** – where does the information in your SBOM come from? Is it direct from the supplier’s website, a third-party source, or from the project documentation? Is the publisher of this information who they say there are? Being able to attest to the accuracy and credibility of your SBOM’s information source could be extremely important to suppliers and users.

**Communications** – how do you communicate internally when a component change is discovered in your software supply chain? How do you alert suppliers or end users of your product? You should have clear and dedicated channels of communication to ensure that all relevant parties – whether internal or external – know that there have been changes to your SBOM.

**Integrity** – is your SBOM secure? How can readers know for sure that it hasn’t been secretly altered since its publication date? Keeping your SBOM secure is just as important as keeping your software secure; you should strongly consider cryptographic and other security techniques that give end users complete certainty that an SBOM is accurate and free of misleading (or even outright malicious) alterations or omissions.

**Pedigree** – very often it’s important to know not just what the software supply chain is made of, but also _how it was put together._ The pedigree of your SBOM gives insights into the production and history of your SBOM components: the processes by which they were built, stored, or altered. This can be extremely valuable, as it could demonstrate whether a piece of software is hardened against or vulnerable to certain classes of attacks or vulnerabilities. 

**Control** – who is responsible for updating the SBOM in your organization? What are the processes and access permissions for updating the SBOM? You need to decide on clear processes to follow to ensure your SBOM is kept up to date and updated as soon as new changes are made in your software supply chain.

## Where to start in your SBOM project

In conclusion, an SBOM is an ‘ingredients list’ of your software supply chain that takes many forms – from websites and downloadable resources, to source code files or printed info pamphlets – and that tells users what it’s made of, where it’s from, and how safe it is. As European and US legislation grows, so too do the requirements for transparency in software supply chains; with SBOMs set to be a mandatory part of publicly accessible documentation, you, your developers, and your compliance teams should already be creating clear processes and plans for your SBOM, or risk falling prey to regulatory non-compliance and the eye-watering penalties that come with it.

## Further reading

- Get secure open source from Canonical 
- Watch: Secure your stack with Canonical | Open Source Cybersecurity 
- Download our guide to vulnerability management 

Image: Photo by Jefferson Santos on Unsplash

Go to Source
