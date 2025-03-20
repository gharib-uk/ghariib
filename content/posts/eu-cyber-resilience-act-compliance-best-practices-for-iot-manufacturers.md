---
title: "EU Cyber Resilience Act compliance: best practices for IoT manufacturers"
date: 2025-02-06
---

The European Union’s Cyber Resilience Act (EU CRA, or CRA for short) is coming. I’ve talked about this piece of upcoming regulation in some depth before, having covered its background and stipulations in previous pieces on our website and for the Forbes Technology Council, and explored what it means for the businesses who consume open source in later articles (you can also read a version of this blog on Forbes). However, with the clock ticking and time running out to make your devices and software compliant with the Act’s new cybersecurity requirements, I thought I’d talk about how you should be approaching your device design and cybersecurity foundation. In this blog, I’ll run through a blueprint for cybersecurity, and outline what you should be doing to meet EU CRA compliance if you’re an IoT or device manufacturer. 

## What is the EU Cyber Resilience Act?

The CRA is a piece of European Union legislation that aims to make devices safer by implementing more rigorous cybersecurity, documentation, and vulnerability reporting requirements into the EU’s IT industry. The legislation would apply to developers, distributors, manufacturers and retailers of hardware, devices, software, applications, and other “products with digitally connected elements”. The CRA’s requirements, rules and regulations would cover those products throughout their life cycle.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen frameborder="0" height="304" loading="lazy" referrerpolicy="strict-origin-when-cross-origin" src="https://www.youtube.com/embed/aesdxS_JqFc?feature=oembed" title="Understand how the Cyber Resilience Act will impact device manufacturers" width="540"></iframe>

## How device manufacturers can prepare for the EU CRA

Broadly speaking, the CRA concerns itself with several key areas that all device manufacturers and developers should be focusing on. These are: 

1. Robust organizational security processes
2. Good cybersecurity principles at the design level of the device
3. Robust device security, such as better identity management and default access
4. Clear processes for monitoring and patching vulnerabilities
5. A clear roadmap for ongoing support and security updates for devices on the market
6. Proper and standardized reporting channels and processes for vulnerability discoveries
7. Public information and awareness of security flaws, vulnerabilities
8. A comprehensive listing of the device’s  software supply chain
9. Clear and compliant policies for the legal collection, storage, and processing of private user data

Of course, a concept like “improve device security” can seem overwhelming. Where would you even begin? Well, as a starting point, I’d highly recommend the Ubuntu blueprint that we use for device security. Let’s take a look at how our blueprint guides compliant device design.

## The Ubuntu blueprint for creating secure devices that comply with the EU CRA

### Start at the OS level

You should always assume that your devices are going to be deployed in an insecure environment. Therefore the OS they employ is the most critical security factor you need to consider, as it is the basis of all other security measures. 

### The OS should be simple, functional, and have a minimal attack surface

IoT devices often have very few resources available in terms of CPU cycles, memory, and storage. As a result, the OS they use needs to be streamlined and take up as little space as possible. 

However, reducing data usage and increasing simplicity is about much more than getting an OS to run on a device; it also helps to make an OS more secure. The less complexity, the fewer points of vulnerability.

### Your IoT OS needs to include security updates

New vulnerabilities are discovered every single day, and the CRA mandates that you discover, patch and report exploited risks as soon as they occur. Your device design, and the device OS itself, needs to have a strong support foundation, with easy methods for consistent patches and remote updates.

### The OS must have a centralized update mechanism 

The ability to update software reliably and automatically is non-negotiable for any IoT OS that has to meet CRA readiness. This is especially important for IoT deployments that feature large numbers of devices, devices without a regular or predictable update plan, devices without a clear UI for end-user updates, or devices that are deployed in locations that are very difficult to access.

Authenticating these updates is equally important. Authenticated updates provide a mechanism to shield users from the negative consequences of human error or vendor failures. When your devices receive updates, it’s vital to be certain that these updates are verified and correct. 

