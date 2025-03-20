---
title: "Enforce resource configuration to control access to new features with AWS"
date: 2025-01-02
categories: 
  - "security"
---

Establishing and maintaining an effective security and governance posture has never been more important for enterprises. This post explains how you, as a security administrator, can use Amazon Web Services (AWS) to enforce resource configurations in a manner that is designed to be secure, scalable, and primarily focused on feature gating.

In this context, _feature gatin_g means that newly supported AWS features and configurations can’t be used unless you explicitly approve them. With feature gating, you maintain control over your AWS environment when new services and capabilities are introduced.

This blog post demonstrates a unique approach to giving users, such as DevOps teams, controlled flexibility within safe boundaries by allowing resource provisioning that uses only approved configurations. This approach also accommodates configurations that will be supported in future versions of the resource, keeping them restricted until explicitly approved, as shown in Figure 1.

![Figure 1: Restrict resource provisioning to approved configurations only](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img1-2-1024x376.png)

Figure 1: Restrict resource provisioning to approved configurations only

## Apply your resource configuration enforcement

As shown in Figure 2, our solution for resource configuration enforcement (RCFGE) uses AWS CloudFormation Hooks. By using Hooks, you can run custom logic during the provisioning of resources. These are proactive controls because you inspect and enforce resource configurations _before_ the resource is created, updated, or deleted.

Your Hook will only be effective if CloudFormation supports the AWS resources that you are using and if you implement a service control policy (SCP) that helps prevent users from provisioning resources outside of CloudFormation.

![Figure 2: How CloudFormation Hooks work](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img2-1-1024x522.png)

Figure 2: How CloudFormation Hooks work

The flow shown in Figure 2 consists of the following five steps:

1. DevSecOps registers and configures a CloudFormation Hook in the account.
2. DevOps specifies a CloudFormation template that defines the required resources and configurations.
3. CloudFormation creates a new stack resource, starting the provisioning process based on the template.
4. The Hook is triggered before provisioning for each resource that’s defined in the template, and runs custom validation logic.
5. If the validation checks pass, CloudFormation proceeds with provisioning; if not, the process is terminated.

## Make your solution scalable

To achieve scalable operations, you should implement a reusable and generic Hook that targets all supported CloudFormation resource types. This Hook enforces resource configuration by loading resource specification files from an external object storage, such as an Amazon Simple Storage Service (Amazon S3) bucket.

These specification files define validation rules in a declarative language. Using this approach, you can add and remove resource configuration validation rules by editing the declarative files. When you externalize custom logic as decoupled validation rules from the Hook, DevSecOps personnel can manage these rules at scale without affecting your infrastructure.

![Figure 3: Externalize custom logic as validation rule files in an S3 bucket](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img3-1024x522.png)

Figure 3: Externalize custom logic as validation rule files in an S3 bucket

Figure 3 shows how the solution has been revised to support this approach. Steps 1–3 are the same as in the flow shown in Figure 2:

1. DevSecOps registers and configures a CloudFormation Hook in the account.
2. DevOps specifies a CloudFormation template that defines the required resources and configurations.
3. CloudFormation creates a new stack resource, starting the provisioning process based on the template.
4. The Hook is triggered before provisioning for each resource that’s defined in the template.
5. The Hook loads the relevant resource specification file from the S3 bucket and executes the validation rules against the current resource in the CloudFormation template.
6. If the validation checks pass, CloudFormation proceeds with provisioning; if not, the process is terminated.

You need to configure the Hook schema and the Hook configuration schema to evaluate the configurations of all supported resources across your AWS accounts before changes are provisioned. This setup should cover create, update, and delete operations so that the Hook can help prevent non-approved configurations across stacks.

By using AWS CloudFormation Guard, you can externalize validation rules from the Hook, as described in Extend your pre-commit hooks with AWS CloudFormation Guard. Guard is an open source, general purpose, policy-as-code (PaC) evaluation tool that validates CloudFormation templates against custom rules to help you stay aligned with your organizational policies. For example, the CT.S3.PR.1 rule specification demonstrates a Guard rule that requires an S3 bucket to have its settings configured to block public access. These validation rules apply to currently supported AWS resource configurations and features, but they don’t restrict potential future properties.

