---
title: "<div>From code to production: A guide to continuous deployment with GitLab</div>"
date: 2025-01-29
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Continuous deployment is a game-changing practice that enables teams to deliver value faster, with higher confidence. However, diving into advanced deployment workflows — such as GitOps, container orchestration with Kubernetes, or dynamic environments — can be intimidating for teams just starting out.

At GitLab, we're committed to making delivery seamless and scalable. By enabling teams to focus on the fundamentals, we empower them to build a strong foundation that supports growth into more complex strategies over time. This guide provides essential steps to begin implementing continuous deployment with GitLab, laying the foundation for your long-term success.

## Start with a workflow plan

Before diving into the technical implementation, take time to map out your deployment workflow. Success lies in careful planning and a methodical approach.

### Artifact management strategy

In the context of continuous deployment, artifacts are the packaged outputs of your build process that need to be stored, versioned, and deployed. These could be:

- container images for your applications
- packages
- compiled binaries or executables
- libraries
- configuration files
- documentation packages
- other artifacts

Each type of artifact plays a specific role in your deployment process. For example, a typical web application might generate:

- a container image for the backend service
- a ZIP archive of compiled frontend assets
- SQL files for database changes
- environment-specific configuration files

Managing these artifacts effectively is crucial for successful deployments. Here's how to approach artifact management.

#### Artifacts and releases versioning strategies

A best practice to get you started with a clean structure is to establish a clear versioning strategy for your artifacts. When creating releases:

- Use semantic versioning (major.minor.patch) for release tags
    - Example: `myapp:1.2.3` for a stable release
    - Major version changes (2.0.0) for breaking changes
    - Minor version changes (1.3.0) for new features
    - Patch version changes (1.2.4) for bug fixes
- Maintain a 'latest' tag for the most recent stable version
    - Example: `myapp:latest` for automated deployments
- Include commit SHA for precise version tracking
    - Example: `myapp:1.2.3-abc123f` for debugging
- Consider branch-based tags for development environments
    - Example: `myapp:feature-user-auth` for feature testing

#### Build artifacts retention

Implement defined retention rules:

- Set explicit expiration timeframes for temporary artifacts
- Define which artifacts need permanent retention
- Configure cleanup policies to manage storage

#### Registry access and authentication

Secure your artifacts with proper access controls:

- Implement Personal Access Tokens for developer access
- Configure CI/CD variables for pipeline authentication
- Set up proper access scopes

### Environment strategy

Consider your environments early, as they shape your entire deployment pipeline:

- Development, staging, and production environment configurations
- Environment-specific variables and secrets
- Access controls and protection rules
- Deployment tracking and monitoring approach

### Deployment targets

Be intentional as to where and how you'll deploy, these decisions matter and the benefits and drawbacks of each should be consider:

- Infrastructure requirements (VMs, containers, cloud services)
- Network access and security configurations
- Authentication mechanisms (SSH keys, access tokens)
- Resource allocation and scaling considerations

With our strategy defined and foundational decisions made, we can now translate these plans into a working pipeline. We'll build a practical example that demonstrates these concepts, starting with a simple application and progressively adding deployment capabilities.

## Implementing your CD pipeline

### A step-by-step example

Let's walk through implementing a basic continuous deployment pipeline for a web application. We'll use a simple HTML application as an example, but these principles apply to any type of application. We’re also going to deploy our application as a Docker image on a simple virtual machine. This will allow us to lean on a curated image with minimum dependencies, and to ensure no environment specific requirements are unintentionally brought in. By working on a virtual machine, we won’t be leveraging GitLab’s native integrations, allowing us to work on an easier but less scalable setup to begin with.

#### Prerequisites

In this example, we’ll aim to containerize an application that we’ll run on a virtual machine hosted on a cloud provider. We’ll also test this application locally on our machine. This list of prerequisites is only needed for this scenario.

##### Virtual machine setup

- Provision a VM in your preferred cloud provider (e.g., GCP, AWS, Azure)
- Configure network rules to allow access on ports 22, 80, and 443
- Record the machine's public IP address for deployment

##### Set up SSH authentication:

