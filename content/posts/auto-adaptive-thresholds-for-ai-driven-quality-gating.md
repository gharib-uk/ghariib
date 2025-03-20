---
title: "Auto-adaptive thresholds for AI-driven quality gating"
date: 2025-01-07
categories: 
  - "ci-cd"
  - "devops"
  - "devops-cloud"
  - "linux"
  - "open-source"
tags: 
  - "davis-ai"
  - "dynatrace"
  - "dynatrace-version-1-293"
  - "forcasting"
  - "regression-testing"
  - "site-reliability-guardian"
  - "srg"
---

![auto-adaptive thresholds](https://dt-cdn.net/wp-content/uploads/2024/06/Blog_-OTP-0146_-high-res-version-300x169.png)

The latest release of the Site Reliability Guardian incorporates assistance from Dynatrace Davis® AI to automatically derive appropriate threshold targets and adjust them over time to protect your quality improvements. This process, known as auto-adaptive thresholding, eliminates the need to define a static threshold upfront. Instead, it derives the suitable thresholds from previous validation results.

## Build an umbrella for Development and Operations

In modern software engineering, the discipline of platform engineering delivers DevSecOps practices to developers to bridge the gaps between development, security, and operations and enhance the developer experience. A key element in platform engineering is the establishment of fast feedback cycles regarding the quality and security measures of new software releases. To provide automated feedback for developers, the concept of quality gates for static code analysis in continuous integration is widely adopted throughout the industry. However, this perspective differs in the continuous deployment practices of various organizations, where the feedback is either delayed or not returned to the developer.

While receiving no feedback on the quality or security of new features leaves developers uncertain about feature performance, delayed feedback also increases a developer’s cognitive load. The developer must pause their current engineering work to address the reported issue and consider the code changes they worked on a few days or weeks prior.

To reduce developers’ cognitive load by providing timely information, platform engineers must create tools that allow validations to be run in the early phases of development, with direct and fast feedback loops. Ideally, this should be a self-service offering that enables individual adoption by teams. While platform engineers can build and prepare the necessary infrastructure and templates for self-adoption, developers must still provide some customization. For example, the team must establish specific thresholds for desired service performance behavior. Setting these thresholds upfront can be challenging because the team might not know how a service will behave in its environment.

## How we define auto-adaptive thresholds at Dynatrace

This blog post explores how Dynatrace leveraged the Site Reliability Guardian to establish a fast feedback loop for Davis AI model improvements. The conducted validations avoided regression within the models, and the outcomes were immediately fed back to the data science team when deviations were detected. While the data science team appreciated the quick insights and validations of their improvements, they initially struggled with the setup. Consequently, this blog post highlights the new capability of the Site Reliability Guardian to define auto-adaptive thresholds that tackle the challenge of configuring static thresholds and protect your quality and security investments with relative comparisons to previous validations.

## Fast feedback cycles on model improvements

While the Site Reliability Guardian was originally designed to validate new software releases, Dynatrace has internally extended its application area to include validation of models for Davis AI.

The Dynatrace data science team continuously improves the machine learning models used by Davis AI, for example, by adding new features to forecasting or refining mathematical calculations. A single change can influence multiple models, as features are often used across several models. To ensure that model changes don’t lead to regressions, the data scientists set up Site Reliability Guardian, which is automatically triggered whenever a change is made in the codebase via CI/CD pipelines.

A series of models are continuously trained on Dynatrace tenants to effectively set objectives. The training times and other quality metrics, such as the RMSE (Root Mean Squared Error), SMAPE (Scaled Mean Absolute Percentage Error), and coverage probability, are monitored using Dynatrace. Our data scientists utilize metrics and events to store these quality metrics. However, other data formats, like logs, can also be employed. The quality metrics can then be easily queried using DQL and utilized for the objectives of a guardian. Validations are automatically triggered when a change is committed to the code base via the Dynatrace API. This helps data scientists quickly respond to recently introduced regressions. For instance, if an objective is violated, they’re immediately notified, for example, through a Slack channel.

<figure>

![Validation history](https://dt-cdn.net/wp-content/uploads/2024/06/forecaster_srg.png)

<figcaption>

Figure 1. Validation history

</figcaption>

</figure>

One difficulty encountered when setting up objectives in the guardians was selecting an appropriate threshold for the quality metrics, as this is typically heavily dependent on the data.

## Leverage Davis AI to quickly start validating

To address the challenge of defining a static threshold for an objective, the Site Reliability Guardian enables switching objective thresholds to auto-adaptive mode, as depicted in the screenshot below.

<figure>

![Activating an auto-adaptive threshold for the response time objective](https://dt-cdn.net/wp-content/uploads/2024/06/activate_auto_adaptive_thresholds.png)

<figcaption>

Figure 2. Activating an auto-adaptive threshold for the response time objective

</figcaption>

</figure>

Davis AI controls auto-adaptive thresholds. It analyzes the next five validations to derive this objective’s proper warning and failure thresholds. Once the learning phase is complete, all subsequent validation results are fed into Davis AI to fine-tune the thresholds based on changed behavior.

Considering previous validation results, the latest validation is always relative to the past, protecting quality and security improvements. If, for example, recent performance improvements change a service’s response time from 200 ms to 175 ms, the auto-adaptive threshold is adjusted to reflect the new behavior. Nevertheless, the Site Reliability Guardian detects sudden behavior changes by reporting a warning or failure if response time returns to 200 ms or above.

### Learning phase

Unless an objective has been validated five times, it’s still in the learning phase. During this phase, the measured values are informative, allowing observation of the objective’s development without affecting deployment or delivery processes. The Site Reliability Guardian denotes the learning phase of an objective with a loading symbol on the heatmap and in the objective details.

<figure>

![Representation of the learning phase of an auto-adaptive threshold](https://dt-cdn.net/wp-content/uploads/2024/06/detailed_analysis.png)

<figcaption>

Figure 3. Representation of the learning phase of an auto-adaptive threshold

</figcaption>

</figure>

The warning and failure thresholds will be automatically set if sufficient validations are available to establish a solid baseline for an objective’s auto-adaptive thresholds. Consequently, the next objective validation will impact the overall validation result.

### Auto-adaptive thresholds as code

To enhance the developer’s experience in adopting the Site Reliability Guardian in a self-service manner, the configuration for a guardian and its workflow can be provided in a configuration-as-code fashion. This enables the management of the configuration within the service’s code repository. Incorporating the new capability of auto-adaptive thresholds into configuration-as-code is as simple as adding the `autoAdaptiveThresholdEnabled` flag to an objective.

<figure>

![Configuration-as-code example for activating an auto-adaptive threshold](https://dt-cdn.net/wp-content/uploads/2024/06/config_as_code_snippet.png)

<figcaption>

Figure 4. Configuration-as-code example for activating an auto-adaptive threshold

</figcaption>

</figure>

Before concluding, we wish to announce the change of the Site Reliability Guardian icon from purple to shiny golden. This new appearance enhances the icon’s geometry while preserving its core values. Therefore, the icon continues to feature the infinity loop as a symbol for the DevSecOps loop and the fast forward sign to expedite delivery while ensuring quality and security standards.

## The new and shiny appearance of Site Reliability Guardian

<figure>

![Evolution of the Site Reliability Guardian icon](https://dt-cdn.net/wp-content/uploads/2024/06/srg_icon.png)

<figcaption>

Figure 5. Evolution of the Site Reliability Guardian icon

</figcaption>

</figure>

## What’s next

The new auto-adaptive thresholds capability is now available in Site Reliability Guardian. Open the app and switch from static to auto-adaptive thresholds for those objectives where Davis AI should derive the thresholds for you. For full details, see Dynatrace Documentation.

If you haven’t used Site Reliability Guardian yet, try it out in the Dynatrace Playground or watch the latest app spotlight recording.  

Explore Site Reliability Guardian

The post Auto-adaptive thresholds for AI-driven quality gating appeared first on Dynatrace news.

Go to Source
