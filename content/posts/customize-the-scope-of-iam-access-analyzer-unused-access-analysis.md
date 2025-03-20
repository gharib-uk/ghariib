---
title: "Customize the scope of IAM Access Analyzer unused access analysis"
date: 2025-01-08
tags: 
  - "data"
  - "management"
  - "tech"
  - "ui"
  - "un"
---

AWS Identity and Access Management Access Analyzer simplifies inspecting unused access to guide you towards least privilege. You can use unused access findings to identify over-permissive access granted to AWS Identity and Access Management (IAM) roles and users in your accounts or organization. From a delegated administrator account for IAM Access Analyzer, you can use the dashboard to review unused access findings across your organization and prioritize the accounts to inspect based on the volume and type of findings. The findings highlight unused roles, unused access keys for IAM users, and unused passwords for IAM users. For active IAM users and roles, the findings provide visibility into unused services and actions. Recently, IAM Access Analyzer launched new configuration capabilities that you can use to customize the analysis. You can select accounts, roles, and users to exclude, and focus on the areas that matter the most to you. You can use identifiers such as account ID or scale configuration using tags. By scoping the IAM Access Analyzer to monitor a subset of accounts and roles, you can reduce noise from unwanted findings. You can update the configuration when needed to change the scope of analysis. With this new offering, IAM Access Analyzer provides enhanced controls to help you tailor the analysis more closely to your organization’s security needs.

In this post, we walk you through an example scenario. Imagine that you’re a cloud administrator in a company that uses Amazon Web Services (AWS). You use AWS Organizations to organize your workload into several organizational units (OUs) and accounts. You have dedicated accounts for testing and experimenting with new AWS features called _sandbox_ accounts across your organization. The sandbox accounts can be created by anyone in your company and are centrally recorded. You’re using tags on IAM resources and have followed AWS best practices and strategies when tagging your AWS resources. Tags are applied to the IAM roles created by your teams.

To make sure that your teams are following the principle of least privilege and are working with only the required permissions to access the AWS accounts, you use IAM Access Analyzer. You created an unused access analyzer at the organization level so it will monitor the AWS accounts in your organization. You noticed that you have multiple unused access findings. After analysis, your security team suggests the exclusion of some AWS accounts, IAM roles, and users so they can focus on the relevant findings. They want the sandbox accounts and the IAM roles they use for security purposes (such as auditing, incident response) to be excluded from the unused access analysis.

You can select accounts and roles to exclude when you create a new analyzer or update the analyzer later. In this post, we show you how to configure IAM Access Analyzer unused access finding to exclude specific accounts across your organization and specific principals (IAM roles and IAM users) once you have set up an analyzer. There is no additional pricing for using the prescriptive recommendations after you have enabled unused access findings.

## Prerequisites

The following are the prerequisites to configure IAM Access Analyzer for unused access analysis:

- An unused access analyzer created at the organization level
- Administrative level access to the IAM Access Analyzer delegated administrator account
- A list of account IDs that you want to exclude
- IAM roles with tags

In the following sections, you will learn how to customize your IAM Access Analyzer to better suit your organization’s needs. This includes the following:

1. Explore how to exclude specific AWS accounts from the analyzer’s unused access findings.
2. See how to exclude tagged IAM roles from the analysis, allowing you to focus on the most relevant security insights and you see how to review exclusions on your analyzer to modify them as needed.
3. By the end, you will have a tailored unused access analyzer that provides more meaningful and actionable results for your organization.

## Exclude specific accounts across your organization

In this section, you will see how to update your existing unused access analyzer at the organization level through the AWS Management Console and AWS Command Line Interface (AWS CLI) to exclude specific AWS account IDs from its analysis.

If you don’t have an unused access analyzer in the organization, see this post for instructions on how to create one.

**Use the console to update your unused access analyzer:**

1. Connect to your IAM Access Analyzer delegated administrator account (by default, your organization management account).
2. Open the IAM Access Analyzer console in your management account. You will see the dashboard with your active finding by selecting the analyzer of your choice on the top right. In this example, the analyzer has 251 active findings.  
    
    ![Figure 1: Unused access findings dashboard without exclusions](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-1.jpeg)
    
    Figure 1: Unused access findings dashboard without exclusions
    
3. You can see the split of active findings per account. The example account has 57 active findings that you want to exclude from it.  
    
    ![Figure 2: Unused access findings per account](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-2.png)
    
    Figure 2: Unused access findings per account
    
4. Select **Analyzer settings** under **Access Analyzer** in that navigation pane.
5. The analyzer settings page presents the analyzers in your AWS Region and their status.
6. Select your unused access analyzer in the list based on its name.  
    
    ![Figure 3: Active access analyzers](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-3.png)
    
    Figure 3: Active access analyzers
    
7. On the **Analyzer** page, you can see the analyzer settings and a new tab called **Exclusion**. Because you have no excluded AWS accounts, the count of **Excluded AWS accounts** is **0** and there are no accounts displayed.  
    
    ![Figure 4: Unused access analyzer exclusion tab](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-4.png)
    
    Figure 4: Unused access analyzer exclusion tab
    
