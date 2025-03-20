---
title: "How to secure your SaaS tenant data in DynamoDB with ABAC and client-side encryption"
date: 2025-01-05
categories: 
  - "cryptography"
  - "encryption"
  - "security"
tags: 
  - "dataprotection"
  - "dynamodb"
  - "iam"
  - "kms"
  - "saas"
  - "securityidentitycompliance"
  - "securityblog"
  - "technicalhow-to"
---

If you’re a SaaS vendor, you may need to store and process personal and sensitive data for large numbers of customers across different geographies. When processing sensitive data at scale, you have an increased responsibility to secure this data end-to-end. Client-side encryption of data, such as your customers’ contact information, provides an additional mechanism that \[…\]

If you’re a SaaS vendor, you may need to store and process personal and sensitive data for large numbers of customers across different geographies. When processing sensitive data at scale, you have an increased responsibility to secure this data end-to-end. Client-side encryption of data, such as your customers’ contact information, provides an additional mechanism that can help you protect your customers and earn their trust.

In this blog post, we show how to implement client-side encryption of your SaaS application’s tenant data in Amazon DynamoDB with the Amazon DynamoDB Encryption Client. This is accomplished by leveraging AWS Identity and Access Management (IAM) together with AWS Key Management Service (AWS KMS) for a more secure and cost-effective isolation of the client-side encrypted data in DynamoDB, both at run-time and at rest.

## Encrypting data in Amazon DynamoDB

Amazon DynamoDB supports data encryption at rest using encryption keys stored in AWS KMS. This functionality helps reduce operational burden and complexity involved in protecting sensitive data. In this post, you’ll learn about the benefits of adding client-side encryption to achieve end-to-end encryption in transit and at rest for your data, from its source to storage in DynamoDB. Client-side encryption helps ensure that your plaintext data isn’t available to any third party.

You can use the Amazon DynamoDB Encryption Client to implement client-side encryption with DynamoDB. In the solution in this post, _client-side encryption_ refers to the cryptographic operations that are performed on the application-side in the application’s Lambda function, before the data is sent to or retrieved from DynamoDB. The solution in this post uses the DynamoDB Encryption Client with the Direct KMS Materials Provider so that your data is encrypted by using AWS KMS. However, the underlying concept of the solution is not limited to the use of the DynamoDB Encryption Client, you can apply it to any client-side use of AWS KMS, for example using the AWS Encryption SDK.

For detailed information about using the DynamoDB Encryption Client, see the blog post How to encrypt and sign DynamoDB data in your application. This is a great place to start if you are not yet familiar with DynamoDB Encryption Client. If you are unsure about whether you should use client-side encryption, see Client-side and server-side encryption in the Amazon DynamoDB Encryption Client Developer Guide to help you with the decision.

## AWS KMS encryption context

AWS KMS gives you the ability to add an additional layer of authentication for your AWS KMS API decrypt operations by using encryption context. The encryption context is one or more key-value pairs of additional data that you want associated with AWS KMS protected information.

Encryption context helps you defend against the risks of ciphertexts being tampered with, modified, or replaced — whether intentionally or unintentionally. Encryption context helps defend against both an unauthorized user replacing one ciphertext with another, as well as problems like operational events. To use encryption context, you specify associated key-value pairs on encrypt. You must provide the exact same key-value pairs in the encryption context on decrypt, or the operation will fail. Encryption context is not secret, and is not an access-control mechanism. The encryption context is a means of authenticating the data, not the caller.

The Direct KMS Materials Provider used in this blog post transparently generates a unique data key by using AWS KMS for each item stored in the DynamoDB table. It automatically sets the item’s partition key and sort key (if any) as AWS KMS encryption context key-value pairs.

The solution in this blog post relies on the partition key of each table item being defined in the encryption context. If you encrypt data with your own implementation, make sure to add your tenant ID to the encryption context in all your AWS KMS API calls.

