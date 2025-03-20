---
title: "<div>Tutorial: Advanced use case for GitLab Pipeline Execution Policies</div>"
date: 2025-01-22
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Pipeline execution policies are a newer addition to the GitLab DevSecOps platform and a powerful mechanism to enforce CI/CD jobs across applicable projects. They enable platform engineering or security teams to inject jobs into developers’ YAML pipeline definition files, guaranteeing that certain CI/CD jobs will execute no matter what a developer defines in their \`.gitlab-ci.yml\` file.

This article will explain how to utilize pipeline execution policies to create guardrails around the stages or jobs that a developer can use in their pipeline definition. In regulated environments, this may be necessary to ensure developers adhere to a standard set of jobs or stages in their GitLab pipeline. Any job or stage that a developer adds to their pipeline that does not adhere to a corporate standard will cause the pipeline to fail.

One example use case for pipeline execution policies is ensuring a security scanner job runs. Let’s say an organization has made an investment in a third-party security scanner and they have a requirement that the external scan runs before any merge is made into the main branch. Without a pipeline execution policy, a developer could easily skip this step by not including the required code in their `.gitlab-ci.yml` file. With a pipeline execution policy in place, a security team can guarantee the external security scanning job executes regardless of how a developer defines their pipeline.

To use pipeline execution policies to enforce these restrictions requires two parts: a shell script to make calls to the GitLab API and the policy itself. This tutorial uses a bash script; if your runner uses a different scripting language, it is easy to adapt to other languages.

Here is the example shell script I will use for this exercise:

```
`#!/bin/bash`

`echo "Checking pipeline stages and jobs..."`

`# Pull the group access token from the environment variable`  
`GROUP_ACCESS_TOKEN="$PIPELINE_TOKEN"`

`echo "PROJECT_ID: $PROJECT_ID"`  
`echo "PIPELINE_ID: $PIPELINE_ID"`

`if [ -z "$GROUP_ACCESS_TOKEN" ]; then`  
  `echo "GROUP_ACCESS_TOKEN (MR_GENERATOR) is not set"`  
  `exit 1`  
`fi`

`if [ -z "$PROJECT_ID" ]; then`  
  `echo "PROJECT_ID is not set"`  
  `exit 1`  
`fi`

`if [ -z "$PIPELINE_ID" ]; then`  
  `echo "PIPELINE_ID is not set"`  
  `exit 1`  
`fi`

`# Use the group access token for the API request`  
`api_url="$GITLAB_API_URL/projects/$PROJECT_ID/pipelines/$PIPELINE_ID/jobs"`  
`echo "API URL: $api_url"`

`# Fetch pipeline jobs using the group access token`  
`jobs=$(curl --silent --header "PRIVATE-TOKEN: $GROUP_ACCESS_TOKEN" "$api_url")`  
`echo "Fetched Jobs: $jobs"`

`if [[ "$jobs" == *"404 Project Not Found"* ]]; then`  
  `echo "Failed to authenticate with GitLab API: Project not found"`  
  `exit 1`  
`fi`

`# Extract stages and jobs`  
`pipeline_stages=$(echo "$jobs" | grep -o '"stage":"[^"]*"' | cut -d '"' -f 4 | sort | uniq | tr 'n' ',')`  
`pipeline_jobs=$(echo "$jobs" | grep -o '"name":"[^"]*"' | cut -d '"' -f 4 | sort | uniq | tr 'n' ',')`

`echo "Pipeline Stages: $pipeline_stages"`  
`echo "Pipeline Jobs: $pipeline_jobs"`

`# Check if pipeline stages are approved`  
`for stage in $(echo $pipeline_stages | tr ',' ' '); do`  
  `echo "Checking stage: $stage"`  
  `if ! [[ ",$APPROVED_STAGES," =~ ",$stage," ]]; then`  
    `echo "Stage $stage is not approved."`  
    `exit 1`  
  `fi`  
`done`

`# Check if pipeline jobs are approved`  
`for job in $(echo $pipeline_jobs | tr ',' ' '); do`  
  `echo "Checking job: $job"`  
  `if ! [[ ",$APPROVED_JOBS," =~ ",$job," ]]; then`  
    `echo "Job $job is not approve`  
