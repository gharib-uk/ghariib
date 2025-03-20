---
title: "The Early Access Program for ReSharper and the .NET Tools 2025.1 Is Here!"
date: 2025-01-19
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

We are excited to announce the launch of the Early Access Program (EAP) for the next major release of ReSharper and the .NET Tools! This initial EAP build is now available for download and offers a preview of the upcoming features and enhancements. Your feedback is crucial in helping us refine these updates, so we \[…\]

We are excited to announce the launch of the Early Access Program (EAP) for the next major release of ReSharper and the .NET Tools! This initial EAP build is now available for download and offers a preview of the upcoming features and enhancements. Your feedback is crucial in helping us refine these updates, so we encourage you to try out the new build and share your thoughts.

Download ReSharper 2025.1 EAP 1

If you’re new to the Early Access Program, we encourage you to read this blog post where we outline what the program is all about and the benefits you can expect from participating in it.

Now let’s take a look at the feature highlights of the first preview build!

## dotMemory

### dotMemory is now fully integrated with Visual Studio

With the 2025.1 update we’re elevating dotMemory from a standalone companion application to a fully integrated part of your Visual Studio workflow. The integration brings dotMemory’s complete feature set right into your development environment, accessible through the familiar _ReSharper | Profile_ menu.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXet-6EvdfHBUdIKGUzkNSlTPZcbNCHzG5Sz1L1E1Xm5TWODpb_FwWGsYz1ZRiLel3-j7kFBU1kTPVjL1tXX9hNIAsgUCcVqXVkNb9j9uYojN69dMCIakDKJuZM2CUkCg5Z9wIQi?key=ITEi2m6W1qveNV2PUm8_-bRH)

#### Here’s how it works

You can now start profiling your application, capture memory snapshots, and analyze memory usage patterns without leaving Visual Studio. The dedicated _dotMemory_ tool window gives you instant access to the collected profiling data. The profiling data is shown in the corresponding document tabs.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXePnG-ZF4zDpeLlPasSZbH1Xk1c0LfEZzLbEcNc49MRkO5YoMz6tJwbYc7pbb7sumHkIXqv18FMhzcV09z1Lu69_amyIXX3FyYA3uD__7uDItiFlYhs93qGopKY9TNFwzruT63oTw?key=ITEi2m6W1qveNV2PUm8_-bRH)

_The dotMemory workspace inside Visual Studio_

A flashing memory profiling icon appears in the bottom-right corner of the Visual Studio window whenever a profiling session is running. Right-clicking this status bar icon allows you to:

- Quickly stop the current profiling session.

- Open the dotMemory tool window if it’s not already visible.

This status bar menu also offers a faster way to access dotMemory than navigating through the main menu (_Extensions | ReSharper | Profile | Show dotMemory Profiler_), ensuring you can manage profiling sessions with ease. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfodgXlxFdnRltP2ppzKK1DzKABa2fXIf5_PY9FXqXCTOr6S26v0-HDYLBNMOys7jVV8BiPdbbAEPCAfFdJV_4UK_TxG7m48JcURK-0OsDb0WzvRGMX8w3fBBWqlmHREinSWlvx_Q?key=ITEi2m6W1qveNV2PUm8_-bRH)

You can also find your way to the dotMemory tool window by looking it up using the Search Everywhere action. 

#### Getting started

Follow these steps to get started with dotMemory inside Visual Studio:

1. Install the ReSharper 2025.1 EAP 1 build

4. Go to _Extensions | ReSharper | Profile_ in the main menu

7. Choose your target process or start a new profiling session

10. Configure your profiling parameters and start the profiling session.

13. Investigate the memory usage patterns in the dedicated Analysis tabs in the document window inside Visual Studio.  

The integration requires Visual Studio 2019 or later, and all your existing dotMemory workspaces will work seamlessly with the integrated version.

dotMemory is part of the **dotUltimate** suite of tools, so if you already have a dotUltimate license, you can start using it right away!

Should you encounter any issues with using the integrated version of dotMemory, please report them to this issue.

## ReSharper C++

The 2025.1 EAP 1 build for ReSharper C++ comes with the following improvements:

**Cross-platform code enhancements**

- Improved support for conditionals with omitted operands. \[RSCPP-35483\]

- Added support for the GNU `#import` directive. \[RSCPP-35415\]

- Added support for `_Float16`, `__bf16`, and `__float128` types. \[RSCPP-35004, RSCPP-35814\]

**Code editing and navigation**

- If no file with a matching name exists, the _Switch header/source_ feature now suggests files containing symbols from the current file. \[RSCPP-36341\]

- The `__declspec(property)` feature now parses get and put arguments and also supports _Rename_ refactoring for them. \[RSCPP-14090\]

**Code analysis and formatting**

- Improved handling of the _Indent class member from access specifier_ formatter setting in typing assistance for a smoother editing experience. \[RSCPP-31862\]

- An inspection has been added to detect redundant forward declarations within a single file. \[RSCPP-35102\]

* * *

For the full list of changes included in this build please see our issue tracker.

\[Download the EAP 1 build\]

We encourage you to download the latest build and explore these new features. Your feedback is invaluable as we continue to refine and improve ReSharper for the upcoming release. Please share your thoughts and suggestions in the comments below or via our issue tracker.

Go to Source
