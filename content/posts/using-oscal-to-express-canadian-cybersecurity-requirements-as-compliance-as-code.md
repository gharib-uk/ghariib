---
title: "Using OSCAL to express Canadian cybersecurity requirements as compliance-as-code"
date: 2025-01-22
---

The Open Security Controls Assessment Language (OSCAL) is a project led by the National Institute of Standards and Technology (NIST) that allows security professionals to express control-related information in machine-readable formats. Expressing compliance information in this way allows security practitioners to use automated tools to support data analysis, while making it easier to address downstream requirements such as translation and accessibility. In the United States, Amazon Web Services (AWS) has collaborated closely with NIST and the FedRAMP program to advance the adoption of OSCAL, and was the first cloud service provider to submit a FedRAMP system security plan (SSP) in OSCAL format in 2022.

In Canada, the Canadian Centre for Cyber Security (CCCS) is the national technical authority for cybersecurity. CCCS publishes cybersecurity advice and guidance, including ITSG-33 Annex 3A, a catalog of security controls based on NIST Special Publication 800-53. When CCCS recently published new cloud security profiles based on NIST 800-53 Revision 5, we undertook a project to encode the relevant information in OSCAL. Expressing CCCS’s catalog and profile information in OSCAL facilitates automated analysis, including comparisons with OSCAL catalogs and profiles published by NIST and FedRAMP. This post explores the approach we took to express CCCS’s profiles in OSCAL, in addition to opportunities for future work.

## OSCAL fundamentals

For the purposes of this discussion, there are two important OSCAL concepts to understand: catalogs and profiles. A catalog is a collection of security controls, such as NIST 800-53 or ITSG-33. An OSCAL catalog expresses control-specific information, including statements, parameters, and implementation guidance, in a structured and machine-readable format using either JSON, XML, or YAML.

OSCAL profiles import controls from catalogs (and other profiles) and express more specific implementation guidance. For example, the FedRAMP Moderate profile selects a subset of controls from NIST 800-53, specifies constraints for certain parameters, and provides assessment guidance. Profiles can also modify controls as they’re imported, which proved very useful for our purposes.

## Expressing CCCS controls in OSCAL

Because CCCS’s ITSG-33 is based on NIST 800-53, most NIST controls can be used in CCCS profiles without modification. However, in some cases CCCS has modified the language of NIST 800-53 controls; for example, to replace mentions of a US agency or standard with a Canadian equivalent, or to add additional content specific to CCCS. Therefore, the first step in expressing CCCS requirements in OSCAL was to create a profile that makes the necessary control-level modifications. In some cases, CCCS has also created controls that are not part of NIST 800-53; these are specified in a separate catalog.

When an OSCAL profile is resolved, the information from the upstream catalogs and profiles that it’s importing controls from is assembled—along with modifications—and expressed as a catalog. By resolving the ITSG-33 modifications profile, we can programmatically generate the complete ITSG-33 catalog, incorporating NIST 800-53 controls, CCCS controls, and required modifications.

## CCCS cloud security profiles

CCCS has created two profiles that are used to assess the security of cloud services: CCCS Medium and Protected B High Value Assets (PBHVA). Each of these profiles specifies a selection of controls from ITSG-33, in addition to the values for a number of parameters. Working backwards from the profiles published by CCCS as spreadsheets, we extracted the control and parameter information from each profile and expressed them in OSCAL. This exercise also informed the creation of the ITSG-33 modifications profile discussed previously, which captured control-level changes made by CCCS to NIST 800-53 controls, as well as the separate catalog of CCCS-specific controls.

## Resources

In support of furthering this work within the Canadian security community, we’ve published the OSCAL files that we created as part of this project on GitHub, including:

- CCCS-specific control catalog
- ITSG-33 modifications profile and resolved catalog
- CCCS Medium profile, resolved catalog, and CSV
- PBVHA profile, resolved catalog, and CSV

We used an open-source tool, oscal-cli, to validate the structure of the OSCAL files that we created and to resolve the profiles into catalogs.

## Future work

AWS is interested in further exploring the use of OSCAL to help us and our customers adhere to CCCS requirements as efficiently as possible. In the future, we want to explore how OSCAL data and tools can be used to support the efficient translation of the ITSG-33 catalog and CCCS profiles into French and the presentation of compliance information in accessible formats.

If you have feedback about this post, submit comments in the **Comments** section below.

![Michael Davie](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/10/12/mldavie.jpeg)

### Michael Davie

Michael is the Canada lead for Amazon Web Services (AWS) Security Assurance. He works with customers, regulators, and AWS teams to help raise the bar on secure cloud adoption and usage. Michael has more than 20 years of experience working in the defence, intelligence, and technology sectors in Canada, and is a licensed professional engineer.

Go to Source
