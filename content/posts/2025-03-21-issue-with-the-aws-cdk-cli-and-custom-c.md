---
title: "Issue with the AWS CDK CLI and custom credential plugins CVE-2025-2598"
date: Fri, 21 Mar 2025 14:04:13 +0000
draft: false
type: posts
---
# Issue with the AWS CDK CLI and custom credential plugins CVE-2025-2598





Publication Date:&nbsp;2025/03/21 07:00 AM PDT Description AWS&nbsp;identified CVE-2025-2598, an issue in the AWS Cloud Development Kit (AWS CDK) Command Line Interface (AWS CDK CLI), versions 2.172.0 through 2.178.1. The AWS CDK CLI is a command line tool that deploys AWS CDK applications onto AWS accounts. When&nbsp;customers run AWS CDK

**Publication Date: 2025/03/21 07:00 AM PDT  
**

**Description**

AWS identified [CVE-2025-2598](https://www.cve.org/CVERecord?id=CVE-2025-2598), an issue in the [AWS Cloud Development Kit (AWS CDK) Command Line Interface (AWS CDK CLI)](https://docs.aws.amazon.com/cdk/v2/guide/cli.html), versions 2.172.0 through 2.178.1. The [AWS CDK CLI](https://docs.aws.amazon.com/cdk/v2/guide/cli.html) is a command line tool that deploys AWS CDK applications onto AWS accounts.

When customers run AWS CDK CLI commands with credential plugins and configure those plugins to return temporary credentials by including an expiration property, this issue can potentially result in the AWS credentials retrieved by the plugin to be printed to the console output. Any user with access to where the CDK CLI was ran would have access to this output. We have released a fix for this issue and recommend customers upgrade to [version 2.178.2](https://github.com/aws/aws-cdk/releases/tag/v2.178.2) or later to address this issue. Plugins that omit the expiration property are not affected.

To validate if credentials have been printed to the console output, customers can take the following actions:

1.  Identify executions running CDK CLI that have started after December 6, 2024.
2.  Scan any logs of those executions to locate statements similar to the following:  
    {  
    accessKeyId: '<secret>',  
    secretAccessKey: '<secret>',  
    sessionToken: '<secret>',  
    expiration: <date>,  
    '$source': <object>  
    }
3.  If you identify credentials, these can be viewed by users who have access to the console where the CDK CLI was ran. As such, we recommend you take appropriate action, which can include (but is not limited to):
    -   Revoke all temporary credentials obtained from the AWS IAM role used by the plugin.
    -   Limit the users who have access to the console output.
    -   Rotate long lived credentials of the AWS IAM user used by the plugin (if any).

Please refer to our "[AWS CDK CLI Library](https://www.npmjs.com/package/@aws-cdk/cli-plugin-contract)" for more information about custom credential plugins.

**Affected versions:** 2.172.0 through 2.178.1  

**Resolution:**

The issue has been addressed in version 2.178.2. We recommend upgrading to the latest version and ensuring any forked or derivative code is patched to incorporate the new fixes.

**References:**

-   [GHSA-v63m-x9r9-8gqp](https://github.com/aws/aws-cdk/security/advisories/GHSA-v63m-x9r9-8gqp)
-   [CVE-2025-2598](https://www.cve.org/CVERecord?id=CVE-2025-2598)

Please email [aws-security@amazon.com](mailto:aws-security@amazon.com) with any security questions or concerns.

#### [Source](https://aws.amazon.com/security/security-bulletins/AWS-2025-005/)

