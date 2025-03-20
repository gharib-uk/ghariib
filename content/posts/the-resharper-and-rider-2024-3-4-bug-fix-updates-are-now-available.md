---
title: "The ReSharper and Rider 2024.3.4 Bug-Fix Updates Are Now Available"
date: 2025-01-23
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

The new bug fixes for the 2024.3 release are available to download.  ReSharper 2024.3.4 For the full list of changes included in this build, please refer to our issue tracker. Rider 2024.3.4 We’ve also fixed several issues affecting .NET MAUI development: For the full list of changes included in this build, please refer to our \[…\]

The new bug fixes for the 2024.3 release are available to download. 

## ReSharper 2024.3.4

- We’ve fixed local privilege escalation in the ETW Host Service (CVE-2025-23385).

- We’ve fixed an issue where the `foreach` statement would fail to compile when iterating over a `ref` struct in an `async` method, even though the code compiled successfully using `dotnet build`.

- We’ve resolved a code analysis bug where implicit type arguments involving cast operators caused incorrect errors, even though the code compiled successfully. \[RSRP-499411\]

- We’ve resolved a bug in the testing platform where navigation to the test source did not work correctly for tests created with TUnit. \[RSRP-498832\]

- We’ve fixed an issue in the testing platform where test gutter marks were not displayed near tests, particularly for classes or methods with parameters. \[RSRP-498834\]

For the full list of changes included in this build, please refer to our issue tracker.

Download ReSharper 2024.3.4

* * *

## Rider 2024.3.4

- We’ve fixed local privilege escalation in the ETW Host Service. (CVE-2025-23385)

- We’ve fixed a problem where the Azure DevOps credential provider was broken on Windows, causing authentication issues when accessing private NuGet feeds. \[RIDER-121245\]

- We’ve resolved an issue where source generators failed to work on Unix-based systems when .NET SDK is installed with a package manager. \[RIDER-121116\]

We’ve also fixed several issues affecting .NET MAUI development:

- Run/debug configurations for MAUI apps are now correctly displayed. \[RIDER-120425\]

- Debugging support for MAUI projects on the Windows platform has been restored. \[RIDER-119363, RIDER-121970\]

For the full list of changes included in this build, please refer to our issue tracker.

Download Rider 2024.3.4

Go to Source
