---
title: "<div>How to scan a full commit history to detect sensitive secrets</div>"
date: 2025-02-06
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Secrets left exposed in outdated repositories pose significant risk for data breaches. For example, a still-active secret key can be exposed, leaving it vulnerable to exploitation. Secrets include access keys, API tokens, private keys, and other sensitive values.

In this article, you'll learn how to use GitLab Secret Detection to scan a repository’s full commit history, including all branches, to detect sensitive secrets. In addition, you will discover how to view the results directly within the GitLab UI without the need for any integration. All it takes is just a couple of lines of code in your `.gitlab-ci.yml` pipeline file.

## Scan every corner of your repository

We will use the sample repository shown in the screenshot below as an example. To keep things simple, there is only a `README.md` file present in the default branch of this repository.

![Sample repository to scan](https://images.ctfassets.net/r9o86ar0p03f/rhoOE36wJZx4w1ZWTD571/e85df919b7c53f84dbf425c4c9ba4c48/image1.png)

At first glance, it may seem like the repository is empty and that there are probably no sensitive secrets in this repository. But what we are looking at is only the state of the default branch, which is the main branch in this example. There could be feature branches in this repository created weeks, months, or years ago with sensitive secrets. It is also possible that a file with a secret was accidentally pushed to the repo and then deleted right after. However, it likely was not deleted correctly and is still in the commit history.

We are going to enable GitLab Secret Detection scanner and set the `SECRET_DETECTION_HISTORIC_SCAN` variable to **true** so that the content of all branches in the repository is scanned.

![Enable GitLab Secret Detection variable to true](https://images.ctfassets.net/r9o86ar0p03f/762Y7d6HS57rEiOsfAN5T1/96f1ff93b74d08a8d117d398e9670562/image3.png)

```
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml
secret_detection:
  variables:
    SECRET_DETECTION_HISTORIC_SCAN: "true"
```

By setting the `SECRET_DETECTION_HISTORIC_SCAN` variable to **true**, GitLab Secret Detection looks into every branch and commit of your repository. It ensures that no sensitive information — whether from a feature branch or an old commit — is left unchecked.

## Results of the scan

Two sensitive secrets were identified in the repository. One is a password in a `.env` file that was deleted from the repository, but the commit containing it was not removed from the git history. The other is an AWS Access Token found in a feature branch. These exposed secrets could compromise the organization’s security.

![AWS Access Token screen](https://images.ctfassets.net/r9o86ar0p03f/3hIefsd5jRRYOTrgC8XXls/0d61653d48ff3f77cd2e5a0ff4409c8d/image2.png)

You can click on the AWS Access Token result to see more details, including the file location. You can also create a GitLab issue to triage the vulnerability with one click. If you’re using the Jira integration, you can create a Jira ticket directly from the vulnerability page as well.

## Why scanning for secrets matters

Anyone with access to the repository can misuse the secret to gain unauthorized access to private resources and sensitive data.

In addition to scanning a repository’s full commit history across all branches, GitLab Secret Detection also helps you take a multilayered approach to detecting secrets:

- Secret push protection - scans commits for secrets during a push and blocks it if secrets are detected, unless skipped, reducing the risk of leaks.
- Pipeline secret detection - scans files after they’ve been committed and pushed to a GitLab repository.
- Client-side secret detection - scans comments and descriptions in issues and merge requests for secrets before they're saved to GitLab. \* Automatic response to leaked secrets - automatically revokes certain types of leaked secrets and notifies the partner that issued the secret.

You can adjust pipeline secret detection to suit your needs by modifying, extending, or replacing the default ruleset. For instance, you can define custom rules using regex patterns to detect sensitive data like credit card numbers, phone numbers, or other information specific to your organization.

## Try GitLab Secret Detection

1. Enable Secret Detection in your GitLab pipeline.
2. Set `SECRET_DETECTION_HISTORIC_SCAN: true`.
3. Push and trigger a pipeline to scan all branches and commits.

GitLab makes securing your code simple and comprehensive. Don’t let an old branch or commit compromise your security — give historical scans a try today!

> #### Sign up for a free 60-day trial of GitLab Ultimate to get started with security scanners like Secret Detection.

Secrets left exposed in outdated repositories pose significant risk for data breaches. For example, a still-active secret key can be exposed, leaving it vulnerable to exploitation. Secrets include access keys, API tokens, private keys, and other sensitive values.

In this article, you'll learn how to use GitLab Secret Detection to scan a repository’s full commit history, including all branches, to detect sensitive secrets. In addition, you will discover how to view the results directly within the GitLab UI without the need for any integration. All it takes is just a couple of lines of code in your `.gitlab-ci.yml` pipeline file.

## Scan every corner of your repository

We will use the sample repository shown in the screenshot below as an example. To keep things simple, there is only a `README.md` file present in the default branch of this repository.

![Sample repository to scan](https://images.ctfassets.net/r9o86ar0p03f/rhoOE36wJZx4w1ZWTD571/e85df919b7c53f84dbf425c4c9ba4c48/image1.png)

At first glance, it may seem like the repository is empty and that there are probably no sensitive secrets in this repository. But what we are looking at is only the state of the default branch, which is the main branch in this example. There could be feature branches in this repository created weeks, months, or years ago with sensitive secrets. It is also possible that a file with a secret was accidentally pushed to the repo and then deleted right after. However, it likely was not deleted correctly and is still in the commit history.

We are going to enable GitLab Secret Detection scanner and set the `SECRET_DETECTION_HISTORIC_SCAN` variable to **true** so that the content of all branches in the repository is scanned.

![Enable GitLab Secret Detection variable to true](https://images.ctfassets.net/r9o86ar0p03f/762Y7d6HS57rEiOsfAN5T1/96f1ff93b74d08a8d117d398e9670562/image3.png)

```
include:
  - template: Jobs/Secret-Detection.gitlab-ci.yml
secret_detection:
  variables:
    SECRET_DETECTION_HISTORIC_SCAN: "true"
```

By setting the `SECRET_DETECTION_HISTORIC_SCAN` variable to **true**, GitLab Secret Detection looks into every branch and commit of your repository. It ensures that no sensitive information — whether from a feature branch or an old commit — is left unchecked.

## Results of the scan

Two sensitive secrets were identified in the repository. One is a password in a `.env` file that was deleted from the repository, but the commit containing it was not removed from the git history. The other is an AWS Access Token found in a feature branch. These exposed secrets could compromise the organization’s security.

![AWS Access Token screen](https://images.ctfassets.net/r9o86ar0p03f/3hIefsd5jRRYOTrgC8XXls/0d61653d48ff3f77cd2e5a0ff4409c8d/image2.png)

You can click on the AWS Access Token result to see more details, including the file location. You can also create a GitLab issue to triage the vulnerability with one click. If you’re using the Jira integration, you can create a Jira ticket directly from the vulnerability page as well.

## Why scanning for secrets matters

Anyone with access to the repository can misuse the secret to gain unauthorized access to private resources and sensitive data.

In addition to scanning a repository’s full commit history across all branches, GitLab Secret Detection also helps you take a multilayered approach to detecting secrets:

- Secret push protection - scans commits for secrets during a push and blocks it if secrets are detected, unless skipped, reducing the risk of leaks.
- Pipeline secret detection - scans files after they’ve been committed and pushed to a GitLab repository.
- Client-side secret detection - scans comments and descriptions in issues and merge requests for secrets before they're saved to GitLab. \* Automatic response to leaked secrets - automatically revokes certain types of leaked secrets and notifies the partner that issued the secret.

You can adjust pipeline secret detection to suit your needs by modifying, extending, or replacing the default ruleset. For instance, you can define custom rules using regex patterns to detect sensitive data like credit card numbers, phone numbers, or other information specific to your organization.

## Try GitLab Secret Detection

1. Enable Secret Detection in your GitLab pipeline.
2. Set `SECRET_DETECTION_HISTORIC_SCAN: true`.
3. Push and trigger a pipeline to scan all branches and commits.

GitLab makes securing your code simple and comprehensive. Don’t let an old branch or commit compromise your security — give historical scans a try today!

> #### Sign up for a free 60-day trial of GitLab Ultimate to get started with security scanners like Secret Detection.

Go to Source
