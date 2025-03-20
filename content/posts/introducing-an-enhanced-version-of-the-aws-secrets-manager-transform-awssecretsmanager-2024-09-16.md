---
title: "Introducing an enhanced version of the AWS Secrets Manager transform: AWS::SecretsManager-2024-09-16"
date: 2025-01-02
categories: 
  - "security"
---

We’re pleased to announce an enhanced version of the AWS Secrets Manager transform: `AWS::SecretsManager-2024-09-16`. This update is designed to simplify infrastructure management by reducing the need for manual security updates, bug fixes, and runtime upgrades.

AWS Secrets Manager helps you manage, retrieve, and rotate database credentials, API keys, and other secrets throughout their lifecycles. Some AWS services offer managed rotation of secrets, but for other secrets, you perform rotation by using an AWS Lambda function that updates your secret and the database or service.

Transforms are macros hosted by AWS CloudFormation that enable you to create or manage complex infrastructure setups. For general information on transforms, see the AWS CloudFormation documentation.

The `AWS::SecretsManager` transforms are used in conjunction with the `AWS::SecretsManager::RotationSchedule` resource type and `HostedRotationLambda` property to automatically extend your CloudFormation template to include a nested stack that creates the appropriate rotation Lambda function for your database or service. The transforms provide a convenient way to deploy an AWS vended rotation Lambda function into your own account as part of your CloudFormation templates, without having to rely on creating rotation Lambda functions through the AWS Serverless Application Repository or the AWS Management Console.

In this post, we’ll explore the new features of the transform, compare them to the previous version, and guide you through updating an existing Lambda function that was created using the old transform version to use the new transform version.

## New features in `AWS::SecretsManager-2024-09-16`

The new transform version introduces several enhancements over the previous version (`AWS::SecretsManager-2020-07-23`):

- **Automatic Lambda upgrades**: Your rotation Lambda functions’ runtime configuration and internal dependencies now update automatically when you update your CloudFormation stacks. This helps you verify that you’re using the most secure and stable versions of Secrets Manager vended rotation Lambda function code and runtimes. Currently, AWS Lambda supports Python runtimes 3.9 and above. With Python 3.8 being deprecated, this feature allows for a seamless transition to newer supported runtimes. For more information on runtime deprecations, see the AWS Lambda runtimes documentation and the Python version guide.
- **Additional resource attributes**: The new transform now supports additional resource attributes for the `AWS::SecretsManager::RotationSchedule` resource type when used with the `HostedRotationLambda` property. The following attributes are applied to the nested stack (of type `AWS::CloudFormation::Stack`) that creates the rotation Lambda function:
    - `CreationPolicy`
    - `DependsOn`
    - `Metadata`
    - `UpdatePolicy`
    - `Condition`

For more information on these resource attributes, see the AWS CloudFormation resource attribute reference.

### Resource attributes comparison

The following table shows which resource attributes are supported by the two versions of the Secrets Manager transform.

| **Attribute** | **AWS::SecretsManager-2020-07-23** | **AWS::SecretsManager-2024-09-16** |
| --- | --- | --- |
| DeletionPolicy | Supported | Supported |
| UpdateReplacePolicy | Supported | Supported |
| CreationPolicy | Not Supported | Supported |
| DependsOn | Not Supported | Supported |
| Metadata | Not Supported | Supported |
| UpdatePolicy | Not Supported | Supported |
| Condition | Not Supported | Supported |

## Important considerations

Before you use the `AWS::SecretsManager-2024-09-16` transform, it’s essential to be aware of the following considerations so that you can make sure your CloudFormation stacks are properly created or updated:

- **Non-backward compatibility**: The new transform version isn’t backward compatible with the previous version. If you downgrade from `AWS::SecretsManager-2024-09-16` to `AWS::SecretsManager-2020-07-23`, the additional resource attributes won’t be supported, which might change the behavior of existing stacks.
- **Rollback behavior during upgrade**: When you upgrade to the `AWS::SecretsManager-2024-09-16` transform from the previous version and a stack rollback occurs for any reason, the rotation Lambda function might not revert to its previous state. This is because the older transform’s nested stack might not use the same Lambda deployment package that was used before the upgrade.
- **Direct Lambda changes**: If you make direct changes to the Lambda function created by the new transform outside of a CloudFormation stack update, those modifications might be overwritten during subsequent CloudFormation stack updates or rollbacks.
- **Lambda runtime management**: When you use the new transform version, the rotation Lambda function’s runtime aligns with the compiled binaries that are vended in Secrets Manager rotation Lambda templates, without you needing to specify a `Runtime` value in the `HostedRotationLambda` property. If you specify a `Runtime` value, make sure it’s the same version that is supported by Secrets Manager vended rotation Lambda templates. Otherwise, the Lambda runtime will be incompatible with the binaries that are published in the rotation Lambda function. For more information on the supported runtime, see the rotation function templates documentation.
- **Future support plans**: While this is not an end-of-support announcement, upgrading the transform version as soon as convenient for you will reduce the impact on your CloudFormation stacks in the future when AWS Secrets Manager ends support for the previous transform version `AWS::SecretsManager-2020-07-23`).

## How to upgrade

To upgrade to the new transform version, follow these steps:

1. Review your existing CloudFormation stacks that use the `AWS::SecretsManager-2020-07-23` transform.
2. Update your CloudFormation stack templates to use `AWS::SecretsManager-2024-09-16` in the `Transform` key at the top of your template: `Transform: AWS::SecretsManager-2024-09-16`
3. If you have previously defined a `Runtime` value in the `HostedRotationLambda` property, remove it from your template so that your rotation Lambda function’s runtime is updated properly in future stack updates.
4. Incorporate the new resource attributes as needed. We recommend that you minimize all other template changes while upgrading to reduce the likelihood of rollbacks.
5. Deploy the changes by updating your CloudFormation stack with the revised template.

By following these steps, your Secrets Manager vended rotation Lambda functions will benefit from the latest improvements and security enhancements. Remember to test the changes in a non-production environment before you apply them to your production stacks. If you encounter any issues during the upgrade process, refer to our documentation or contact AWS Support for assistance.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Sanjay Varma Datla](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/04/sanjayvd.jpg) Sanjay Varma Datla  
Sanjay is a Software Development Engineer at AWS Secrets Manager. With a strong background in software engineering and a passion for secure application design, he focuses on empowering developers to manage sensitive information safely. Outside of work, Sanjay enjoys hiking and exploring new cuisines.

![Rob Stevens](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/04/robcop.jpg) Rob Stevens  
Rob is a System Development Engineer for AWS Secrets Manager and is committed to building secure and scalable distributed systems for AWS and its customers. Outside of work, Rob is a fitness enthusiast and also enjoys traveling.

Go to Source