- Generate a public/private key pair for the machine
- In GitLab, go to **Settings > CI/CD > Variables**
- Create a variable called `GITLAB_KEY`
- Set Type to "File" (required for SSH authentication)
- Paste the private key in the Value field
- Define a USER variable, this is the user logging in and running the scripts on your VM

##### Configure deployment variables

- Create variables for your deployment targets:
    - `STAGING_TARGET`: Your staging server IP/domain
    - `PRODUCTION_TARGET`: Your production server IP/domain

##### Local development setup

- Install Docker on your local machine for testing deployments

##### GitLab Container Registry access

- Locate your registry path:
    - Navigate to **Deploy > Container Registry**
    - Copy the registry path (e.g., registry.gitlab.com/group/project)
- Set up authentication:
    - Go to **Settings > Access Tokens**
    - Create a new token with registry access
    - Token expiration: Maximum 1 year
    - Save the token securely
- Configure local registry access:

```
docker login registry.gitlab.com
# The username if you are using a PAT is gitlab-ci-token
# Password: your-access-token
```

#### 1\. Create your application

Start with a basic web application. For our example, we're using a simple HTML page:

```
<!-- index.html -->
<html>
  <head>
    <style>
      body {
        background-color: #171321; /* GitLab dark */
      }
    </style>
  </head>
  <body>
    <!-- Your content here -->
  </body>
</html>
```

#### 2\. Containerize your application

Create a Dockerfile to package your application:

```
FROM nginx:1.26.2
COPY index.html /usr/share/nginx/html/index.html
```

This Dockerfile:

- Uses nginx as a base image for serving web content
- Copies your HTML file to the correct location in the nginx directory structure

#### 3\. Set up your CI/CD pipeline

Create a `.gitlab-ci.yml` file to define your pipeline stages:

```
variables:
  TAG_LATEST: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:latest
  TAG_COMMIT: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:$CI_COMMIT_SHA

stages:
  - publish
  - deploy
```

Let's break it down:

`TAG_LATEST` is made up of three parts:

- `$CI_REGISTRY_IMAGE` is the path to your project's container registry in GitLab

For example: `registry.gitlab.com/your-group/your-project`

- `$CI_COMMIT_REF_NAME` is the name of your branch or tag

For example, if you're on main branch: `/main`, and if you're on a feature branch: `/feature-login`

- `:latest` is a fixed suffix

So if you're on the main branch, `TAG_LATEST` becomes: `registry.gitlab.com/your-group/your-project/main:latest`.

`TAG_COMMIT` is almost identical, but instead of `:latest`, it uses: `$CI_COMMIT_SHA` which is the commit identifier, for example: `:abc123def456`.

So for that same commit on main branch, `TAG_COMMIT` becomes:`registry.gitlab.com/your-group/your-project/main:abc123def456`.

The reason for having both is `TAG_LATEST` gives you an easy way to always get the newest version, and `TAG_COMMIT` gives you a specific version you can return to if needed.

#### 4\. Publish to the container registry

Add the publish job to your pipeline:

```
publish:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $TAG_LATEST -t $TAG_COMMIT .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $TAG_LATEST
    - docker push $TAG_COMMIT
```

This job:

- Uses Docker-in-Docker to build images
- Creates two tagged versions of your image
- Authenticates with the GitLab registry
- Pushes both versions to the registry

Now that our images are safely stored in the registry, we can focus on deploying them to our target environments. Let's start with local testing to validate our setup before moving to production deployments.

#### 5\. Deploy to your environment

Before deploying to production, you can test locally. We just published our image to the GitLab repository, which we’ll pull locally. If you’re unsure of the exact path, navigate to **Deploy > Container Registry**, and you should see an icon to copy the path of your image at the end of the line for the container image you want to test.

```
docker login registry.gitlab.com 
docker run -p 80:80 registry.gitlab.com/your-project-path/main:latest
```

By doing so you should be able to access your application locally on your localhost address through your web browser.

You can now add a deployment job to your pipeline:

```
deploy:
  stage: deploy
  image: alpine:latest
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$TARGET_SERVER 
      docker pull $TAG_COMMIT &&
      docker rm -f myapp || true &&
      docker run -d -p 80:80 --name myapp $TAG_COMMIT
```