For more information about the concept of AWS KMS encryption context, see the blog post How to Protect the Integrity of Your Encrypted Data by Using AWS Key Management Service and EncryptionContext. You can also see another example in Exercise 3 of the Busy Engineer’s Document Bucket Workshop.

## Attribute-based access control for AWS

Attribute-based access control (ABAC) is an authorization strategy that defines permissions based on attributes. In AWS, these attributes are called _tags_. In the solution in this post, ABAC helps you create tenant-isolated access policies for your application, without the need to provision tenant specific AWS IAM roles.

If you are new to ABAC, or need a refresher on the concepts and the different isolation methods, see the blog post How to implement SaaS tenant isolation with ABAC and AWS IAM.

## Solution overview

If you are a SaaS vendor expecting large numbers of tenants, it is important that your underlying architecture can cost effectively scale with minimal complexity to support the required number of tenants, without compromising on security. One way to meet these criteria is to store your tenant data in a single pooled DynamoDB table, and to encrypt the data using a single AWS KMS key.

Using a single shared KMS key to read and write encrypted data in DynamoDB for multiple tenants reduces your per-tenant costs. This may be especially relevant to manage your costs if you have users on your organization’s free tier, with no direct revenue to offset your costs.

When you use shared resources such as a single pooled DynamoDB table encrypted by using a single KMS key, you need a mechanism to help prevent cross-tenant access to the sensitive data. This is where you can use ABAC for AWS. By using ABAC, you can build an application with strong tenant isolation capabilities, while still using shared and pooled underlying resources for storing your sensitive tenant data.

You can find the solution described in this blog post in the aws-dynamodb-encrypt-with-abac GitHub repository. This solution uses ABAC combined with KMS encryption context to provide isolation of tenant data, both at rest and at run time. By using a single KMS key, the application encrypts tenant data on the client-side, and stores it in a pooled DynamoDB table, which is partitioned by a tenant ID.

### Solution Architecture

![Figure 1: Components of solution architecture](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/11/28/img1-7.png)

Figure 1: Components of solution architecture

The presented solution implements an API with a single AWS Lambda function behind an Amazon API Gateway, and implements processing for two types of requests:

1. GET request: fetch any key-value pairs stored in the tenant data store for the given tenant ID.
2. POST request: store the provided key-value pairs in the tenant data store for the given tenant ID, overwriting any existing data for the same tenant ID.

The application is written in Python, it uses AWS Lambda Powertools for Python, and you deploy it by using the AWS CDK.

It also uses the DynamoDB Encryption Client for Python, which includes several helper classes that mirror the AWS SDK for Python (Boto3) classes for DynamoDB. This solution uses the EncryptedResource helper class which provides Boto3 compatible get\_item and put\_item methods. The helper class is used together with the KMS Materials Provider to handle encryption and decryption with AWS KMS transparently for the application.

> **Note**: This example solution provides no authentication of the caller identity. See chapter “Considerations for authentication and authorization” for further guidance.

### How it works

![Figure 2: Detailed architecture for storing new or updated tenant data](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2022/11/28/img2-10.png)

Figure 2: Detailed architecture for storing new or updated tenant data

As requests are made into the application’s API, they are routed by API Gateway to the application’s Lambda function (1). The Lambda function begins to run with the IAM permissions that its IAM execution role (DefaultExecutionRole) has been granted. These permissions do not grant any access to the DynamoDB table or the KMS key. In order to access these resources, the Lambda function first needs to assume the ResourceAccessRole, which does have the necessary permissions. To implement ABAC more securely in this use case, it is important that the application maintains clear separation of IAM permissions between the assumed ResourceAccessRole and the DefaultExecutionRole.

As the application assumes the ResourceAccessRole using the AssumeRole API call (2), it also sets a TenantID session tag. Session tags are key-value pairs that can be passed when you assume an IAM role in AWS Simple Token Service (AWS STS), and are a fundamental core building block of ABAC on AWS. When the session credentials (3) are used to make a subsequent request, the request context includes the aws:PrincipalTag context key, which can be used to access the session’s tags. The chapter “The ResourceAccessRole policy” describes how the aws:PrincipalTag context key is used in IAM policy condition statements to implement ABAC for this solution. Note that for demonstration purposes, this solution receives the value for the TenantID tag directly from the request URL, and it is not authenticated.

