---
title: "Blog: Foreign aliases"
date: 2025-01-03
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "general"
  - "linux"
tags: 
  - "jenkins"
---

## Background

In an organisation with many repositories and developers that are frequently shifting the maintenance of OWNERS and OWNERS\_ALIASES files can be tedious. In the passing year a couple of functionalities has been added to help with this.

## Foreign aliases

To avoid maintaining the OWNERS\_ALIASES file in many repositories you can now refer to the OWNERS\_ALIASES file in another repository. In the Jenkins X project we have the main OWNERS\_ALIASES file in the jx-community repository. So in the jx repository the OWNERS\_ALIASES file only looks like this:

```yaml
foreignAliases:
- name: jx-community
```

The organisation defaults to be the same as for the repository, but can specify as well. So in the jx-project repository the OWNERS\_ALIASES file looks like this:

```yaml
foreignAliases:
- name: jx-community
  org: jenkins-x
```

Using the filed `ref` you can also specify a branch or tag to use instead of the default one of the repository.

## OWNERS and OWNERS\_ALIASES in new repositories

When creating or importing a repository using `jx project` the default content of OWNERS and OWNERS\_ALIASES isn’t that useful since only the current user are put in the files.

If you create your own quickstarts you place the OWNERS and / or OWNERS\_ALIASES files with the content of your liking in those.

A recent new functionality is that you can put OWNERS and / or OWNERS\_ALIASES files in the `extensions` directory of your cluster repository. These files will then be used as the default content of the files in new repositories.

## Background

In an organisation with many repositories and developers that are frequently shifting the maintenance of OWNERS and OWNERS\_ALIASES files can be tedious. In the passing year a couple of functionalities has been added to help with this.

## Foreign aliases

To avoid maintaining the OWNERS\_ALIASES file in many repositories you can now refer to the OWNERS\_ALIASES file in another repository. In the Jenkins X project we have the main OWNERS\_ALIASES file in the jx-community repository. So in the jx repository the OWNERS\_ALIASES file only looks like this:

```yaml
foreignAliases:
- name: jx-community
```

The organisation defaults to be the same as for the repository, but can specify as well. So in the jx-project repository the OWNERS\_ALIASES file looks like this:

```yaml
foreignAliases:
- name: jx-community
  org: jenkins-x
```

Using the filed `ref` you can also specify a branch or tag to use instead of the default one of the repository.

## OWNERS and OWNERS\_ALIASES in new repositories

When creating or importing a repository using `jx project` the default content of OWNERS and OWNERS\_ALIASES isn’t that useful since only the current user are put in the files.

If you create your own quickstarts you place the OWNERS and / or OWNERS\_ALIASES files with the content of your liking in those.

A recent new functionality is that you can put OWNERS and / or OWNERS\_ALIASES files in the `extensions` directory of your cluster repository. These files will then be used as the default content of the files in new repositories.

Go to Source