### The OS should feature automatic, inbuilt rollback of updates

Of course, sometimes things go wrong. The wrong update is pushed, or an update introduces new, unforeseen vulnerabilities. Your on-device OS needs a mechanism that insures against situations like this: should an update fail, your OS must be able to automatically roll back to the last known working configuration, thus maximizing the chances that devices remain not only secure, but also functional.

### Full secure system and other critical files

Preventing unauthorized tampering with trusted files is vital. As a baseline, your essential device and application files should be authenticated and protected against unwanted changes through read-only protection. These files should also require universal digital authentication to be replaced. This allows a far greater opportunity to examine the integrity of each snap before installation and guarantee this integrity over time. 

### Applications should be self-contained and sandboxed 

Sometimes apps or devices will need to share data with one another, but these situations should be treated as specific cases and specifically designed as a function or process. This is because allowing free flow of data between applications or files by default gives malicious actors a potent attack vector. By default, your files and applications should be segregated into their own restrictive sandboxes, minimizing the damage that can be done by malevolent or malfunctioning applications. 

### The OS should feature familiar architectures and known coding methods 

Simpler is always better. It can be tempting to use niche frameworks, architecture, or languages to build your device or applications, but it’s almost always better to operate within a known framework and ecosystem. In general, using more conventional stacks and on-device OSes brings more support, faster fixes, and better vulnerability discovery, which helps you to respond to the security and disclosure requirements of the CRA.

### Ongoing support in the long term must be a day-one consideration

Short product runs and low profit margins leave many companies feeling little incentive to provide costly long-term support. However, the CRA requires manufacturers to provide a schedule of updates to their devices across their entire life cycle. 

Your market approach should consider support strategies that make it cost-effective and simple to ensure your products remain viable and functional, even years past their intended life cycle. 

### Documentation and software supply chains should be clear and comprehensive

The CRA will require you to document and disclose quite a large amount of information about your products or services. Given the long list of requirements – which  includes everything from the manufacturer information and product’s use cases and functionality, to your device’s entire software bill of materials(SBOM) and security properties – this could be a highly intensive process. Beyond improving your internal documentation, you should refine your choice of upstream providers to take advantage of software stacks with well-documented, already-available lists of their dependencies, components and other vital information. It is likely that the easiest way you can get all this information is to consume software through commercial vendors or large-scale open source organizations with a long track record of thorough documentation. 

## Consider using CRA-ready providers

Meeting CRA compliance on your own is a real challenge. If you’re unsure about your software supply chain and its ability to meet the CRA’s regulatory standards, documentation requirements, vulnerability disclosure demands and transparency expectations, you should evaluate your service and software providers to choose those who make it effortless to meet your CRA obligations. 

For some configurations and device designs, the easier route is to use a stack of open source tools that are designed around CRA compliance. The many, many tools and products we develop and maintain at Canonical – from Ubuntu, to Kubernetes, to MicroCloud, OpenStack, and hundreds more – are designed with security in mind, supported through security maintenance and vulnerability patching, and aligned with the regulatory oversight in the CRA. On top of this, services like Ubuntu Pro for Devices ensure your devices will receive security maintenance for up to 12 years. 

In conclusion, the CRA will have considerable effects on your IoT devices. From documentation requirements and update schedules to long-term device life cycle management.  There are many factors of your IoT device design and security that you need to be thinking about consistently. If you want your product to remain viable for EU markets, you should be reexamining not just the OS and tech stack your IoT device runs on, but also the cybersecurity approaches and processes that you rely on to get your products ready to market.  

## Learn more about the EU CRA

- Watch our introductory webinar on the CRA
- Get insights from compliance experts to understand the impact on devices
- Explore the impacts of the CRA on open source
- Cyber Resilience Act: Yocto or Ubuntu Core for embedded devices?

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1200,h_400/https://ubuntu.com/wp-content/uploads/af9c/CRA-Yocto-vs-Core-wp-banner-1.jpg)

Go to Source
