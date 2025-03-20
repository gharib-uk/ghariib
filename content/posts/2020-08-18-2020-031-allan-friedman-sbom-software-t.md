---
title: "2020-031-Allan Friedman SBOM software transparency and knowing how the sausage is made"
date: Tue, 18 Aug 2020 22:54:51 +0000
draft: false
type: posts
---
# 2020-031-Allan Friedman SBOM software transparency and knowing how the sausage is made

<br/>

<br/>
Ms. Berlin: Tabletop D&D exercise

Blumira is hiring [https://www.blumira.com/career/lead-backend-engineer/](https://www.blumira.com/career/lead-backend-engineer/) 

Allan Friedman - Director of Cybersecurity Initiatives, NTIA, US Department of Commerce

NTIA.gov - National Telecommunications and Information Administration

[https://www.ntia.gov/sbom](https://www.ntia.gov/sbom)  SBOM guidance

Healthcare SBOM PoC - [https://www.ntia.gov/files/ntia/publications/ntia\_sbom\_healthcare\_poc\_report\_2019\_1001.pdf](https://www.ntia.gov/files/ntia/publications/ntia_sbom_healthcare_poc_report_2019_1001.pdf)

Allan’s talk at Bsides San Francisco: [https://www.youtube.com/watch?v=9j1KYLfklMQ](https://www.youtube.com/watch?v=9j1KYLfklMQ)

Questions (more may be added during the show, depending on answers given)  
  

**What is NTIA?**

**What is SBOM?**

**Why do we need one? Is it poor communications between vendors?** 

**Is there any difference between “Software transparency” and “Software bill of materials”?**

**How do you make an SBOM? What data formats make sharing easier? What does a company do with an SBOM?**

**Where in the development (hardware or software) would you be creating an SBOM?**

**You mention in your BSSF talk about ‘how detailed it should be’. Can you give us an example of a high level SBOM, versus a more detailed one? Does it become a risk/reward effort concerning detail?**

**IoT device creators are working with their 3rd parties, who are working with their 3rd parties. Someone at home with a webcam cannot easily ask for an SBOM, so how do we convince device makers to want to ask for them?**

**How do you get your 3rd party that is a multi-national corporation to supply you with the information you need to ?**

**As we saw with RIPPLE20, many companies don’t know what they have. How would SBOM help keep another RIPPLE20 from happening?**

**Rob Graham’s blog post highlighted that vulns like HeartBleed would not have been stopped.** 

**How does this help us track potential vulns?** 

**Sharing information**

**Best way to share information about IoT components?** 

**Could an information sharing org (ISAC) track these more readily?**

**vendor assessments:**

**Vendor does not have an SBOM,** **any specific questions we might ask that will allow an org to get more resolution into a potential vendor?**

**Interesting feedback from NTIA’s RFC**

**Other SBOM types (clonedx, openbom, FDA’s CBOM)**

**Companies are out there creating SBOM, other government agencies have SBOM implementations. How do we keep this from being the XKCD “927” issue?** [**https://xkcd.com/927/**](https://xkcd.com/927/)

**non-US implementations of SBOM?**

**How do we get our companies to implement these?** 

**SBOM could easily be something that could give hackers a lot of information about your org and depending on the information contained, I could see why you might not want to get super specific… Thoughts?**

  
  
  
  
  

What is a ‘Bill of Materials’?  
“A **bill of materials** (BOM) is a comprehensive inventory of the raw **materials**, assemblies, subassemblies, parts and components, as well as the quantities of each, needed to manufacture a product. In a nutshell, it is the complete list of all the items that are required to build a product.”

SBOM - Definition

As of November 2019, an estimated 126 different platforms - [https://www.postscapes.com/internet-of-things-platforms/](https://www.postscapes.com/internet-of-things-platforms/)

NTIA did an RFC on “promoting the sharing of Supply Chain Security Risk Information”

[https://www.ntia.gov/federal-register-notice/2020/request-comments-promoting-sharing-supply-chain-security-risk](https://www.ntia.gov/federal-register-notice/2020/request-comments-promoting-sharing-supply-chain-security-risk)

[https://www.ntia.gov/federal-register-notice/2020/comments-promoting-sharing-supply-chain-security-risk-information-0](https://www.ntia.gov/federal-register-notice/2020/comments-promoting-sharing-supply-chain-security-risk-information-0)

Secure and Trusted Communications Network Act of 2019 (Act) - Calling it “CBOM”

Other groups working on similar: FDA [https://www.fda.gov/media/119933/download](https://www.fda.gov/media/119933/download)

SPDX: LInux Foundation:[https://spdx.org/licenses/](https://spdx.org/licenses/) 

OpenBOM [https://medium.com/@openbom/friday-discussion-why-boms-are-essential-for-the-iot-platform-e2c4bd397afd](https://medium.com/@openbom/friday-discussion-why-boms-are-essential-for-the-iot-platform-e2c4bd397afd)

[https://github.com/CycloneDX/specification](https://github.com/CycloneDX/specification)

[https://www.fda.gov/medical-devices/digital-health/cybersecurity](https://www.fda.gov/medical-devices/digital-health/cybersecurity)

[https://www.fda.gov/regulatory-information/search-fda-guidance-documents/guidance-content-premarket-submissions-software-contained-medical-devices](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/guidance-content-premarket-submissions-software-contained-medical-devices)

Medical device IR Playbook: [https://www.mitre.org/sites/default/files/publications/pr-18-1550-Medical-Device-Cybersecurity-Playbook.pdf](https://www.mitre.org/sites/default/files/publications/pr-18-1550-Medical-Device-Cybersecurity-Playbook.pdf)

Companies are helping to get “CBOM” for devices:

““It can take anywhere from six months to a year for a new medical device to get its 510(k) clearance from the FDA,” [said](https://www.prnewswire.com/news-releases/medcrypt-acquires-medisao-becoming-the-only-healthcare-cybersecurity-company-proactively-addressing-fda-premarket-and-postmarket-guidances-301111458.html) MedCrypt CEO Mike Kijewski in a news release” 

[https://www.medicaldesignandoutsourcing.com/medcrypt-acquires-medisao-in-medtech-cybersecurity-deal/](https://www.medicaldesignandoutsourcing.com/medcrypt-acquires-medisao-in-medtech-cybersecurity-deal/)

SBOM doesn’t work in DevOps: [https://cybersecurity.att.com/blogs/security-essentials/software-bill-of-materials-sbom-does-it-work-for-devsecops](https://cybersecurity.att.com/blogs/security-essentials/software-bill-of-materials-sbom-does-it-work-for-devsecops)

Intoto software development: [https://www.intotosystems.com/](https://www.intotosystems.com/)

510k process: [https://www.drugwatch.com/fda/510k-clearance/](https://www.drugwatch.com/fda/510k-clearance/)

#### [Source](http://brakeingsecurity.com/2020-031-allan-friedman-sbom-software-transparency-and-knowing-how-the-sausage-is-made)

<br/>
---
