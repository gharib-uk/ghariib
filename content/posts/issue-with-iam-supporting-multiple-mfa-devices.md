---
title: "Issue With IAM Supporting Multiple MFA Devices"
date: 2025-01-02
categories: 
  - "security"
---

**Initial Publication Date: 04/25/2023 10:00AM EST**

A security researcher recently reported an issue with AWS’s recently-released (November 16th, 2022) support for multiple multi-factor authentication (MFA) devices for IAM user principals. The reported issue could have potentially arisen only when the following three conditions were met: (1) An IAM user had possession of long-term access key (AK)/secret key (SK) credentials, (2) that IAM user had the privilege to add an MFA to their own identity without using an MFA, and (3) that IAM user’s overall access privileges beyond console sign-in had been configured by an administrator to be greater after adding the MFA. Under those narrow conditions, possession of AK/SK alone was equivalent to possession of AK/SK and a previously configured MFA.

While IAM users with the ability to add or delete an MFA device associated with their own identity have always been able to do so solely with AK/SK credentials, an issue arose when the new feature was combined with the self-management by IAM users of their own MFA devices, with restricted access prior to an MFA being added by the user. This self-management pattern was documented **here**, and that page included a sample IAM policy for implementing the pattern. The combination of the new multi-MFA feature created an inconsistency with that approach. Given the new feature, a user with only AK/SK credentials could add an additional MFA without using a previously-configured MFA, thus allowing possession of AK/SK alone without a previously configured MFA to potentially gain broader access than expected by customers using the sample policy.

This issue did not affect AWS Management Console-based access, since an existing MFA is always required at sign-in. Nor did it affect federated principals, who manage MFA through their identity provider.

As of April 21, 2023, the identified issue has been remediated by requiring that IAM users who already have one or more MFAs and who use AK/SK credentials to manage their own MFA devices to first use sts:GetSessionToken and an existing MFA to obtain MFA-enabled temporary credentials to sign their CLI commands or API requests prior to enabling or disabling MFA devices for themselves. We have directly notified a very small number of customers via their Personal Health Dashboard who had previously associated an additional MFA device using a mechanism other than the AWS Management Console. We recommended that those notified customers confirm the correctness of their MFA configurations. No further customer action is required.

We would like to thank researchers at MWR Cybersec for identifying and responsibly disclosing this issue to AWS. Security-related questions or concerns can be brought to our attention via **aws-security@amazon.com**.  

Go to Source