```

Let’s break this down a bit.

The first few lines of this code perform some sanity checks, ensuring that a pipeline ID, project ID, and group access token exist.

- A GitLab pipeline ID is a unique numerical identifier that GitLab automatically assigns to each pipeline run.
- A GitLab project ID is a unique numerical identifier assigned to each project in GitLab.
- A GitLab group access token is a token that authenticates and authorizes access to resources at the group level in GitLab. This is in contrast to a GitLab personal access token (PAT), which is unique to each user.

The bulk of the work comes from the GitLab Projects API call where the script requests the jobs for the specified pipeline. Once you have job information for the currently running pipeline, you can use a simple grep command to parse out stage and job names, and store them in variables for comparison. The last portion of the script checks to see if pipeline stages and jobs are on the approved list. Where do these parameters come from?

This is where GitLab Pipeline Execution Policies come into play. They enable injection of YAML code into a pipeline. How can we leverage injected YAML to execute this shell script? Here’s a code snippet showing how to do this.

```
`## With this config, the goal is to create a pre-check job that evaluates the pipeline and fails the job/pipeline if any checks do not pass`

`variables:`  
  `GITLAB_API_URL: "https://gitlab.com/api/v4"`  
  `PROJECT_ID: $CI_PROJECT_ID`  
  `PIPELINE_ID: $CI_PIPELINE_ID`  
  `APPROVED_STAGES: ".pipeline-policy-pre,pre_check,build,test,deploy"`  
  `APPROVED_JOBS: "pre_check,build_job,test_job,deploy_job"`

`pre_check:`  
  `stage: .pipeline-policy-pre`  
  `script:`  
    `- curl -H "PRIVATE-TOKEN:${REPO_ACCESS_TOKEN}" --url "https://<gitlab_URL>/api/v4/projects/<project_id>/repository/files/check_settings.sh/raw" -o pre-check.sh`  
    `- ls -l`  
    `- chmod +x pre-check.sh`  
    `- DEBUG_MODE=false ./pre-check.sh  # Set DEBUG_MODE to true or false`  
  `allow_failure: true`  
```

In this YAML snippet, we set a few variables used in the shell script. Most importantly, this is where approved stages and approved jobs are defined. After the `variables` section, we then add a new job to the `.pipeline-policy-pre` stage. This is a reserved stage for pipeline execution policies and is guaranteed to execute before any stages defined in a `.gitlab-ci.yml` file. There is a corresponding `.pipeline-policy-post` stage as well, though we will not be using it in this scenario.

The script portion of the job does the actual work. Here, we leverage a curl command to execute the shell script defined above. This example includes authentication if it’s located in a private repository. However, if it’s publicly accessible, you can forgo this authentication. The last line controls whether or not the pipeline will fail. In this example, the pipeline will continue. This is useful for testing – in practice, you would likely set `allow_failure: false` to cause the pipeline to fail. This is desired as the goal of this exercise is to not allow pipelines to continue execution if a developer adds a rogue job or stage.

To utilize this YAML, save it to a `.yml` file in a repository of your choice. We’ll see how to connect it to a policy shortly.

Now, we have our script and our YAML to inject into a developer’s pipeline. Next, let’s see how to put this together using a pipeline execution policy.

Like creating other policies in GitLab, start by creating a new Pipeline Execution Policy by navigating to **Secure > Policies** in the left hand navigation menu. Then, choose **New Policy** at the top right, and select **Pipeline Execution Policy** from the policy creation options.

For this exercise, you can leave the **Policy Scope** set to the default options. In the **Actions** section, be sure to choose **Inject** and select the project and file where you’ve saved your YAML code snippet. Click on **Update via Merge Request** at the very bottom to create an MR that you can then merge into your project.

If this is your first security policy, clicking on **Merge** in the MR will create a Security Policy Project, which is a project to store all security policies. When implementing any type of security policy in a production environment, access to this project should be restricted so developers cannot make changes to security policies. In fact, you may also want to consider storing YAML code that’s used by pipeline execution policies in this project to restrict access as well, though this is not a requirement.  
Executing a pipeline where this pipeline execution policy is enabled should result in the following output when you attempt to add an invalid stage to the project `.gitlab-ci.yml` file.

![Output of attempting an invalid stage to project gitlab-ci.yml file](https://images.ctfassets.net/r9o86ar0p03f/5ahf1xaxSEDbT5rKGc2T0C/19340d225c9581569a6804a771f236da/image1.png)

While this use case is very focused on one aspect of security and compliance in your organization, this opens the door to other use cases. For example, you may want to make group-level variables accessible to every project within a group; this is possible with pipeline execution policies. Or, you may want to create a golden pipeline and have developers add to it. The possibilities are endless. GitLab customers are finding new and exciting ways to use this new functionality every day.

If you’re a GitLab Ultimate customer, try this out today and let us know how you’re using pipeline execution policies. Not a GitLab Ultimate customer? Sign up for a free 60-day trial to get started.

## Read more

- How to integrate custom security scanners into GitLab
- Integrate external security scanners into your DevSecOps workflow
- Why GitLab is deprecating compliance pipelines in favor of security policies

Pipeline execution policies are a newer addition to the GitLab DevSecOps platform and a powerful mechanism to enforce CI/CD jobs across applicable projects. They enable platform engineering or security teams to inject jobs into developers’ YAML pipeline definition files, guaranteeing that certain CI/CD jobs will execute no matter what a developer defines in their \`.gitlab-ci.yml\` file.

This article will explain how to utilize pipeline execution policies to create guardrails around the stages or jobs that a developer can use in their pipeline definition. In regulated environments, this may be necessary to ensure developers adhere to a standard set of jobs or stages in their GitLab pipeline. Any job or stage that a developer adds to their pipeline that does not adhere to a corporate standard will cause the pipeline to fail.

One example use case for pipeline execution policies is ensuring a security scanner job runs. Let’s say an organization has made an investment in a third-party security scanner and they have a requirement that the external scan runs before any merge is made into the main branch. Without a pipeline execution policy, a developer could easily skip this step by not including the required code in their `.gitlab-ci.yml` file. With a pipeline execution policy in place, a security team can guarantee the external security scanning job executes regardless of how a developer defines their pipeline.

To use pipeline execution policies to enforce these restrictions requires two parts: a shell script to make calls to the GitLab API and the policy itself. This tutorial uses a bash script; if your runner uses a different scripting language, it is easy to adapt to other languages.

Here is the example shell script I will use for this exercise:

```
`#!/bin/bash`

