---
title: "How to implement IAM policy checks with Visual Studio Code and IAM Access Analyzer"
date: 2025-01-15
tags: 
  - "comprehensive"
---

In a previous blog post, we introduced the IAM Access Analyzer custom policy check feature, which allows you to validate your policies against custom rules. Now we’re taking a step further and bringing these policy checks directly into your development environment with the AWS Toolkit for Visual Studio Code (VS Code).

In this blog post, we show how you can integrate IAM Access Analyzer custom policy check capability into VS Code, so you can identify overly permissive IAM policies and fine-tune access controls early in the development process. This proactive approach to security and compliance helps to ensure that your IAM policies are validated before they are deployed, reducing the risk of introducing misconfigurations or granting unintended access. It also saves developer time by providing fast feedback to developers when they write a policy that does not meet organizational standards.

## What is the problem?

Although security teams oversee an organization’s overall security posture, developers create applications that require specific permissions. To enable developers to work efficiently while maintaining high security standards, organizations often seek ways to safely delegate the authoring of AWS Identity and Access Management (IAM) policies to developers. Many AWS customers manually review developer-authored IAM policies before deploying them to production environments to help prevent granting excessive or unintended permissions. However, depending on the volume and complexity of policies, these manual reviews can be time-consuming, leading to development delays and potential bottlenecks in the deployment of applications and services. Organizations need to balance secure access management with the agility required for rapid application development and deployment.

## How to use IAM Access Analyzer custom policy checks in VS Code

Custom policy checks are a feature in IAM Access Analyzer that are designed to help security teams proactively identify and analyze critical permissions within their IAM policies. In this section, we provide step-by-step instructions for using custom policy checks directly in VS Code.

### Prerequisites

To complete the examples in our walkthrough, you first need to do the following:

1. Install Python version 3.6 or later.
2. Assuming you are already using the VS Code Integrated Development Environment (IDE), search for and install the AWS Toolkit extension.
3. Configure your AWS role credentials to connect the toolkit to AWS.
4. Install the IAM Policy Validator for AWS CloudFormation, available on GitHub. Alternatively, you can install the IAM Policy Validator for Terraform from GitHub if you are using Terraform as infrastructure-as-code in your organization.
5. So that you can open IAM Access Analyzer policy checks in the VS Code editor, open the VS Code Command Palette by pressing **Ctrl+Shift+P**, search for **IAM Policy Checks**, and then choose **AWS: Open IAM Policy Checks** as shown in Figure 1. 
    
    ![Figure 1: Search for the AWS: Open IAM Policy Checks option](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-1.png)
    
    Figure 1: Search for the AWS: Open IAM Policy Checks option
    

By using the IAM policy checks option in VS Code, you can perform four types of checks:

- ValidatePolicy
- CheckNoPublicAccess
- CheckAccessNotGranted
- CheckNoNewAccess

We’ll walk through examples of each of these checks in the sections that follow.

### Example 1: ValidatePolicy

In this example, we use the `ValidatePolicy` option provided by the IAM policy check plugin to validate IAM policies against IAM policy grammar and AWS best practices. When you run this check, you can view policy validation check findings that include security warnings, errors, general warnings, and suggestions for your policy. These actionable recommendations help you author policies that are aligned with AWS best practices.

**To run the ValidatePolicy check**

1. Let’s use the following IAM policy for illustration purposes. You can see that resource `*` (a wildcard) is being used in the first statement, which indicates that the `iam:PassRole` action is allowed for all resources.
    
    ```
    {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "iam:PassRole",	
            "Resource": "*"
          },
          {
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:PutObject"],
            "Resource": "arn:aws:s3:::amzn-s3-demo-bucket/*"
          }
        ]
      }
    ```
    
