---
title: "<div>Getting started with GitLab: How to manage users</div>"
date: 2025-01-15
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

Ensuring a safe, compliant, and collaborative environment starts with the most basic of tasks - managing users. In this tutorial, we show you how to establish project members, assign roles and permissions, and create groups and subgroups.

Note: To follow along with this tutorial, you should have a GitLab account either through GitLab.com or your organization's self-managed instance. If you need help, visit our fundamentals area on GitLab University.

Let's get started.

When you create GitLab users, they only have access to their private projects, public projects, and projects set with internal visibility. For the purposes of this tutorial, your project is super secret and only invited members should have access to it – at varying permissions settings. To ensure this, you can invite users as members of the project.

## Project members

![Project members screen](https://images.ctfassets.net/r9o86ar0p03f/cTFRMv1Sg4raXl0m3kfXI/ba4fe32f2124dc47c0c4d5b29ab0884f/image1.png)

GitLab users can be invited to a project and assigned a role, which determines what they can do in the project. The owner of a project can delegate administrative tasks to other users as maintainers, who can do almost everything an owner does, aside from changes to a project such as deleting, archiving, or transferring a project.

![Invite members screen](https://images.ctfassets.net/r9o86ar0p03f/oR5cVlHEhrdBCg40UbXMI/3785f62326eb938831b71365ce013c24/image2.png)

Maintainers of the project can invite other members as developers who have access to all the features to create, build, and deploy software. Users who are not developers but need project management access can be invited to the project as planners, reporters, and guests with varying levels of permissions. These roles can also be used to determine who can make changes to certain branches with protected branches.

If you are working with contractors or your use requires user permissions to expire, you can set an expiry date after which the user loses access to the project. Project members can also be identified as direct or indirect members, based on their membership type. Direct members are invited directly into the project, whereas indirect members are often inherited from a GitLab group a project belongs to.

Now, let's look at Group memberships.

## Group memberships

Groups in GitLab can be a top level created at the root of a GitLab instance like the gitlab.com/gitlab-org. which is a parent group used to organize other subgroups like gitlab.com/gitlab-org/charts. Groups are useful even if you only have one project.

Groups can be used for different reasons:

- organizing similar or related projects
- organizing users into groups for better team coordination

When using groups to organize users, you can organize teams in groups and invite a group to a project with a specific role for an entire team. You can have a `dev` group for the developers of the team, `pm` group for the project managers and `leads` for team leads. When inviting the groups, `dev` can be assigned the Developer role, `pm` the Planner role, and `leads` the Maintainer role.

![Invite a group screen](https://images.ctfassets.net/r9o86ar0p03f/6RHSZ4L42t5tmMynmYFTgN/2b14adbc253a07b7bef0d927d59c02b4/image3.png)

Members of each group can be can be added or removed without needing to update project permissions. This is particularly useful when your team has grown to have several projects. However, it is important to observe best practices for using groups for collaboration.

Another helpful aspect of having users organized in groups is that you can mention the entire group in issues, merge requests, or comments, which makes keeping an entire team informed easier.

### Create subgroups

Subgroups can be used to further organize users in a group and you can keep adding subgroups up to 20 nested levels. Users in a subgroup inherit the the permissions they have in a parent group. If you want to grant a user in a subgroup a role higher than what they inherited, you will need to invite them to the subgroup with the new higher role. Note: You can not give them a lower role in the subgroup.

### Manage groups

Group Owners have several management options to determine how users function in a group. For instance, you can set how a user can request access to a group, enable/disable group mentions, restrict access, or moderate users, among other options. An exciting new feature, which is still under development at the time of this article's publication, is the automatic removal of dormant users after a minimum of 90 days and a maximum of five years. This will help keep groups clean and better manage the release of license seats.

## Learn more

Managing users on GitLab depends on your use case. If your organization is larger with more advanced workflows and user management, GitLab provides more advanced ways to manage enterprise users. You can also explore more options on how to manage your organization and with GitLab Ultimate, you get more granularity and compliance features.

> #### Want to take your learning to the next level? Sign up for GitLab University courses. Or you can get going right away with a free 60-day trial of GitLab Ultimate.

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

Ensuring a safe, compliant, and collaborative environment starts with the most basic of tasks - managing users. In this tutorial, we show you how to establish project members, assign roles and permissions, and create groups and subgroups.

Note: To follow along with this tutorial, you should have a GitLab account either through GitLab.com or your organization's self-managed instance. If you need help, visit our fundamentals area on GitLab University.

Let's get started.

When you create GitLab users, they only have access to their private projects, public projects, and projects set with internal visibility. For the purposes of this tutorial, your project is super secret and only invited members should have access to it – at varying permissions settings. To ensure this, you can invite users as members of the project.

## Project members

![Project members screen](https://images.ctfassets.net/r9o86ar0p03f/cTFRMv1Sg4raXl0m3kfXI/ba4fe32f2124dc47c0c4d5b29ab0884f/image1.png)

GitLab users can be invited to a project and assigned a role, which determines what they can do in the project. The owner of a project can delegate administrative tasks to other users as maintainers, who can do almost everything an owner does, aside from changes to a project such as deleting, archiving, or transferring a project.

![Invite members screen](https://images.ctfassets.net/r9o86ar0p03f/oR5cVlHEhrdBCg40UbXMI/3785f62326eb938831b71365ce013c24/image2.png)

Maintainers of the project can invite other members as developers who have access to all the features to create, build, and deploy software. Users who are not developers but need project management access can be invited to the project as planners, reporters, and guests with varying levels of permissions. These roles can also be used to determine who can make changes to certain branches with protected branches.

If you are working with contractors or your use requires user permissions to expire, you can set an expiry date after which the user loses access to the project. Project members can also be identified as direct or indirect members, based on their membership type. Direct members are invited directly into the project, whereas indirect members are often inherited from a GitLab group a project belongs to.

Now, let's look at Group memberships.

## Group memberships

Groups in GitLab can be a top level created at the root of a GitLab instance like the gitlab.com/gitlab-org. which is a parent group used to organize other subgroups like gitlab.com/gitlab-org/charts. Groups are useful even if you only have one project.

Groups can be used for different reasons:

- organizing similar or related projects
- organizing users into groups for better team coordination

When using groups to organize users, you can organize teams in groups and invite a group to a project with a specific role for an entire team. You can have a `dev` group for the developers of the team, `pm` group for the project managers and `leads` for team leads. When inviting the groups, `dev` can be assigned the Developer role, `pm` the Planner role, and `leads` the Maintainer role.

![Invite a group screen](https://images.ctfassets.net/r9o86ar0p03f/6RHSZ4L42t5tmMynmYFTgN/2b14adbc253a07b7bef0d927d59c02b4/image3.png)

Members of each group can be can be added or removed without needing to update project permissions. This is particularly useful when your team has grown to have several projects. However, it is important to observe best practices for using groups for collaboration.

Another helpful aspect of having users organized in groups is that you can mention the entire group in issues, merge requests, or comments, which makes keeping an entire team informed easier.

### Create subgroups

Subgroups can be used to further organize users in a group and you can keep adding subgroups up to 20 nested levels. Users in a subgroup inherit the the permissions they have in a parent group. If you want to grant a user in a subgroup a role higher than what they inherited, you will need to invite them to the subgroup with the new higher role. Note: You can not give them a lower role in the subgroup.

### Manage groups

Group Owners have several management options to determine how users function in a group. For instance, you can set how a user can request access to a group, enable/disable group mentions, restrict access, or moderate users, among other options. An exciting new feature, which is still under development at the time of this article's publication, is the automatic removal of dormant users after a minimum of 90 days and a maximum of five years. This will help keep groups clean and better manage the release of license seats.

## Learn more

Managing users on GitLab depends on your use case. If your organization is larger with more advanced workflows and user management, GitLab provides more advanced ways to manage enterprise users. You can also explore more options on how to manage your organization and with GitLab Ultimate, you get more granularity and compliance features.

> #### Want to take your learning to the next level? Sign up for GitLab University courses. Or you can get going right away with a free 60-day trial of GitLab Ultimate.

Go to Source