`echo "Checking pipeline stages and jobs..."`

`# Pull the group access token from the environment variable`  
`GROUP_ACCESS_TOKEN="$PIPELINE_TOKEN"`

`echo "PROJECT_ID: $PROJECT_ID"`  
`echo "PIPELINE_ID: $PIPELINE_ID"`

`if [ -z "$GROUP_ACCESS_TOKEN" ]; then`  
  `echo "GROUP_ACCESS_TOKEN (MR_GENERATOR) is not set"`  
  `exit 1`  
`fi`

`if [ -z "$PROJECT_ID" ]; then`  
  `echo "PROJECT_ID is not set"`  
  `exit 1`  
`fi`

`if [ -z "$PIPELINE_ID" ]; then`  
  `echo "PIPELINE_ID is not set"`  
  `exit 1`  
`fi`

`# Use the group access token for the API request`  
`api_url="$GITLAB_API_URL/projects/$PROJECT_ID/pipelines/$PIPELINE_ID/jobs"`  
`echo "API URL: $api_url"`

`# Fetch pipeline jobs using the group access token`  
`jobs=$(curl --silent --header "PRIVATE-TOKEN: $GROUP_ACCESS_TOKEN" "$api_url")`  
`echo "Fetched Jobs: $jobs"`

`if [[ "$jobs" == *"404 Project Not Found"* ]]; then`  
  `echo "Failed to authenticate with GitLab API: Project not found"`  
  `exit 1`  
`fi`

`# Extract stages and jobs`  
`pipeline_stages=$(echo "$jobs" | grep -o '"stage":"[^"]*"' | cut -d '"' -f 4 | sort | uniq | tr 'n' ',')`  
`pipeline_jobs=$(echo "$jobs" | grep -o '"name":"[^"]*"' | cut -d '"' -f 4 | sort | uniq | tr 'n' ',')`

`echo "Pipeline Stages: $pipeline_stages"`  
`echo "Pipeline Jobs: $pipeline_jobs"`

`# Check if pipeline stages are approved`  
`for stage in $(echo $pipeline_stages | tr ',' ' '); do`  
  `echo "Checking stage: $stage"`  
  `if ! [[ ",$APPROVED_STAGES," =~ ",$stage," ]]; then`  
    `echo "Stage $stage is not approved."`  
    `exit 1`  
  `fi`  
`done`

`# Check if pipeline jobs are approved`  
`for job in $(echo $pipeline_jobs | tr ',' ' '); do`  
  `echo "Checking job: $job"`  
  `if ! [[ ",$APPROVED_JOBS," =~ ",$job," ]]; then`  
    `echo "Job $job is not approve`  