## Boost your solution with feature gating

Your risk model might lead you to look for mechanisms that further restrict the AWS resource configurations that you allow in your environments. As you will see, the proposed solution restricts authorized workforce users so that they can use new configurations only if you enable them. The proposed approach uses feature gating because it continues to enforce your configurations even when AWS adds new options for your resources.

Guard aims to validate required constraints; but to meet the feature gating objective, you should implement validation rules that check whether resource configurations fulfill structural constraints described by the restricted version of CloudFormation resource schemas. These schemas help you confine the possible resource configurations that can be provisioned in your environment no matter what new configurations AWS introduces.

![Figure 4: Enforce resource configuration with restricted resource schema templates](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img4-1024x560.png)

Figure 4: Enforce resource configuration with restricted resource schema templates

Figure 4 shows an updated version of the same flow where validation rules are implemented by using restricted resource schema templates, which are stored in an S3 bucket. These templates are based on the original CloudFormation resource schemas, representing a snapshot of these schemas at a specific point in time. Steps 1–4 are the same as in the flow shown in Figure 3:

1. DevSecOps registers and configures a CloudFormation Hook in the account.
2. DevOps specifies a CloudFormation template that defines the required resources and configurations.
3. CloudFormation creates a new stack resource, starting the provisioning process based on the template.
4. The Hook is triggered before provisioning for each resource that’s defined in the template.
5. The Hook loads the relevant restricted resource schema template file from the S3 bucket and uses it to execute schema validation against the current resource in the CloudFormation template.
6. If the validation checks pass, CloudFormation proceeds with provisioning; if not, the process is terminated.

A restricted resource schema template is a subset of its corresponding original CloudFormation resource schema. It includes additional constraints that limit certain properties to specific values and patterns or exclude certain properties entirely. Furthermore, these templates contain placeholders that you fill in with runtime values, such as the account ID, which your Hook provides as part of the Hook context.

![Figure 5: Resource configuration enforcement (RCFGE) CloudFormation Hook flow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img5-1024x518.png)

Figure 5: Resource configuration enforcement (RCFGE) CloudFormation Hook flow

As shown in Figure 5, the flow within the RCFGE CloudFormation Hook involves the following steps:

1. The CloudFormation Hook is invoked with the Hook context and the resource’s configuration JSON object.
2. The Hook loads the restricted resource schema template from the S3 bucket and substitutes placeholders with the Hook context runtime values, producing a valid JSON schema.
3. The Hook validates the stack’s resource configuration JSON object against the schema. If it returns `OperationStatus.SUCCESS`, then CloudFormation proceeds with the provisioning process. If it returns `OperationStatus.FAILED`, then CloudFormation terminates the provisioning process.

If a restricted resource schema template for a CloudFormation resource type isn’t found in the S3 bucket, the schema validation step fails by default.

### Sample excerpt of a restricted schema template for an S3 bucket resource

The following is an excerpt from a restricted schema template for an S3 bucket. At runtime, your Hook processes this template, substituting the placeholders with relevant values from the Hook context. In this example, the Hook replaces the `<accountID>` placeholder in the topic’s `pattern` with the actual account ID. The resulting JSON schema disallows additional properties beyond those defined by the schema and restricts the Amazon Simple Notification Service (Amazon SNS) topics that can be used for event notifications.

> **Note**: In the code samples that follow, we’ve omitted some code for brevity—we’ve indicated these omissions with three periods: `...`

```
{
  "type": "object",
  "required": [],
  "additionalProperties": false,
  "properties": {
        ...
      "NotificationConfiguration": {
          "$ref": "#/definitions/NotificationConfiguration"
      },
        ...
  },
  "definitions": {
        ...
      "NotificationConfiguration": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            ...
              "TopicConfigurations": {
                  "type": "array",
                  "uniqueItems": true,
                  "items": {
                      "$ref": "#/definitions/TopicConfiguration"
                  }
              }
          }
      },
        ...
      "TopicConfiguration": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
        ...
              "Topic": {
                  "type": "string",
                  "pattern": "^arn:aws:sns::$<accountID>:.*$"
              },
        ...
            }
      },
  }
}
```

### CloudFormation template for an S3 bucket that adheres to the restricted schema