This job:

- Sets up SSH access to your deployment target
- Pulls the latest image
- Removes any existing container
- Deploys the new version

#### 6\. Track deployments

Enable deployment tracking by adding environment configuration:

```
deploy:
  environment:
    name: production
    url: https://your-application-url.com 
```

This creates an environment object in GitLab's **Operate > Environments** section, providing:

- Deployment history
- Current deployment status
- Quick access to your application

While a single environment pipeline is a good starting point, most teams need to manage multiple environments for proper testing and staging. Let's expand our pipeline to handle this more realistic scenario.

#### 7\. Set up multiple environments

For a more robust pipeline, configure staging and production deployments:

```
stages:
  - publish
  - staging
  - release
  - version
  - production

staging:
  stage: staging
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  environment:
    name: staging
    url: https://staging.your-app.com
  # deployment script here

production:
  stage: production
  rules:
    - if: $CI_COMMIT_TAG
  environment:
    name: production
    url: https://your-app.com
  # deployment script here
```

This setup:

- Deploys to staging from your main branch
- Uses GitLab tags to trigger production deployments
- Provides separate tracking for each environment

Here and in our next step, we’re leveraging a very useful GitLab feature: tags. By manually creating a tag in the **Code > Tags** section, the `$CI_COMMIT_TAG` gets created, which allows us to trigger jobs accordingly.

#### 8\. Create automated release notes

We'll be using GitLab's release capabilities through our CI/CD pipeline. First, update your stages in `.gitlab-ci.yml`:

```
stages:

- publish
- staging
- release # New stage for releases
- version
- production
```

Next, add the release job:

```
release_job:
  stage: release
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG                  # Only run when a tag is created
  script:
    - echo "Creating release for $CI_COMMIT_TAG"
  release:                                # Release configuration
    name: 'Release $CI_COMMIT_TAG'
    description: 'Release created from $CI_COMMIT_TAG'
    tag_name: '$CI_COMMIT_TAG'           # The tag to create
    ref: '$CI_COMMIT_TAG'                # The tag to base release on
```

You can enhance this by adding links to your container images:

```
release:
  name: 'Release $CI_COMMIT_TAG'
  description: 'Release created from $CI_COMMIT_TAG'
  tag_name: '$CI_COMMIT_TAG'
  ref: '$CI_COMMIT_TAG'
  assets:
    links:
      - name: 'Container Image'
        url: '$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG'
        link_type: 'image'
```

For meaningful automated release notes:

