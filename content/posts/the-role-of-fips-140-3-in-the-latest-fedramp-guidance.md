---
title: "The role of FIPS 140-3 in the latest FedRAMP guidance"
date: 2025-02-06
---

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1280,h_720/https://ubuntu.com/wp-content/uploads/37b4/Latest-Fedramp-guidance.png)

There’s good news in the US federal compliance space. The latest FedRAMP policy on the use of cryptographic modules relaxes some of the past restrictions that prevented organizations from applying critical security updates.

There has long been a tension between the requirements for strictly certified FIPS crypto modules and the need to keep software patched and up to date with the latest security vulnerability fixes. The new guidance goes a ways to resolving this tension – and best of all, it aligns perfectly with how we’ve already been approaching FIPS module updates for Ubuntu.

In this post we cover the basics of FIPS certification along with the new FedRAMP guidance that prioritizes security updates.

## What is FIPS 140-3?

FIPS 140-3 is a NIST standard for ensuring that cryptography has been implemented correctly, protecting users against common pitfalls such as misconfigurations or weak algorithms. All US Government departments, federal agencies, contractors, and service providers to the Government are required to use FIPS validated crypto modules, and a number of industries have also adopted the FIPS 140-3 standard as a security best practice. A FIPS-compliant technology stack is therefore essential in these sectors, and Ubuntu provides the building blocks for a modern and innovative open source solution.

## The latest FedRAMP guidance

A major use-case for FIPS-validated cryptography is for FedRAMP-accredited service providers. Up till now, the FedRAMP guidelines have mandated a strict adherence to using certified modules. This approach is changing, however, and new FedRAMP policy is now in place, aimed at giving cloud service providers and their independent assessors a more modern risk-based approach to security and compliance:

-  https://www.fedramp.gov/cryptographic-module/

These requirements highlight the fact that whilst using FIPS-certified crypto is important, there is greater risk facing organizations who deploy code that contains known vulnerabilities. This new approach balances the use of FIPS-certified modules with vendors supplying critical security fixes, which matches exactly what Canonical provides with our module updates. 

## FIPS 140-3 certification

For Ubuntu, we have chosen a set of cryptographic libraries and utilities that have the widest usage and converted them to operate in FIPS mode. This means that we have disabled various disallowed algorithms and ciphers from the libraries, and made sure that they work by default in a FIPS compatible mode of operation. The exact modules may vary between LTS releases, though we currently include the userspace libraries OpenSSL, Libgcrypt & GnuTLS, along with the Linux kernel, which also provides the validated source of random entropy data.

In order to ensure that we have implemented the FIPS specifications correctly, we work with an accredited Cryptographic and Security Testing Laboratory (CSTL) which validates the implementation through a rigorous series of functional and operational tests. Our current lab partner is **atsec information security**, who we have engaged since 2016 for this work.

The final certification step is for NIST’s Cryptographic Module Validation Program to perform their own checks; when they are satisfied they will publish the official certificates and policy documents.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_400/https://ubuntu.com/wp-content/uploads/55ac/FIPS-140-3-webinar.png)

## How we apply patches

Ubuntu packages are built up of layers: we start with the upstream source code, and then apply subsequent patch sets. The first patches comprise any modifications we choose to make for compatibility – the changes that we apply in order to make the software package operate seamlessly with the rest of the software in the distribution. Security patches for fixing vulnerabilities are then applied on top of this.

When we convert the crypto packages to operate in FIPS mode, we make a set of patches that are applied after all the rest of the previous patches. In particular, the FIPS changes sit on top of the security fixes (where the security fixes don’t affect the FIPS-specific code, which is the majority case). This means that we can have a high degree of certainty that the FIPS functionality remains unchanged and the security fixes won’t affect the FIPS status of the modules, and hence why we can offer the updated modules to our customers for deployment in their FIPS environments.

## Strict versions and updates

NIST assesses a particular version of the cryptographic module and issues the FIPS 140-3 certificates and security policy documents for this specific set of binary files. For Ubuntu, that means that we submit versions of the packages for certification and these versions then remain effectively frozen in time from the moment that they are handed over for certification. In particular, these packages don’t receive any further security updates to fix any vulnerabilities that are discovered in the time after submission. The certification process can take months, or even years, and then the certificates are valid for 5 years henceforth. Even at the point when the certificates are issued, the modules may well contain unpatched vulnerabilities, and the longer the period since they were frozen in time the more vulnerable they become.

In order to address this obvious shortcoming, Canonical also provides updated versions of the FIPS 140-3 modules that include all the relevant security fixes. These modules are therefore no longer the exact same set of binary files that were submitted for validation, but we assert that the FIPS functionality that was assessed by the testing lab and by NIST remains unchanged, and we strongly recommend that everybody uses these updated versions so that your systems remain fully secure.

## Which FIPS to use? -updates, -preview, strict

Canonical has provided FIPS modules in several forms up till now: 

- “fips” – strict certified modules
- “fips-updates” – certified modules with security updates
- “fips-preview” – the strict modules prior to certification

We recommend that everyone uses the “**fips-updates**” modules to remain fully secure against security exploits. In the past, we have provided “fips” modules, which are not updated and likely contain known vulnerabilities, for customers who had a strong need to satisfy strict regimes – such as FedRAMP – that required exact binary versions of the certified modules. Now that the new FedRAMP guidance is being updated to take a more integrated view of security, we will be deprecating the strict “fips” modules, and encouraging everyone to use the “**fips-updates**”.

With Ubuntu 22.04 we introduced another version of the modules called “fips-preview”, which contained the strict binary packages that were submitted to NIST for validation, again without the security fixes applied. This will also be deprecated in favour of “**fips-updates**”.

## Conclusion

Canonical welcomes the holistic approach to security espoused by the latest FedRAMP guidelines, which aligns perfectly with the FIPS 140-3 modules update strategy that we have already been supporting to provide our customers with the best possible security posture. This combines the NIST-validated crypto implementation needed to meet the strictest government standards with Canonical’s 10+ years of security patching, all within the popular Ubuntu ecosystem.

For questions, contact us here.

Register for the webinar: **Taking advantage of FIPS 140-3 Certification for Ubuntu 22.04 LTS**

Go to Source