2. In the VS Code editor, navigate to the **IAM Policy Checks** pane. Choose the document type **JSON Policy Language** and policy type **Identity**. Then choose **Run Policy Validation**.
    
    ![Figure 2: IAM Access Analyzer ValidatePolicy check results](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-2.png)
    
    Figure 2: IAM Access Analyzer ValidatePolicy check results
    
    You can see that Access Analyzer has detected an issue, which is shown in the **PROBLEMS** pane.
    
    ![Figure 3: Problems pane with finding details for the ValidatePolicy check ](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-3.png)
    
    Figure 3: Problems pane with finding details for the ValidatePolicy check
    
    The security warning shown in Figure 3 states that the `iam:PassRole` action with a wildcard (`*`) in the resource can be overly permissive because it allows the ability to pass any IAM role in that account.
    
3. Now, let’s modify the IAM policy by replacing the wildcard (`*`) with a specific role Amazon Resource Name (ARN).
    
    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": "iam:PassRole",
          "Resource": "arn:aws:iam::111122223333:role/sample_role"
        },
        {
          "Effect": "Allow",
          "Action": ["s3:GetObject", "s3:PutObject"],
          "Resource": "arn:aws:s3:::amzn-s3-demo-bucket/*"
        }
      ]
    }
    ```
    
4. Verify the policy again by running the `ValidatePolicy` check to make sure that it doesn’t generate findings after you updated the IAM policy.  
    
    ![Figure 4: Results of the ValidatePolicy check after IAM policy correction](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-4.png)
    
    Figure 4: Results of the ValidatePolicy check after IAM policy correction
    

### Example 2: CheckNoPublicAccess

With the `CheckNoPublicAccess` option, you can verify whether your resource policy grants public access for supported resource types.

**To run the CheckNoPublicAccess check**

1. To test whether a policy does not allow public access, create a new bucket using a CloudFormation template and attach a resource policy that grants access to any principal to see the objects in this bucket.  
    
    > **WARNING:** This sample bucket policy should not be used in production. Using a wildcard in the principal element of a bucket policy would allow any IAM principal to view the contents of the bucket.
    
    ```
    Resources:
              MyBucket:
                Type: 'AWS::S3::Bucket'
                Properties:
                  BucketName: amzn-s3-demo-bucket
    
              MyBucketPolicy:
                Type: 'AWS::S3::BucketPolicy'
                Properties:
                  Bucket:
                    Ref: 'MyBucket'
                  PolicyDocument:
                    Version: '2012-10-17'
                    Statement:
                      - Effect: Allow
                        Principal: "*"
                        Action: 's3:GetObject'
                        Resource:
                          Fn::Join:
                            - ''
                            - - 'arn:aws:s3:::'
                              - Ref: 'MyBucket'
                              - '/*'
    ```
    
2. Select the document type **CloudFormation template** and then choose **Run Custom Policy Check** to see whether this resource policy passes the `CheckNoPublicAccess` check.
    
    ![Figure 5: IAM Access Analyzer CheckNoPublicAccess check results](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-5.png)
    
    Figure 5: IAM Access Analyzer CheckNoPublicAccess check results
    
    The policy check returns a failed result because this bucket does allow public access.
    
    ![Figure 6: Problems pane finding details for CheckNoPublicAccess check](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-6.png)
    
    Figure 6: Problems pane finding details for CheckNoPublicAccess check
    
3. Next, fix this policy to allow access from a role within the same account by restricting the policy to a specific role ARN.
    
    ```
    Resources:
              MyBucket:
                Type: 'AWS::S3::Bucket'
                Properties:
                  BucketName: amzn-s3-demo-bucket
    
              MyBucketPolicy:
                Type: 'AWS::S3::BucketPolicy'
                Properties:
                  Bucket:
                    Ref: 'MyBucket'
                  PolicyDocument:
                    Version: '2012-10-17'
                    Statement:
                      - Effect: Allow
                        Principal: 
                          "AWS": 'arn:aws:iam::111122223333:role/sample_role'
                        Action: 's3:GetObject'
                        Resource:
                          Fn::Join:
                            - ''
                            - - 'arn:aws:s3:::'
                              - Ref: 'MyBucket'
                              - '/*'
    ```
    
4. Re-run the `CheckNoPublicAccess` check. The resource policy no longer grants public access and the status of the policy check is **PASS**.

### Example 3: CheckAccessNotGranted

The `CheckAccessNotGranted` option allows you to check whether a policy allows access to a list of IAM actions and resource ARNs. You can use this check to give developers fast feedback that certain permissions or access to certain resources are not allowed.

**To run the CheckAccessNotGranted check**

1. Identify sensitive actions and resources.
    
    In the VS Code editor, under **Custom Policy Checks**, choose the check type `CheckAccessNotGranted`. Using a comma-separated list, create a list of actions and resource ARNs that you don’t want to allow in your IAM policy. You can also create a JSON file with your actions and resources by using the syntax shown in Figure 7. For this example, set the `s3:PutBucketPolicy` and `dynamodb:DeleteTable` IAM actions to “not allowed” in the IAM policy.
    
    ![Figure 7: Configure the CheckAccessNotGranted check](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-7.png)
    
    Figure 7: Configure the CheckAccessNotGranted check
    
2. Create a sample CloudFormation template that contains an IAM policy attached to an IAM role, as follows. This policy grants access to some of the actions that you deemed sensitive in Figure 7.
    
    ```
    Resources:
      CreateTagsLambdaRole:
        Type: AWS::IAM::Role
        Properties:
          AssumeRolePolicyDocument:
            Version: '2012-10-17'
            Statement:
            - Effect: Allow
              Principal:
                Service: lambda.amazonaws.com
              Action: sts:AssumeRole
          Policies:
          - PolicyName: my-application-access
            PolicyDocument:
              Version: '2012-10-17'
              Statement:
              - Effect: Allow
                Action:
                - ec2:DescribeInstances
                Resource: "*"
              - Effect: Allow
                Action:
                - s3:GetObject
                - s3:PutBucketPolicy
                - dynamodb:DeleteTable
                Resource: "*"            
              
          RoleName: sample-role
    ```
    
3. In the VS Code editor, choose **Run Custom Policy Check** to identify whether one of the sensitive actions or resources is allowed in the IAM policy. The policy check returns **FAIL** because the policy has the actions `s3:PutBucketPolicy` and `dynamodb:DeleteTable`, which you marked as actions that you don’t want developers to grant access to. Remove the restricted actions from the policy and run the check again to see a **PASS** result for the policy check.

### Example 4: CheckNoNewAccess

The `CheckNoNewAccess` option is a custom policy check that verifies whether your policy grants new access compared to a reference policy.

You use a reference policy to check whether a candidate policy allows more access than the reference policy does. In other words, the check passes if the candidate policy is a subset of the reference policy. A reference policy typically starts by allowing all access. You then add a statement or statements that deny the access that you want the reference policy to check for. For more details and examples of reference policies, see the iam-access-analyzer-custom-policy-check-samples repository on GitHub.

The ability to use a reference policy provides you with the flexibility to look for almost anything in an IAM policy. This is useful when you have custom requirements for your organization that may not be met with some of the other custom policy checks.

**To run the CheckNoNewAccess check**

1. Create a reference policy: In your project, create a new JSON policy document that will serve as your reference policy.
    
    The following reference policy checks that an IAM role trust policy only grants access to an allowlisted set of AWS services. This enables you to allow builders to create roles, but constrain the use of those roles to the set of AWS services specified.
    
    In this reference policy, only the specified AWS service principals `ec2.amazonaws.com`, `lambda.amazonaws.com`, and `ecs-tasks.amazonaws.com` are allowed to assume the role.
    
    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "AllowThisSetOfServicePrincipals",
          "Effect": "Allow",
          "Principal": {
            "Service": [
              "ec2.amazonaws.com",
              "lambda.amazonaws.com",
              "ecs-tasks.amazonaws.com"
            ]
          },
          "Action": "sts:AssumeRole"
        },
        {
          "Sid": "AllowOtherSTSActions",
          "Effect": "Allow",
          "Principal": "*",
          "NotAction": "sts:AssumeRole"
        }
      ]
    }
    ```
    
