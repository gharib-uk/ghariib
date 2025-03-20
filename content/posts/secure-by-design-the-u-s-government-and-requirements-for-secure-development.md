---
title: "Secure by Design? The U.S. Government and Requirements for Secure Development"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "cybersecurity"
  - "dev"
  - "developers"
  - "development"
  - "security"
  - "soc"
  - "software"
---

The last two months have seen the release of three new U.S. Government documents related to software security:

- The National Cybersecurity Strategy released in early March covers the landscape of cybersecurity concerns and introduces the concept of shifting the liability for insecure software products and services from consumers to suppliers.
- In mid-April, The Cybersecurity and Infrastructure Security Agency (CISA) of the Department of Homeland Security, in collaboration with the National Security Agency, the Federal Bureau of Investigation, and cybersecurity agencies of several other countries, released a paper titled “Shifting the Balance of Cybersecurity Risk: Principles and Approaches for Security-by-Design and -Default.”, which reinforced some aspects of the National Cybersecurity Strategy but also added additional requirements and expectations.
- And in late April, the Office of Management and Budget (OMB) and CISA released a “Secure Software Development Attestation Form” that organizations that supply software to the government will be required to complete.

These documents could have a significant impact on the operations of software developers that sell to the government, both in the near term and for the foreseeable future. In this blog, we’ll review some key points of each and discuss their relationship to each other, to the US Presidential Executive Order 14028 released in May 2021 on improving the nation’s cybersecurity, and to the NIST Secure Software Development Framework (SSDF). Then we’ll provide some SAFECode comments on the relationship among the documents, and recommendations on the way that the government should use them.

### **The National Strategy**

The National Cybersecurity Strategy is, as its name suggests, a very broad document. Its scope ranges from protecting critical infrastructure to disrupting threat actors to international partnerships. Section 3 of the strategy focuses on shaping market forces to drive security and resilience, and Section 3.3 proposes to shift liability for insecure products and services to development organizations. Consistent with this objective, the strategy commits the Administration to work with Congress on legislation establishing liability for security vulnerabilities in software products and services.

The strategy does propose that development organizations be given a “safe harbor” from liability if they adopt secure development practices, and it acknowledges that even the most advanced software security programs cannot prevent all vulnerabilities. The strategy makes specific references to the Executive Order 14028, “secure-by-design” principles, pre-release security testing, and the NIST SSDF as approaches and guidance that would presumably be reflected in granting a safe harbor. Section 3.5 specifically mentions making secure development practices a requirement for government procurements of software – as  directed in the Executive Order and detailed in the attestation requirements.

On the whole, the strategy appears to seek a reasonable path for improving developers’ practices without going overboard and mandating a level of perfection that is unachievable. The author of this blog has made very skeptical comments on liability in Viewpoint articles in the Communications of the ACM in 2015 and 2023. A liability scheme that included a safe harbor for secure development practices might be a reasonable compromise and avoid some of the shortcomings mentioned in those articles, but designing and legislating such a scheme will be extraordinarily challenging.

The strategy refers to the responsibilities of the U.S. Government, of critical infrastructure owners and operators, and of product suppliers in several places. However, it falls short of making a clear statement that, while some software developers should do more, cybersecurity is and will remain a shared responsibility for organizations. While consumers and individual end users may expect that the systems they use will be secure by default and updated automatically, enterprises have and will retain a responsibility to manage their systems securely and to update or upgrade them as threats and security defenses evolve.

Similarly, although the strategy refers to the need to strengthen the nation’s cybersecurity workforce, it misses the opportunity to advocate for the integration of basic competence in building secure products and services in the nation’s software and hardware engineering curricula – as advocated recently by the director of CISA.

It will take a significant amount of time for the Congress and the White House to come to agreement on the liability regime envisioned by the national strategy. Meanwhile, the government should focus on insisting that software developers implement the practices and tasks in the SSDF and attest that they’ve done so.

### **Secure by Design**

The Secure by Design document has the laudable goal of encouraging software development organizations to treat security as a fundamental consideration as they design and develop products and services. The document represents a consensus of three U.S. Government agencies as well as agencies of six other national governments.

The diversity of contributors is evident: the document refers to a wide range of techniques and technologies but is very unfocused. Although a number of the techniques cited are drawn from the NIST SSDF, the document fails to distinguish between these techniques and approaches such as the adoption of memory-safe languages and capability-based hardware that, while promising, are probably years from generally supplanting existing programming languages and hardware architectures. The document includes a useful list of measures for making software secure by default.

Given its focus on “secure by design”, we were surprised that the document omits any mention of the practices and tasks in the SSDF that focus on secure design (PW.1). Those parts of the SSDF are based on widely accepted industry practices, actionable for development organizations, and supported by a large number of valuable references.

Overall, the Secure by Design document is valuable as a concept paper, but it should be understood as a supplement to the SSDF and not as a substitute.

### **Attestation Requirements**

SAFECode recently released a blog that discusses the effectiveness of vendor attestation as a mechanism for gaining confidence that a software development organization has in fact implemented a secure development process. So we were pleased to see that OMB and CISA have followed through on adopting vendor attestation as the government’s way of confirming that vendors have met the requirements of the Executive Order and the SSDF.

On reviewing the draft attestation requirements, we found that they largely align well with Section 4(c) of the Executive Order. We did note that while the Executive Order and SSDF refer to specific activities for protecting software development environments as examples (the Executive Order uses the words “such as”), the attestation form refers to those activities as mandatory (“at a minimum”). The change is likely to have a negative impact on developers’ ability to apply a risk-based approach in selecting security measures. We will be pointing out this change and its implications in our formal comments to OMB and CISA.

The draft attestation collects all aspects of secure software development – a large number of SSDF tasks – into a single long list that is aligned with the Executive Order requirements that developers assure the security of the code that they create. While this approach is consistent with the structure of the Executive Order, the attestation requirements omit some important SSDF practices in particular those that focus on secure design, root cause analysis of reported software vulnerabilities, and continuous improvement of the development process. The omitted practices are fundamental to the effectiveness of a secure software development process and their omission is a major oversight as well as being inconsistent with the objective of building software that is “secure by design”. Again, we will be pointing out these omissions in our formal comments.

The attestation requirements leave the door open for individual agencies to require vendors to supplement their attestation with additional artifacts such as Software Bills of Materials (SBOMs). This flexibility opens the possibility that different agencies will impose different requirements and force software developers to spend their time creating supplemental artifacts rather than making their software more secure.  The Executive Branch should strongly encourage agencies to ask for common attestation artifacts in response to the Executive Order and the SSDF. The National Cybersecurity Center of Excellence project on Software Supply Chain and DevOps Security Practices is likely to create a valuable “worked example” that both agencies and developers can rely on.

### **Seeking Consistency**

The discussion above makes it clear that there are some significant inconsistencies among the national strategy, the Secure by Design document, the draft attestation requirements, and the SSDF. SAFECode has been a leader in creating and advocating effective secure software development practices. We have been a strong supporter of the Executive Order and the SSDF since they were released and support the government’s efforts to drive industry to implement better secure development practices. We believe it’s important that the government make every effort to provide software developers with a consistent message about the practices that they should adopt and the ways that they should provide assurance to government customers.

We’re interested in your feedback on the views we’ve presented above, so please email info@safecode.org.

The post Secure by Design? The U.S. Government and Requirements for Secure Development appeared first on SAFECode.

Go to Source
