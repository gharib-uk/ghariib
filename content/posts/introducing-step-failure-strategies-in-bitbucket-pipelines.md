---
title: "Introducing step failure strategies in Bitbucket Pipelines"
date: 2025-02-06
categories: 
  - "bitbucket"
  - "bitbucket-cloud"
  - "bitbucket-pipelines"
  - "cloud"
  - "cloudnative"
  - "continuous-delivery"
  - "developer"
  - "devops"
  - "linux"
  - "pipelines"
  - "software"
---

We are excited to introduce a new capability in Bitbucket Pipelines – Step Failure Strategies. This is the first of a set of new features allowing developers to implement more comprehensive logic and control-flow inside their CI/CD pipelines.

Failure Strategies are designed to give you explicit control over how your pipeline behaves in the event that an individual step within the pipeline fails. This initial release will include the `ignore` strategy, which is particularly useful for complex pipelines with steps like code styling suggestions or informational tests, where certain steps failing within the pipeline shouldn’t necessarily cause the entire build to fail.

If a step configured with this strategy fails, the step will be marked as “Failed” in the UI but the failure will be ignored by the overall pipeline and the remaining steps will continue running. A step that fails using the `ignore` strategy will _**not**_ cause the overall pipeline to fail.

Coming soon we will be introducing additional Failure Strategies such as automatic retries and manual approvals. If you have other strategies you would like to see implemented, please drop us a comment in the Pipelines Community Space.

## Quick start

To configure the failure strategy, you can provide the new step-level `on-fail` option with a required `strategy` property.

* * *

```
- step:
    // Other options
    on-fail:
      strategy: ignore
    // Other options
```

* * *

Here is a full example, in which we have a first step containing low-priority flaky tests with an ignore failure strategy configured. Step 2 will always be executed regardless of the outcome of Step 1 and the pipeline’s overall result will depend **_solely_** on Step 2.

* * *

```
pipelines:
  default:
    - step:
        name: 'Step 1'
        on-fail:
          strategy: ignore
        script:
          - npm flaky-test
    - step:
        name: 'Step 2'
        script:
          - npm build
```

* * *

If the failure strategy is not configured, the step will default to the existing behavior, which will fail and stop the whole pipeline immediately. You can also use `strategy: fail` to declare that a step is following the default behaviour explicitly if you want.

## Visual updates

We have also implemented a range of visual updates to indicate the status of your configured failure strategy. If a step fails while using `strategy: ignore`, an `Ignored` label will be shown in the UI, alerting you that the step failure was ignored and that the pipeline execution has continued according to the Failure Strategy configuration.

![](https://atlassianblog.wpengine.com/wp-content/uploads/2025/02/image-600x975.png)

You can also hover over the step duration to inspect the failure strategy configuration if there is one.

![](https://atlassianblog.wpengine.com/wp-content/uploads/2025/02/image-1-600x683.png)

## Compatibility and Limitations

In this release, we are bringing the `on-fail` option to both individual & parallel steps, but with some limitations:

- As of now, the Failure Strategy won’t support deployment steps.

- Rerunning failed steps will not rerun the completed steps with ignored failures.

As always, if you have any questions, concerns, or feedback – please get in touch with the team via the Pipelines Community Space.

The post Introducing step failure strategies in Bitbucket Pipelines appeared first on Work Life by Atlassian.

Go to Source