- Use conventional commits (feat:, fix:, etc.)
- Include issue numbers (#123)
- Separate subject from body with blank line

If you want custom release notes with deployment info:

```
release_job:
  script:
    - |
      DEPLOY_TIME=$(date '+%Y-%m-%d %H:%M:%S')
      CHANGES=$(git log $(git describe --tags --abbrev=0 @^)..@ --pretty=format:"- %s")
      cat > release_notes.md << EOF
      ## Deployment Info
      - Deployed on: $DEPLOY_TIME
      - Environment: Production
      - Version: $CI_COMMIT_TAG

      ## Changes
      $CHANGES

      ## Artifacts
      - Container Image: `$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG`
      EOF
  release:
    description: './release_notes.md'
```

Once configured, releases will be created automatically when you create a Git tag. You can view them in GitLab under **Deploy > Releases**.

#### 9\. Put it all together

This is what our final YAML file looks like:

```
variables:
  TAG_LATEST: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:latest
  TAG_COMMIT: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:$CI_COMMIT_SHA
  STAGING_TARGET: $STAGING_TARGET    # Set in CI/CD Variables
  PRODUCTION_TARGET: $PRODUCTION_TARGET  # Set in CI/CD Variables

stages:
  - publish
  - staging
  - release
  - version
  - production

# Build and publish to registry
publish:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  script:
    - docker build -t $TAG_LATEST -t $TAG_COMMIT .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $TAG_LATEST
    - docker push $TAG_COMMIT

# Deploy to staging
staging:
  stage: staging
  image: alpine:latest
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$STAGING_TARGET "
        docker pull $TAG_COMMIT &&
        docker rm -f myapp || true &&
        docker run -d -p 80:80 --name myapp $TAG_COMMIT"
  environment:
    name: staging
    url: http://$STAGING_TARGET

# Create release
release_job:
  stage: release
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - |
      DEPLOY_TIME=$(date '+%Y-%m-%d %H:%M:%S')
      CHANGES=$(git log $(git describe --tags --abbrev=0 @^)..@ --pretty=format:"- %s")
      cat > release_notes.md << EOF
      ## Deployment Info
      - Deployed on: $DEPLOY_TIME
      - Environment: Production
      - Version: $CI_COMMIT_TAG

      ## Changes
      $CHANGES

      ## Artifacts
      - Container Image: `$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG`
      EOF
  release:
    name: 'Release $CI_COMMIT_TAG'
    description: './release_notes.md'
    tag_name: '$CI_COMMIT_TAG'
    ref: '$CI_COMMIT_TAG'
    assets:
      links:
        - name: 'Container Image'
          url: '$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG'
          link_type: 'image'

# Version the image with release tag
version_job:
  stage: version
  image: docker:latest
  services:
    - docker:dind
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - docker pull $TAG_COMMIT
    - docker tag $TAG_COMMIT $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG

# Deploy to production
production:
  stage: production
  image: alpine:latest
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$PRODUCTION_TARGET "
        docker pull $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG &&
        docker rm -f myapp || true &&
        docker run -d -p 80:80 --name myapp $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG"
  environment:
    name: production
    url: http://$PRODUCTION_TARGET
```

This complete pipeline:

- Publishes images to the registry (main branch)
- Deploys to staging (main branch)
- Creates releases (on tags)
- Versions images with release tags Deploys to production (on tags)

Key benefits:

- Clean reproducible, local development and testing environment
- Clear path to production environments with structure to build confidence in what is deployed
- Pattern to recover from unexpected failures, etc.
- Ready to scale/adopt more complex deployment strategies

### Best practices

Throughout implementation, maintain these principles:

- Document everything, from variable usage to deployment procedures
- Use GitLab's built-in features (environments, releases, registry)
- Implement proper access controls and security measures
- Plan for failure with robust rollback procedures
- Keep your pipeline configurations DRY (Don't Repeat Yourself)

## Scale your deployment strategy

What next? Here are some aspects to consider as your continuous deployment strategy matures.

### Advanced security measures

Enhance security through:

- Protected environments with restricted access
- Required approvals for production deployments
- Integrated security scanning
- Automated vulnerability assessments
- Branch protection rules for deployment-related changes

### Progressive delivery strategies

Implement advanced deployment strategies:

- Feature flags for controlled rollouts
- Canary deployments for risk mitigation
- Blue-green deployment strategies
- A/B testing capabilities
- Dynamic environment management

### Monitoring and optimization

Establish robust monitoring practices:

- Track deployment metrics
- Set up performance monitoring
- Configure deployment alerts
- Establish deployment SLOs
- Regular pipeline optimization

## Why GitLab?

GitLab's continuous deployment capabilities make it a standout choice for modern deployment workflows. The platform excels in streamlining the path from code to production, offering built-in container registry, environment management, and deployment tracking all within a single interface. GitLab's environment-specific variables, deployment approval gates, and rollback capabilities provide the security and control needed for production deployments, while features like review apps and feature flags enable progressive delivery approaches. As part of GitLab's complete DevSecOps platform, these CD capabilities seamlessly integrate with your entire software lifecycle.

## Get started today

The journey to continuous deployment is an evolution, not a revolution. Start with the fundamentals, build a solid foundation, and gradually incorporate advanced features as your team's needs grow. GitLab provides the tools and flexibility to support you at every stage of this journey, from your first automated deployment to complex, multi-environment delivery pipelines.

> Sign up for a free, 60-day trial of GitLab Ultimate to get started with continous deployment today.

Continuous deployment is a game-changing practice that enables teams to deliver value faster, with higher confidence. However, diving into advanced deployment workflows — such as GitOps, container orchestration with Kubernetes, or dynamic environments — can be intimidating for teams just starting out.

At GitLab, we're committed to making delivery seamless and scalable. By enabling teams to focus on the fundamentals, we empower them to build a strong foundation that supports growth into more complex strategies over time. This guide provides essential steps to begin implementing continuous deployment with GitLab, laying the foundation for your long-term success.

## Start with a workflow plan

Before diving into the technical implementation, take time to map out your deployment workflow. Success lies in careful planning and a methodical approach.

### Artifact management strategy

In the context of continuous deployment, artifacts are the packaged outputs of your build process that need to be stored, versioned, and deployed. These could be:

- container images for your applications
- packages
- compiled binaries or executables
- libraries
- configuration files
- documentation packages
- other artifacts

Each type of artifact plays a specific role in your deployment process. For example, a typical web application might generate:

- a container image for the backend service
- a ZIP archive of compiled frontend assets
- SQL files for database changes
- environment-specific configuration files

Managing these artifacts effectively is crucial for successful deployments. Here's how to approach artifact management.

#### Artifacts and releases versioning strategies

A best practice to get you started with a clean structure is to establish a clear versioning strategy for your artifacts. When creating releases:

- Use semantic versioning (major.minor.patch) for release tags
    - Example: `myapp:1.2.3` for a stable release
    - Major version changes (2.0.0) for breaking changes
    - Minor version changes (1.3.0) for new features
    - Patch version changes (1.2.4) for bug fixes
- Maintain a 'latest' tag for the most recent stable version
    - Example: `myapp:latest` for automated deployments
- Include commit SHA for precise version tracking
    - Example: `myapp:1.2.3-abc123f` for debugging
- Consider branch-based tags for development environments
    - Example: `myapp:feature-user-auth` for feature testing

#### Build artifacts retention

Implement defined retention rules:

- Set explicit expiration timeframes for temporary artifacts
- Define which artifacts need permanent retention
- Configure cleanup policies to manage storage

#### Registry access and authentication

Secure your artifacts with proper access controls:

- Implement Personal Access Tokens for developer access
- Configure CI/CD variables for pipeline authentication
- Set up proper access scopes

### Environment strategy

Consider your environments early, as they shape your entire deployment pipeline:

- Development, staging, and production environment configurations
- Environment-specific variables and secrets
- Access controls and protection rules
- Deployment tracking and monitoring approach

### Deployment targets

Be intentional as to where and how you'll deploy, these decisions matter and the benefits and drawbacks of each should be consider:

- Infrastructure requirements (VMs, containers, cloud services)
- Network access and security configurations
- Authentication mechanisms (SSH keys, access tokens)
- Resource allocation and scaling considerations

With our strategy defined and foundational decisions made, we can now translate these plans into a working pipeline. We'll build a practical example that demonstrates these concepts, starting with a simple application and progressively adding deployment capabilities.

## Implementing your CD pipeline

### A step-by-step example

Let's walk through implementing a basic continuous deployment pipeline for a web application. We'll use a simple HTML application as an example, but these principles apply to any type of application. We’re also going to deploy our application as a Docker image on a simple virtual machine. This will allow us to lean on a curated image with minimum dependencies, and to ensure no environment specific requirements are unintentionally brought in. By working on a virtual machine, we won’t be leveraging GitLab’s native integrations, allowing us to work on an easier but less scalable setup to begin with.

#### Prerequisites

In this example, we’ll aim to containerize an application that we’ll run on a virtual machine hosted on a cloud provider. We’ll also test this application locally on our machine. This list of prerequisites is only needed for this scenario.

##### Virtual machine setup

- Provision a VM in your preferred cloud provider (e.g., GCP, AWS, Azure)
- Configure network rules to allow access on ports 22, 80, and 443
- Record the machine's public IP address for deployment

##### Set up SSH authentication:

- Generate a public/private key pair for the machine
- In GitLab, go to **Settings > CI/CD > Variables**
- Create a variable called `GITLAB_KEY`
- Set Type to "File" (required for SSH authentication)
- Paste the private key in the Value field
- Define a USER variable, this is the user logging in and running the scripts on your VM

##### Configure deployment variables

- Create variables for your deployment targets:
    - `STAGING_TARGET`: Your staging server IP/domain
    - `PRODUCTION_TARGET`: Your production server IP/domain

##### Local development setup

- Install Docker on your local machine for testing deployments

##### GitLab Container Registry access

- Locate your registry path:
    - Navigate to **Deploy > Container Registry**
    - Copy the registry path (e.g., registry.gitlab.com/group/project)
- Set up authentication:
    - Go to **Settings > Access Tokens**
    - Create a new token with registry access
    - Token expiration: Maximum 1 year
    - Save the token securely
- Configure local registry access:

```
docker login registry.gitlab.com
# The username if you are using a PAT is gitlab-ci-token
# Password: your-access-token
```

#### 1\. Create your application

Start with a basic web application. For our example, we're using a simple HTML page:

```
<!-- index.html -->
<html>
  <head>
    <style>
      body {
        background-color: #171321; /* GitLab dark */
      }
    </style>
  </head>
  <body>
    <!-- Your content here -->
  </body>
</html>
```

#### 2\. Containerize your application

Create a Dockerfile to package your application:

```
FROM nginx:1.26.2
COPY index.html /usr/share/nginx/html/index.html
```

This Dockerfile:

- Uses nginx as a base image for serving web content
- Copies your HTML file to the correct location in the nginx directory structure

#### 3\. Set up your CI/CD pipeline

Create a `.gitlab-ci.yml` file to define your pipeline stages:

```
variables:
  TAG_LATEST: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:latest
  TAG_COMMIT: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:$CI_COMMIT_SHA

stages:
  - publish
  - deploy
```

Let's break it down:

`TAG_LATEST` is made up of three parts:

- `$CI_REGISTRY_IMAGE` is the path to your project's container registry in GitLab

For example: `registry.gitlab.com/your-group/your-project`

- `$CI_COMMIT_REF_NAME` is the name of your branch or tag

For example, if you're on main branch: `/main`, and if you're on a feature branch: `/feature-login`

- `:latest` is a fixed suffix

So if you're on the main branch, `TAG_LATEST` becomes: `registry.gitlab.com/your-group/your-project/main:latest`.

`TAG_COMMIT` is almost identical, but instead of `:latest`, it uses: `$CI_COMMIT_SHA` which is the commit identifier, for example: `:abc123def456`.

So for that same commit on main branch, `TAG_COMMIT` becomes:`registry.gitlab.com/your-group/your-project/main:abc123def456`.

The reason for having both is `TAG_LATEST` gives you an easy way to always get the newest version, and `TAG_COMMIT` gives you a specific version you can return to if needed.

#### 4\. Publish to the container registry

Add the publish job to your pipeline:

```
publish:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $TAG_LATEST -t $TAG_COMMIT .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $TAG_LATEST
    - docker push $TAG_COMMIT
```

This job:

- Uses Docker-in-Docker to build images
- Creates two tagged versions of your image
- Authenticates with the GitLab registry
- Pushes both versions to the registry

Now that our images are safely stored in the registry, we can focus on deploying them to our target environments. Let's start with local testing to validate our setup before moving to production deployments.

#### 5\. Deploy to your environment

Before deploying to production, you can test locally. We just published our image to the GitLab repository, which we’ll pull locally. If you’re unsure of the exact path, navigate to **Deploy > Container Registry**, and you should see an icon to copy the path of your image at the end of the line for the container image you want to test.

```
docker login registry.gitlab.com 
docker run -p 80:80 registry.gitlab.com/your-project-path/main:latest
```

By doing so you should be able to access your application locally on your localhost address through your web browser.

You can now add a deployment job to your pipeline:

```
deploy:
  stage: deploy
  image: alpine:latest
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$TARGET_SERVER 
      docker pull $TAG_COMMIT &&
      docker rm -f myapp || true &&
      docker run -d -p 80:80 --name myapp $TAG_COMMIT
```

This job:

- Sets up SSH access to your deployment target
- Pulls the latest image
- Removes any existing container
- Deploys the new version

#### 6\. Track deployments

Enable deployment tracking by adding environment configuration:

```
deploy:
  environment:
    name: production
    url: https://your-application-url.com 
```

This creates an environment object in GitLab's **Operate > Environments** section, providing:

- Deployment history
- Current deployment status
- Quick access to your application

While a single environment pipeline is a good starting point, most teams need to manage multiple environments for proper testing and staging. Let's expand our pipeline to handle this more realistic scenario.

#### 7\. Set up multiple environments

For a more robust pipeline, configure staging and production deployments:

```
stages:
  - publish
  - staging
  - release
  - version
  - production

staging:
  stage: staging
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  environment:
    name: staging
    url: https://staging.your-app.com
  # deployment script here

production:
  stage: production
  rules:
    - if: $CI_COMMIT_TAG
  environment:
    name: production
    url: https://your-app.com
  # deployment script here
```

This setup:

- Deploys to staging from your main branch
- Uses GitLab tags to trigger production deployments
- Provides separate tracking for each environment

Here and in our next step, we’re leveraging a very useful GitLab feature: tags. By manually creating a tag in the **Code > Tags** section, the `$CI_COMMIT_TAG` gets created, which allows us to trigger jobs accordingly.

#### 8\. Create automated release notes

We'll be using GitLab's release capabilities through our CI/CD pipeline. First, update your stages in `.gitlab-ci.yml`:

```
stages:

- publish
- staging
- release # New stage for releases
- version
- production
```

Next, add the release job:

```
release_job:
  stage: release
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG                  # Only run when a tag is created
  script:
    - echo "Creating release for $CI_COMMIT_TAG"
  release:                                # Release configuration
    name: 'Release $CI_COMMIT_TAG'
    description: 'Release created from $CI_COMMIT_TAG'
    tag_name: '$CI_COMMIT_TAG'           # The tag to create
    ref: '$CI_COMMIT_TAG'                # The tag to base release on
```

You can enhance this by adding links to your container images:

```
release:
  name: 'Release $CI_COMMIT_TAG'
  description: 'Release created from $CI_COMMIT_TAG'
  tag_name: '$CI_COMMIT_TAG'
  ref: '$CI_COMMIT_TAG'
  assets:
    links:
      - name: 'Container Image'
        url: '$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG'
        link_type: 'image'
```

For meaningful automated release notes:

- Use conventional commits (feat:, fix:, etc.)
- Include issue numbers (#123)
- Separate subject from body with blank line

If you want custom release notes with deployment info:

```
release_job:
  script:
    - |
      DEPLOY_TIME=$(date '+%Y-%m-%d %H:%M:%S')
      CHANGES=$(git log $(git describe --tags --abbrev=0 @^)..@ --pretty=format:"- %s")
      cat > release_notes.md << EOF
      ## Deployment Info
      - Deployed on: $DEPLOY_TIME
      - Environment: Production
      - Version: $CI_COMMIT_TAG

      ## Changes
      $CHANGES

      ## Artifacts
      - Container Image: `$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG`
      EOF
  release:
    description: './release_notes.md'
```

Once configured, releases will be created automatically when you create a Git tag. You can view them in GitLab under **Deploy > Releases**.

#### 9\. Put it all together

This is what our final YAML file looks like:

```
variables:
  TAG_LATEST: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:latest
  TAG_COMMIT: $CI_REGISTRY_IMAGE/$CI_COMMIT_REF_NAME:$CI_COMMIT_SHA
  STAGING_TARGET: $STAGING_TARGET    # Set in CI/CD Variables
  PRODUCTION_TARGET: $PRODUCTION_TARGET  # Set in CI/CD Variables

stages:
  - publish
  - staging
  - release
  - version
  - production

# Build and publish to registry
publish:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  script:
    - docker build -t $TAG_LATEST -t $TAG_COMMIT .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $TAG_LATEST
    - docker push $TAG_COMMIT

# Deploy to staging
staging:
  stage: staging
  image: alpine:latest
  rules:
    - if: $CI_COMMIT_BRANCH == "main" && $CI_COMMIT_TAG == null
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$STAGING_TARGET "
        docker pull $TAG_COMMIT &&
        docker rm -f myapp || true &&
        docker run -d -p 80:80 --name myapp $TAG_COMMIT"
  environment:
    name: staging
    url: http://$STAGING_TARGET

# Create release
release_job:
  stage: release
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - |
      DEPLOY_TIME=$(date '+%Y-%m-%d %H:%M:%S')
      CHANGES=$(git log $(git describe --tags --abbrev=0 @^)..@ --pretty=format:"- %s")
      cat > release_notes.md << EOF
      ## Deployment Info
      - Deployed on: $DEPLOY_TIME
      - Environment: Production
      - Version: $CI_COMMIT_TAG

      ## Changes
      $CHANGES

      ## Artifacts
      - Container Image: `$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG`
      EOF
  release:
    name: 'Release $CI_COMMIT_TAG'
    description: './release_notes.md'
    tag_name: '$CI_COMMIT_TAG'
    ref: '$CI_COMMIT_TAG'
    assets:
      links:
        - name: 'Container Image'
          url: '$CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG'
          link_type: 'image'

# Version the image with release tag
version_job:
  stage: version
  image: docker:latest
  services:
    - docker:dind
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - docker pull $TAG_COMMIT
    - docker tag $TAG_COMMIT $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG

# Deploy to production
production:
  stage: production
  image: alpine:latest
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - chmod 400 $GITLAB_KEY
    - apk add openssh-client
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - ssh -i $GITLAB_KEY -o StrictHostKeyChecking=no $USER@$PRODUCTION_TARGET "
        docker pull $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG &&
        docker rm -f myapp || true &&
        docker run -d -p 80:80 --name myapp $CI_REGISTRY_IMAGE/main:$CI_COMMIT_TAG"
  environment:
    name: production
    url: http://$PRODUCTION_TARGET
```

This complete pipeline:

- Publishes images to the registry (main branch)
- Deploys to staging (main branch)
- Creates releases (on tags)
- Versions images with release tags Deploys to production (on tags)

Key benefits:

- Clean reproducible, local development and testing environment
- Clear path to production environments with structure to build confidence in what is deployed
- Pattern to recover from unexpected failures, etc.
- Ready to scale/adopt more complex deployment strategies

### Best practices

Throughout implementation, maintain these principles:

- Document everything, from variable usage to deployment procedures
- Use GitLab's built-in features (environments, releases, registry)
- Implement proper access controls and security measures
- Plan for failure with robust rollback procedures
- Keep your pipeline configurations DRY (Don't Repeat Yourself)

## Scale your deployment strategy

What next? Here are some aspects to consider as your continuous deployment strategy matures.

### Advanced security measures

Enhance security through:

- Protected environments with restricted access
- Required approvals for production deployments
- Integrated security scanning
- Automated vulnerability assessments
- Branch protection rules for deployment-related changes

### Progressive delivery strategies

Implement advanced deployment strategies:

- Feature flags for controlled rollouts
- Canary deployments for risk mitigation
- Blue-green deployment strategies
- A/B testing capabilities
- Dynamic environment management

### Monitoring and optimization

Establish robust monitoring practices:

- Track deployment metrics
- Set up performance monitoring
- Configure deployment alerts
- Establish deployment SLOs
- Regular pipeline optimization

## Why GitLab?

GitLab's continuous deployment capabilities make it a standout choice for modern deployment workflows. The platform excels in streamlining the path from code to production, offering built-in container registry, environment management, and deployment tracking all within a single interface. GitLab's environment-specific variables, deployment approval gates, and rollback capabilities provide the security and control needed for production deployments, while features like review apps and feature flags enable progressive delivery approaches. As part of GitLab's complete DevSecOps platform, these CD capabilities seamlessly integrate with your entire software lifecycle.

## Get started today

The journey to continuous deployment is an evolution, not a revolution. Start with the fundamentals, build a solid foundation, and gradually incorporate advanced features as your team's needs grow. GitLab provides the tools and flexibility to support you at every stage of this journey, from your first automated deployment to complex, multi-environment delivery pipelines.

> Sign up for a free, 60-day trial of GitLab Ultimate to get started with continous deployment today.

Go to Source