Let’s assume that your account ID is `111122223333`. The account ID is propagated to the Hook through the Hook context.

The following is an excerpt from a CloudFormation template that aligns with the restricted schema for an S3 bucket instantiated from the template shown previously. As a result, your Hook allows the corresponding CloudFormation stack to proceed.

```
{
   "AWSTemplateFormatVersion": "2010-09-09",
   "Resources": {
     "S3Bucket": {
       "Type": "AWS::S3::Bucket",
       "Properties": {
         "BucketName":
            "valid-bucket-sns-notification-configuration-template",
         "NotificationConfiguration": {
           "TopicConfigurations": [
             {
              "Topic":
                "arn:aws:sns:eu-west-1:111122223333:this-is-my-topic-and-I-trust-it",
              "Event": "s3:ObjectCreated:*"
             }
           ]
         }
       }
    }
  }
}
```

### CloudFormation template for an S3 bucket that diverges from the restricted schema (example 1)

The following is an excerpt from a CloudFormation template that doesn’t align with the restricted schema for an S3 bucket instantiated from the template shown previously because it attempts to configure the Amazon SNS topic for the notification configuration, which uses an Amazon Resource Name (ARN) of another account. As a result, your Hook causes the corresponding CloudFormation stack to fail.

```
{
   "AWSTemplateFormatVersion": "2010-09-09",
   "Resources": {
     "S3Bucket": {
       "Type": "AWS::S3::Bucket",
       "Properties": {
         "BucketName":
           "invalid-bucket-sns-notification-configuration-template",
         "NotificationConfiguration": {
            "TopicConfigurations": [
              {
               "Topic":
                 "arn:aws:sns:eu-west-1:444455556666:this-is-not-my-topic",
               "Event": "s3:ObjectCreated:*"
              }
            ]
         }
       }
     }
   }
}
```

### CloudFormation template for an S3 bucket that diverges from the restricted schema (example 2)

The following is an excerpt from a CloudFormation template that doesn’t align with the restricted schema for an S3 bucket instantiated from the template shown previously. This time, it violates your feature gating objective by attempting to use a new, imaginary feature of an S3 bucket that isn’t approved for use by your restricted schema for an S3 bucket. As a result, your Hook causes the corresponding CloudFormation stack to fail.

```
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "S3Bucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName":
           "valid-bucket-sns-notification-configuration-template",
        "NewFeature": {
           "property-1": true,
           "property-2": "public"
        },                
        "NotificationConfiguration": {
          "TopicConfigurations": [
            {
              "Topic":
                 "arn:aws:sns:eu-west-1:111122223333:this-is-my-topic-and-I-trust-it",
              "Event": "s3:ObjectCreated:*"
            }
          ]
        }
      }
    }
  }
}
```

## Protect your controls

If a security control itself isn’t protected adequately, it becomes a weak link in the security chain. For example, a surveillance camera (a physical security control) that isn’t securely mounted can be removed, rendering it useless. This principle also applies to your RCFGE solution.

Next, we will show you how to isolate management activities to a dedicated account and use SCPs as preventative controls.

### Isolate RCFGE management in a dedicated account

Organizing your AWS environment by using multiple accounts is a best practice because it enhances security, simplifies management, and allows for better resource isolation and cost tracking. Isolating the operation and management of your RCFGE solution in its own dedicated account is essential for securing the solution’s resources.

With AWS CloudFormation StackSets, you can deploy and manage RCFGE stacks across multiple accounts and AWS Regions from a single central administrator account. This provides consistent and scalable infrastructure while maintaining centralized governance. With this functionality, you can deploy the RCFGE resources to existing accounts and automatically include new accounts as you add them to your organization, simplifying RCFGE management and providing uniformity across your environments. For more information, see Deploy CloudFormation Hooks to an Organization with service-managed StackSets.

Figure 6 shows how to extend that idea so that you can operate the RCFGE solution at scale while maintaining isolation and the separation of duties. The solution operates across three key account types:

- **Management account** –use this account to create your organization and designate the CloudFormation StackSets delegated administrator account.
- **Delegated administrator account** – this account serves as the centralized management point for the RCFGE solution. It contains a continuous integration and continuous delivery (CI/CD) pipeline that provisions RCFGE resources across the organization by using CloudFormation StackSets with service managed permissions. The account hosts a centralized S3 bucket that stores the RCFGE restricted resource schema templates. The security engineering team uses this account to submit Hook code and restricted resource schema template changes, which trigger the CI/CD pipeline.
- **Member accounts** – each member account contains an RCFGE StackSet instance and an AWS Identity and Access Management (IAM) role for provisioning RCFGE resources. It also includes a CloudFormation Hook and an IAM role that allows the Hook to access the centralized S3 bucket with RCFGE restricted resource schema templates.

![Figure 6: Securely operate the RCFGE solution](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img6-1024x847.png)

Figure 6: Securely operate the RCFGE solution

Let’s explore how the RCFGE solution architecture enforces resource configuration step by step, as shown in Figure 7.

![Figure 7: CloudFormation stack deployment flow with RCFGE validation and enforcement](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/img7-1024x359.png)

Figure 7: CloudFormation stack deployment flow with RCFGE validation and enforcement

1. DevOps initiates the deployment by specifying a CloudFormation template that defines the resources and configurations needed.
2. CloudFormation creates a new stack resource, initiating the resource provisioning process based on the provided template.
3. The RCFGE CloudFormation Hook is triggered for each resource defined in the CloudFormation template.
4. The Hook loads the corresponding restricted resource schema template from the S3 bucket.
5. The Hook validates a resource configuration:
    - The Hook processes the restricted resource schema template to create a JSON schema.
    - It uses this JSON schema to validate the current resource in the CloudFormation template.
    - If the resource is invalid according to the schema, the provisioning process is terminated.
6. If the current resource passes validation, CloudFormation proceeds with the resource provisioning process by creating and configuring the resources as specified in the template.

### Use SCPs as preventive controls for your organization to help protect RCFGE

The following SCP excerpt accomplishes three objectives:

- Implements a statement (see `AllowedListActions`) to explicitly specify the access that is allowed while other access is implicitly blocked.
- Implements control objectives to help prevent changes to resources set up by the RCFGE solution (see `ProtectRCFGEResources` and `ProtectStackSetExecutionRole`).
- Makes sure that AWS resource provisioning does not occur outside of CloudFormation (see `ProvisionResourcesViaCloudFormationOnly`).

In this SCP excerpt, the `ProvisionResourcesViaCloudFormationOnly` statement restricts CloudFormation stacks to being managed only through forward access sessions (FAS) in AWS IAM.

The `ProvisionResourcesViaCloudFormationOnly` statement explicitly prohibits direct create, update, and delete actions for all supported resources used in your environment. If needed, split this statement into multiple parts so you don’t exceed SCP size limits, while providing comprehensive coverage of your resources to make sure that they are provisioned and managed only through CloudFormation.

The `ProtectStackSetExecutionRole` statement in this example assumes that CloudFormation trusted access is activated with AWS Organizations, which is required by StackSets to deploy across accounts and Regions by using service managed permissions.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowedListActions",
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:DeleteBucketPolicy",
        "s3:PutAnalyticsConfiguration",
        "s3:PutBucketLogging",
        "s3:PutBucketNotification",
        "s3:PutBucketObjectLockConfiguration",
        "s3:PutBucketPolicy",
        "s3:PutBucketTagging",
        "s3:PutBucketVersioning",
        "s3:PutLifecycleConfiguration",
        "s3:PutMetricsConfiguration",
        "s3:PutReplicationConfiguration",
        "s3:GetObject",
        ...
      ],
      "Resource": "*"
    },
    {
      "Sid": "ProtectRCFGEResources",
      "Effect": "Deny",
      "Action": "*",
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/RCFGEStackSet",
        "arn:aws:cloudformation:*:*:*/hook/RCFGEHook/*",
        "arn:aws:iam::*:role/RCFGEHookExecutionRole"
      ],
      "Condition": {
        "ArnNotLike": {
          "aws:PrincipalArn": [
            "arn:aws:iam::*:role/stacksets-exec-*"
          ]
        }
      }
    },
    {
      "Sid": "ProtectStackSetExecutionRole",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "arn:aws:iam::*:role/stacksets-exec-*"
    },
    {
      "Sid": "ProvisionResourcesViaCloudFormationOnly",
      "Effect": "Deny",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:DeleteBucketPolicy",
        "s3:PutAnalyticsConfiguration",
        "s3:PutBucketLogging",
        "s3:PutBucketNotification",
        "s3:PutBucketObjectLockConfiguration",
        "s3:PutBucketPolicy",
        "s3:PutBucketTagging",
        "s3:PutBucketVersioning",
        "s3:PutLifecycleConfiguration",
        "s3:PutMetricsConfiguration",
        "s3:PutReplicationConfiguration",
        ...
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:CalledViaFirst": "cloudformation.amazonaws.com"
        }
      }
    }
  ]
}
```

To allow the Hook to retrieve the necessary restricted resource schema templates, member accounts must be able to access the S3 bucket that contains the RCFGE templates. The following code sample shows the bucket policy for the S3 bucket that contains the RCFGE templates.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRCFGEHookExecutionRoleGetRCFGETemplates",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Principal": "*",
      "Resource": "arn:aws:s3:::RCFGETemplates/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgID": "o-abcdef0123"
        },
        "ArnLike": {
          "aws:PrincipalArn": "arn:aws:iam::*:role/RCFGEHookExecutionRole"
        }
      }
    }
  ]
}
```