2. Enter the reference policy in the VS Code editor. In the **IAM Policy Checks** pane, select the check type **CheckNoNewAccess**. Then set the reference policy type to **Resource**, because this is a trust policy that defines which principals can assume the role. In addition, provide the path of the reference policy that you created in Step 1. You can also directly enter the reference policy as a JSON policy document, as shown in Figure 8. 
    
    ![Figure 8: Enter the reference policy for the CheckNoNewAccess check](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-8.png)
    
    Figure 8: Enter the reference policy for the CheckNoNewAccess check
    
3. Create a CloudFormation template, as follows. This template creates an IAM role that allows the AWS service principals `lambda.amazonaws.com` and `glue.amazonaws.com` to assume the `sample-application-role` IAM role.
    
    ```
    Resources:
      SampleApplicationRole:
        Type: AWS::IAM::Role
        Properties:
          AssumeRolePolicyDocument:
            Version: '2012-10-17'
            Statement:
            - Effect: Allow
              Principal:
                Service: 
                - lambda.amazonaws.com
                - glue.amazonaws.com
              Action: sts:AssumeRole
          Policies:
          - PolicyName: my-application-access
            PolicyDocument:
              Version: '2012-10-17'
              Statement:
              - Effect: Allow
                Action:
                - s3:GetObject
                Resource: "arn:aws:s3::111122223333:amzn-s3-demo-bucket/*"            
          RoleName: sample-application-role
    ```
    
