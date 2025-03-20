---
title: "Accelerate Your Docker Builds Using AWS CodeBuild and Docker Build Cloud"
date: 2025-01-02
categories: 
  - "general"
---

Containerized application development has revolutionized modern software delivery, but slow image builds in CI/CD pipelines can bring developer productivity to a halt. Even with AWS CodeBuild automating application testing and building, teams face challenges like resource constraints, inefficient caching, and complex multi-architecture builds that lead to delays, lower release frequency, and prolonged recovery times.

Enter Docker Build Cloud, a high-performance cloud service designed to streamline image builds, integrate seamlessly with AWS CodeBuild, and reduce build times dramatically. With Docker Build Cloud, you gain powerful cloud-based builders, shared caching, and native multi-architecture support — all while keeping your CI/CD pipelines efficient and your developers focused on delivering value faster.

In this post, we’ll explore how AWS CodeBuild combined with Docker Build Cloud tackles common bottlenecks, boosts build performance, and simplifies workflows, enabling teams to ship more quickly and reliably.

![2400x1260 generic dbc blog e](https://www.docker.com/wp-content/uploads/2024/10/2400x1260_generic-dbc-blog_e-1110x583.png "- 2400x1260 generic dbc blog e")

By using AWS CodeBuild, you can automate the build and testing of container applications, enabling the construction of efficient CI/CD workflows. AWS CodeBuild is also integrated with AWS Identity and Access Management (IAM), allowing detailed configuration of access permissions for build processes and control over AWS resources.

Container images built with AWS CodeBuild can be stored in Amazon Elastic Container Registry (Amazon ECR) and deployed to various AWS services, such as Amazon Elastic Container Service (Amazon ECS), Amazon Elastic Kubernetes Service (Amazon EKS), AWS Fargate, or AWS Lambda (Figure 1). Additionally, these services can leverage AWS Graviton, which adopts Arm-based architectures, to improve price performance for compute workloads.

<figure>

![Illustration of CI/CD pipeline outlining steps for check in code, source code commit, build code, and deploy code.](https://www.docker.com/wp-content/uploads/2025/01/F1-CI-CD-pipeline-1110x971.png "Illustration of CI/CD pipeline outlining steps for check in code, source code commit, build code, and deploy code. - F1 CI CD pipeline")

<figcaption>

**Figure 1:** CI/CD pipeline for AWS ECS using AWS CodeBuild (ECS Workshop).

</figcaption>

</figure>

## Challenges of container image builds with AWS CodeBuild

Regardless of the tool used, building container images in a CI pipeline often takes a significant amount of time. This can lead to the following issues:

- Reduced development productivity
- Lower release frequency
- Longer recovery time in case of failures

The main reasons why build times can be extended include:

### 1\. Machines for building

Building container images requires substantial resources (CPU, RAM). If the machine specifications used in the CI pipeline are inadequate, build times can increase.

For simple container image builds, the impact may be minimal, but in cases of multi-stage builds or builds with many dependencies, the effect can be significant.

AWS CodeBuild allows changing instance types to improve these situations. However, such changes can apply to parts of the pipeline beyond container image builds, and they also increase costs.

Developers need to balance cost and build speed to optimize the pipeline.

### 2\. Container image cache

In local development environments, Docker’s build cache can shorten rebuild times significantly by reusing previously built layers, avoiding redundant processing for unchanged parts of the Dockerfile. However, in cloud-based CI services, clean environments are used by default, so cache cannot be utilized, resulting in longer build times.

Although there are ways to use storage or container registries to leverage caching, these often are not employed because they introduce complexity in configuration and overhead from uploading and downloading cache data.

### 3\. Multi-architecture builds (AMD64, Arm64)

To use Arm-based architectures like AWS Graviton in Amazon EKS or Amazon ECS, Arm64-compatible container image builds are required.

With changes in local environments, such as Apple Silicon, cases requiring multi-architecture support for AMD64 and Arm64 have increased. However, building images for different architectures (for example, building x86 on Arm, or vice versa) often requires emulation, which can further increase build times (Figure 2).

Although AWS CodeBuild provides both AMD64 and Arm64 instances, running them as separate pipelines is necessary, leading to more complex configurations and operations.

<figure>

![Illustration of steps for creating multi-architecture Docker images including Build and push, Test, Build/push multi-arch manifest, Deploy.](https://www.docker.com/wp-content/uploads/2025/01/F2-multi-arch-images.png "Illustration of steps for creating multi-architecture Docker images including Build and push, Test, Build/push multi-arch manifest, Deploy. - F2 multi arch images")

<figcaption>

**Figure 2:** Creating multi-architecture Docker images using AWS CodeBuild.

</figcaption>

</figure>

## Accelerating container image builds with Docker Build Cloud

The Docker Build Cloud service executes the Docker image build process in the cloud, significantly reducing build time and improving developer productivity (Figure 3).

<figure>

![Illustration of how Docker Build Cloud works, showing CI Runner/CI job, Local Machine, and Cloud Builder elements.](https://www.docker.com/wp-content/uploads/2025/01/F3-Docker-Build-Cloud.jpg "Illustration of how Docker Build Cloud works, showing CI Runner/CI job, Local Machine, and Cloud Builder elements. - F3 Docker Build Cloud")

<figcaption>

**Figure 3:** How Docker Build Cloud works.

</figcaption>

</figure>

Particularly in CI pipelines, Docker Build Cloud enables faster container image builds without the need for significant changes or migrations to existing pipelines.

Docker Build Cloud includes the following features:

- **High-performance cloud builders:** Cloud builders equipped with 16 vCPUs and 32GB RAM are available. This allows for faster builds compared to local environments or resource-constrained CI services.
- **Shared cache utilization:** Cloud builders come with 200 GiB of shared cache, significantly reducing build times for subsequent builds. This cache is available without additional configuration, and Docker Build Cloud handles the cache maintenance for you.
- **Multi-architecture support (AMD64, Arm64):** Docker Build Cloud supports native builds for multi-architecture with a single command. By specifying `--platform linux/amd64,linux/arm64` in the `docker buildx build` command or using Bake, images for both Arm64 and AMD64 can be built simultaneously. This approach eliminates the need to split the pipeline for different architectures.

## Architecture of AWS CodeBuild + Docker Build Cloud

Figure 4 shows an example of how to use Docker Build Cloud to accelerate container image builds in AWS CodeBuild:

<figure>

![Illustration of of AWS CodeBuild pipeline showing flow from Source Code to AWS CodeBuild, to Docker Build Cloud to Amazon ECR.](https://www.docker.com/wp-content/uploads/2025/01/F4-AWS-CodeBuild-pipeline-1110x347.jpg "Illustration of of AWS CodeBuild pipeline showing flow from Source Code to AWS CodeBuild, to Docker Build Cloud to Amazon ECR. - F4 AWS CodeBuild pipeline")

<figcaption>

**Figure 4:** AWS CodeBuild + Docker Build Cloud architecture.

</figcaption>

</figure>

1. The AWS CodeBuild pipeline is triggered from a commit to the source code repository (AWS CodeCommit, GitHub, GitLab).
2. Preparations for running Docker Build Cloud are made in AWS CodeBuild (Buildx installation, specifying Docker Build Cloud builders).
3. Container images are built on Docker Build Cloud’s AMD64 and Arm64 cloud builders.
4. The built AMD64 and Arm64 container images are pushed to Amazon ECR.

## Setting up Docker Build Cloud

First, set up Docker Build Cloud. (Note that new Docker subscriptions already include a free tier for Docker Build Cloud.)

Then, log in with your Docker account and visit the Docker Build Cloud Dashboard to create new cloud builders.

Once the builder is successfully created, a guide is displayed for using it in local environments (Docker Desktop, CLI) or CI/CD environments (Figure 5).

<figure>

![Screenshot from Docker Build Cloud showing setup instructions with local installation selected.](https://www.docker.com/wp-content/uploads/2025/01/F5-Setup-instructions-1110x802.png "Screenshot from Docker Build Cloud showing setup instructions with local installation selected. - F5 Setup instructions")

<figcaption>

**Figure 5:** Setup instructions of Docker Build Cloud.

</figcaption>

</figure>

Additionally, to use Docker Build Cloud from AWS CodeBuild, a Docker personal access token (PAT) is required. Store this token in AWS Secrets Manager for secure access.

## Setting up the AWS CodeBuild pipeline

Next, set up the AWS CodeBuild pipeline. You should prepare an Amazon ECR repository to store the container images beforehand.

The following settings are used to create the AWS CodeBuild pipeline:

- AMD64 instance with 3GB memory and 2 vCPUs.
- Service role with permissions to push to Amazon ECR and access the Docker personal access token from AWS Secrets Manager.

The `buildspec.yml` file is configured as follows:

```
version: 0.2

env:
  variables:
    ARCH: amd64
    ECR_REGISTRY: [ECR Registry]
    ECR_REPOSITORY: [ECR Repository]
    DOCKER_ORG: [Docker Organization]
  secrets-manager:
    DOCKER_USER: ${SECRETS_NAME}:DOCKER_USER
    DOCKER_PAT: ${SECRETS_NAME}:DOCKER_PAT

phases:
  install:
    commands:
      # Installing Buildx
      - BUILDX_URL=$(curl -s https://raw.githubusercontent.com/docker/actions-toolkit/main/.github/buildx-lab-releases.json | jq -r ".latest.assets[] | select(endswith("linux-$ARCH"))")
      - mkdir -vp ~/.docker/cli-plugins/
      - curl --silent -L --output ~/.docker/cli-plugins/docker-buildx $BUILDX_URL
      - chmod a+x ~/.docker/cli-plugins/docker-buildx

  pre_build:
    commands:
      # Logging in to Amazon ECR
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY
      # Logging in to Docker (Build Cloud)
      - echo "$DOCKER_PAT" | docker login --username $DOCKER_USER --password-stdin
      # Specifying the cloud builder
      - docker buildx create --use --driver cloud $DOCKER_ORG/demo

  build:
    commands:
      # Image tag
      - IMAGE_TAG=$(echo ${CODEBUILD_RESOLVED_SOURCE_VERSION} | head -c 7)
      # Build container image & push to Amazon ECR
      - docker buildx build --platform linux/amd64,linux/arm64 --push --tag "${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}" .
```

In the `install` phase, Buildx, which is necessary for using Docker Build Cloud, is installed.

Although Buildx may already be installed in AWS CodeBuild, it might be an unsupported version for Docker Build Cloud. Therefore, it is recommended to install the latest version.

In the `pre_build` phase, the following steps are performed:

- Log in to Amazon ECR.
- Log in to Docker (Build Cloud).
- Specify the cloud builder.

In the `build` phase, the image tag is specified, and the container image is built and pushed to Amazon ECR.

Instead of separating the build and push commands, using `--push` to directly push the image to Amazon ECR helps avoid unnecessary file transfers, contributing to faster builds.

## Results comparison

To make a comparison, an AWS CodeBuild pipeline without Docker Build Cloud is created. The same instance type (AMD64, 3GB memory, 2vCPU) is used, and the build is limited to AMD64 container images.

Additionally, Docker login is used to avoid the pull rate limit imposed by Docker Hub.

```
version: 0.2

env:
  variables:
    ECR_REGISTRY: [ECR Registry]
    ECR_REPOSITORY: [ECR Repository]
  secrets-manager:
    DOCKER_USER: ${SECRETS_NAME}:DOCKER_USER
    DOCKER_PAT: ${SECRETS_NAME}:DOCKER_PAT

phases:
  pre_build:
    commands:
      # Logging in to Amazon ECR
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY
      # Logging in to Docker
      - echo "$DOCKER_PAT" | docker login --username $DOCKER_USER --password-stdin

  build:
    commands:
      # Image tag
      - IMAGE_TAG=$(echo ${CODEBUILD_RESOLVED_SOURCE_VERSION} | head -c 7)
      # Build container image & push to Amazon ECR
      - docker build --push --tag "${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}" .
```

Figure 6 shows the result of the execution:

<figure>

![Screenshot of results using AWS CodeBuild pipeline without Docker Build Cloud, showing execution time of 5 minutes and 59 seconds.](https://www.docker.com/wp-content/uploads/2025/01/F6-Execution-time-without-Build-Cloud-1110x199.png "Screenshot of results using AWS CodeBuild pipeline without Docker Build Cloud, showing execution time of 5 minutes and 59 seconds. - F6 Execution time without Build Cloud")

<figcaption>

**Figure 6:** The result of the execution without Docker Build Cloud.

</figcaption>

</figure>

Figure 7 shows the execution result of the AWS CodeBuild pipeline using Docker Build Cloud:

<figure>

![Screenshot of results using AWS CodeBuild pipeline with Docker Build Cloud, showing execution time of 1 minutes and 4 seconds.](https://www.docker.com/wp-content/uploads/2025/01/F7-Execution-time-with-Build-Cloud-1110x238.png "Screenshot of results using AWS CodeBuild pipeline with Docker Build Cloud, showing execution time of 1 minutes and 4 seconds. - F7 Execution time with Build Cloud")

<figcaption>

**Figure 7:** The result of the execution with Docker Build Cloud.

</figcaption>

</figure>

The results may vary depending on the container images being built and the state of the cache, but it was possible to build container images much faster and achieve multi-architecture builds (AMD64 and Arm64) within a single pipeline.

## Conclusion

Integrating Docker Build Cloud into a CI/CD pipeline using AWS CodeBuild can dramatically reduce build times and improve release frequency. This allows developers to maximize productivity while delivering value to users more quickly.

As mentioned previously, the new Docker subscription already includes a free tier for Docker Build Cloud. Take advantage of this opportunity to test how much faster you can build container images for your current projects.

## Learn more

- Subscribe to the Docker Newsletter. 
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, AWS, containers, Docker Build Cloud, Docker images
