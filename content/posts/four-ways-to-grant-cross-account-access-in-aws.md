---
title: "Four ways to grant cross-account access in AWS"
date: 2025-03-19
---

As your Amazon Web Services (AWS) environment grows, you might develop a need to grant cross-account access to resources. This could be for various reasons, such as enabling centralized operations across multiple AWS accounts, sharing resources across teams or projects within your organization, or integrating with third-party services. However, granting cross-account access requires careful consideration of your security, availability, and manageability requirements.

In this blog post, we explore four different ways to grant cross-account access using resource-based policies. Each method has its own unique tradeoffs, and the best choice depends on your specific requirements and use case.

## Evaluating different techniques for granting cross-account access

Cross-account access is granted by identity-based policies and resource-based policies in AWS Identity and Access Management (IAM). Identity-based policies attach to an IAM role, while resource-based polices attach to resources like Amazon Simple Storage Service (Amazon S3) buckets and AWS Key Management Service (AWS KMS) keys. Resource-based policies require you to specify one or more principals (IAM users or roles) that are allowed to access the resource.

Your choice of how to specify the principal in a resource-based policy impacts some aspects of both the confidentiality and the availability of your solution. Understanding this impact and making the right tradeoffs for your use case is the focus of this post.

## An example scenario

Imagine that you have an S3 bucket in your AWS account (Account A) that needs to be accessed by different principals in another AWS account (Account B). For this scenario, we assume that the principals in Account B have the necessary access to S3 in their identity-based policies, and we will focus on authoring the resource-based policies in Account A. While the methods explained here use Amazon S3, the concepts discussed apply to all AWS services that support resource-based policies. In the following sections, we walk through four different ways to grant cross-account access in this scenario and discuss the tradeoffs of each.

### Method 1: Grant access to a specific IAM role using the Principal element of the resource-based policy

In this example, you use an S3 bucket policy to grant access to a specific IAM role (`RoleFromAccountB`) in Account B by specifying the IAM role’s Amazon Resource Name (ARN) in the `Principal` element of the policy in Account A.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRoleInThePrincipalElement",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/RoleFromAccountB"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}
```

Using this bucket policy, if someone in Account B deletes or recreates the role (`RoleFromAccountB`), then that role can no longer access the `amzn-s3-demo-bucket-account-a` bucket, even if that role is recreated with the same name. The reason is that when you save this policy, the role ARN is mapped to the unique ID of the role, which looks something like this: `AROADBQP57FF2AEXAMPLE`. You will see a role identifier in the `Principal` element of your resource-based policies if you view them after you delete the role that they referenced.

This behavior is intentional. The resource-based policy only allows the specific instance of the role that you set as principal at the time of policy creation. This helps prevent unintended access to your resources if you delete a role, but forget to update your resource-based policy to remove that role. This behavior can also cause an availability risk because the role (`RoleFromAccountB`) will have a new unique ID when it is recreated and will no longer have access to the bucket. Roles can be recreated for a number of reasons, including accidentally when you use tools such as infrastructure as code.

**You might consider choosing this method if:**

- You own the roles in both Account A and Account B and can control the creation and deletion of these roles.
- You want your resource-based policy in Account A to stop granting access when the specified role (`RoleFromAccountB`) is deleted.
- You prioritize granular access control over potential availability concerns if the role (`RoleFromAccountB`) is deleted.

### Method 2: Grant access to an account using the Principal element of the resource-based policy

In this example, you grant access to a specific account in the `Principal` element of the resource-based policy. This resource-based policy of Account A allows any user or role from Account B that also has an identity-based policy that grants them access to read the objects.

> **Note:** You can use either `"Principal": {"AWS": "111122223333"}` or `"Principal": {"AWS": "arn:aws:iam::111122223333:root"}` in the `Principal` element. They are equivalent, and the long-form ARN does not represent the root user.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInThePrincipalElement",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}
```

This resource-based policy helps avoid the potential availability issue discussed for Method 1. If a role in Account B that needs to have access to the bucket is recreated, it will still have access after the recreation of that role. This is because you don’t specify a role in the `Principal` element—instead, you specify an account. If you use Method 2, you must be comfortable delegating access control decisions to the owner of that account.

This approach explicitly delegates access control decisions to IAM in the other account (Account B). Principals in Account B have access to this bucket if allowed by their identity-based policies.

**You might consider choosing this method if:**

