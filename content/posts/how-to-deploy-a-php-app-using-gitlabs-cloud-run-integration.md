---
title: "<div>How to deploy a PHP app using GitLab's Cloud Run integration</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Writing PHP application code and ensuring the application is running smoothly in production are often two different skills sets owned by two different engineers. GitLab aims to bridge the gap by enabling the engineer who has written the PHP application code to also deploy it into Google Cloud Platform with little effort.

Whether you own event-driven, long-running services or deploy containerized jobs to process data, Google Cloud Run automatically scales your containers up and down from zero — this means you only pay when your code is running.

If you are a PHP developer who would like to deploy your application with minimal effort to Google Cloud Platform, this guide will show you how using the GitLab Google Cloud Run integration.

# Overview

- Create a new project in GitLab
- Set up your PHP application
- Utilizing the Google Cloud integration, create a Service account
- Utilizing the Google Cloud integration, configure Cloud Run via merge request
- Try adding another endpoint
- Clean up

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of PHP, an open-source, general-purpose scripting language
- Working knowledge of GitLab CI
- 10 minutes

## 1\. Create a new project in GitLab.

We decided to call our project `PHP cloud-run` for simplicity.

![PHP cloud- run project](https://images.ctfassets.net/r9o86ar0p03f/5zvHbq2hYShOUa0eDpAj7u/a239eca41ebe546dde43e62f5a339d67/image4.png)

Then, create an index.php apphttps://gitlab.com/demos/templates/php-cloud-run/-/blob/main/index.php.

```php
<?php

$name = getenv('NAME', true) ?: 'World';
echo sprintf('Hello %s!', $name);
```

## 2\. Utilizing the Google Cloud integration, create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Create Service account screen](https://images.ctfassets.net/r9o86ar0p03f/58NQZOmfYRSDda0bLXBwUm/dd614f7b14b84311e5d4147581107e93/image10.png)

Then configure the region you would like the Cloud Run instance deployed to.

![Configure region screen](https://images.ctfassets.net/r9o86ar0p03f/4c9wwYQ0Ed0BgFsi9zcv3J/5b68cb5b3d342cbb61b6317b65ead76f/image5.png)

## 3\. Utilizing the Google Cloud integration, configure **Cloud Run via merge request**.

![Deployment configuration screen](https://images.ctfassets.net/r9o86ar0p03f/7BP2tD3jdyDATOyG0AtuOg/21e47c833cf9bfb3861f900fb9267b49/image6.png)

This will open a merge request. Immediately merge this merge request.

![Enable Deployments to Cloud run screen](https://images.ctfassets.net/r9o86ar0p03f/2lkQJVDEWYwQ9ZVKlxb0i5/c22f6f50f5337ad232e95a969c003e23/image3.png)

**Note:** `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, and `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Variables screen](https://images.ctfassets.net/r9o86ar0p03f/6ZdVzLReOn0kb2OWuR8qW9/9c7c664835d41b9413067632db0ca53b/image1.png)

Check your pipeline and you will see you have successfully deployed to Google Cloud Run utilizing GitLab CI.

![merge branch screen](https://images.ctfassets.net/r9o86ar0p03f/5W2vK07Xpu9E9zIwgwsQbQ/2e3849bc7c34236c949b9185c31b8a98/image7.png)

<br></br>

![Google Cloud Run deployed with GitLab CI](https://images.ctfassets.net/r9o86ar0p03f/Oe93r0MQuh0o0pruk8SmG/8bf2707a861e991fb1d19e2c4d7492e2/image2.png)

## 4\. Click the **Service URL** to view your newly deployed Flask server.

In addition, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Environments screen](https://images.ctfassets.net/r9o86ar0p03f/1gAGuE8H4Mqs2kTaxKNirC/66d29efb8e271c4e9a8412714c01b0d7/image9.png)

By clicking on the environment called **main**, you’ll be able to view a complete list of deployments specific to that environment.

![Main environment](https://images.ctfassets.net/r9o86ar0p03f/7cDS1f5hT6D8Vfsnzk9Dvk/c51f203194c6d0a14a1a661541728633/image8.png)

## 5\. Add another endpoint

To get started with developing your PHP application, try adding another endpoint. For example, in your main file, you can add a `/bye` endpoint like this:

```

<?php

$name = getenv('NAME', true) ?: 'World';

if ($_SERVER['REQUEST_URI'] == '/bye') {
    echo sprintf('Goodbye %s!', $name);
} else {
    echo sprintf('Hello %s!', $name);
}

```

Push the changes to the repo, and watch the `deploy-to-cloud-run` job deploy the updates. Once the job is complete, go back to the Service URL and navigate to the `/bye` endpoint to see the new functionality in action.

### Clean up

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide here.

> Check out more easy-to-follow tutorials from our Solutions Architecture team.

Writing PHP application code and ensuring the application is running smoothly in production are often two different skills sets owned by two different engineers. GitLab aims to bridge the gap by enabling the engineer who has written the PHP application code to also deploy it into Google Cloud Platform with little effort.

Whether you own event-driven, long-running services or deploy containerized jobs to process data, Google Cloud Run automatically scales your containers up and down from zero — this means you only pay when your code is running.

If you are a PHP developer who would like to deploy your application with minimal effort to Google Cloud Platform, this guide will show you how using the GitLab Google Cloud Run integration.

# Overview

- Create a new project in GitLab
- Set up your PHP application
- Utilizing the Google Cloud integration, create a Service account
- Utilizing the Google Cloud integration, configure Cloud Run via merge request
- Try adding another endpoint
- Clean up

## Prerequisites

- Owner access on a Google Cloud Platform project
- Working knowledge of PHP, an open-source, general-purpose scripting language
- Working knowledge of GitLab CI
- 10 minutes

## 1\. Create a new project in GitLab.

We decided to call our project `PHP cloud-run` for simplicity.

![PHP cloud- run project](https://images.ctfassets.net/r9o86ar0p03f/5zvHbq2hYShOUa0eDpAj7u/a239eca41ebe546dde43e62f5a339d67/image4.png)

Then, create an index.php apphttps://gitlab.com/demos/templates/php-cloud-run/-/blob/main/index.php.

```php
<?php

$name = getenv('NAME', true) ?: 'World';
echo sprintf('Hello %s!', $name);
```

## 2\. Utilizing the Google Cloud integration, create a Service account.

Navigate to **Operate > Google Cloud > Create Service account**.

![Create Service account screen](https://images.ctfassets.net/r9o86ar0p03f/58NQZOmfYRSDda0bLXBwUm/dd614f7b14b84311e5d4147581107e93/image10.png)

Then configure the region you would like the Cloud Run instance deployed to.

![Configure region screen](https://images.ctfassets.net/r9o86ar0p03f/4c9wwYQ0Ed0BgFsi9zcv3J/5b68cb5b3d342cbb61b6317b65ead76f/image5.png)

## 3\. Utilizing the Google Cloud integration, configure **Cloud Run via merge request**.

![Deployment configuration screen](https://images.ctfassets.net/r9o86ar0p03f/7BP2tD3jdyDATOyG0AtuOg/21e47c833cf9bfb3861f900fb9267b49/image6.png)

This will open a merge request. Immediately merge this merge request.

![Enable Deployments to Cloud run screen](https://images.ctfassets.net/r9o86ar0p03f/2lkQJVDEWYwQ9ZVKlxb0i5/c22f6f50f5337ad232e95a969c003e23/image3.png)

**Note:** `GCP_PROJECT_ID`, `GCP_REGION`, `GCP_SERVICE_ACCOUNT`, and `GCP_SERVICE_ACCOUNT_KEY` will all be automatically populated from the previous steps.

![Variables screen](https://images.ctfassets.net/r9o86ar0p03f/6ZdVzLReOn0kb2OWuR8qW9/9c7c664835d41b9413067632db0ca53b/image1.png)

Check your pipeline and you will see you have successfully deployed to Google Cloud Run utilizing GitLab CI.

![merge branch screen](https://images.ctfassets.net/r9o86ar0p03f/5W2vK07Xpu9E9zIwgwsQbQ/2e3849bc7c34236c949b9185c31b8a98/image7.png)

<br></br>

![Google Cloud Run deployed with GitLab CI](https://images.ctfassets.net/r9o86ar0p03f/Oe93r0MQuh0o0pruk8SmG/8bf2707a861e991fb1d19e2c4d7492e2/image2.png)

## 4\. Click the **Service URL** to view your newly deployed Flask server.

In addition, you can navigate to **Operate > Environments** to see a list of deployments for your environments.

![Environments screen](https://images.ctfassets.net/r9o86ar0p03f/1gAGuE8H4Mqs2kTaxKNirC/66d29efb8e271c4e9a8412714c01b0d7/image9.png)

By clicking on the environment called **main**, you’ll be able to view a complete list of deployments specific to that environment.

![Main environment](https://images.ctfassets.net/r9o86ar0p03f/7cDS1f5hT6D8Vfsnzk9Dvk/c51f203194c6d0a14a1a661541728633/image8.png)

## 5\. Add another endpoint

To get started with developing your PHP application, try adding another endpoint. For example, in your main file, you can add a `/bye` endpoint like this:

```

<?php

$name = getenv('NAME', true) ?: 'World';

if ($_SERVER['REQUEST_URI'] == '/bye') {
    echo sprintf('Goodbye %s!', $name);
} else {
    echo sprintf('Hello %s!', $name);
}

```

Push the changes to the repo, and watch the `deploy-to-cloud-run` job deploy the updates. Once the job is complete, go back to the Service URL and navigate to the `/bye` endpoint to see the new functionality in action.

### Clean up

To prevent incurring charges on your Google Cloud account for the resources used in this tutorial, you can either delete the specific resources or delete the entire Google Cloud project. For detailed instructions, refer to the cleanup guide here.

> Check out more easy-to-follow tutorials from our Solutions Architecture team.

Go to Source