As shown in the following code sample, the `RCFGEHookExecutionRole` IAM role in member accounts has a policy that grants read-only access to the RCFGE templates that are stored in an S3 bucket in the RCFGE delegated administrator account, where `555555555555` represents the account ID.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRCFGEHookExecutionRoleGetRCFGETemplates",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::RCFGETemplates/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceAccount": "555555555555"
        }
      }
    }
  ]
}
```

In the following code sample, the `RCFGEHookExecutionRole` IAM role in member accounts has a trust policy that allows it to be assumed only by the relevant CloudFormation service principals, where `444455556666` represents the account ID of the member account.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "AllowRCFGEHookExecutionRoleGetRCFGETemplatesTrust",
    "Effect": "Allow",
    "Principal": {
      "Service": [
        "hooks.cloudformation.amazonaws.com",
        "resources.cloudformation.amazonaws.com"
      ]
    },
    "Action": "sts:AssumeRole",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:aws:cloudformation:eu-west-1:444455556666:type/hook/RCFGEHook/*"
      }
    }
  }
}
```

### Define baseline configuration for RCFGE and continuous monitoring with AWS Config

Defense in depth is an effective strategy because if one line of defense fails, additional layers are in place to help stop threats at subsequent points. With AWS Config, you can capture the configuration of RCFGE resources over time. You can set up AWS Config custom rules to automatically assess the compliance of your RCFGE resources against predefined policies. For example, you can use an AWS Config custom rule to make sure that the RCFGE Hook hasn’t been altered or removed.

## Conclusion

In this post, you learned how to use CloudFormation Hooks to create a resource configuration enforcement (RCFGE) solution on AWS that is designed to be secure and scalable and that supports feature gating. Using this approach, you, as a security administrator, can maintain strict control over resource configurations and feature adoption across your AWS environments. The solution provides a balanced approach to governance, so that DevOps teams have the flexibility to work within approved boundaries while making sure that new AWS features are only accessible after explicit approval.

If you have feedback about this post, submit comments in the **Comments** section. For questions, start a new thread on the CloudFormation re:Post or contact AWS Support.  
 

![Yossi Cohen](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/ycohen.jpg) Yossi Cohen  
Yossi is a Senior Security Specialist Solutions Architect at AWS for the public sector in the EMEA region. Yossi has over two decades of experience in cloud-native architecture development, design, operations, technical due diligence, and governance in highly regulated environments. At AWS, Yossi collaborates closely with defense, intelligence, government, and public sector clients, helping them navigate their unique threat landscapes.

![Yaniv Rozenboim](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/12/yanivr.jpg) Yaniv Rozenboim  
Yaniv is a Senior Solutions Architect at AWS with extensive experience in cloud architecture and security. He specializes in designing and implementing secure, scalable, and efficient cloud infrastructures. Yaniv works closely with clients to help them achieve their business goals through AWS technologies.

Go to Source
