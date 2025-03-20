---
title: "<div>Getting started with GitLab: How to import your projects to GitLab</div>"
date: 2025-01-29
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

Knowing how to import your projects to GitLab is an essential skill to make the most of the GitLab DevSecOps platform. You’ve set up your account, invited users, and organized them based on your use case or team structure. Now, you need to bring your existing projects into GitLab and start collaborating. These projects can be local files on your computer or hosted on a different source code management platform. Let's explore the options.

## Importing local project files

You don't want to start from scratch every time you import a project. Follow these steps to get into GitLab existing legacy projects or applications that exist without version control or use version control.

### Git project

1. If Git is already initiated in your local project, create a new project in GitLab and obtain the SSH or HTTPS URL by clicking on the **Code** button in the top right corner of your project page.

![create a new project in GitLab with SSH/HTTPS URLs](https://images.ctfassets.net/r9o86ar0p03f/JodhM2vA2zOXV51EXkseM/9f5bf272ec0f6dbce4268a9d3e475bc1/image8.png)

2. Switch to your terminal and ensure you are in your project folder:

```bash
cd /project_folder  
```

3. Backup your existing Git origin:

```bash

git remote rename origin old-origin

```

4. Add the GitLab remote URL for the new origin, when using SSH:

```bash
git remote add origin [git@gitlab.com](mailto:git@gitlab.com):gitlab-da/playground/abubakar/new-test-repo.git  
```

And for HTTPS:

```bash
git remote add origin https://gitlab.com/gitlab-da/playground/abubakar/new-test-repo.git  
```

5. Then push all existing branches and tags to GitLab:

```bash
git push --set-upstream origin --all  
git push --set-upstream origin --tags  
```

All your file project files, branches, and tags will be pushed to GitLab and you can start collaborating.

### Non-Git project

Alternatively, if you have not initiated Git in your project, you will need to initialize Git, commit existing files, and push to GitLab as follows:

```bash
git init --initial-branch=main  
git remote add origin git@gitlab.com:gitlab-da/playground/abubakar/new-test-repo.git  
git add .  
git commit -m "Initial commit"  
git push --set-upstream origin main  
```

## Importing from online sources

If you have your project on GitLab.com or other platforms and you want to move it to another GitLab instance (like a self-managed instance) or from another platform to GitLab.com, GitLab provides the import project feature when you want to create a new project.

![Create a new project screen](https://images.ctfassets.net/r9o86ar0p03f/7wDEsJUuoNKGQ9GlfHfBOx/9e100dd74315db1ed5152ae8f94cff48/image7.png)

Importing a project migrates the project files and some other components of the project depending on the source. You can import from different sources like Bitbucket, GitHub, Gitea, and a GitLab instance, among other sources. Import sources are enabled by default on GitLab.com, but they need to be enabled for self-managed by an administrator. We will look at a few of these sources in the following sections.

![Import project from third-party sources](https://images.ctfassets.net/r9o86ar0p03f/KwAdfu5Bw1WMBIT2vcH61/8af69e5c7bc932bc85c3a07b7f490577/image6.png)

## GitLab sources

You can export projects from GitLab.com and GitLab Self-Managed instances using the Export project feature in a project’s settings.

![Export project screen](https://images.ctfassets.net/r9o86ar0p03f/2swSLi5u64N4woz9EPkZPK/98deb22fb2270870ffacad4c4d2cedc0/image9.png)

To access it:

- Go to your project’s settings and click into the **General** area.
- Scroll to and **Expand Advanced** section.
- Select **Export project**.
- A notification will be shown stating: “Project export started. A download link will be sent by email and made available on this page.”
- After the export is generated, you can follow the link contained in the email or refresh the project settings page to reveal the “Download export” option.

### Importing the project

![Import an exported GitLab project](https://images.ctfassets.net/r9o86ar0p03f/5hX6PExmH217Tscr30pWRr/893e064239d2a186bce278d19a66d1d5/image10.png)

- Click on the **New project** button in your target GitLab instance.
- Select **Import project** and click on **GitLab Export** in the list of import sources.
- Specify a project name and select the export file, then click **Import project**.
- An "import in progress" page will be shown and once complete, you will be redirected to the imported project.

Depending on the size of your project, the import time may vary. It's important to note that not everything in a project might be exported and a few things might change after import. Review the documentation to understand the limitations. If you want to migrate a whole group instead of individual projects, the Direct Transfer method is recommended; this creates a copy of an entire group.

## Third-party providers

GitLab supports importing from Bitbucket Cloud, Bitbucket Server, FogBugz, Gitea, and GitHub. The import process is similar across all the supported third parties — the main difference is in the method of authentication. Let's look at a few of them.

### GitHub

![Authenticate with GitHub screen](https://images.ctfassets.net/r9o86ar0p03f/6wYtslMUNeijb3XEWnY9FL/35b56843de91ec85f5ee279ac002b174/image3.png)

There are three methods to import GitHub projects in to GitLab:

- Using GitHub OAuth
- Using a GitHub personal access token
- Using the API

Importing using GitHub OAuth and personal access token are similar. The difference lies in how your authorize GitLab to access your repositories. The OAuth method is easier because you only need to click on the “Authorize with GitHub” button and your are redirected to your GitHub account to authorize the connection. Then the list of your projects is loaded for you to pick those you want to import.

![Import repositories from GitHub screen](https://images.ctfassets.net/r9o86ar0p03f/XoEnB39K89NIGGnqnEDsX/ccd3fa8f263155316281c9881a5efff3/image2.png)

Alternatively, you will need to generate a GitHub personal access token, selecting the `repo` and `read:org` scopes, and then provide it on the "Import" page. For API imports, you can use the same personal access token with our Import REST API endpoints in your script or application.

In this demo, GitLab Senior Developer Advocate Fernando Diaz explains how to import a project from GitHub using the OAuth method:

<!-- blank line -->  
<figure class="video\_container"> <iframe src="https://www.youtube.com/embed/0Id5oMl1Kqs?si=esF6wbz2j2JlhDVL" frameborder="0" allowfullscreen="true"> </iframe>  
</figure> <!-- blank line -->

You can learn about prerequisites, known issues, importing from GitHub Enterprise, and other valuable information from the GitLab import documentation.

### Bitbucket

Importing projects from Bitbucket is similar to importing them from GitHub. While using OAuth is applicable to Bitbucket Cloud, the SaaS version of Bitbucket, you'll need to provide a URL, username, and personal access token for Bitbucket Server, the enterprise self-hosted version. Clicking on the Bitbucket Cloud option on the "Import" screen automatically takes you to Atlassian authentication for Bitbucket.

![Import project from BitBucket](https://images.ctfassets.net/r9o86ar0p03f/5Eizn75bHBmfDM4yp1DAKD/7e5572d0a52cc8769b5b2940dc87a819/image4.png)

You can also import Bitbucket projects using the GitLab Import API.

### Gitea

![Import project from Gitea](https://images.ctfassets.net/r9o86ar0p03f/5pGzgxIDe6d8Y5evo2dxz8/bc1da0e107dca645a4d6d6db7b8c8f03/image5.png)

Importing projects from Gitea requires the creation of a personal access token on the Gitea platform and providing it along with the Gitea server URL on the GitLab import page. OAuth authentication is not supported.

### Generic remote Git repository

![Import project from remote Git repository](https://images.ctfassets.net/r9o86ar0p03f/7AHq1ELVMFWvPT3tpYwx2T/cba452487afc9acc6987f0426168e167/image1.png)

Where your Git provider is not supported or import is not possible using the supported methods, a repository can be imported using its accessible `https://` or `git://` URL. If it's not publicly accessible, you will provide the repository URL along with username and password (or access token where applicable due to multifactor authentication).

This method can also be used for maintaining a copy of a remote project and keeping it in sync, i.e., mirroring. Mirroring allows you to maintain repositories across different platforms and keep them synced. This can be to separate private and public access to project while ensuring both ends have the same copy, which is useful when open-sourcing internal projects. It can also be used when working with contractors and both parties use different platforms, and access to codebase is necessary on both ends.

## Summary

Importing and migrating between GitLab instances and from other sources is an important process that needs to be planned to ensure the expectations are clear on what gets imported and with which method. While most third-party methods import project items, including files, issues, and merge requests, some methods have known issues and limitations. The GitLab import section of the documentation has detailed information on all the supported methods that can help you plan your migration.

> #### Want to take your learning to the next level? Sign up for GitLab University courses. Or you can get going right away with a free 60-day trial of GitLab Ultimate.

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

Knowing how to import your projects to GitLab is an essential skill to make the most of the GitLab DevSecOps platform. You’ve set up your account, invited users, and organized them based on your use case or team structure. Now, you need to bring your existing projects into GitLab and start collaborating. These projects can be local files on your computer or hosted on a different source code management platform. Let's explore the options.

## Importing local project files

You don't want to start from scratch every time you import a project. Follow these steps to get into GitLab existing legacy projects or applications that exist without version control or use version control.

### Git project

1. If Git is already initiated in your local project, create a new project in GitLab and obtain the SSH or HTTPS URL by clicking on the **Code** button in the top right corner of your project page.

![create a new project in GitLab with SSH/HTTPS URLs](https://images.ctfassets.net/r9o86ar0p03f/JodhM2vA2zOXV51EXkseM/9f5bf272ec0f6dbce4268a9d3e475bc1/image8.png)

2. Switch to your terminal and ensure you are in your project folder:

```bash
cd /project_folder  
```

3. Backup your existing Git origin:

```bash

git remote rename origin old-origin

```

4. Add the GitLab remote URL for the new origin, when using SSH:

```bash
git remote add origin [git@gitlab.com](mailto:git@gitlab.com):gitlab-da/playground/abubakar/new-test-repo.git  
```

And for HTTPS:

```bash
git remote add origin https://gitlab.com/gitlab-da/playground/abubakar/new-test-repo.git  
```

5. Then push all existing branches and tags to GitLab:

```bash
git push --set-upstream origin --all  
git push --set-upstream origin --tags  
```

All your file project files, branches, and tags will be pushed to GitLab and you can start collaborating.

### Non-Git project

Alternatively, if you have not initiated Git in your project, you will need to initialize Git, commit existing files, and push to GitLab as follows:

```bash
git init --initial-branch=main  
git remote add origin git@gitlab.com:gitlab-da/playground/abubakar/new-test-repo.git  
git add .  
git commit -m "Initial commit"  
git push --set-upstream origin main  
```

## Importing from online sources

If you have your project on GitLab.com or other platforms and you want to move it to another GitLab instance (like a self-managed instance) or from another platform to GitLab.com, GitLab provides the import project feature when you want to create a new project.

![Create a new project screen](https://images.ctfassets.net/r9o86ar0p03f/7wDEsJUuoNKGQ9GlfHfBOx/9e100dd74315db1ed5152ae8f94cff48/image7.png)

Importing a project migrates the project files and some other components of the project depending on the source. You can import from different sources like Bitbucket, GitHub, Gitea, and a GitLab instance, among other sources. Import sources are enabled by default on GitLab.com, but they need to be enabled for self-managed by an administrator. We will look at a few of these sources in the following sections.

![Import project from third-party sources](https://images.ctfassets.net/r9o86ar0p03f/KwAdfu5Bw1WMBIT2vcH61/8af69e5c7bc932bc85c3a07b7f490577/image6.png)

## GitLab sources

You can export projects from GitLab.com and GitLab Self-Managed instances using the Export project feature in a project’s settings.

![Export project screen](https://images.ctfassets.net/r9o86ar0p03f/2swSLi5u64N4woz9EPkZPK/98deb22fb2270870ffacad4c4d2cedc0/image9.png)

To access it:

- Go to your project’s settings and click into the **General** area.
- Scroll to and **Expand Advanced** section.
- Select **Export project**.
- A notification will be shown stating: “Project export started. A download link will be sent by email and made available on this page.”
- After the export is generated, you can follow the link contained in the email or refresh the project settings page to reveal the “Download export” option.

### Importing the project

![Import an exported GitLab project](https://images.ctfassets.net/r9o86ar0p03f/5hX6PExmH217Tscr30pWRr/893e064239d2a186bce278d19a66d1d5/image10.png)

- Click on the **New project** button in your target GitLab instance.
- Select **Import project** and click on **GitLab Export** in the list of import sources.
- Specify a project name and select the export file, then click **Import project**.
- An "import in progress" page will be shown and once complete, you will be redirected to the imported project.

Depending on the size of your project, the import time may vary. It's important to note that not everything in a project might be exported and a few things might change after import. Review the documentation to understand the limitations. If you want to migrate a whole group instead of individual projects, the Direct Transfer method is recommended; this creates a copy of an entire group.

## Third-party providers

GitLab supports importing from Bitbucket Cloud, Bitbucket Server, FogBugz, Gitea, and GitHub. The import process is similar across all the supported third parties — the main difference is in the method of authentication. Let's look at a few of them.

### GitHub

![Authenticate with GitHub screen](https://images.ctfassets.net/r9o86ar0p03f/6wYtslMUNeijb3XEWnY9FL/35b56843de91ec85f5ee279ac002b174/image3.png)

There are three methods to import GitHub projects in to GitLab:

- Using GitHub OAuth
- Using a GitHub personal access token
- Using the API

Importing using GitHub OAuth and personal access token are similar. The difference lies in how your authorize GitLab to access your repositories. The OAuth method is easier because you only need to click on the “Authorize with GitHub” button and your are redirected to your GitHub account to authorize the connection. Then the list of your projects is loaded for you to pick those you want to import.

![Import repositories from GitHub screen](https://images.ctfassets.net/r9o86ar0p03f/XoEnB39K89NIGGnqnEDsX/ccd3fa8f263155316281c9881a5efff3/image2.png)

Alternatively, you will need to generate a GitHub personal access token, selecting the `repo` and `read:org` scopes, and then provide it on the "Import" page. For API imports, you can use the same personal access token with our Import REST API endpoints in your script or application.

In this demo, GitLab Senior Developer Advocate Fernando Diaz explains how to import a project from GitHub using the OAuth method:

<!-- blank line -->  
<figure class="video\_container"> <iframe src="https://www.youtube.com/embed/0Id5oMl1Kqs?si=esF6wbz2j2JlhDVL" frameborder="0" allowfullscreen="true"> </iframe>  
</figure> <!-- blank line -->

You can learn about prerequisites, known issues, importing from GitHub Enterprise, and other valuable information from the GitLab import documentation.

### Bitbucket

Importing projects from Bitbucket is similar to importing them from GitHub. While using OAuth is applicable to Bitbucket Cloud, the SaaS version of Bitbucket, you'll need to provide a URL, username, and personal access token for Bitbucket Server, the enterprise self-hosted version. Clicking on the Bitbucket Cloud option on the "Import" screen automatically takes you to Atlassian authentication for Bitbucket.

![Import project from BitBucket](https://images.ctfassets.net/r9o86ar0p03f/5Eizn75bHBmfDM4yp1DAKD/7e5572d0a52cc8769b5b2940dc87a819/image4.png)

You can also import Bitbucket projects using the GitLab Import API.

### Gitea

![Import project from Gitea](https://images.ctfassets.net/r9o86ar0p03f/5pGzgxIDe6d8Y5evo2dxz8/bc1da0e107dca645a4d6d6db7b8c8f03/image5.png)

Importing projects from Gitea requires the creation of a personal access token on the Gitea platform and providing it along with the Gitea server URL on the GitLab import page. OAuth authentication is not supported.

### Generic remote Git repository

![Import project from remote Git repository](https://images.ctfassets.net/r9o86ar0p03f/7AHq1ELVMFWvPT3tpYwx2T/cba452487afc9acc6987f0426168e167/image1.png)

Where your Git provider is not supported or import is not possible using the supported methods, a repository can be imported using its accessible `https://` or `git://` URL. If it's not publicly accessible, you will provide the repository URL along with username and password (or access token where applicable due to multifactor authentication).

This method can also be used for maintaining a copy of a remote project and keeping it in sync, i.e., mirroring. Mirroring allows you to maintain repositories across different platforms and keep them synced. This can be to separate private and public access to project while ensuring both ends have the same copy, which is useful when open-sourcing internal projects. It can also be used when working with contractors and both parties use different platforms, and access to codebase is necessary on both ends.

## Summary

Importing and migrating between GitLab instances and from other sources is an important process that needs to be planned to ensure the expectations are clear on what gets imported and with which method. While most third-party methods import project items, including files, issues, and merge requests, some methods have known issues and limitations. The GitLab import section of the documentation has detailed information on all the supported methods that can help you plan your migration.

> #### Want to take your learning to the next level? Sign up for GitLab University courses. Or you can get going right away with a free 60-day trial of GitLab Ultimate.

Go to Source
