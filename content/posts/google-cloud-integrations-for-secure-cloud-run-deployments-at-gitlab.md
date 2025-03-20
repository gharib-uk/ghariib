---
title: "<div>Google Cloud integrations for secure Cloud Run deployments at GitLab</div>"
date: 2025-01-17
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

_This tutorial is from a recent Arctiq, GitLab, and Google in-person workshop. The goal was to explore common security challenges faced by organizations as they journey to the cloud._

This tutorial will help you learn about the Google Cloud integrations in GitLab. These features are meant to help accelerate and improve security of deployments to Google Cloud.

![Google integrations list](https://images.ctfassets.net/r9o86ar0p03f/75hahkyZZ8T7tCKDuTZ4eI/6ba8dbc29e97c1440d201b5f9794075e/image2.png)

## Prerequisites

1. Google Cloud project
2. Appropriate IAM permissions for security, Artifact Registry, and Cloud Run usage. For this tutorial, ensure you have the "Owner" role with the aforementioned project.

## Setting up Workload Identity Federation

In this step, we configure GitLab to connect Google Cloud's Workload Identity Federation to reduce the need for service accounts and let the two platforms use short-lived credentials on-demand.

1. On the left sidebar, select **Search** or go to and find your group or project. If you configure this in a group, settings apply to all projects within by default.
2. Select **Settings > Integrations**.
3. Select **Google Cloud IAM**.
4. Input the Project ID and Project number in the respective fields. This information can be obtained from the Google Cloud console Welcome page of your project.
5. Input the desired Pool ID and Provider ID in the respective fields. These are values that you provide and must be unique from other Pool and Provider IDs.
6. Copy the generated command and then go to the **Google Cloud console**.
7. Run **Cloud Shell** and execute the generated command from the Workload Identity Federation integration page.
8. Once successful, the **Google Cloud IAM** integration will be designated as active in the Integrations list at the GitLab project.

## Artifact Registry configuration

As an alternative to GitLab's own place to host artifacts, deploying to Google Cloud's Artifact Registry is another way to leverage their infrastructure. This section will provide steps on how to use GitLab's native integration with Artifact Registry. Note that Workload Identity Federation must already be configured prior to this.

1. At the **Google Cloud** console, go to **Artifact Registry** via search or the main navigation.
2. Create a new repository by clicking the **"+"** icon. At the creation page, provide a name and keep the **Docker** format and **Standard** mode selected. Select **Region** and choose **us-central1**. Leave the rest at the default settings and click **Create**.
3. Once the repository is created and confirmed, go back to your GitLab project.
4. In your GitLab project, on the left sidebar, select **Settings > Integrations**. Then select **Google Artifact Registry**.
5. Under Enable integration, select the **Active** checkbox, then complete the fields:
    - Google Cloud project ID: The ID of the Google Cloud project where your Artifact Registry repository is located.
    - Repository name: The name of your Artifact Registry repository.
    - Repository location: The location of your Artifact Registry repository. (`us-central1` is assumed.)
6. In **Configure Google Cloud IAM policies**, follow the onscreen instructions to set up the IAM policies in Google Cloud. These policies are required to use the Artifact Registry repository in your GitLab project. Select **Save** changes.
7. To view your Google Cloud artifacts, on the left sidebar, select **Deploy > Google Artifact Registry**.

## Cloud Run configuration

1. Enable the Cloud Run API, if not done already. Go to **APIs & Services > Enabled APIs & Services**. From there, click **Enable APIs & Services** at the top and search for **Cloud Run Admin API**. Select the search result and enable the API.
2. Configure the IAM policies in Google Cloud to grant permissions to allow the Cloud Run CI/CD component to deploy to Cloud Run.

```
GCP_PROJECT_ID="<PROJECT ID>"
GCP_PROJECT_NUMBER="<PROJECT NUMBER>"
GCP_WORKLOAD_IDENTITY_POOL="<POOL ID>"

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/run.admin'

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/iam.serviceAccountUser'

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/cloudbuild.builds.editor'
```

## Deploy to Cloud Run

In this section, you will use Gitlab's CI/CD components to deploy to Cloud Run, Google Cloud's serverless runtime for containers.

1. Go to the GitLab project and from the list of files in the source code, find `.gitlab-ci.yaml`. Click the **file name** and the single file editor will show up. Click the **Edit** button and select the **Open in Web IDE** option.
2. In Web IDE, copy-paste the following code:

```
stages:
    - build
    - upload
    - deploy
```

This code snippet sets up three stages in the pipeline: build, upload, and deploy.

1. The next step is to create two CI/CD variables in the same YAML file:

```
variables:
    GITLAB_IMAGE: $CI_REGISTRY_IMAGE/main:$CI_COMMIT_SHORT_SHA
    AR_IMAGE: $GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_LOCATION-docker.pkg.dev/$GOOGLE_ARTIFACT_REGISTRY_PROJECT_ID/$GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_NAME/main:$CI_COMMIT_SHORT_SHA
```

The first variable, `GITLAB_IMAGE`, denotes the container image that the pipeline creates by default. The second one, `AR_IMAGE`, denotes the location at Google Cloud's Artifact Registry where the container image will be pushed to.

2. Next, define the code that will build the container image:

```
build:
    image: docker:24.0.5
    stage: build
    services:
        - docker:24.0.5-dind
    before_script:
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    script:
        - docker build -t $GITLAB_IMAGE .
        - docker push $GITLAB_IMAGE
```

This code uses pre-defined CI/CD variables for the Docker commands.

3. The final step is using two CI/CD components to deploy to Google Cloud. The first component integrates with Artifact Registry and the second is the deployment to Cloud Run:

```
include:
    - component: gitlab.com/google-gitlab-components/artifact-registry/upload-artifact-registry@main
      inputs:
        stage: upload
        source: $GITLAB_IMAGE
        target: $AR_IMAGE

    - component: gitlab.com/google-gitlab-components/cloud-run/deploy-cloud-run@main
      inputs:
        stage: deploy
        project_id: "<PROJECT_ID>"
        service: "tanuki-racing"
        region: "<REGION>"
        image: $AR_IMAGE
```

Replace <PROJECT\_ID> with your Google Cloud Project ID. Replace with the Google Cloud region most appropriate to your location. `us-central1` is assumed.

Commit the changes and push to the main branch. For reference, the final `.gitlab-ci.yaml` should look like this, noting to replace the <PROJECT ID> and <REGION> with the appropriate values:

```
stages:
    - build
    - upload
    - deploy
variables:
    GITLAB_IMAGE: $CI_REGISTRY_IMAGE/main:$CI_COMMIT_SHORT_SHA
    AR_IMAGE: $GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_LOCATION-docker.pkg.dev/$GOOGLE_ARTIFACT_REGISTRY_PROJECT_ID/$GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_NAME/main:$CI_COMMIT_SHORT_SHA

build:
    image: docker:24.0.5
    stage: build
    services:
        - docker:24.0.5-dind
    before_script:
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    script:
        - docker build -t $GITLAB_IMAGE .
        - docker push $GITLAB_IMAGE

include:
    - component: gitlab.com/google-gitlab-components/artifact-registry/upload-artifact-registry@main
      inputs:
        stage: upload
        source: $GITLAB_IMAGE
        target: $AR_IMAGE

    - component: gitlab.com/google-gitlab-components/cloud-run/deploy-cloud-run@main
      inputs:
        stage: deploy
        project_id: "<PROJECT_ID>"
        service: "tanuki-racing"
        region: "<REGION>"
        image: $AR_IMAGE
```

1. Go back to the main GitLab project and view the pipeline that was just initiated. Take note of the stages that should be the same stages that were defined in Step 2.
2. Once the pipeline is complete, go to the Google Cloud console and then **Cloud Run** via search or navigation. A new Cloud Run service called `tanuki-racing` should be created.
3. Click the **service name** and then go to the **Security** tab. Ensure that the service is set to **Allow unauthenticated invocations**. This will make the deployed app publicly available. The app URL posted on screen is now available and should open a new browser tab when clicked.

By utilizing GitLab’s CI/CD pipelines to build and push a containerized application to Google Artifact Registry, you can see the power of GitLab’s AI-powered DevSecOps Platform as a means to building secure applications. GitLab also deployed the containerized application to Google’s Cloud Run as a low-cost running application on the public internet. Using GitLab to instrument building an application, pushing a container and triggering a cloud run deployment allows DevOps engineers to have the assurance that secure applications are being run on the public-facing internet.

> Sign up for a 60-day free trial of GitLab Ultimate to begin working with these integrations. Also, check out our solutions architecture area for more Gitlab and Google Cloud tutorials.

_This tutorial is from a recent Arctiq, GitLab, and Google in-person workshop. The goal was to explore common security challenges faced by organizations as they journey to the cloud._

This tutorial will help you learn about the Google Cloud integrations in GitLab. These features are meant to help accelerate and improve security of deployments to Google Cloud.

![Google integrations list](https://images.ctfassets.net/r9o86ar0p03f/75hahkyZZ8T7tCKDuTZ4eI/6ba8dbc29e97c1440d201b5f9794075e/image2.png)

## Prerequisites

1. Google Cloud project
2. Appropriate IAM permissions for security, Artifact Registry, and Cloud Run usage. For this tutorial, ensure you have the "Owner" role with the aforementioned project.

## Setting up Workload Identity Federation

In this step, we configure GitLab to connect Google Cloud's Workload Identity Federation to reduce the need for service accounts and let the two platforms use short-lived credentials on-demand.

1. On the left sidebar, select **Search** or go to and find your group or project. If you configure this in a group, settings apply to all projects within by default.
2. Select **Settings > Integrations**.
3. Select **Google Cloud IAM**.
4. Input the Project ID and Project number in the respective fields. This information can be obtained from the Google Cloud console Welcome page of your project.
5. Input the desired Pool ID and Provider ID in the respective fields. These are values that you provide and must be unique from other Pool and Provider IDs.
6. Copy the generated command and then go to the **Google Cloud console**.
7. Run **Cloud Shell** and execute the generated command from the Workload Identity Federation integration page.
8. Once successful, the **Google Cloud IAM** integration will be designated as active in the Integrations list at the GitLab project.

## Artifact Registry configuration

As an alternative to GitLab's own place to host artifacts, deploying to Google Cloud's Artifact Registry is another way to leverage their infrastructure. This section will provide steps on how to use GitLab's native integration with Artifact Registry. Note that Workload Identity Federation must already be configured prior to this.

1. At the **Google Cloud** console, go to **Artifact Registry** via search or the main navigation.
2. Create a new repository by clicking the **"+"** icon. At the creation page, provide a name and keep the **Docker** format and **Standard** mode selected. Select **Region** and choose **us-central1**. Leave the rest at the default settings and click **Create**.
3. Once the repository is created and confirmed, go back to your GitLab project.
4. In your GitLab project, on the left sidebar, select **Settings > Integrations**. Then select **Google Artifact Registry**.
5. Under Enable integration, select the **Active** checkbox, then complete the fields:
    - Google Cloud project ID: The ID of the Google Cloud project where your Artifact Registry repository is located.
    - Repository name: The name of your Artifact Registry repository.
    - Repository location: The location of your Artifact Registry repository. (`us-central1` is assumed.)
6. In **Configure Google Cloud IAM policies**, follow the onscreen instructions to set up the IAM policies in Google Cloud. These policies are required to use the Artifact Registry repository in your GitLab project. Select **Save** changes.
7. To view your Google Cloud artifacts, on the left sidebar, select **Deploy > Google Artifact Registry**.

## Cloud Run configuration

1. Enable the Cloud Run API, if not done already. Go to **APIs & Services > Enabled APIs & Services**. From there, click **Enable APIs & Services** at the top and search for **Cloud Run Admin API**. Select the search result and enable the API.
2. Configure the IAM policies in Google Cloud to grant permissions to allow the Cloud Run CI/CD component to deploy to Cloud Run.

```
GCP_PROJECT_ID="<PROJECT ID>"
GCP_PROJECT_NUMBER="<PROJECT NUMBER>"
GCP_WORKLOAD_IDENTITY_POOL="<POOL ID>"

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/run.admin'

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/iam.serviceAccountUser'

gcloud projects add-iam-policy-binding ${GCP_PROJECT_ID} 
  --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/${GCP_WORKLOAD_IDENTITY_POOL}/attribute.developer_access/true" 
  --role='roles/cloudbuild.builds.editor'
```

## Deploy to Cloud Run

In this section, you will use Gitlab's CI/CD components to deploy to Cloud Run, Google Cloud's serverless runtime for containers.

1. Go to the GitLab project and from the list of files in the source code, find `.gitlab-ci.yaml`. Click the **file name** and the single file editor will show up. Click the **Edit** button and select the **Open in Web IDE** option.
2. In Web IDE, copy-paste the following code:

```
stages:
    - build
    - upload
    - deploy
```

This code snippet sets up three stages in the pipeline: build, upload, and deploy.

1. The next step is to create two CI/CD variables in the same YAML file:

```
variables:
    GITLAB_IMAGE: $CI_REGISTRY_IMAGE/main:$CI_COMMIT_SHORT_SHA
    AR_IMAGE: $GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_LOCATION-docker.pkg.dev/$GOOGLE_ARTIFACT_REGISTRY_PROJECT_ID/$GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_NAME/main:$CI_COMMIT_SHORT_SHA
```

The first variable, `GITLAB_IMAGE`, denotes the container image that the pipeline creates by default. The second one, `AR_IMAGE`, denotes the location at Google Cloud's Artifact Registry where the container image will be pushed to.

2. Next, define the code that will build the container image:

```
build:
    image: docker:24.0.5
    stage: build
    services:
        - docker:24.0.5-dind
    before_script:
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    script:
        - docker build -t $GITLAB_IMAGE .
        - docker push $GITLAB_IMAGE
```

This code uses pre-defined CI/CD variables for the Docker commands.

3. The final step is using two CI/CD components to deploy to Google Cloud. The first component integrates with Artifact Registry and the second is the deployment to Cloud Run:

```
include:
    - component: gitlab.com/google-gitlab-components/artifact-registry/upload-artifact-registry@main
      inputs:
        stage: upload
        source: $GITLAB_IMAGE
        target: $AR_IMAGE

    - component: gitlab.com/google-gitlab-components/cloud-run/deploy-cloud-run@main
      inputs:
        stage: deploy
        project_id: "<PROJECT_ID>"
        service: "tanuki-racing"
        region: "<REGION>"
        image: $AR_IMAGE
```

Replace <PROJECT\_ID> with your Google Cloud Project ID. Replace with the Google Cloud region most appropriate to your location. `us-central1` is assumed.

Commit the changes and push to the main branch. For reference, the final `.gitlab-ci.yaml` should look like this, noting to replace the <PROJECT ID> and <REGION> with the appropriate values:

```
stages:
    - build
    - upload
    - deploy
variables:
    GITLAB_IMAGE: $CI_REGISTRY_IMAGE/main:$CI_COMMIT_SHORT_SHA
    AR_IMAGE: $GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_LOCATION-docker.pkg.dev/$GOOGLE_ARTIFACT_REGISTRY_PROJECT_ID/$GOOGLE_ARTIFACT_REGISTRY_REPOSITORY_NAME/main:$CI_COMMIT_SHORT_SHA

build:
    image: docker:24.0.5
    stage: build
    services:
        - docker:24.0.5-dind
    before_script:
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    script:
        - docker build -t $GITLAB_IMAGE .
        - docker push $GITLAB_IMAGE

include:
    - component: gitlab.com/google-gitlab-components/artifact-registry/upload-artifact-registry@main
      inputs:
        stage: upload
        source: $GITLAB_IMAGE
        target: $AR_IMAGE

    - component: gitlab.com/google-gitlab-components/cloud-run/deploy-cloud-run@main
      inputs:
        stage: deploy
        project_id: "<PROJECT_ID>"
        service: "tanuki-racing"
        region: "<REGION>"
        image: $AR_IMAGE
```

1. Go back to the main GitLab project and view the pipeline that was just initiated. Take note of the stages that should be the same stages that were defined in Step 2.
2. Once the pipeline is complete, go to the Google Cloud console and then **Cloud Run** via search or navigation. A new Cloud Run service called `tanuki-racing` should be created.
3. Click the **service name** and then go to the **Security** tab. Ensure that the service is set to **Allow unauthenticated invocations**. This will make the deployed app publicly available. The app URL posted on screen is now available and should open a new browser tab when clicked.

By utilizing GitLab’s CI/CD pipelines to build and push a containerized application to Google Artifact Registry, you can see the power of GitLab’s AI-powered DevSecOps Platform as a means to building secure applications. GitLab also deployed the containerized application to Google’s Cloud Run as a low-cost running application on the public internet. Using GitLab to instrument building an application, pushing a container and triggering a cloud run deployment allows DevOps engineers to have the assurance that secure applications are being run on the public-facing internet.

> Sign up for a 60-day free trial of GitLab Ultimate to begin working with these integrations. Also, check out our solutions architecture area for more Gitlab and Google Cloud tutorials.

Go to Source