```

Let’s break this down a bit.

The first few lines of this code perform some sanity checks, ensuring that a pipeline ID, project ID, and group access token exist.

- A GitLab pipeline ID is a unique numerical identifier that GitLab automatically assigns to each pipeline run.
- A GitLab project ID is a unique numerical identifier assigned to each project in GitLab.
- A GitLab group access token is a token that authenticates and authorizes access to resources at the group level in GitLab. This is in contrast to a GitLab personal access token (PAT), which is unique to each user.

The bulk of the work comes from the GitLab Projects API call where the script requests the jobs for the specified pipeline. Once you have job information for the currently running pipeline, you can use a simple grep command to parse out stage and job names, and store them in variables for comparison. The last portion of the script checks to see if pipeline stages and jobs are on the approved list. Where do these parameters come from?

This is where GitLab Pipeline Execution Policies come into play. They enable injection of YAML code into a pipeline. How can we leverage injected YAML to execute this shell script? Here’s a code snippet showing how to do this.

```
`## With this config, the goal is to create a pre-check job that evaluates the pipeline and fails the job/pipeline if any checks do not pass`

`variables:`  
  `GITLAB_API_URL: "https://gitlab.com/api/v4"`  
  `PROJECT_ID: $CI_PROJECT_ID`  
  `PIPELINE_ID: $CI_PIPELINE_ID`  
  `APPROVED_STAGES: ".pipeline-policy-pre,pre_check,build,test,deploy"`  
  `APPROVED_JOBS: "pre_check,build_job,test_job,deploy_job"`

`pre_check:`  
  `stage: .pipeline-policy-pre`  
  `script:`  
    `- curl -H "PRIVATE-TOKEN:${REPO_ACCESS_TOKEN}" --url "https://<gitlab_URL>/api/v4/projects/<project_id>/repository/files/check_settings.sh/raw" -o pre-check.sh`  
    `- ls -l`  
    `- chmod +x pre-check.sh`  
    `- DEBUG_MODE=false ./pre-check.sh  # Set DEBUG_MODE to true or false`  
  `allow_failure: true`  
```

In this YAML snippet, we set a few variables used in the shell script. Most importantly, this is where approved stages and approved jobs are defined. After the `variables` section, we then add a new job to the `.pipeline-policy-pre` stage. This is a reserved stage for pipeline execution policies and is guaranteed to execute before any stages defined in a `.gitlab-ci.yml` file. There is a corresponding `.pipeline-policy-post` stage as well, though we will not be using it in this scenario.

The script portion of the job does the actual work. Here, we leverage a curl command to execute the shell script defined above. This example includes authentication if it’s located in a private repository. However, if it’s publicly accessible, you can forgo this authentication. The last line controls whether or not the pipeline will fail. In this example, the pipeline will continue. This is useful for testing – in practice, you would likely set `allow_failure: false` to cause the pipeline to fail. This is desired as the goal of this exercise is to not allow pipelines to continue execution if a developer adds a rogue job or stage.

To utilize this YAML, save it to a `.yml` file in a repository of your choice. We’ll see how to connect it to a policy shortly.

Now, we have our script and our YAML to inject into a developer’s pipeline. Next, let’s see how to put this together using a pipeline execution policy.

Like creating other policies in GitLab, start by creating a new Pipeline Execution Policy by navigating to **Secure > Policies** in the left hand navigation menu. Then, choose **New Policy** at the top right, and select **Pipeline Execution Policy** from the policy creation options.

For this exercise, you can leave the **Policy Scope** set to the default options. In the **Actions** section, be sure to choose **Inject** and select the project and file where you’ve saved your YAML code snippet. Click on **Update via Merge Request** at the very bottom to create an MR that you can then merge into your project.

If this is your first security policy, clicking on **Merge** in the MR will create a Security Policy Project, which is a project to store all security policies. When implementing any type of security policy in a production environment, access to this project should be restricted so developers cannot make changes to security policies. In fact, you may also want to consider storing YAML code that’s used by pipeline execution policies in this project to restrict access as well, though this is not a requirement.  
Executing a pipeline where this pipeline execution policy is enabled should result in the following output when you attempt to add an invalid stage to the project `.gitlab-ci.yml` file.

![Output of attempting an invalid stage to project gitlab-ci.yml file](https://images.ctfassets.net/r9o86ar0p03f/5ahf1xaxSEDbT5rKGc2T0C/19340d225c9581569a6804a771f236da/image1.png)

While this use case is very focused on one aspect of security and compliance in your organization, this opens the door to other use cases. For example, you may want to make group-level variables accessible to every project within a group; this is possible with pipeline execution policies. Or, you may want to create a golden pipeline and have developers add to it. The possibilities are endless. GitLab customers are finding new and exciting ways to use this new functionality every day.

If you’re a GitLab Ultimate customer, try this out today and let us know how you’re using pipeline execution policies. Not a GitLab Ultimate customer? Sign up for a free 60-day trial to get started.

## Read more

- How to integrate custom security scanners into GitLab
- Integrate external security scanners into your DevSecOps workflow
- Why GitLab is deprecating compliance pipelines in favor of security policies

Go to Source
