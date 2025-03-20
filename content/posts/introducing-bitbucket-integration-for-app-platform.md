---
title: "<div>Introducing Bitbucket Integration for App Platform</div>"
date: 2025-03-19
---

DigitalOcean’s App Platform, our Platform as a Service offering, provides seamless integration with major Git providers, enabling developers to deploy applications directly from their repositories. App Platform can automatically analyze and build code from GitHub, GitLab, and public Git repositories. This integration allows developers to focus on writing code while App Platform handles the infrastructure management and deployment process.

We are excited to add Bitbucket Cloud to the list of our supported Git providers. Bitbucket is a widely recognized and used source code management tool for both large and small-scale projects, and this integration with Bitbucket Cloud now enables users to create an app and re-deploy it automatically when an update is made to the branch containing the source code.

![image alt text](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/EngineeringBlogImages_Grace/bitbucket%20image%20one.png)

Let’s dive into some technical aspects of Bitbucket integration.

## Git provider permissions

Like GitHub and GitLab, the integration with Bitbucket is implemented using OAuth 2.0 authentication flow. We create a Bitbucket OAuth 2.0 consumer app managed by App Platform and request users to grant permissions to this consumer app. This ensures that we have the correct set of scope and permissions to interact with Bitbucket APIs on behalf of the user. We only ask for minimum required permissions, which include:

- Read access to repositories
- Read access to account information
- Read and write access to webhooks

OAuth 2.0 access tokens are used for authenticating with the Bitbucket APIs as well as cloning a repository in order to build and deploy an application. We have token refresh mechanisms to maintain continuous access.

## API Integration

App Platform’s backend integrates with Bitbucket’s RESTful APIs 2.0. Some of the APIs that we make use of are:

- List user’s workspaces
- List repositories in a workspace and branches in a repository. Note that we list only the repositories that a user has admin access to, since a user is not permitted to create webhooks without it.
- Fetch latest commits from a branch
- Fetch general user information like name and email
- List webhooks and create webhook in a repository

### Webhooks

Webhooks are callback APIs that are triggered by specific events. App Platform creates a webhook on a source code repository with trigger set to repository push events. Once configured, Bitbucket sends an HTTP Post request to this configured URL, delivering a payload with information about the push event. This information helps us automate the process of deploying source code from Bitbucket to your applications.

![image alt text](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/EngineeringBlogImages_Grace/bitbucket%20image%202.png)

You can see the configured **App Platform Integration Webhook** in your repository by navigating to Repository Settings -> Webhooks.

## Other Considerations

To maximize the efficiency and effectiveness of our interactions with the Bitbucket API, we have incorporated standard best practices.

### Rate Limiting

Bitbucket imposes rate limits on API requests to ensure fair usage and prevent abuse. We handle these limits by implementing strategies like retries with exponential backoff and jitter when receiving 429 status codes. We also use appropriate page size limits to minimize the number of requests.

### Security

Standard measures like using HTTPS to help ensure secure data transmission and storing encrypted tokens in the database. We only ask for bare minimum permissions to be granted by the user which includes read-only access to repositories. The webhook deliveries are also secured by means of signature verification to help ensure that our service only processes deliveries from Bitbucket Cloud.

### Multi-Provider Limits

A single DigitalOcean team member can now:

- Link multiple service accounts from different Git providers simultaneously.
- Cannot link multiple accounts from the same provider. Must remove existing connection before adding a new one from the same provider.

## Conclusion

With the addition of Bitbucket, App Platform now provides support for most of the popular Git version control systems, thus enabling seamless code deployment from repositories. To learn more, read the docs and let us know what you think!

Go to Source