The trust policy of the ResourceAccessRole defines the principals that are allowed to assume the role, and to tag the assumed role session. Make sure to limit the principals to the least needed for your application to function. In this solution, the application Lambda function is the only trusted principal defined in the trust policy.

Next, the Lambda function prepares to encrypt or decrypt the data (4). To do so, it uses the DynamoDB Encryption Client. The KMS Materials Provider and the EncryptedResource helper class are both initialized with sessions by using the temporary credentials from the AssumeRole API call. This allows the Lambda function to access the KMS key and DynamoDB table resources, with access restricted to operations on data belonging only to the specific tenant ID.

Finally, using the EncryptedResource helper class provided by the DynamoDB Encryption Library, the data is written to and read from the DynamoDB table (5).

### Considerations for authentication and authorization

The solution in this blog post intentionally does not implement authentication or authorization of the client requests. Instead, the requested tenant ID from the request URL is passed as the tenant identity. Your own applications should always authenticate and authorize tenant requests. There are multiple ways you can achieve this.

Modern web applications commonly use OpenID Connect (OIDC) for authentication, and OAuth for authorization. JSON Web Tokens (JWTs) can be used to pass the resulting authorization data from client to the application. You can validate a JWT when using AWS API Gateway with one of the following methods:

1. When using a REST or a HTTP API, you can use a Lambda authorizer
2. When using a HTTP API, you can use a JWT authorizer
3. You can validate the token directly in your application code

If you write your own authorizer code, you can pick a popular open source library or you can choose the AWS provided open source library. To learn more about using a JWT authorizer, see the blog post How to secure API Gateway HTTP endpoints with JWT authorizer.

Regardless of the chosen method, you must be able to map a suitable claim from the user’s JWT, such as the subject, to the tenant ID, so that it can be used as the session tag in this solution.

### The ResourceAccessRole policy

