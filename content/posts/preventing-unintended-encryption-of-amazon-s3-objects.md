---
title: "Preventing unintended encryption of Amazon S3 objects"
date: 2025-01-17
---

At Amazon Web Services (AWS), the security of our customers’ data is our top priority, and it always will be. Recently, the AWS Customer Incident Response Team (CIRT) and our automated security monitoring systems identified an increase in unusual encryption activity associated with Amazon Simple Storage Service (Amazon S3) buckets.

Working with customers, our security teams detected an increase of data encryption events in S3 that used an encryption method known as _server-side encryption using client-provided keys (SSE-C)_. While this is a feature used by many customers, we detected a pattern where a large number of S3 `CopyObject` operations using SSE-C began to overwrite objects, which has the effect of re-encrypting customer data with a new encryption key. Our analysis uncovered that this was being done by malicious actors who had obtained valid customer credentials, and were using them to re-encrypt objects.

It’s important to note that these actions do not take advantage of a vulnerability within an AWS service—but rather require valid credentials that an unauthorized user uses in an unintended way. Although these actions occur in the customer domain of the shared responsibility model, AWS recommends steps that customers can use to prevent or reduce the impact of such activity.

Using active defense tools, we have implemented automatic mitigations that will help to prevent this type of unauthorized activity in many cases. These mitigations have already prevented a high percentage of attempts from succeeding, without customers taking steps to protect themselves. However, the threat actors used valid credentials, and it is difficult for AWS to reliably distinguish valid usage from malicious use. Therefore, we recommend that customers follow best practices to mitigate risk.

We recommend that customers implement these four security best practices to protect against the unauthorized use of SSE-C:

1. Block the use of SSE-C unless required by an application
2. Implement data recovery procedures
3. Monitor AWS resources for unexpected access patterns
4. Implement short-term credentials

## I. Block the use of SSE-C encryption

If your applications don’t use SSE-C as an encryption method, you can block the use of SSE-C with a resource policy applied to an S3 bucket, or by a resource control policy (RCP) applied to an organization in AWS Organizations.

Resource policies for S3 buckets are commonly referred to as bucket policies and allow customers to specify permissions for individual buckets in S3. A bucket policy can be applied using the S3 `PutBucketPolicy` API operation, the AWS Command Line Interface (CLI), or through the AWS Management Console. Learn more about how bucket policies work in the S3 documentation. The following example shows a bucket policy that blocks SSE-C request for a bucket called `your-bucket-name>`.

```
{
    "Version": "2012-10-17",    
    "Statement": [
        {
            "Sid": "RestrictSSECObjectUploads",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<your-bucket-name>/*",
            "Condition": {
                "Null": {
                    "s3:x-amz-server-side-encryption-customer-algorithm": "false"
                }
            }
        }
    ]
 }
```

RCPs allow customers to specify the maximum available permissions that apply to resources across an entire organization in AWS Organizations. An RCP can be applied by using the AWS Organizations `UpdatePolicy` API operation, the AWS Command Line Interface (CLI), or through the AWS Management Console. Learn more about how RCPs work in the AWS Organizations documentation. The following example shows an RCP that blocks SSE-C requests for buckets in the organization.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RestrictSSECObjectUploads",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "Null": {
          "s3:x-amz-server-side-encryption-customer-algorithm": "false"
        }
      }
    }
  ]
 }
```

## II. Implement data recovery procedures

Without data protection mechanisms in place, data recovery times can be longer. As a data protection best practice, we recommend that you protect against data being overwritten and that you maintain a second copy of critical data.

Enable S3 Versioning to keep multiple versions of an object in a bucket, so that you can restore objects that are accidentally deleted or overwritten. It is important to note that versioning may increase storage costs, especially for applications that frequently overwrite objects in a bucket. In this case, consider implementing S3 Lifecycle policies to manage older versions and control storage costs.

Additionally, copy or take backups of critical data to a different bucket and perhaps to a different AWS account or AWS Region. To do this, you can use S3 replication to automatically copy objects between buckets. These buckets can reside in the same or in different AWS accounts, as well as in the same or in different AWS Regions. S3 replication also offers an SLA for customers that have more stringent RPO (Recovery Point Objective) and RTO (Recovery Time Objective) requirements. Alternatively, you can use AWS Backup for S3, which is a managed service that automates periodic backup of S3 buckets.

## III. Monitor AWS resources for unexpected access patterns

Without monitoring, unauthorized actions on S3 buckets may go unnoticed. We recommend that you use tools such as AWS CloudTrail or S3 server access logs to monitor access to your data.

You can use AWS CloudTrail to log events across AWS services (including Amazon S3) and even combine logs into a single account to make them available to your security teams to access and monitor. You can also create CloudWatch alarms based on specific S3 metrics or logs to alert on unusual activity. These alerts can help you identify anomalous behavior quickly. You can also set up automation that uses Amazon EventBridge and AWS Lambda to automatically take corrective measures. See this topic in the S3 documentation for an example implementation of a setup used to scan all buckets across an organization and apply S3 Block Public Access.

## IV. Implement short-term credentials

The most effective approach to mitigating the risk of compromised credentials is to never create long-term credentials in the first place. Credentials that do not exist cannot be exposed or stolen, and AWS provides a rich set of capabilities that alleviate the need to ever store credentials in source code or in configuration files.

IAM roles enable applications to securely make signed API requests from Amazon Elastic Compute Cloud (Amazon EC2) instances, Amazon Elastic Container Service (Amazon ECS), or Amazon Elastic Kubernetes Service (Amazon EKS) containers, or Lambda functions by using short-term credentials. Even systems outside the AWS Cloud can make authenticated calls without long-term AWS credentials by using the IAM Roles Anywhere feature. Additionally, AWS IAM Identity Center enables developer workstations to obtain short-term credentials backed by their longer-term user identities that are protected by Multi-factor Authentication (MFA).

All of these technologies rely on the AWS Security Token Service (AWS STS) to issue temporary security credentials that can control access to AWS resources without distributing or embedding long-term AWS security credentials within an application, whether in code or in configuration files.

## Summary

Detecting unintended encryption techniques like this in your environment requires vigilance and support. In this post, we highlighted the most common indicators to look for. As your security teams work to constantly protect your environment, know that a number of teams at AWS—including the AWS Customer Incident Response Team (CIRT), Amazon Threat Intelligence, and services teams like the Amazon S3 team—are working diligently to innovate, collaborate, and share insights to help protect your valuable data.

In this post, we provided an update on this recent threat to customer data and highlighted four security best practices that customers can use to protect against the risk of bad actors using SSE-C to encrypt data by using lost or stolen AWS credentials.

As threat actor tactics evolve, our commitment to customer security remains unwavering. Together, we are building a more secure cloud environment, allowing you to innovate with confidence.

If you ever suspect unauthorized activity, please don’t hesitate to contact AWS Support immediately.

![Steve de Vera](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/10/28/Steve-de-vera-author.jpg) Steve de Vera  
Steve is a manager in the AWS Customer Incident Response Team (CIRT) with a focus on threat research and threat intelligence. He is passionate about American-style BBQ and is a certified competition BBQ judge. He has a dog named Brisket.

![Jennifer Paz](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/16/jennpaz.jpg) Jennifer Paz  
Jennifer is a Security Engineer with over a decade of experience, currently serving on the AWS Customer Incident Response Team (CIRT). Jennifer enjoys helping customers tackle security challenges and implementing complex solutions to enhance their security posture. When not at work, Jennifer is an avid walker, jogger, pickleball enthusiast, traveler, and foodie, always on the hunt for new culinary adventures.

Go to Source
