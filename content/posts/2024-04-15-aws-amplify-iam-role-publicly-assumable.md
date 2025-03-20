---
title: "AWS Amplify IAM role publicly assumable exposure"
date: Mon, 15 Apr 2024 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# AWS Amplify IAM role publicly assumable exposure

<br/>

<br/>
The AWS Amplify service was found to be misconfiguring IAM roles associated with Amplify projects. This misconfiguration caused these roles to be assumable by any other AWS account. Both the Amplify Studio and the Amplify CLI exhibited this behavior. Any Amplify project created using the Amplify CLI built between July 3, 2018 and August 8, 2019 had IAM roles that were assumable by anyone in the world. The same was true if the authentication component was removed from an Amplify project using the Amplify CLI or Amplify Studio built between August 2019 and January 2024. AWS mitigated this vulnerability through backend changes to STS and IAM, and also released a patch for the Amplify CLI to ensure that newly created roles are properly configured in accordance with these changes.

#### [Source](https://www.cloudvulndb.org/aws-amplify-iam-role-publicly-assumable-exposure)

<br/>
---
