---
title: "How to Unlock S3 Bucket Policy in a Organization Member Account"
date: 2025-01-17
---

While working on a POC I accidentally set a bucket policy like this one ...  

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Action": "s3:*",
        "Effect": "Deny",
        "Resource": [
            "arn:aws:s3:::dummybucket",
            "arn:aws:s3:::dummybucket/*"
        ],
        "Principal": "*"
    }]
}
```

Yeah ... That's the problem of copy&paste and a barely edit without double check. A policy that caused a bunch of errors and locked me out of the bucket, even though I had an AdministratorAccess policy.

Initially, I thought the fix would be easy—just delete the bucket policy using root access. But there was a catch, the bucket was in an account that’s part of an AWS Organization, and by default, member accounts don’t have root credentials.

After some research and trial and error, I found the solution. I’m sharing it here to save you some time if you ever find yourself in the same situation!

- Log in to the AWS Console using the management account ( the one that manages AWS Organization).
    
- Enable _Centralized root access for member accounts_ at IAM Console
    

![IAM Panel](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Foeyhuj3hd704ha1pyi72.png)

![Enabling Root Access](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpkjk69wqz28mrvkp44ns.png)

- After enabling root access, reload the page, select the account with the misconfigured bucket, and choose the Take Privileged Action option.

![Take privileged action](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fn77uwyd5oqixctwpr29b.png)

- Choose Delete Amazon S3 bucket policy, select the affected bucket, and remove the problematic policy.

![Delete Amazon S3 bucket policy](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fba21jvqb36fghnjd3kck.png)

![Confirm Delete Amazon S3 bucket policy](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ff4o8pmlpsdl9of2cyvq4.png)

And that's it, a simple and quick solution to what can be a headache.

_Optionally disable Centralized root access for member accounts_

Go to Source