8. Choose **Manage** in the **Excluded AWS accounts** section.
9. Select **Choose from organization** and **Hierarchy** and choose **Exclude** next to the sandbox account that you want to exclude.  
    
    ![Figure 5: Exclude sandbox account](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-5.png)
    
    Figure 5: Exclude sandbox account
    
10. After you select **Exclude** for the sandbox account, the account will be deselected and will appear in **AWS accounts to exclude**. The count of accounts to exclude has changed from **0** to **1**. After you have finished, choose **Save changes**.  
    
    ![Figure 6: Verify that the account is excluded and save changes](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-6.png)
    
    Figure 6: Verify that the account is excluded and save changes
    
11. The page will be automatically updated with your changes. You can then review the **Excluded AWS accounts** and verify that your excluded account is correctly configured.  
    
    ![Figure 7: Analyzer configuration updated with excluded account](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-7.png)
    
    Figure 7: Analyzer configuration updated with excluded account
    
12. You can go back to the console dashboard and see the results. In this example, the exclusion of the sandbox account has caused the total number of active findings to go down from 251 to 194. 
    
    ![Figure 8: Dashboard showing a reduction in active findings](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-8.png)
    
    Figure 8: Dashboard showing a reduction in active findings
    

**Use AWS CLI to update your unused access analyzer:**

You can update your existing analyzer using the AWS CLI command `aws accessanalyzer update-analyzer`. Use the following command, replacing `_<YOUR-ANALYZER-NAME>_` with the name of your analyzer.

```
aws accessanalyzer update-analyzer 
--analyzer-name <YOUR-ANALYZER-NAME>
--configuration '{
  "unusedAccess": {
    "analysisRule": {
      "exclusions": [
        {
          "accountIds": [
            "222222222222"
          ]
        }
      ]
    }
  }
}'
```

You will obtain a result similar to the following:

```
{
    "revisionId": "<UNIQUE-REVISION-NUMBER>", 
    "configuration": {
        "unusedAccess": {
            "analysisRule": {
                "exclusions": [
                    {
                        "accountIds": [
                            "222222222222"
                        ]
                    }
                ]
            }, 
            "unusedAccessAge": 90
        }
    }
```

You have successfully excluded a sandbox account from the unused access analysis. Now you will exclude the IAM roles used by the security team to audit your accounts based on tags.

## Excluding specific principals in your organization using tags

In this section, you will see how to update an existing unused access analyzer by excluding tagged IAM roles in your organization using the console and then AWS CLI.

**Use the console to update your unused access analyzer:**

1. Open the IAM Access Analyzer console.
2. Review the summary dashboard containing your unused findings. Choose **Analyzer settings** at the top of the screen.  
    
    ![Figure 9: IAM Access Analyzer summary dashboard](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-9.png)
    
    Figure 9: IAM Access Analyzer summary dashboard
    
3. You will see a list of analyzers created in your account in that Region. Select the analyzer that you want to update.
4. Review the analyzer page. On the **Exclusion** tab, you will see **Exclude IAM users and roles with tags** with a count of **0**.  
    
    ![Figure 10: Configure exclusion of IAM roles using tag](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-10.png)
    
    Figure 10: Configure exclusion of IAM roles using tag
    
5. Choose **Manage** in the **Excluded IAM users and roles with tags** section.
6. Add the tags attached to the roles that you want to exclude from the analysis and choose **Save changes**.  
    
    ![Figure 11: Add tag to exclude](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-11.png)
    
    Figure 11: Add tag to exclude
    
7. You can now see that **Excluded IAM users and roles with tags** now has a count of **1**, and you can see the tags in the list.  
    
    ![Figure 12: List of exclusion tags](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-12.png)
    
    Figure 12: List of exclusion tags
    

**Use AWS CLI to update your unused access analyzer:**

You can also update your existing analyzer using the AWS CLI command `aws accessanalyzer update-analyzer`. Using the following command, replace `_<YOUR-ANALYZER-NAME>_` with the name of your analyzer.

```
aws accessanalyzer update-analyzer 
--analyzer-name <YOUR-ANALYZER-NAME> 
--configuration '{
  "unusedAccess": {
    "analysisRule": {
      "exclusions": [
        {
          "accountIds": [
            "222222222222"
          ]
        },
        {
          "resourceTags": [
            {
              "team": "security"
            }
          ]
        }
      ]
    }
  }
}'

```

A successful response will look like the following:

```
{
    "revisionId": "<UNIQUE-REVISION-NUMBER>", 
    "configuration": {
        "unusedAccess": {
            "analysisRule": {
                "exclusions": [
                    {
                        "accountIds": [
                            "222222222222"
                        ]
                    }, 
                    {
                        "resourceTags": [
                            {
                                "team": "security"
                            }
                        ]
                    }
                ]
            }, 
            "unusedAccessAge": 90
        }
    }
}
```

