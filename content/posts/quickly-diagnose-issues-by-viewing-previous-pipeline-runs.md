---
title: "Quickly diagnose issues by viewing previous Pipeline runs"
date: 2025-02-06
categories: 
  - "bitbucket"
  - "bitbucket-cloud"
  - "cloud"
  - "cloudnative"
  - "continuous-delivery"
  - "devops"
  - "git"
  - "linux"
  - "pipelines"
  - "software"
---

We are excited to announce the release of a new capability in Bitbucket Pipelines that will improve your experience diagnosing issues with your pipeline runs, by allowing you to view and access previous runs within the Pipelines UI.

![](https://atlassianblog.wpengine.com/wp-content/uploads/2025/01/image-1-600x491.png)

The previous pipeline **Runs** dropdown will not show up if you re-run an entire pipeline, because re-running an entire pipeline creates an entirely separate pipeline record, rather than an additional run on an existing pipeline.

## Why It Matters

The ability to view previous pipeline runs provides valuable insights into the history of your builds. This feature empowers you to:

**Compare Results:** Easily compare the results of different pipeline runs to analyze performance and identify areas for improvement.

**Track Progress:** Gain visibility into the status and outcomes of past pipeline runs, allowing you to track progress and identify trends over time.

**Troubleshoot Issues:** Quickly pinpoint and investigate failed or problematic pipeline runs to diagnose issues and take corrective actions.

## How It Works

On the Pipeline result page for a specific pipeline, you’ll see the **Runs** dropdown (if there were previous runs for that particular pipeline) that will display a list of these previous runs when selected. Select a run from the list to access and review the information associated with that previous run. The list displays the most recent 10 runs.

We have also added the run number into the URL. So, when you are viewing a previous run, you can easily save and share a link directly to it.

The post Quickly diagnose issues by viewing previous Pipeline runs appeared first on Work Life by Atlassian.

Go to Source
