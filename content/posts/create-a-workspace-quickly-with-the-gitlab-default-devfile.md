---
title: "<div>Create a workspace quickly with the GitLab default devfile</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Software development environments can be complex to set up and maintain. Developers often spend a significant amount of time configuring their local environments with the right dependencies, tools, and settings. GitLab aims to solve this by providing a default devfile that enables you to create workspaces and to start developing quickly.

## GitLab Workspaces

GitLab Workspaces provide isolated development environments for making changes to your GitLab projects without the complexity of setting up local dependencies. Workspaces ensure reproducible development setups, allowing developers to share their environment configurations effortlessly.

By default, GitLab Workspaces are configured to use the GitLab VS Code fork and include the GitLab Workflow extension. To learn more, visit the GitLab Workspaces documentation.

## Understand devfiles

A **devfile** is a YAML-based declarative configuration file that defines a project's development environment. It specifies the necessary tools, languages, runtimes, and other components required for development.

Previously, setting up a workspace required a custom devfile at the root of the repository. For example, a `.devfile.yaml` file. A typical devfile looked like this:

![typical default devfile](https://images.ctfassets.net/r9o86ar0p03f/3m6D3KyMTaojCt34gDfBaw/0736f517d9288524f835debcc4f1e118/Screenshot_2025-02-26_at_8.15.58_AM.png)

## GitLab default devfile

Starting in GitLab 17.9, a GitLab default devfile is available for all projects when creating a workspace. This eliminates the need to manually create a devfile before starting a workspace. Here is the content of the default devfile:

![GitLab default devfile content](https://images.ctfassets.net/r9o86ar0p03f/6DZe8DY3tMVDhcosgJ867d/699857a668d416b018d52414ab18929e/Screenshot_2025-02-26_at_8.16.20_AM.png)

When creating a workspace with the GitLab UI, the option **Use GitLab default devfile** is always available – regardless of whether custom devfiles exist in the repository. Simply select this option to start exploring GitLab Workspaces with one less setup step.

![Use GitLab default devfile screenshot](https://images.ctfassets.net/r9o86ar0p03f/1XLkxewYbKAGoy1EjoTTGP/b15ab16547932cd4fd7791d7dc2bade1/image1.png)

## Create your own custom devfiles

While the GitLab default devfile provides a quick way to start a workspace, you may want to customize your development environment to better fit your project's needs. By creating a custom devfile, you can tailor your development environment with the exact tools, dependencies, and configurations needed for your workflow.

Consider creating a custom devfile if you need to:

- Add project-specific dependencies beyond the base development image.
- Adjust CPU and memory resource limits.
- Configure multiple containers for additional services like databases.
- Define custom, project-specific, environment variables.
- Set up specific port mappings.
- Integrate specialized development tools like debuggers or language servers.

For more details, see the Workspaces devfile documentation.

## Read more

- Build and run containers in Remote Development workspaces
- Use GitLab AI features out-of-the-box in a GitLab Workspace
- Quickstart guide for GitLab Remote Development workspaces
- Enable secure sudo access for GitLab Remote Development workspaces

Software development environments can be complex to set up and maintain. Developers often spend a significant amount of time configuring their local environments with the right dependencies, tools, and settings. GitLab aims to solve this by providing a default devfile that enables you to create workspaces and to start developing quickly.

## GitLab Workspaces

GitLab Workspaces provide isolated development environments for making changes to your GitLab projects without the complexity of setting up local dependencies. Workspaces ensure reproducible development setups, allowing developers to share their environment configurations effortlessly.

By default, GitLab Workspaces are configured to use the GitLab VS Code fork and include the GitLab Workflow extension. To learn more, visit the GitLab Workspaces documentation.

## Understand devfiles

A **devfile** is a YAML-based declarative configuration file that defines a project's development environment. It specifies the necessary tools, languages, runtimes, and other components required for development.

Previously, setting up a workspace required a custom devfile at the root of the repository. For example, a `.devfile.yaml` file. A typical devfile looked like this:

![typical default devfile](https://images.ctfassets.net/r9o86ar0p03f/3m6D3KyMTaojCt34gDfBaw/0736f517d9288524f835debcc4f1e118/Screenshot_2025-02-26_at_8.15.58_AM.png)

## GitLab default devfile

Starting in GitLab 17.9, a GitLab default devfile is available for all projects when creating a workspace. This eliminates the need to manually create a devfile before starting a workspace. Here is the content of the default devfile:

![GitLab default devfile content](https://images.ctfassets.net/r9o86ar0p03f/6DZe8DY3tMVDhcosgJ867d/699857a668d416b018d52414ab18929e/Screenshot_2025-02-26_at_8.16.20_AM.png)

When creating a workspace with the GitLab UI, the option **Use GitLab default devfile** is always available – regardless of whether custom devfiles exist in the repository. Simply select this option to start exploring GitLab Workspaces with one less setup step.

![Use GitLab default devfile screenshot](https://images.ctfassets.net/r9o86ar0p03f/1XLkxewYbKAGoy1EjoTTGP/b15ab16547932cd4fd7791d7dc2bade1/image1.png)

## Create your own custom devfiles

While the GitLab default devfile provides a quick way to start a workspace, you may want to customize your development environment to better fit your project's needs. By creating a custom devfile, you can tailor your development environment with the exact tools, dependencies, and configurations needed for your workflow.

Consider creating a custom devfile if you need to:

- Add project-specific dependencies beyond the base development image.
- Adjust CPU and memory resource limits.
- Configure multiple containers for additional services like databases.
- Define custom, project-specific, environment variables.
- Set up specific port mappings.
- Integrate specialized development tools like debuggers or language servers.

For more details, see the Workspaces devfile documentation.

## Read more

- Build and run containers in Remote Development workspaces
- Use GitLab AI features out-of-the-box in a GitLab Workspace
- Quickstart guide for GitLab Remote Development workspaces
- Enable secure sudo access for GitLab Remote Development workspaces

Go to Source