A critical part of the correct operation of ABAC in this solution is with the definition of the IAM access policy for the ResourceAccessRole. In the following policy, be sure to replace <region>, <account-id>, <table-name>, and <key-id> with your own values.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:DescribeTable",
                "dynamodb:GetItem",
                "dynamodb:PutItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:<region>:<account-id>:table/<table-name>",
           ],
            "Condition": {
                "ForAllValues:StringEquals": {
                    "dynamodb:LeadingKeys": [
                        "${aws:PrincipalTag/TenantID}"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey",
            ],
            "Resource": "arn:aws:kms:<region>:<account-id>:key/<key-id>",
            "Condition": {
                "StringEquals": {
                    "kms:EncryptionContext:tenant_id": "${aws:PrincipalTag/TenantID}"
                }
            }
        }
    ]
}
```

The policy defines two access statements, both of which apply separate ABAC conditions:

1. The first statement grants access to the DynamoDB table with the condition that the partition key of the item matches the TenantID session tag in the caller’s session.
2. The second statement grants access to the KMS key with the condition that one of the key-value pairs in the encryption context of the API call has a key called tenant\_id with a value that matches the TenantID session tag in the caller’s session.

> **Warning**: Do not use a ForAnyValue or ForAllValues set operator with the kms:EncryptionContext single-valued condition key. These set operators can create a policy condition that does not require values you intend to require, and allows values you intend to forbid.

## Deploying and testing the solution

### Prerequisites

To deploy and test the solution, you need the following:

- An AWS account
- The AWS Command Line Interface (AWS CLI)
- NodeJS version compatible with AWS CDK version 2.37.0
- Python 3.9
- Git
- Docker

### Deploying the solution

After you have the prerequisites installed, run the following steps in a command line environment to deploy the solution. Make sure that your AWS CLI is configured with your AWS account credentials. Note that standard AWS service charges apply to this solution. For more information about pricing, see the AWS Pricing page.

**To deploy the solution into your AWS account**

1. Use the following command to download the source code:
    
    ```
    git clone https://github.com/aws-samples/aws-dynamodb-encrypt-with-abac
    cd aws-dynamodb-encrypt-with-abac
    ```
    
2. (Optional) You will need an AWS CDK version compatible with the application (2.37.0) to deploy. The simplest way is to install a local copy with npm, but you can also use a globally installed version if you already have one. To install locally, use the following command to use npm to install the AWS CDK:
    
    ```
    npm install cdk@2.37.0
    ```
    
3. Use the following commands to initialize a Python virtual environment:
    
    ```
    python3 -m venv demoenv
    source demoenv/bin/activate
    python3 -m pip install -r requirements.txt
    ```
    
4. (Optional) If you have not used AWS CDK with this account and Region before, you first need to bootstrap the environment:
    
    ```
    npx cdk bootstrap
    ```
    
5. Use the following command to deploy the application with the AWS CDK:
    
    ```
    npx cdk deploy
    ```
    
6. Make note of the API endpoint URL https://<api url>/prod/ in the Outputs section of the CDK command. You will need this URL for the next steps.
    
    ```
    Outputs:
    DemoappStack.ApiEndpoint4F160690 = https://<api url>/prod/
    ```
    

### Testing the solution with example API calls

With the application deployed, you can test the solution by making API calls against the API URL that you captured from the deployment output. You can start with a simple HTTP POST request to insert data for a tenant. The API expects a JSON string as the data to store, so make sure to post properly formatted JSON in the body of the request.

An example request using curl -command looks like:

```
curl https://<api url>/prod/tenant/<tenant-name> -X POST --data '{"email":"<tenant-email@example.com>"}'
```

You can then read the same data back with an HTTP GET request:

```
curl https://<api url>/prod/tenant/<tenant-name>
```

You can store and retrieve data for any number of tenants, and can store as many attributes as you like. Each time you store data for a tenant, any previously stored data is overwritten.

### Additional considerations

A tenant ID is used as the DynamoDB table’s partition key in the example application in this solution. You can replace the tenant ID with another unique partition key, such as a product ID, as long as the ID is consistently used in the IAM access policy, the IAM session tag, and the KMS encryption context. In addition, while this solution does not use a sort key in the table, you can modify the application to support a sort key with only a few changes. For more information, see Working with tables and data in DynamoDB.

### Clean up

To clean up the application resources that you deployed while testing the solution, in the solution’s home directory, run the command cdk destroy.

Then, if you no longer plan to deploy to this account and Region using AWS CDK, you can also use the AWS CloudFormation console to delete the bootstrap stack (CDKToolKit).

## Conclusion

In this post, you learned a method for simple and cost-efficient client-side encryption for your tenant data. By using the DynamoDB Encryption Client, you were able to implement the encryption with less effort, all while using a standard Boto3 DynamoDB Table resource compatible interface.

Adding to the client-side encryption, you also learned how to apply attribute-based access control (ABAC) to your IAM access policies. You used ABAC for tenant isolation by applying conditions for both the DynamoDB table access, as well as access to the KMS key that is used for encryption of the tenant data in the DynamoDB table. By combining client-side encryption with ABAC, you have increased your data protection with multiple layers of security.

You can start experimenting today on your own by using the provided solution. If you have feedback about this post, submit comments in the **Comments** section below. If you have questions on the content, consider submitting them to AWS re:Post

**Want more AWS Security news? Follow us on Twitter.**

![](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2021/10/13/Jani-Muuriaisniemi-Profile.jpg)

### Jani Muuriaisniemi

Jani is a Principal Solutions Architect at Amazon Web Services based out of Helsinki, Finland. With more than 20 years of industry experience, he works as a trusted advisor with a broad range of customers across different industries and segments, helping the customers on their cloud journey.

Go to Source
