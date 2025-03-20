---
title: "Fixing Unreal Engine Project Issues With Qodana"
date: 2025-01-22
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
  - "specify"
---

If you saw our blog post about using Qodana in Unity and .NET projects, you know that we’ve been striving to explore Qodana’s potential for game development. What’s our next stop on this mission? Seeing our code quality platform in action with Unreal Engine, one of the most popular engines for different types of projects \[…\]

![](https://blog.jetbrains.com/wp-content/uploads/2025/01/qd-featured_blog_1280x720_en-5.png)

If you saw our blog post about using Qodana in Unity and .NET projects, you know that we’ve been striving to explore Qodana’s potential for game development. What’s our next stop on this mission? Seeing our code quality platform in action with Unreal Engine, one of the most popular engines for different types of projects – from virtual reality prototypes to triple-A games. 

In this post, we’ll demonstrate how we used our static analysis tool Qodana on Lyra Starter Game, a widely known sample project from Epic Games. We chose this project for its large codebase, which provides a wider range of potential issues to identify, analyze, and fix. 

The analysis and the resolution of issues were carried out by a junior developer. Our goal was to check how someone still building their game development knowledge can use Qodana to improve code and product quality. 

## Table of Contents

- Running Qodana from the IDE
- Setting up the CI/CD pipeline
- Fixing problems
- Qodana’s Unreal Engine analysis summarized
- Switch to Qodana for code analysis and get 25% off

## Running Qodana from the IDE

We started by running Qodana from an IDE (Rider) to see the initial results and set up filtering. As Qodana is integrated with the most popular JetBrains IDEs, it can be easily launched directly from the _Tools_ menu. For a better team experience, we also recommend using the JetBrains CI/CD solution, TeamCity. After setting up we will switch that on as well to set up a seamless process and quality gate.

<figure>

![ Running Qodana in the IDE](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdEt-Nfyo6qMaah211mz1Gwe7aEWajoRyPESCL3cgbKpF4fy8nm83pSv_gdurQRGpQyBJ_MDE9CZ6iRxeWphGKzaoEbgQVbWA7YwtsMhqMiaQggNlM23Y1WwTdCSm0mRN2n_HZp2Qgg0QMT6XfQ4l3oQag?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

 Running Qodana in the IDE

</figcaption>

</figure>

You can run Qodana in your IDE without a token, but we wanted our results to be accessible from Qodana Cloud, a cloud-based tool for Qodana reports. To upload a report to the cloud, you need a license token, which you can get from the Qodana Cloud project. We will also use the token to integrate our analysis into the CI/CD pipeline.

The qodana.yaml file is created automatically and shown in the popup below. 

<figure>

![The qodana.yaml popup](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf8BG5AR8GRsxqeKwpi8y4MU4aea-pBTwfjqPi0kc2GEgwBzeWVHjbt0h2mTgA905Eydx27i-roc8pYWOKp0wpFpIbAl2pwvC9LusdblJhsjv14aIYrhGT9o9ED8KmpXX0vgWhIb6MDxJ8xChmT4NXYdw?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_T_he qodana.yaml popup

</figcaption>

</figure>

You can modify this file directly in the popup window if you need to, and you can run inspections with the qodana.starter profile to check if there are any critical errors. Once you run it, the file will be saved in the project root. We wanted to use a custom profile, so we modified this file to reference the custom profile.yaml.

In the qodana.yaml file, we left a link to the profile. QDNET is a linter based on the Rider distribution and designed to work with Unreal Engine projects.

```
version: "1.0"

#Specify IDE code to run analysis without container (Applied in CI/CD pipeline)
ide: QDNET

#Specify inspection profile for code analysis
profile:
  path: profile.yaml
```

qodana.yaml

In the profile.yaml file, we changed the profile to the more extensive qodana.recommended and identified scope to be excluded from the analysis. We wanted to analyze only the project codebase, without Unreal Engine or plugin sources.

```
baseProfile: "qodana.recommended"
name: "UnrealEngine"

inspections:
  - group: ALL
    ignore:
      - "scope#!file:Source/LyraGame//*"
```

profile.yaml

These changes provided a relatively comprehensive analysis report.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf5LpMoBPuKJzva_JO2TNVDh2NS4lDAL6vkwnxQvnr6Bg_5I-iWui8ph9gsR4o_lH1lxb2pIa93gvoYgpwMCVpE1CT8ZolB8M_MiH-Enp4UFIOT1yafL6_MKcbzECkFqx72mbB3Pm6pJEho3VXMKX8Tbg?key=S9b9SXPJZlN2xMPLYjUjeA)

 We then linked our project to Qodana Cloud. 

<figure>

![ Linking the project to Qodana Cloud](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYT9rZvU4jfrD68cgj0ZMoU_vrGDLlIGdgE2-V_loaC80XXZR1xQUUsmnJTUEtet4BY6PLBwd3tke_lteHtNEDZHNlwkF11wEXI2Kzsz9Tdn1WrxP6cr2IcyThDrLBX_qoUYCd2JqbQNVigehMYy0uEA?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

 _Linking the project to Qodana Cloud_

</figcaption>

</figure>

This will allow us to access future reports withinin the IDE and view problems locally.

<figure>

![The report in the IDE](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfxKBvJJWQBIs-1zpKHFWB2aiW44jyQhdJWWl2aj14aozs1t0SGZjOD8HyagLcR6Xy8n-M7ok0bLdVzFn7KEu-A53Z8FkseAUWPeFR1odls2mb2WRK7zi_CH_iAnjj7WypcLA7oRC-e3zzERJRXqZ1gAQ?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_The report in the IDE_

</figcaption>

</figure>

## Setting up the CI/CD pipeline

We already had a CI/CD pipeline in TeamCity, which we used to build the project every time we pushed changes to the main branch. There are several ways to complete the build. One such method is with the Unreal Engine plugin for TeamCity which can be downloaded from JetBrains Marketplace. You don’t have to run the build before running Qodana, but it is convenient to put it in the same pipeline. This allows TeamCity to mark the build as _Failed_ if Qodana finds any issues.

To run the Qodana analysis, we added a PowerShell step that loaded and ran our Qodana CLI tool. We opted for the latest AMD64-compatible version and assets for our agents. If you are working with a different operating system and architecture, you will have to choose the assets designed for them. 

Before implementing this in any project, you should discuss the security implications with the people responsible for this in your organization. Downloading a third-party binary without checking its integrity and checksum can be risky. You may need to save a fixed version of the binary yourself or verify the checksum of the downloaded distribution. 

```
Invoke-WebRequest -Uri "https://github.com/JetBrains/qodana-cli/releases/download/%VERSION%/qodana_windows_x86_64.exe" -OutFile "qodana_windows_x86_64.exe" 

./qodana_windows_x86_64.exe scan --ide QDNET
```

PowerShell build step

The linter requires a Qodana token, a value from the project page on Qodana Cloud. To pass the token through the pipeline, we added the QODANA\_TOKEN environment variable with the _Password_ value type to ensure the token remains secure.

Then, in the same way, we added the desired version of Qodana CLI as a configuration parameter.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc-LfQ5Fptyx9UB3IdiW0WQaNdapLolrPnaJ8N3cum98sZMoS_C6f6PeQ8S-Em51EfQJ2_6fBejCd-xMNIKMxeSojst8RkCdVS2l84YcTPqzpy1P4eFMZBOrFaPOchTJipJjMW82yC7ZKy65oRNNmJ1W30?key=S9b9SXPJZlN2xMPLYjUjeA)

## Fixing problems

For this Unreal Engine project, we were particularly interested in issues specific to projects created with the game engine. We used filters on Qodana Сloud to show only these problems.

<figure>

![Analyzing an Unreal Engine project](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf5LpMoBPuKJzva_JO2TNVDh2NS4lDAL6vkwnxQvnr6Bg_5I-iWui8ph9gsR4o_lH1lxb2pIa93gvoYgpwMCVpE1CT8ZolB8M_MiH-Enp4UFIOT1yafL6_MKcbzECkFqx72mbB3Pm6pJEho3VXMKX8Tbg?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Unreal Engine problems_

</figcaption>

</figure>

Qodana’s sunburst diagram provides a convenient visualization of the detected issues, as well as an easy way to navigate to them. In our case, we could see there were seven types of problems, some of which could be resolved using context actions:

- _BlueprintCallable_ _function can be made_ _const_

- _BlueprintCallable_ _function can be made_ _static_

- _Use of a class that has not been declared previously_

To quickly navigate to these issues in the IDE, we can click on the _Open file in Rider_ button.  
  
Please note: to get this functionality to work you have to install JetBrains Toolbox on your computer.

<figure>

![Example of a problem in Qodana Cloud](https://lh7-rt.googleusercontent.com/docsz/AD_4nXclvhgZc0i72iUD3m6Zpll9QJBkdwE7hRey20dpZT255dvYpcIfAZGWL1E54jiNnh_YSOu3fIf6py7R4j-_p5x2vlTkikD9SDMmJFm92Fa84t_2J_v9FsqgF_49i25M2GNoMLJxSGRctbokIviRGNirkQ0?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Example of a problem in Qodana Cloud_

</figcaption>

</figure>

After opening the file in the IDE, we resolved this problem using the relevant quick-fix with a context action.

<figure>

![Example of a problem in the IDE](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcSCkWoEOZlt9i4Y26Hqc23tnXpFi5wysiQD9vxfMXiYxVhDcJPBE2KcpEVvUjSV1BrLEHn1d3u-tkDgjMKEvd_fM0b5xnxYYeA5QvTmdC3Bi_FBtZHjdyZl3euzdFSG_GoNSP_KJ8o9bcYGytbKYwoJwg?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Example of a problem in the IDE_

</figcaption>

</figure>

You can also navigate by opening a report locally in the IDE and going through the list of problems grouped there by type, fixing them one by one. As you fix problems in the IDE, the number of issues detected will decrease.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXej8LzQJlTFFjUbWPE5Os6Zia1tmkN1n4QDe9xM0A8j0t3MioSHA63LItyGRqKLYI26FpkEz2YuxDxwoixfcPvaPV4I-q-ZTo_zogiOAqXervHv63JkTjjT0OcqggBSu6DwuOwBm_C37VQQddbHnePw7w?key=S9b9SXPJZlN2xMPLYjUjeA)

We decided not to fix problems like _BlueprintCallable_ _function is never used in Blueprint or C++ code_, as the Lyra project is considered learning material, and it’s actively maintained. The project contains methods that are not currently being used but may be in the future. 

Additionally, we decided not to fix inconsistent Unreal Engine naming problems because the project uses a different naming convention, where upper case is used for all abbreviations.

<figure>

![Example of a naming problem in the IDE](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeAgpDvmZCIGCi0YxGGcc3KBQehBBLE245zuTf5SnSs8esMdfvPou7xjBvclMUbieUUg0SAGFA9vgYjzp5mccUkzTTL4tfSS5YztvA7bsqxmXLGPJ42hacsV79T6I1mYJA2tx2tp20PVMH2BK01fmhYMEs?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Example of a naming problem in the IDE_

</figcaption>

</figure>

To deactivate these inspections, we added their inspection IDs to profile.yaml and set the enabled property to false, ensuring these types of problems will no longer be shown in reports. The problem ID can be found in the qodana.sarif.json file.

Next, we moved to C++ problems. We decided to only fix the problems that were categorized as higher than moderate severity, as most low-level issues could be fixed with quick-fixes. We excluded the non-critical issues by changing the profile.yaml file.

```
baseProfile: "qodana.recommended"
name: "UnrealEngine"

inspections:
 - group: ALL
   ignore:
     - "scope#!file:Source/LyraGame//*"
 - group: "severity:TYPO"
   enabled: false
 - inspection: "CppUE4CodingStandardNamingViolationWarning"
   enabled: false
 - inspection: "CppUEBlueprintCallableFunctionUnused"
   enabled: false
```

profile.yaml

This produced a report with less than a thousand problems. To experiment with different filtering strategies, we used separate branches for each YAML file. This allowed us to divide the problems into groups based on type, tackling each type in a separate branch with different settings and then merging the branches.

<figure>

![Low-severity C++ problems excluded](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeGKlXC7I069tFmLWAeBIqJe8EYzXUgAfYipCLJIDZPB3uzMj8xfJvyVh7eSSw7Ubw60HHQIUTUNRANzbdl-6I4mSiHU0Un07hVQT5tplIPqzxeK9KjBKTvIwRe2YaTG2LsEkBTMl0QcrxY-nnVco6KPlg?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Low-severity C++ problems excluded_

</figcaption>

</figure>

Even without the low-level results, we saw many types of problems that were not in the Unreal Engine category. 

In total, our team fixed 822 of 937 problems from the categories we examined next.

As you can see below, the most common problems fell into the C_ommon Practices and Code Improvements_ category and included issues like variables that could be made const or static. We resolved most of them with quick-fixes. We left problems like _Function is not implemented,_ as they could be fixed in future development. We decided not to fix some of the problems, as the changes required to mitigate them would make the project uncompilable and in need of further refactoring.

<figure>

![Problems classified as Common Practices and Code Improvements](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYcKEnOkerG88rWQonC3As2S3d10xip4MRj0Q66hpuTnoojUyGpLSZjPblb22XHUH-OtC6vgST3p7fAGoK1cMn6uPIUvUIZGbeOGr0LFEmKOKBFyLREiGaTfr7DSKpqSUQRibYRolVWr5hgHMJ54A66A?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Problems classified as_ Common Practices and Code Improvements

</figcaption>

</figure>

As a result, we were left with only 26 problems in the _Common Practices and Code Improvements_ category, which we could deal with later. 

<figure>

![Remaining problems classified as Common Practices and Code Improvements](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc6Dx2FVJ5EWn8Y7eBi0S994vlKkqKvQXlim3rJBDw4BQoCEw4gF9Ve24do45s_H7-FkRKeu7orTVZm5LBOEE1wh-DJyZi_MvNwntv8yRkD5BlLvagrpNGG21SpkHQKqaTJpnTINm_7GQcTz3B9cPUXA2c?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Remaining problems classified as_ Common Practices and Code Improvements

</figcaption>

</figure>

Up next were potential code quality issues. From this category, we fixed problems where we needed to remove unused declarators or directives. We then moved to redundancies in code, most of which were resolved easily with a quick-fix. We did not address any problems where developers left comments with their plans or TODOs because we assumed that these problems would be fixed with future changes.

<figure>

![Example of a TODO](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeWi0iiVWaTwqula4oazqMmKWMPRRK7ee6jinu_4LZ41ZIdbpOrBbqwK9p6bkAioQWDlOZNTp-HiXE8LowZhBLX1pDPdxMGTDEpzsSMRUUfbVd0bApgeL9bdsoojkn1PLbsuOqKXm7bL75fTmelpzhrhbI?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Example of a TODO_

</figcaption>

</figure>

Last but not least, the S_yntax Style_ category contained only two types of problems, both of which concerned the use of virtual and override specifiers when overriding functions. We fixed all of them by adding the missing specifiers.

<figure>

![Syntax Style problems](https://lh7-rt.googleusercontent.com/docsz/AD_4nXccmwGgEJzn8ZcqZZWMssE-cLVUf5qRvNkeWfZ6Xex8plKElzLZmnP_2Lf6RrVqSYsW7TL9mes4c5AtROvzOa3xXJCaoU5XfKSvnTQN5quSQ5tGC8Om8oIvG4AQGbKdSlNQBbnzmVCoP-UimUpDVbc2F-M?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

Syntax Style _problems_

</figcaption>

</figure>

We were left with 123 unresolved problems, either due to ongoing development or the lack of a feasible solution. We moved these issues to the baseline. To apply the baseline, we downloaded the baseline file and stored it in the repository.

<figure>

![Selecting problems](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdhHDbEbDNtSqnhYBBQoACWcKtRs8_3_h8nmk9B8nDLCyBtWl2syA71icgobciVz6cmPdgM1-nguV9B8f810wa7Y8VZf_PJqOY7YCzIVVL5G-x7B-zETBu2DPwDEOPMS3Z4hF0RSdyUCdifRz9ul92P7XY?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Selecting problems_

</figcaption>

</figure>

<figure>

![Downloading the baseline file](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdfyMdsJh_GsBHeOQxGm8A9_-rGQTmYSc4UEzvVKTcdy6Tq1ialIf10FvOXxYfa_uI7vlItDfIZbQHkE0Kbl9qORafcRH-vYc89A2AIHqooa-U7rbwju06QWJKGjNnu7p_XK6CHKCI-fN8egJwE2sVOm2U?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_Downloading the baseline file_

</figcaption>

</figure>

Then, by adding the –baseline parameter and path to the file, we adjusted the pipeline to include the baseline in future analyses.

```
Invoke-WebRequest -Uri "https://github.com/JetBrains/qodana-cli/releases/download/%VERSION%/qodana_windows_x86_64.exe" -OutFile "qodana_windows_x86_64.exe" 
./qodana_windows_x86_64.exe scan --ide QDNET --baseline qodana.sarif.json
```

PowerShell build step

And finally, we had a flawless report.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf9wGy5sCEQ1S1rMTf0ZN-eIFonnPo22WTQC_3bFsVSippChSz9GNXLD6YJqcDM-HWqR_g5VjrXhmIE5ZzD7t_aDzX0l-FWQrR6H_CFnvHlaCBWJ8AlKTZ8xuHENNxlpqbKFIUG1H8JT8mnwd3Gre3Yiw?key=S9b9SXPJZlN2xMPLYjUjeA)

If our team decided to continue working on this project, we could fix new problems as they appeared or we could focus on eliminating problems from the baseline, depending on our priorities.

We set up a quality gate to enforce the standards we had achieved with these efforts, and we added a several failureConditions section to qodana.yaml to configure additional quality gates for the total number of problems, as well as the numbers of critical and high-severity issues. Going forward, if any of these limits are exceeded, the build will fail.

```
failureConditions:
  severityThresholds:
    any: 10 # Total problems
    critical: 0 # Critical and other severities
    high: 5

```

Added qodana.yaml configuration

We also adjusted the execution of qodana-cli to consider exit code, failing the build if the result fails the quality gates. By failing builds that don’t meet our quality criteria, we can identify and address issues immediately.

```
Invoke-WebRequest -Uri "https://github.com/JetBrains/qodana-cli/releases/download/%VERSION%/qodana_windows_x86_64.exe" -OutFile "qodana_windows_x86_64.exe" 
./qodana_windows_x86_64.exe scan --ide QDNET 

# Capture the exit code of the command
$exitCode = $LASTEXITCODE

# Print the exit code
Write-Output "Exit code: $exitCode"

# Exit the script with the same exit code
exit $exitCode
```

PowerShell build step

<figure>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXddDCIwT1B55BLN9PKLf_BPoMJR97466cYhplP2vmiJ3LSKI4u7-OkbNYCgLbOO6_mR-KdQv4TCSGw4HpNUSwVa234VWJpmsX04eg3t4q2PRYXQPzQ7bYnShp_Gs3Rx2cGitLP9IBAoSVc2ibumB7DnhEo?key=S9b9SXPJZlN2xMPLYjUjeA)

<figcaption>

_A failed build in TeamCity_

</figcaption>

</figure>

## Qodana’s Unreal Engine analysis summarized

We successfully analyzed the Lyra project, got a detailed report, and fixed more than 800 problems. While conducting professional reviews will likely require a deeper understanding of Unreal Engine, Qodana’s analysis still helped a single junior developer clean up the code and make it more concise. 

For large-scale projects like Lyra, Qodana can effectively highlight and prioritize critical code issues that may be overlooked in manual reviews.

Since Lyra is a private repo, we can’t share the outcome, but we hope we’ve shown you how this process could work for your team and what kind of results it can deliver.

If you’d like more information, visit our website, view Qodana’s features, or try it in your next game development project. 

## **Switch to Qodana for code analysis and get 25% off**

Qodana gets better with every release and provides a cost-effective way for teams to build confidence in code quality. 

With this in mind, we’re offering you 25% off your first year of Qodana if you switch from a comparable commercial solution. Click on the button below to speak to our team. 

Switch To Qodana

Thank you to Software Developer Ekaterina Trukhan for her contribution to this analysis. 

Go to Source
