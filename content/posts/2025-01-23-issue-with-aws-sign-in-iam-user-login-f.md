---
title: "Issue with AWS Sign-in IAM User Login Flow Possible Username Enumeration CVE-2025-0693"
date: Thu, 23 Jan 2025 21:20:02 +0000
draft: false
type: posts
categories: 
- 
---
# Issue with AWS Sign-in IAM User Login Flow Possible Username Enumeration CVE-2025-0693

<br/>

<br/>
**Publication Date: 2025/01/23 1:30 PM PDT  
**

We have identified CVE-2025-0693 in the AWS Identity and Access Management (AWS IAM) Sign-in login flow. This issue could allow an actor to enumerate AWS IAM usernames by measuring server response times during login attempts. Variations in those response times could allow an actor to discern whether a submitted AWS IAM username existed in the account.

Please note that username information alone is insufficient to authenticate or access any AWS resources. Full authentication, including account identifier, username, password, and multi-factor authentication (if enabled), is required to access an account. Additionally, AWS leverages multiple layers of protection to monitor and respond to potential misuse of our sign-in endpoints.

**Affected versions:** AWS Sign-in IAM User login flow prior to January 16, 2025.

**Resolution:**

AWS introduced a delay in response times across all authentication failure scenarios. This enhancement protects against valid username enumeration by removing any time variation between valid and invalid usernames in failure responses.

No customer action is required. Customers can monitor sign-in activity, including failed and successful sign-in events, using AWS CloudTrail. For more information, refer to the [CloudTrail Event Reference](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-aws-console-sign-in-events.html#cloudtrail-event-reference-aws-console-sign-in-events-iam-user) documentation.

We would like to thank Rhino Security Labs for collaborating on this issue through the coordinated vulnerability disclosure process.

**References:**

-   [CVE-2025-0693](https://www.cve.org/CVERecord?id=CVE-2025-0693)

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2025-002/)

<br/>
---
