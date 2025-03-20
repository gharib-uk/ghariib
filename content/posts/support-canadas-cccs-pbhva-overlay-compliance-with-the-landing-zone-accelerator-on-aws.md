---
title: "Support Canada’s CCCS PBHVA overlay compliance with the Landing Zone Accelerator on AWS"
date: 2025-03-19
---

Organizations seeking to adhere to the Canadian Centre for Cyber Security (CCCS) Protected B High Value Assets (PBHVA) overlay requirements can use the Landing Zone Accelerator (LZA) on AWS solution with the CCCS Medium configuration to accelerate their compliance journey. To further support customers, AWS recently collaborated with Coalfire to assess and verify the LZA solution’s ability to support CCCS PBHVA overlay controls.

By implementing the PBHVA control overlay over a CCCS Medium baseline, you can better protect your organization’s most critical assets from potential threats and vulnerabilities, providing continuity of essential government operations and safeguarding sensitive information.

## Understanding CCCS PBHVA overlay requirements

The CCCS PBHVA overlay consists of 137 controls designed to protect high-value assets, including 69 new controls and 68 controls from CCCS Medium. These controls provide enhanced data protection, particularly for integrity and availability, and are based on NIST SP 800-53 Revision 5.

## Key findings from the Coalfire assessment

Coalfire’s assessment found that the LZA on AWS solution significantly supports CCCS PBHVA overlay compliance requirements:

- 71 percent of in-scope controls (97 of 137) are supported by the AWS contribution to compliance in the shared responsibility model
- The solution uses over 35 AWS services to provide comprehensive security capabilities
- Strong network segmentation is achieved through network account and network-boundary VPC design
- Infrastructure-as-code (IaC) enables reliable build and deployment results

The 29 percent of controls not addressed by the LZA are on the customer side of the shared responsibility model. They are addressed in the customer’s application stack or as non-technical controls such as policies and procedures.

## Key security capabilities

The LZA solution implements several critical security features:

- Identity and access management
    - Comprehensive account management through AWS Identity and Access Management (IAM) and AWS IAM Identity Center
    - Multi-factor authentication and single sign-on (SSO) federation options
    - Role-based access control (RBAC)
- Security monitoring and protection
    - Integrated threat detection using Amazon GuardDuty
    - Centralized security monitoring through AWS Security Hub
    - Automated compliance checking with AWS Config
- Network security
    - Advanced network segmentation using AWS Transit Gateway
    - Boundary protection with AWS Network Firewall
    - Secure Amazon Virtual Private Cloud (Amazon VPC) design patterns
- Logging and audit
    - Centralized logging across accounts
    - Automated audit trail creation
    - Comprehensive event monitoring

## Implementation considerations

While the LZA solution provides significant compliance support, organizations should note:

- The solution alone does not guarantee compliance
- Organizations must implement their own policies, standards, and procedures
- A thorough understanding of the shared responsibility model is essential

The AWS Landing Zone Accelerator Verified Reference Architecture documentation is available for customer download in AWS Artifact. This resource can help organizations reduce the time and effort required to deploy an environment that aligns with CCCS PBHVA overlay requirements.

## Conclusion

The Coalfire assessment confirms that the LZA on AWS solution provides effective support for CCCS PBHVA overlay compliance objectives. However, organizations should remember that compliance is an ongoing process that requires active management and cannot be achieved through technology alone.

For more information about implementing the Landing Zone Accelerator for CCCS PBHVA overlay requirements, contact your AWS account team or the AWS Public Sector team directly.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Naranjan Goklani](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/11/16/narankg.png) Naranjan Goklani  
Naranjan is an Audit Lead for Canada based in Toronto. He has experience leading audits, attestations, certifications, and assessments across North America and Europe. Naranjan has more than 15 years of experience in risk management, security assurance, and performing technology audits. Naranjan previously worked in one of the Big 4 accounting firms and supported clients from the financial services, technology, retail, e-commerce, and utilities industries as part of the first and third line of defense.

![Michael Davie](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/10/12/mldavie.jpeg) Michael Davie  
Michael is the Canada lead for Amazon Web Services (AWS) Compliance and Security Assurance. He works with customers, regulators, and AWS teams to help raise the bar on secure cloud adoption and usage. Michael has more than 20 years of experience working in the defence, intelligence, and technology sectors in Canada, and is a licensed professional engineer.

![James Kierstead](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/24/jkierst.jpg) James Kierstead  
James is a senior solutions architect at Amazon Web Services (AWS) based in Ottawa, Canada. He is passionate about helping Canada’s federal government use AWS to deliver services to Canadians.

Go to Source
