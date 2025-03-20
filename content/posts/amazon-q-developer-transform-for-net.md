---
title: "Amazon Q Developer transform for .NET"
date: 2025-01-20
---

## Introduction

A fantastic use case for AI Coding Assistants is upgrading applications to modern and supported versions of programming languages, libraries and frameworks. All too often, engineering effort is spent building new features, whilst existing applications are left untouched, until they become unsupported with all kinds of inherent vulnerabilities.

`Amazon Q Developer` introduced a code transformation agent for Java when launched in preview back in November 2023. This agent has undergone multiple iterations and become more accurate and powerful since then. On 3rd December 2024 at AWS re:Invent, the public preview of new transformation capabilities for .NET, mainframe, and VMware workloads was announced.

In this blog post, I take a look at the .NET transformation capability to get a better understanding of how it works. This feature is available in the `Visual Studio IDE`. However, `Visual Studio for Mac` has been retired, which gave me an opportunity to try out the Amazon Q Developer transformation web experience.

## Why port .NET Framework?

.NET Framework is the original implementation of .NET by Microsoft. It supports running websites, services, desktop apps and more, but only on Windows.

.NET (sometimes called .NET Core) is a more modern, open-source version of .NET that can run on multiple operating systems including Windows, Linux and MacOS. The reasons to continue using .NET Framework are specific and limited according to Microsoft, and relate to use cases where your application is using third-party libraries or NuGet packages or .NET Framework technologies that are not available for .NET.

Where possible you should look to utilise .NET.

## Getting started with web experience

To get started with the web experience, I had to subscribe to `Amazon Q Developer` from your management (root) account, as it is not currently possible to use a delegated administrator account. Note that you need to be in the `us-east-1` region at this point.

![Amazon Q Developer Start Page](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzkuthgu1ah2b962wsgnb.png)

This takes you to a `Getting started with Amazon Q` page. I was prompted to switch back to my home region (`eu-west-2`) which then automatically connected my `AWS Organization` instance of `IAM Identity Center` to `Amazon Q`. At this point, I clicked the button to "subscribe" and added a user from `IAM Identity Center`. It is also possible to add a Group instead of an individual user or users.

![Connect to IAM Identity Center](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fu34d1wcqn9bdk9rfrpiv.png)

This successfully created an `Amazon Q Developer` Pro subscription for my chosen user. At this point, I clicked on the button which took me to the `Amazon Q Developer` console to complete the setup.

![Create an Amazon Q Subscription](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1eyese8uxyhn8furwzv3.png)

Opening up `Amazon Q Developer` in the AWS Console gave the option to click on a settings button.

![Amazon Q Developer Console Settings](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxh8ndltjpzpb9hdeu37r.png)

In the settings, I enabled the transform settings, which is required to give access to the transformation web experience.

![Enable Amazon Q Transform Settings](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnc33ddi92qhh6z0ft0i9.png)

At this point, I navigated in a browser window to https://transform.developer.q.aws.com/ and signed in using `IAM Identity Centre`.

![Sign in using IAM Identity Center](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdyufggmv4gm6tvp5ojqc.png)

Once logged in, I was presented with the option of creating my first transformation job with `Amazon Q`.

![Create first transformation job with Amazon Q](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2gqyvh8wuvht95jyz21q.png)

## Running transformation for .NET

Once I asked Q to create a transformation job, I was given the choice of the type of transformation to work on. There are three options available:

- Modernize .NET applications to cross-platform .NET
- Migrate VMware applications to Amazon EC2
- Perform mainframe modernization (z/os to AWS)

![Choose transformation type](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdqrbicb9sh09z0j2fmu9.png)

I chose the option to modernise a .NET application. `Amazon Q` then populated a number of details about the .NET modernisation project. I could change these details, or in this case, confirm they are correct and let `Amazon Q` create the job itself.

![Confirm .NET transformation job](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdcp7ijz0t9t3fhw8dkfx.png)

At this point I had to connect `Amazon Q` to my source repository for the project I want to transform. I have forked the SharpZeroLogon GitHub repository to my own profile. This is an archived repository that was a rework of the NCC Group's tool specifically for .NET Framework 3.5.

The connection is made using `AWS CodeConnections`. Within an AWS account, you use `CodeConnections` to create a connection to a third party Git-based source provider. Currently, the only supported provider is GitHub. To create a connection, you need to go to `AWS CodeArtifact`, click on `Settings` and then `Connections`. I am using GitHub which installs the `AWS Connector for GitHub` as an application in GitHub. You can configure the connector with access to only specific repositories.

To setup the Amazon Q transformation job you first specify the account number for the AWS account where the connection is configured.

![Select AWS Account ID](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhg1h1aafsawo6cvc2lm9.png)

You then specify the `AWS CodeConnection` ARN.

![AWS CodeConnection ARN](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcqdiroy5nof01avm8jpv.png)

You then go back into the AWS console to approve this connection request.

![Approve Connection Request](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxc1g3ycjfwkf645auio4.png)

Once the connection request has been approved, you click on `Send to Q`, which will allow `Amazon Q` to access the repositories in the connected account.

![Create Connector Send to Amazon Q](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fk56ptf6nz5646tcrj8z7.png)

`Amazon Q` analyses all of the repositories it has access too, to discover which ones run a .NET Framework application that is capable of being transformed. `Amazon Q Developer` transformation capabilities for .NET supports porting C# code projects of the following types: console application, class library, unit tests, web API, Windows Communication Foundation (WCF) service, and business logic layers of Model View Controller (MVC) and Single Page Application (SPA). Types of jobs that `Amazon Q` currently cannot transform include WebUI, SQLServer and ASP.NET.

In my example, the `SharpZeroLogin` repository has been detected as a supported project, and I am given the option to specify a target version (.NET 8.0). I can also specify the name of the new branch that will be created or keep the default.

Note that the web experience gives you the option of carrying out a .NET transform of multiple repositories. This is something not available within the IDE, which only allows one .NET solution at a time.

![Confirm Repository to Transform](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8a1zi97degiu30zwvinc.png)

`Amazon Q` now automatically ports the selected .NET application to the target version following a transformation plan it has created. It commits all of the transformed code to a new branch in my GitHub repository, preserving the original source code.

![Job Completed Successfully](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6lae5ijly53zpkmdt02a.png)

I can click on the Dashboard tab to monitor the progress. In this case, I am told that the application has been transformed with no issues.

![Job Completed Dashboard](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmts7vdavps0l348pvqx6.png)

I can now go to my GitHub repository and look at the new branch that has been created. I can also view the diffs to see what changes have been made. In the file below, we can see that the target framework version has been updated from "3.5" to "net8.0".

![View Code Diffs](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2ftey1kf583vh0a3crfx.png)

## Conclusion

The goal of this blog post is to show you how to simple it is to get up and running with the new `Amazon Q Developer` transformation web experience. If you have existing .NET Framework applications that you want to port to .NET to gain performance improvements and cross-platform support, it is definitely worth giving this feature a go.

Go to Source