## Review the exclusion on your analyzer

You can review, remove, or update the exclusions configured on your analyzer by using the console or AWS CLI. For example, as a security administrator managing multiple accounts, you might initially exclude IAM roles that have the tag `security` from analysis. However, you might need to review these exclusions if your policies change, requiring analysis of certain security roles or removing the exclusion entirely. By adjusting your exclusions, you can make sure that your analyzer’s results remain relevant to your organization’s needs and account structure.

**Review the exclusion on unused access analyzer using the console:**

In this section, review the tags that have been excluded from an analyzer.

1. Open the IAM console.
2. Select **Access Analyzer**, under **Access reports**, you will see a summary dashboard of findings from an analyzer.
    1. The **Active findings** section shows the number of active findings for unused roles, the number of active findings for unused credentials and the number of active findings for unused permissions.
    2. The **Findings overview** section includes a breakdown of the active findings.
    3. The **Findings status** section shows the status of findings (whether active, archived or resolved).  
        
        ![Figure 13: Unused access analyzer dashboard ](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-8.png)
        
        Figure 13: Unused access analyzer dashboard
        
3. Select the **Analyzer settings** at the top of the screen.
4. Select the analyzer that you want to review to see the exclusion tags.  
    
    ![Figure 14: Review unused access analyzer exclusions](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-14-1.png)
    
    Figure 14: Review unused access analyzer exclusions
    

5. After applying the tags, the updated dashboard is shown after the next scan.  
    
    ![Figure 15: Dashboard showing reduction of findings after exclusions](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Customize-scope-IAM-Access-Analyzer-15.png)
    
    Figure 15: Dashboard showing reduction of findings after exclusions
    

**Review the exclusion on an unused access analyzer using AWS CLI:**

Using the name of your analyzer, you can run the command `get-analyzer` to see the configured exclusion. Using the following command, replace `_<YOUR-ANALYZER-NAME>_` with the name of your analyzer:

`aws accessanalyzer get-analyzer --analyzer-name _<YOUR-ANALYZER-NAME>_`

You will get a response similar to the following:

```
{
  "analyzer": {
    "status": "ACTIVE",
    "name": "<YOUR-ANALYZER-NAME>",
    "tags": {},
    "revisionId": "<UNIQUE-REVISION-NUMBER>",
    "arn": "arn:aws:access-analyzer:<REGION>:111111111111:analyzer/<YOUR-ANALYZER-NAME>",
    "configuration": {
      "unusedAccess": {
        "analysisRule": {
          "exclusions": [
            {
              "accountIds": [
                "222222222222"
              ]
            },
            {
              "resourceTags": [
                {
                  "team": "security"
                }
              ]
            }
          ]
        },
        "unusedAccessAge": 90
      }
    },
    "type": "ORGANIZATION_UNUSED_ACCESS",
    "createdAt": "2024-10-11T22:26:57Z"
  }
}
```

## Conclusion

In this post, you learned how to tailor your unused access analyzer to your needs by excluding specific accounts and IAM roles. To exclude the accounts in your organization from being monitored by IAM Access Analyzer, you can use a list of account IDs or select them from a hierarchical view of your organization structure. You can exclude IAM roles and IAM users based on tags. By customizing the exclusion on the unused access analyzer, you saw that the number of active findings went down, helping you focus on the findings that matter most. With this new offering, IAM Access Analyzer provides enhanced controls to help you tailor the analysis more closely to your organization’s security needs.

- To learn more about IAM Access Analyzer Unused Access, see Findings for external and unused access.
- To learn about the creation of unused access findings, see IAM Access Analyzer simplifies inspection of unused access in your organization.
- To learn about how to Refine unused access using IAM Access Analyzer recommendations.
- To learn about the strategies for achieving least privilege at scale, see Strategies for achieving least privilege at scale Part 1 and Part 2.

If you have feedback about this post, submit comments in the **Comments** section below.

![Stéphanie Mbappe](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/07/07/StephMbappe.png)

### P. Stéphanie Mbappe

Stéphanie is a Security Consultant with Amazon Web Services. She delights in assisting her customers at any step of their security journey. Stéphanie enjoys learning, designing new solutions, and sharing her knowledge with others.

![Mathangi Ramesh](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/11/01/mathangi_ramesh-120x160-1.jpg)

### Mathangi Ramesh

Mathangi is the product manager for AWS Identity and Access Management. She enjoys talking to customers and working with data to solve problems. Outside of work, Mathangi is a fitness enthusiast and a Bharatanatyam dancer. She holds an MBA degree from Carnegie Mellon University.

![Reke Jarikre](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/02/Reke-Jarikre.png)

### Reke Jarikre

Reke is an Associate Security Consultant at Amazon Web Services. She is passionate about safeguarding client infrastructures and crafting robust security measures. Outside work, she enjoys exploring new technologies, public speaking and contributing to open-source projects.

Go to Source
