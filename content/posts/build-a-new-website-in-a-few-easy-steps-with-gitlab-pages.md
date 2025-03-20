---
title: "<div>Build a new website in a few easy steps with GitLab Pages</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

A personal website is more than just a utility for digital creators and professionals in tech. It's a representation of your brand. But creating one from scratch can be time-consuming and expensive.

With GitLab Pages, you can host your website with built-in features, including SSL certificates and a GitLab-provided domain. All of this is available on GitLab's free tier, making it an efficient solution for hosting your professional presence.

We're going to take you on a fun journey to craft a stunning personal website using GitLab Pages! We’ve got a super simple, versatile template that you can easily jazz up to reflect your unique style. So grab your favorite snack, get comfy, and let’s turn your online presence into something truly fabulous!

## Prerequisites

You will need the following prerequisites before getting started:

- A GitLab account (the free tier is sufficient)
- Basic familiarity with HTML/CSS
- Content and images you want to add to your website (optional)

Once you’re set up with a GitLab account and have your content handy, you can move on to the next steps.

## Step 1: Create a new project

1. Sign on to your GitLab account and create a project.

![GitLab Pages tutorial - welcome screen](https://images.ctfassets.net/r9o86ar0p03f/4dl3nWj98rU70tbmPvpjvz/36581e3c9205adb2be77a0ff199bfb61/Capture-2025-02-27-183716.png)

2. Click **Create blank project**.

![GitLab Pages tutorial - Create new project screen](https://images.ctfassets.net/r9o86ar0p03f/5ohC1jCUPEPoJjPhuZ8Zck/8f46fa26f91b8308cdcb9ac691bb1b66/Capture-2025-02-27-183814.png)

3. Fill in your project details:
    - Name your project `yourusername.gitlab.io`. Replace `yourusername` with your GitLab username. **Tip:** The project name determines your website’s URL. If you name your project `yourusername.gitlab.io`, your website will be available at `https://yourusername.gitlab.io` with no additional path. However, if you use any other project name, your site will be available at `https://yourusername.gitlab.io/project-name`.
    - Make the project public.
4. Click **Create project**.

![GitLab Pages tutorial - Create blank project screen](https://images.ctfassets.net/r9o86ar0p03f/7vA7DCeiekORoxaBW8u1ux/540cc127612cf853945c78239170ab9b/image5.png)

![GitLab Pages tutorial - customized get started page](https://images.ctfassets.net/r9o86ar0p03f/XOxKyJbbv4zZMvsS6smxa/af54d29c7dc3e7ae8cababa1766ad1d3/image2.png)

## Step 2: Add the template files

Start by creating two new files in your repository:

![GitLab Pages tutorial - Add new files to personal page](https://images.ctfassets.net/r9o86ar0p03f/3qE0dXdnvcvJov81eUQ5m6/55f55e483aa80b10704fb5dfab30d62a/image13.png)

1. First, create `index.html`:
    - In your project, click the **+** button and select **New file**.
    - Name the file `index.html`. ![GitLab Pages tutorial - new file page](https://images.ctfassets.net/r9o86ar0p03f/NOKS9Scmevhky1X7ptV2M/c809d34d0b8bfa7597dfb6dfa2fc9856/image14.png)
    - Add your HTML content.
        - Use the example HTML provided below. (Pro tip: Users can ask GitLab Duo Chat to generate HTML for enhanced functionality.)

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>[Your Name] · [Your Title]</title>
    <meta name="description" content="[Your Name] is a [Your Title]."/>
    <meta name="author" content="[Your Name]"/>
    <meta property="og:title" content="[Your Name]" />
    <meta property="og:description" content="[Your Title]" />
    <meta property="og:image" content="og.png" />
    <meta name="viewport" content="width=device-width,initial-scale=1"/>
    <link href="https://unpkg.com/basscss@8.0.2/css/basscss.min.css" rel="stylesheet">
    <link href="style.css" rel="stylesheet">
    <link rel="shortcut icon" type="image/png" href="favicon.png"/>
</head>
<body>
<div class="content" id="content">
  <div class="p2 sm-p4 mt2 sm-mt4 mb2 sm-mb4">  
  <div class="fade mt3">
    <a target="_new" href="[Your Linkedin URL]">
      <img class="photo" src="profile.png" width="64" height="64">
    </a>
  </div>
  <h2 class="mb0 mt4 fade">
    Hello, I'm [Your Name] 
    <span class="smallcaps">(</span>
    <a target="_new" href="[Your Linkedin URL]">@[Your Handle]</a>
    <span class="smallcaps">)</span>
  </h2>
  <h2 class="mt0 mb4 fade gray">
    I'm a [Your Title]
  </h2>
  <p class="mb4 fade">
    I'm a [Your Role] at [Your Company], [Brief company description].
  </p>
  <div class="fade">
    <p class="fade mb4">
      Your personal statement about what you do and what you're interested in. Add your contact preferences here.
    </p>
  </div>
  <p class="fade mb4">
    <span class="gray">—</span> 
    [Your Name] 
    <span class="smallcaps>(</span>
    <a target="_new" href="[Your Linkedin URL]">@[Your Handle]</a>
    <span class="smallcaps">)</span>
  </p>
  </div>
</div>
</body>
</html> 
```

- Add a commit message (e.g., "Added index.html").
    - Click **Commit changes**.

2. Create `style.css` (follow same steps above).

```
body {
  margin: 0;
  padding: 0;
  background: #000;
  color: #f4f4f4;
  font-family: "Graphik Web", system-ui, -apple-system, BlinkMacSystemFont, "Helvetica Neue", "Helvetica", "Segoe UI", Roboto, Ubuntu, sans-serif;
  font-weight: 400;
  font-smooth: antialiased;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

a {
  color: #ff310a;
  text-decoration: none;
}

a:hover {
  color: #CFEF54
}

.content {
  max-width: 40rem;
  margin: 0 auto;
}

img.photo {
  border-radius: 50%;
}

p {
  font-size: 1.5rem;
  line-height: 1.4;
  margin: 0;
  letter-spacing: -0.05rem;
}

h2 {
  font-weight: 400;
  line-height: 1.3;
  letter-spacing: -0.05rem;
}

.smallcaps {
  font-variant: small-caps;
  color:#333;
}

.gray{
  color: #999;
}

.preloader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  height: -moz-available;
  height: -webkit-fill-available;
  height: fill-available;
  width: 100%;
  background: #000;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  transition: opacity 0.3s linear;
  transform: translate3d(0, 0, 0);
}

body.loaded .preloader {
  opacity: 0;
}

.fade {
  animation: fadeIn 1s ease-in-out both;
}

.fade:nth-child(2) {
	animation-delay: 1s;
}

.fade:nth-child(3) {
	animation-delay: 2s;
}

.fade:nth-child(4) {
	animation-delay: 3s;
}

.fade:nth-child(5) {
	animation-delay: 4s;
}

.fade:nth-child(6) {
	animation-delay: 5s;
}

.fade:nth-child(7) {
	animation-delay: 6s;
}

.fade:nth-child(8) {
	animation-delay: 7s;
}

.fade:nth-child(9) {
	animation-delay: 8s;
}

.fade:nth-child(10) {
	animation-delay: 9s;
}

.fade:nth-child(11) {
	animation-delay: 10s;
}

.fade:nth-child(12) {
	animation-delay: 11s;
}

.fade:nth-child(13) {
	animation-delay: 12s;
}

@keyframes fadeIn {
	from {
		opacity: 0;
		transform: translate3d(0, 0%, 0);
	}
	to {
		opacity: 1;
		transform: translate3d(0, 0, 0);
	}
} 

```

## Step 3: Configure GitLab CI file

There are two ways to create the GitLab CI configuration file that tells GitLab how to build and deploy your site:

![GitLab Pages tutorial - optimize your workflow with CI/CD pipelines screen](https://images.ctfassets.net/r9o86ar0p03f/2LcdHfxew2tIeFmOx7fRZZ/5edd324075a5dfadf4e476ba64b1f22e/image3.png)

**Option 1: Use Pipeline Editor (recommended)**

1. Go to your project's **Build > Pipeline Editor**.

![GitLab Pages tutorial - pipeline editor/main branch](https://images.ctfassets.net/r9o86ar0p03f/7wK5SOd7S9CZlofeDmHjq7/895333b4c644f94cdfe38c0454f6661f/image12.png)

2. The `.gitlab-ci.yml` file will be automatically created.
3. Copy and paste the following configuration:

```
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```

![GitLab Pages Tutorial - New file in window](https://images.ctfassets.net/r9o86ar0p03f/1bzQo7okzBVgNZ2SSj8l95/31edb2e98096bae3e8b0c1ff349cb59b/image4.png)

**Option 2: Manual creation**

If you prefer to create the file manually:

1. Create a new file named `.gitlab-ci.yml`.
2. Add the following configuration:

```
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```

The key to getting your site running is the GitLab CI configuration file. This file tells GitLab how to build and deploy your site.

Let's break down what each part does:

**The script part**

```
script:
  - mkdir .public
  - cp -r * .public
  - mv .public public
```

This creates a folder called `public` and copies all your website files into it. GitLab Pages uses this folder to serve your website by default, though you can customize the publishing folder if needed.

**The only part**

```
only:
  - main

```

This tells GitLab to only update your website when changes are made to the main branch. This helps prevent accidental updates from experimental changes.

## Step 4: Watch the magic happen

1. Commit all your changes.
2. Go to **Build > Pipelines** to watch your deployment.
3. Wait for the pipeline to complete successfully (indicated by a green checkmark).

![GitLab Pages tutorial - pipeline running for new page](https://images.ctfassets.net/r9o86ar0p03f/3TWqK19gKq5HvjlEtRqHv5/dd79b1909720c1f923ba7eb48433da18/image6.png)

![GitLab Pages tutorial - pipeline passed for new page](https://images.ctfassets.net/r9o86ar0p03f/45Ue4IruelSnpSAVomoaxk/411fe710e2d8c4c190d3edaa6df45ee6/image1.png)

## Step 5: Access your website

Once the pipeline completes successfully, your website will be available at: **https://\[yourusername\].gitlab.io/** .

You can find an overview of your deployed website and additional settings in your project's **Deploy > Pages** section. Here you'll find useful information. including:

- Your website's access URLs
- Domain settings
    - By default GitLab enables **Unique domain**. Make sure to disable it if you want to use the GitLab-provided domain. Learn more with the unique domain documentation.
- HTTPS certificates status
- Recent deployments
- Additional configuration options
- Custom domains

This section is particularly helpful when setting up custom domains or troubleshooting deployment issues.

**Customize your site**

![GitLab Pages tutorial - customize site](https://images.ctfassets.net/r9o86ar0p03f/4724zwWSfWACRZgHCI1YCb/af53a7a74c0a4520a07b3cd928aa42b6/image8.png)

1. Replace all “Your ...” placeholders in `index.html` with your information.

![GitLab Pages tutorial - upload file to customize page](https://images.ctfassets.net/r9o86ar0p03f/3nBr11ZXG8AHyT3kmFlHkl/fca01562113e48f20371fa3d08233187/image11.png)

2. Add your images:
    - profile.png - your profile photo (64x64px)
    - favicon.png - your site favicon (32x32px)
    - Og.png - OpenGraph image for social media preview (1200x630px)

**See it in action**

If you're familiar with GitLab, feel free to fork my repository to get started quickly.

Here is the final result: https://fracazo.gitlab.io/

**Common issues and solutions**

- By default, GitLab enables "Unique domain" for Pages projects. To use the simpler GitLab-provided domain (like `username.gitlab.io`), go to **Deploy > Pages** and disable the "Use unique domain" option. While unique domains offer some technical advantages, like better asset path handling, you might prefer the cleaner URL structure for a personal website.
- If your pipeline fails, check that you're using `main` instead of `master` in your `.gitlab-ci.yml` file.
- Ensure your group and project is public for GitLab Pages to work.
- If any jobs fail in your pipeline, you can check the job log for detailed error messages to help with troubleshooting.

With GitLab Pages and this template, you can have a professional/personal website up and running in minutes. The template is clean, responsive, and easy to customize. As you grow professionally, you can easily update your site directly through GitLab.

You can automate the deployment process by leveraging GitLab's CI/CD capabilities and focusing on creating great content.

The best part? All of this is available on GitLab's free tier, making it an excellent option for free hosting of your personal projects, documentation sites, or even small business websites. For more advanced features and configurations, check out our Pages documentation.

## What’s next for GitLab Pages?

We're constantly working to make GitLab Pages even better for creators and developers. Here are some exciting improvements coming soon:

### Simplified domain management

We have some exciting updates coming to GitLab Pages that will make managing your domains even easier and more fun! You can look forward to a streamlined dashboard that brings all your domain settings together in one friendly space, making everything easily accessible.

You’ll stay informed with real-time updates on your DNS and SSL certificate statuses, helping you keep your domains secure and running smoothly.

### Custom domain setup

Setting up custom domains will be a breeze with our easy-to-follow process, guiding you every step of the way. Plus, you'll be able to set up your custom domains to automatically redirect visitors from your old website address to your new one – perfect for when you want all your traffic to go to one main website. Learn more about custom domains.

> Get started with GitLab Pages today with GitLab's free tier!

## Learn more

- GitLab Pages features review apps and multiple website deployment
- GitLab Pages: Multiple website deployment documentation
- GitLab Pages examples

A personal website is more than just a utility for digital creators and professionals in tech. It's a representation of your brand. But creating one from scratch can be time-consuming and expensive.

With GitLab Pages, you can host your website with built-in features, including SSL certificates and a GitLab-provided domain. All of this is available on GitLab's free tier, making it an efficient solution for hosting your professional presence.

We're going to take you on a fun journey to craft a stunning personal website using GitLab Pages! We’ve got a super simple, versatile template that you can easily jazz up to reflect your unique style. So grab your favorite snack, get comfy, and let’s turn your online presence into something truly fabulous!

## Prerequisites

You will need the following prerequisites before getting started:

- A GitLab account (the free tier is sufficient)
- Basic familiarity with HTML/CSS
- Content and images you want to add to your website (optional)

Once you’re set up with a GitLab account and have your content handy, you can move on to the next steps.

## Step 1: Create a new project

1. Sign on to your GitLab account and create a project.

![GitLab Pages tutorial - welcome screen](https://images.ctfassets.net/r9o86ar0p03f/4dl3nWj98rU70tbmPvpjvz/36581e3c9205adb2be77a0ff199bfb61/Capture-2025-02-27-183716.png)

2. Click **Create blank project**.

![GitLab Pages tutorial - Create new project screen](https://images.ctfassets.net/r9o86ar0p03f/5ohC1jCUPEPoJjPhuZ8Zck/8f46fa26f91b8308cdcb9ac691bb1b66/Capture-2025-02-27-183814.png)

3. Fill in your project details:
    - Name your project `yourusername.gitlab.io`. Replace `yourusername` with your GitLab username. **Tip:** The project name determines your website’s URL. If you name your project `yourusername.gitlab.io`, your website will be available at `https://yourusername.gitlab.io` with no additional path. However, if you use any other project name, your site will be available at `https://yourusername.gitlab.io/project-name`.
    - Make the project public.
4. Click **Create project**.

![GitLab Pages tutorial - Create blank project screen](https://images.ctfassets.net/r9o86ar0p03f/7vA7DCeiekORoxaBW8u1ux/540cc127612cf853945c78239170ab9b/image5.png)

![GitLab Pages tutorial - customized get started page](https://images.ctfassets.net/r9o86ar0p03f/XOxKyJbbv4zZMvsS6smxa/af54d29c7dc3e7ae8cababa1766ad1d3/image2.png)

## Step 2: Add the template files

Start by creating two new files in your repository:

![GitLab Pages tutorial - Add new files to personal page](https://images.ctfassets.net/r9o86ar0p03f/3qE0dXdnvcvJov81eUQ5m6/55f55e483aa80b10704fb5dfab30d62a/image13.png)

1. First, create `index.html`:
    - In your project, click the **+** button and select **New file**.
    - Name the file `index.html`. ![GitLab Pages tutorial - new file page](https://images.ctfassets.net/r9o86ar0p03f/NOKS9Scmevhky1X7ptV2M/c809d34d0b8bfa7597dfb6dfa2fc9856/image14.png)
    - Add your HTML content.
        - Use the example HTML provided below. (Pro tip: Users can ask GitLab Duo Chat to generate HTML for enhanced functionality.)

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>[Your Name] · [Your Title]</title>
    <meta name="description" content="[Your Name] is a [Your Title]."/>
    <meta name="author" content="[Your Name]"/>
    <meta property="og:title" content="[Your Name]" />
    <meta property="og:description" content="[Your Title]" />
    <meta property="og:image" content="og.png" />
    <meta name="viewport" content="width=device-width,initial-scale=1"/>
    <link href="https://unpkg.com/basscss@8.0.2/css/basscss.min.css" rel="stylesheet">
    <link href="style.css" rel="stylesheet">
    <link rel="shortcut icon" type="image/png" href="favicon.png"/>
</head>
<body>
<div class="content" id="content">
  <div class="p2 sm-p4 mt2 sm-mt4 mb2 sm-mb4">  
  <div class="fade mt3">
    <a target="_new" href="[Your Linkedin URL]">
      <img class="photo" src="profile.png" width="64" height="64">
    </a>
  </div>
  <h2 class="mb0 mt4 fade">
    Hello, I'm [Your Name] 
    <span class="smallcaps">(</span>
    <a target="_new" href="[Your Linkedin URL]">@[Your Handle]</a>
    <span class="smallcaps">)</span>
  </h2>
  <h2 class="mt0 mb4 fade gray">
    I'm a [Your Title]
  </h2>
  <p class="mb4 fade">
    I'm a [Your Role] at [Your Company], [Brief company description].
  </p>
  <div class="fade">
    <p class="fade mb4">
      Your personal statement about what you do and what you're interested in. Add your contact preferences here.
    </p>
  </div>
  <p class="fade mb4">
    <span class="gray">—</span> 
    [Your Name] 
    <span class="smallcaps>(</span>
    <a target="_new" href="[Your Linkedin URL]">@[Your Handle]</a>
    <span class="smallcaps">)</span>
  </p>
  </div>
</div>
</body>
</html> 
```

- Add a commit message (e.g., "Added index.html").
    - Click **Commit changes**.

2. Create `style.css` (follow same steps above).

```
body {
  margin: 0;
  padding: 0;
  background: #000;
  color: #f4f4f4;
  font-family: "Graphik Web", system-ui, -apple-system, BlinkMacSystemFont, "Helvetica Neue", "Helvetica", "Segoe UI", Roboto, Ubuntu, sans-serif;
  font-weight: 400;
  font-smooth: antialiased;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

a {
  color: #ff310a;
  text-decoration: none;
}

a:hover {
  color: #CFEF54
}

.content {
  max-width: 40rem;
  margin: 0 auto;
}

img.photo {
  border-radius: 50%;
}

p {
  font-size: 1.5rem;
  line-height: 1.4;
  margin: 0;
  letter-spacing: -0.05rem;
}

h2 {
  font-weight: 400;
  line-height: 1.3;
  letter-spacing: -0.05rem;
}

.smallcaps {
  font-variant: small-caps;
  color:#333;
}

.gray{
  color: #999;
}

.preloader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  height: -moz-available;
  height: -webkit-fill-available;
  height: fill-available;
  width: 100%;
  background: #000;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  transition: opacity 0.3s linear;
  transform: translate3d(0, 0, 0);
}

body.loaded .preloader {
  opacity: 0;
}

.fade {
  animation: fadeIn 1s ease-in-out both;
}

.fade:nth-child(2) {
	animation-delay: 1s;
}

.fade:nth-child(3) {
	animation-delay: 2s;
}

.fade:nth-child(4) {
	animation-delay: 3s;
}

.fade:nth-child(5) {
	animation-delay: 4s;
}

.fade:nth-child(6) {
	animation-delay: 5s;
}

.fade:nth-child(7) {
	animation-delay: 6s;
}

.fade:nth-child(8) {
	animation-delay: 7s;
}

.fade:nth-child(9) {
	animation-delay: 8s;
}

.fade:nth-child(10) {
	animation-delay: 9s;
}

.fade:nth-child(11) {
	animation-delay: 10s;
}

.fade:nth-child(12) {
	animation-delay: 11s;
}

.fade:nth-child(13) {
	animation-delay: 12s;
}

@keyframes fadeIn {
	from {
		opacity: 0;
		transform: translate3d(0, 0%, 0);
	}
	to {
		opacity: 1;
		transform: translate3d(0, 0, 0);
	}
} 

```

## Step 3: Configure GitLab CI file

There are two ways to create the GitLab CI configuration file that tells GitLab how to build and deploy your site:

![GitLab Pages tutorial - optimize your workflow with CI/CD pipelines screen](https://images.ctfassets.net/r9o86ar0p03f/2LcdHfxew2tIeFmOx7fRZZ/5edd324075a5dfadf4e476ba64b1f22e/image3.png)

**Option 1: Use Pipeline Editor (recommended)**

1. Go to your project's **Build > Pipeline Editor**.

![GitLab Pages tutorial - pipeline editor/main branch](https://images.ctfassets.net/r9o86ar0p03f/7wK5SOd7S9CZlofeDmHjq7/895333b4c644f94cdfe38c0454f6661f/image12.png)

2. The `.gitlab-ci.yml` file will be automatically created.
3. Copy and paste the following configuration:

```
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```

![GitLab Pages Tutorial - New file in window](https://images.ctfassets.net/r9o86ar0p03f/1bzQo7okzBVgNZ2SSj8l95/31edb2e98096bae3e8b0c1ff349cb59b/image4.png)

**Option 2: Manual creation**

If you prefer to create the file manually:

1. Create a new file named `.gitlab-ci.yml`.
2. Add the following configuration:

```
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - main
```

The key to getting your site running is the GitLab CI configuration file. This file tells GitLab how to build and deploy your site.

Let's break down what each part does:

**The script part**

```
script:
  - mkdir .public
  - cp -r * .public
  - mv .public public
```

This creates a folder called `public` and copies all your website files into it. GitLab Pages uses this folder to serve your website by default, though you can customize the publishing folder if needed.

**The only part**

```
only:
  - main

```

This tells GitLab to only update your website when changes are made to the main branch. This helps prevent accidental updates from experimental changes.

## Step 4: Watch the magic happen

1. Commit all your changes.
2. Go to **Build > Pipelines** to watch your deployment.
3. Wait for the pipeline to complete successfully (indicated by a green checkmark).

![GitLab Pages tutorial - pipeline running for new page](https://images.ctfassets.net/r9o86ar0p03f/3TWqK19gKq5HvjlEtRqHv5/dd79b1909720c1f923ba7eb48433da18/image6.png)

![GitLab Pages tutorial - pipeline passed for new page](https://images.ctfassets.net/r9o86ar0p03f/45Ue4IruelSnpSAVomoaxk/411fe710e2d8c4c190d3edaa6df45ee6/image1.png)

## Step 5: Access your website

Once the pipeline completes successfully, your website will be available at: **https://\[yourusername\].gitlab.io/** .

You can find an overview of your deployed website and additional settings in your project's **Deploy > Pages** section. Here you'll find useful information. including:

- Your website's access URLs
- Domain settings
    - By default GitLab enables **Unique domain**. Make sure to disable it if you want to use the GitLab-provided domain. Learn more with the unique domain documentation.
- HTTPS certificates status
- Recent deployments
- Additional configuration options
- Custom domains

This section is particularly helpful when setting up custom domains or troubleshooting deployment issues.

**Customize your site**

![GitLab Pages tutorial - customize site](https://images.ctfassets.net/r9o86ar0p03f/4724zwWSfWACRZgHCI1YCb/af53a7a74c0a4520a07b3cd928aa42b6/image8.png)

1. Replace all “Your ...” placeholders in `index.html` with your information.

![GitLab Pages tutorial - upload file to customize page](https://images.ctfassets.net/r9o86ar0p03f/3nBr11ZXG8AHyT3kmFlHkl/fca01562113e48f20371fa3d08233187/image11.png)

2. Add your images:
    - profile.png - your profile photo (64x64px)
    - favicon.png - your site favicon (32x32px)
    - Og.png - OpenGraph image for social media preview (1200x630px)

**See it in action**

If you're familiar with GitLab, feel free to fork my repository to get started quickly.

Here is the final result: https://fracazo.gitlab.io/

**Common issues and solutions**

- By default, GitLab enables "Unique domain" for Pages projects. To use the simpler GitLab-provided domain (like `username.gitlab.io`), go to **Deploy > Pages** and disable the "Use unique domain" option. While unique domains offer some technical advantages, like better asset path handling, you might prefer the cleaner URL structure for a personal website.
- If your pipeline fails, check that you're using `main` instead of `master` in your `.gitlab-ci.yml` file.
- Ensure your group and project is public for GitLab Pages to work.
- If any jobs fail in your pipeline, you can check the job log for detailed error messages to help with troubleshooting.

With GitLab Pages and this template, you can have a professional/personal website up and running in minutes. The template is clean, responsive, and easy to customize. As you grow professionally, you can easily update your site directly through GitLab.

You can automate the deployment process by leveraging GitLab's CI/CD capabilities and focusing on creating great content.

The best part? All of this is available on GitLab's free tier, making it an excellent option for free hosting of your personal projects, documentation sites, or even small business websites. For more advanced features and configurations, check out our Pages documentation.

## What’s next for GitLab Pages?

We're constantly working to make GitLab Pages even better for creators and developers. Here are some exciting improvements coming soon:

### Simplified domain management

We have some exciting updates coming to GitLab Pages that will make managing your domains even easier and more fun! You can look forward to a streamlined dashboard that brings all your domain settings together in one friendly space, making everything easily accessible.

You’ll stay informed with real-time updates on your DNS and SSL certificate statuses, helping you keep your domains secure and running smoothly.

### Custom domain setup

Setting up custom domains will be a breeze with our easy-to-follow process, guiding you every step of the way. Plus, you'll be able to set up your custom domains to automatically redirect visitors from your old website address to your new one – perfect for when you want all your traffic to go to one main website. Learn more about custom domains.

> Get started with GitLab Pages today with GitLab's free tier!

## Learn more

- GitLab Pages features review apps and multiple website deployment
- GitLab Pages: Multiple website deployment documentation
- GitLab Pages examples

Go to Source
