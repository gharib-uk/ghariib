---
title: "<div>Deploy a NodeJS Express app with GitLab's Cloud Run integration</div>"
date: 2025-01-15
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Are you looking to deploy your NodeJS app to Google Cloud with the least maintenance possible? This tutorial will show you how to utilize GitLab’s Google Cloud integration to deploy your NodeJS app in less than 10 minutes.

Traditionally, deploying an application often requires assistance from production or DevOps engineers. This integration now empowers developers to handle deployments independently. Whether you’re a solo developer or part of a large team, this setup gives everyone the ability to deploy their applications efficiently.

## Overview

- Create a new project in GitLab
- Set up your NodeJS application
- Use the Google Cloud integration to create a Service account
- Use the Google Cloud integration to configure Cloud Run via Merge Request
- Enjoy your newly deployed Java Spring Boot app
- Follow the cleanup guide

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of JavaScript/TypeScript (not playing favorites here!)
- Working knowledge of GitLab CI
- 10 minutes

## Step-by-step guide

### 1\. Create a new project in GitLab

We decided to call our project `nodejs–express-cloud-run` for simplicity.

![Create a new project](https://images.ctfassets.net/r9o86ar0p03f/58du8DdOUEqAOpwTOkbHGQ/8a1a793aeae9babd9042c59ed2e13e71/image1.png)

### 2\. Upload your NodeJS app or use this example to get started.

Demo

**Note:** Make sure to include the `cloud-run` CI template within your project.

![cloud-run CI template include](https://images.ctfassets.net/r9o86ar0p03f/3BeRRhtMDzTMhrjCTcWwkw/bdebb221bf688c8a7810d1d7eeadba65/image6.png)

### 3\. Use the Google Cloud integration to create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Create Service account screen](https://images.ctfassets.net/r9o86ar0p03f/1nekQNPGU3RbPKQyUDwFsN/76c448239122fa0a9951544373395302/image10.png)

Also configure the region you would like the Cloud Run instance deployed to.

![Cloud Run instance deployment region selection](https://images.ctfassets.net/r9o86ar0p03f/2BRD9frHRugvFm54nGLC3J/8caa584233eb372b84df0f6f5611727d/image2.png)

### 4\. Go to the Deployments tab and use the Google Cloud integration to configure **Cloud Run via Merge Request**.

![Deployments - Configuration of Cloud Run via Merge Request](https://images.ctfassets.net/r9o86ar0p03f/4A78xJvstOZudEljO3dxFn/8f250dca0a8d57fc4c5256d14141e792/image5.png)

This will open a merge request – immediately merge it.

![Merge request for deployment](https://images.ctfassets.net/r9o86ar0p03f/7l7jg8eL9bAWwOvnH9w4JY/8fa24ef53d76ff4e806b8dcb3388bfe6/image8.png)

**Note:** `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, and `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Variables listing](https://images.ctfassets.net/r9o86ar0p03f/2c4MdEkUU0ygyB1bosE5HP/b630fb43eaac5aafa4ae519baf332907/image4.png)

### 5\. Voila! Check your pipeline and you will see you have successfully deployed to Google Cloud Run using GitLab CI.

![Successful deployment to Google Cloud Run](https://images.ctfassets.net/r9o86ar0p03f/2qyph9FGS71wGpMt2eJ7ZX/c2c13275937c2576d7dffd143203b430/image3.png)

Click the Service URL to view your newly deployed Node server.

![View newly deployed Node server](https://images.ctfassets.net/r9o86ar0p03f/36o64dX1H8ijuNbpiXCf9L/3ada79b96d570dbd57739c41c1324368/image7.png)

In addition, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Environments view of deployment list](https://images.ctfassets.net/r9o86ar0p03f/5M8Iy1yxkF3vatHc6htkBV/5eacb4762df8183a8eb07514c7dbaa12/image12.png)

By clicking on the environment called `main`, you’ll be able to view a complete list of deployments specific to that environment.

![Main view of deployments to specific environment](https://images.ctfassets.net/r9o86ar0p03f/6byXvhfYCnbysIlMZ48Ejz/41aab96d281a43d75409636c449ec396/image9.png)

### 6\. Next steps

To get started with developing your Node application, try adding another endpoint. For instance, in your `index.js` file, you can add a **/bye** endpoint as shown below:

```
app.get('/bye', (req, res) => {
  res.send(`Have a great day! See you!`);
});

```

Push the changes to the repo, and watch the `deploy-to-cloud-run` job deploy the updates. Once it’s complete, go back to the Service URL and navigate to the **/bye** endpoint to see the new functionality in action.

![Bye message](https://images.ctfassets.net/r9o86ar0p03f/1Wao8In9CGlte4CtnzzRPm/607a3c11a070a57815ad05b6988245ef/image11.png)

## Follow the cleanup guide

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide here.

> Read more of these helpful tutorials from GitLab solutions architects.

Are you looking to deploy your NodeJS app to Google Cloud with the least maintenance possible? This tutorial will show you how to utilize GitLab’s Google Cloud integration to deploy your NodeJS app in less than 10 minutes.

Traditionally, deploying an application often requires assistance from production or DevOps engineers. This integration now empowers developers to handle deployments independently. Whether you’re a solo developer or part of a large team, this setup gives everyone the ability to deploy their applications efficiently.

## Overview

- Create a new project in GitLab
- Set up your NodeJS application
- Use the Google Cloud integration to create a Service account
- Use the Google Cloud integration to configure Cloud Run via Merge Request
- Enjoy your newly deployed Java Spring Boot app
- Follow the cleanup guide

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of JavaScript/TypeScript (not playing favorites here!)
- Working knowledge of GitLab CI
- 10 minutes

## Step-by-step guide

### 1\. Create a new project in GitLab

We decided to call our project `nodejs–express-cloud-run` for simplicity.

![Create a new project](https://images.ctfassets.net/r9o86ar0p03f/58du8DdOUEqAOpwTOkbHGQ/8a1a793aeae9babd9042c59ed2e13e71/image1.png)

### 2\. Upload your NodeJS app or use this example to get started.

Demo

**Note:** Make sure to include the `cloud-run` CI template within your project.

![cloud-run CI template include](https://images.ctfassets.net/r9o86ar0p03f/3BeRRhtMDzTMhrjCTcWwkw/bdebb221bf688c8a7810d1d7eeadba65/image6.png)

### 3\. Use the Google Cloud integration to create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Create Service account screen](https://images.ctfassets.net/r9o86ar0p03f/1nekQNPGU3RbPKQyUDwFsN/76c448239122fa0a9951544373395302/image10.png)

Also configure the region you would like the Cloud Run instance deployed to.

![Cloud Run instance deployment region selection](https://images.ctfassets.net/r9o86ar0p03f/2BRD9frHRugvFm54nGLC3J/8caa584233eb372b84df0f6f5611727d/image2.png)

### 4\. Go to the Deployments tab and use the Google Cloud integration to configure **Cloud Run via Merge Request**.

![Deployments - Configuration of Cloud Run via Merge Request](https://images.ctfassets.net/r9o86ar0p03f/4A78xJvstOZudEljO3dxFn/8f250dca0a8d57fc4c5256d14141e792/image5.png)

This will open a merge request – immediately merge it.

![Merge request for deployment](https://images.ctfassets.net/r9o86ar0p03f/7l7jg8eL9bAWwOvnH9w4JY/8fa24ef53d76ff4e806b8dcb3388bfe6/image8.png)

**Note:** `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, and `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Variables listing](https://images.ctfassets.net/r9o86ar0p03f/2c4MdEkUU0ygyB1bosE5HP/b630fb43eaac5aafa4ae519baf332907/image4.png)

### 5\. Voila! Check your pipeline and you will see you have successfully deployed to Google Cloud Run using GitLab CI.

![Successful deployment to Google Cloud Run](https://images.ctfassets.net/r9o86ar0p03f/2qyph9FGS71wGpMt2eJ7ZX/c2c13275937c2576d7dffd143203b430/image3.png)

Click the Service URL to view your newly deployed Node server.

![View newly deployed Node server](https://images.ctfassets.net/r9o86ar0p03f/36o64dX1H8ijuNbpiXCf9L/3ada79b96d570dbd57739c41c1324368/image7.png)

In addition, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Environments view of deployment list](https://images.ctfassets.net/r9o86ar0p03f/5M8Iy1yxkF3vatHc6htkBV/5eacb4762df8183a8eb07514c7dbaa12/image12.png)

By clicking on the environment called `main`, you’ll be able to view a complete list of deployments specific to that environment.

![Main view of deployments to specific environment](https://images.ctfassets.net/r9o86ar0p03f/6byXvhfYCnbysIlMZ48Ejz/41aab96d281a43d75409636c449ec396/image9.png)

### 6\. Next steps

To get started with developing your Node application, try adding another endpoint. For instance, in your `index.js` file, you can add a **/bye** endpoint as shown below:

```
app.get('/bye', (req, res) => {
  res.send(`Have a great day! See you!`);
});

```

Push the changes to the repo, and watch the `deploy-to-cloud-run` job deploy the updates. Once it’s complete, go back to the Service URL and navigate to the **/bye** endpoint to see the new functionality in action.

![Bye message](https://images.ctfassets.net/r9o86ar0p03f/1Wao8In9CGlte4CtnzzRPm/607a3c11a070a57815ad05b6988245ef/image11.png)

## Follow the cleanup guide

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide here.

> Read more of these helpful tutorials from GitLab solutions architects.

Go to Source