4. In the VS Code editor, choose **Run Custom Policy Check** to check your CloudFormation template against the reference policy you configured in Step 1. The check will return **FAIL** and you will see a security warning in the editor in the **PROBLEMS** pane.
    
    ![Figure 9: Problems pane finding details for the CheckNoNewAccess check ](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Implement-IAM-policy-checks-9.png)
    
    Figure 9: Problems pane finding details for the CheckNoNewAccess check
    
    The issue is that `glue.amazonaws.com` was not listed as a service principal that was allowed to assume a role in your reference policy. You can remove `glue.amazonaws.com` from the CloudFormation template and re-run the check to receive a **PASS** result.
    

## Conclusion

In this post, we explored how you can use the integration of VS Code with IAM Access Analyzer in your development workflow to make sure that your IAM policies align with best practices and adhere to your organization’s security requirements. The four critical checks provided by IAM Access Analyzer can be summarized as follows:

- The `ValidatePolicy` check provides actionable recommendations that help you author policies that are aligned with AWS best practices.
- The `CheckNoPublicAccess` check helps protect resources from being exposed publicly and mitigates the risk of unauthorized public access.
- The `CheckAccesNotGranted` check looks for specific IAM actions and resource ARNs to help enforce access restrictions and help prevent unauthorized access to critical data or services.
- The `CheckNoNewAccess` check validates that the permissions granted in your IAM policies remain within the intended scope, as defined by your organization’s requirements.

Install or update the AWS Toolkit for VS Code today, and make sure that you have the CloudFormation Policy Validator or Terraform Policy Validator, to take advantage of these features.

If you have feedback about this post, submit comments in the **Comments** section below.

![Anshu Bathla](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Anshu-Bathla.jpg)

### Anshu Bathla

Anshu is a Lead Consultant – SRC at AWS, based in Gurugram, India. He works with customers across diverse verticals to help strengthen their security infrastructure and achieve their security goals. Outside of work, Anshu enjoys reading books and gardening at his home garden.

![Manoj Kumar](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/11/Manoj-Kumar.jpg)

### Manoj Kumar

Manoj is a Lead Consultant – SRC at AWS, based in Gurugram, India. He collaborates with diverse clients to design and implement comprehensive AWS Cloud security solutions. His expertise helps organizations fortify their cloud infrastructures, achieve compliance objectives, and provide robust data protection while using the advanced security features of AWS to support their business objectives.

Go to Source