- You need to grant access to many principals in Account B.
- You want to delegate the access decision in the account where the principal exists (Account B).
- You prioritize ease of management and availability over granular access control.

### Method 3: Grant access to a specific IAM role using the aws:PrincipalArn condition

This method expands on Method 2 and adds a condition that grants access only to a specific IAM role. Similar to Method 2, you use the account number as the value of the `Principal` element, but also use the `aws:PrincipalArn` condition key to limit access to a specific principal in Account B.

The aws:PrincipalArn condition key is a global condition key that compares the ARN of the principal that made the request with the ARN that you specify in the policy. For IAM roles, the request context returns the ARN of the role, not the ARN of the user that assumed the role.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInPrincipalAndRoleInPrincipalArn",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "ArnEquals": {
          "aws:PrincipalArn": "arn:aws:iam::111122223333:role/RoleFromAccountB"
        }
      }
    }
  ]
}
```

This policy comes with the same availability benefits as the policy in Method 2: access to this resource will survive role recreation. This is because the role is translated to its unique identifier only when it is used in the `Principal` element. It is not translated to a unique identifier when it is used in a condition. If the role (`RoleFromAccountB`) in Account B is recreated, accidentally or intentionally, the policy will continue to grant access because the role matches the role ARN specified in the condition key of the resource-based policy in Account A. As a result, Method 3 provides a balanced approach to availability and security.

**You might consider choosing this method if:**

- You are comfortable that this policy will continue to grant access to the role specified in the `aws:PrincipalArn` condition key if that role (`RoleFromAccountB`) is recreated.
- You don’t own the Account B you are granting access to and don’t control when that role may be recreated.
- You want a balance of availability and confidentiality.

### Method 4: Grant access to an entire AWS Organizations organization

This method is focused on a different use case and is not an alternative to the methods listed earlier. Use this method if you have a resource (an S3 bucket, in this example) that you want to share with your entire organization, but not share with anyone outside of it.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessToAnEntireOrganization",
      "Principal": {
        "AWS": "*"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgId": "o-12345"
        },
        "StringNotEquals": {
          "aws:PrincipalAccount": "${aws:ResourceAccount}"
        }
      }
    }
  ]
}
```

There is no way to specify an organization by using the `Principal` element of a resource-based policy, so you must use the aws:PrincipalOrgId condition key to restrict access to a specific organization. In this policy, you specify a wildcard in the `Principal` element, which says that anyone can access the bucket. Then the condition reduces “anyone” to just those AWS account principals that belong to the specified organization and have an identity-based policy that allows them access.

You then add an additional conditional block that compares the `aws:PrincipalAccount` condition key to the aws:ResourceAccount condition key by using a policy variable. This extra conditional block is optional and excludes the account that owns the bucket (Account A) from the allow statement. The reason for using this extra conditional block is so that principals in Account A still require an allow statement in their identity-based policy to access this bucket. If you choose to exclude this `aws:PrincipalAccount` comparison, principals in Account A are granted access to the bucket without an explicit allow statement in their identity-based policy. Policy evaluation logic only requires either the identity-based policy or the resource-based policy (but not both) to allow a request when the principal and resource are in the same account.

**You might consider choosing this method if:**

- You have a shared resource that should be accessible to your entire organization.

## Conclusion

Choosing a method to grant cross-account access requires careful consideration of your requirements and use case. Each of the four methods discussed in this blog post has its own advantages and tradeoffs. By understanding these methods and their implications, you can decide on the most appropriate approach to grant cross-account access to your AWS resources. Remember to regularly review and audit your resource-based policies to verify that they align with your security and access requirements.

To learn how resource-based policies work with Amazon S3, see the blog post IAM Policies and Bucket Policies and ACLs! Oh My! Controlling Access to S3 Resources.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Anshu Bathla](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Anshu-Bathla.jpg) Anshu Bathla  
Anshu is a Lead Consultant – SRC at AWS, based in Gurugram, India. He works with customers across diverse verticals to help strengthen their security infrastructure and achieve their security goals. Outside of work, Anshu enjoys reading books and gardening at his home garden.

![Jay Goradia](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/jaygorad.jpg) Jay Goradia  
Jay is a Technical Account Manager (TAM) at AWS who works closely with enterprise customers to accelerate their cloud journey through strategic guidance and technical expertise. Using his security background, he helps organizations understand security best practices in AWS.

Go to Source
