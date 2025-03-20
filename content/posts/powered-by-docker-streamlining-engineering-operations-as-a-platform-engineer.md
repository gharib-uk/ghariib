---
title: "Powered by Docker: Streamlining Engineering Operations as a Platform Engineer"
date: 2025-03-19
---

_The Powered by Docker is a series of blog posts featuring use cases and success stories from Docker partners and practitioners. This story was contributed by Neal Patel from Siimpl.io. Neal has more than ten years of experience developing software and is a Docker Captain._

## Background

As a platform engineer at a mid-size startup, I’m responsible for identifying bottlenecks and developing solutions to streamline engineering operations to keep up with the velocity and scale of the engineering organization. In this post, I outline some of the challenges we faced with one of our clients, how we addressed them, and provide guides on how to tackle these challenges at your company.

One of our clients faced critical engineering challenges, including poor synchronization between development and CI/CD environments, slow incident response due to inadequate rollback mechanisms, and fragmented telemetry tools that delayed issue resolution. Siimpl implemented strategic solutions to enhance development efficiency, improve system reliability, and streamline observability, turning obstacles into opportunities for growth.

**Let’s walk through the primary challenges we encountered.**

### Inefficient development and deployment

- **Problem:** We lacked parity between developer tooling and CI/CD tooling, which made it difficult for engineers to test changes confidently.
- **Goal:** We needed to ensure consistent environments across development, testing, and production.

### Unreliable incident response

- **Problem:** If a rollback was necessary, we did not have the proper infrastructure to accomplish this efficiently.
- **Goal:** We wanted to revert to stable versions in case of deployment issues easily.

### Lack of comprehensive telemetry

- **Problem:** Our SRE team created tooling to simplify collecting and publishing telemetry, but distribution and upgradability were poor. Also, we found adoption to be extremely low.
- **Goal:** We needed to standardize how we configure telemetry collection, and simplify the configuration of auto-instrumentation libraries so the developer experience is turnkey.

## Solution: Efficient development and deployment

![blog Solution Efficient development 1200](https://www-stage.docker.com/app/uploads/2025/02/blog-Solution-Efficient-development_1200-1110x786.png "- blog Solution Efficient development 1200")

### CI/CD configuration with self-hosted GitHub runners and Docker Buildx

We had a requirement for multi-architecture support (arm64/amd64), which we initially implemented in CI/CD with Docker Buildx and QEMU. However, we noticed an extreme dip in performance due to the emulated architecture build times.

We were able to reduce build times by almost 90% by ditching QEMU (emulated builds), and targeting arm64 and amd64 self-hosted runners. This gave us the advantage of blazing-fast native architecture builds, but still allowed us to support multi-arch by publishing the manifest after-the-fact. 

Here’s a working example of the solution we will walk through: https://github.com/siimpl/multi-architecture-cicd

If you’d like to deploy this yourself, there’s a guide in the README.md.

#### Prerequisites

This project uses the following tools:

- Docker Build Cloud (included in all Docker paid subscriptions.)
- DBC cloud driver
- GitHub/GitHub Actions
- A managed container orchestration service like Elastic Kubernetes Service (EKS), Azure Kubernetes Service (AKS), or  Google Kubernetes Engine (GKE)
- Terraform
- Helm

Because this project uses industry-standard tooling like Terraform, Kubernetes, and Helm, it can be easily adapted to any CI/CD or cloud solution you need.

#### Key features

The secret sauce of this solution is provisioning the self-hosted runners in a way that allows our CI/CD to specify which architecture to execute the build on.

The first step is to provision two node pools — an amd64 node pool and an arm64 node pool, which can be found in the aks.tf. In this example, the node\_count is fixed at 1 for both node pools but for better scalability/flexibility you can also enable autoscaling for a dynamic pool.

```
resource "azurerm_kubernetes_cluster_node_pool" "amd64" {
  name                  = "amd64pool"
  kubernetes_cluster_id = azurerm_kubernetes_cluster.cicd.id
  vm_size               = "Standard_DS2_v2" # AMD-based instance
  node_count            = 1
  os_type               = "Linux"
  tags = {
    environment = "dev"
  }
}

resource "azurerm_kubernetes_cluster_node_pool" "arm64" {
  name                  = "arm64pool"
  kubernetes_cluster_id = azurerm_kubernetes_cluster.cicd.id
  vm_size               = "Standard_D4ps_v5" # ARM-based instance
  node_count            = 1
  os_type               = "Linux"
  tags = {
    environment = "dev"
  }
}
```

Next, we need to update the self-hosted runners’ values.yaml to have a configurable nodeSelector. This will allow us to deploy one runner scale set to the arm64pool and one to the amd64pool.

Once the Terraform resources are successfully created, the runners should be registered to the organization or repository you specified in the GitHub config URL. We can now update the REGISTRY values for the emulated-build and the native-build.

After creating a pull request with those changes, navigate to the **Actions** tab to witness the results.

![blog Actions Tab 1200](https://www-stage.docker.com/app/uploads/2025/02/blog-Actions-Tab_1200-1110x185.png "- blog Actions Tab 1200")

You should see two jobs kick off, one using the emulated build path with QEMU, and the other using the self-hosted runners for native node builds. Depending on cache hits or the Dockerfile being built, the performance improvements can be up to 90%. Even with this substantial improvement, utilizing Docker Build Cloud can improve performance 95%. More importantly, you can reap the benefits during development builds! Take a look at the docker-build-cloud.yml workflow for more details. All you need is a Docker Build Cloud subscription and a cloud driver to take advantage of the improved pipeline.

**Getting Started**

1\. Generate GitHub PAT

2\. Update the variables.tf

3\. Initialise AZ CLI

4\. Deploy Cluster

5\. Create a PR to validate pipelines

README.md for reference

### Reliable Incident Response

**Leveraging SemVer Tagged Containers for Easy Rollback**

Recognizing that deployment issues can arise unexpectedly, we needed a mechanism to quickly and reliably rollback production deployments. Below is an example workflow for properly rolling back a deployment based on the tagging strategy we implemented above.

1. **Rollback Process:**
    - In case of a problematic build, deployment was rolled back to a previous stable version using the tagged images.
    - AWS CLI commands were used to update ECS services with the desired image tag:

```
on:
  workflow_call:
    inputs:
      image-version:
        required: true
        type: string
jobs:
  rollback:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      context: read
    steps:
     - name: Rollback to previous version
       run: |
         aws ecs update-service --cluster my-cluster --service my-service --force-new-deployment --image ${{ secrets.REGISTRY }}/myapp:${{ inputs.image-version }}
```

### Comprehensive Telemetry

**Configuring Sidecar Containers in ECS for Aggregating/Publishing Telemetry Data (OTEL)**

As we adopted a OpenTelemetry to standardize observability, we quickly realized that adoption was one of the toughest hurdles. As a team, we decided to bake in as much configuration as possible into the infrastructure (Terraform modules) so that we could easily distribute and maintain observability instrumentation.

1. **Sidecar Container Setup:**
    - Sidecar containers were defined in the ECS task definitions to run OpenTelemetry collectors.
    - The collectors were configured to aggregate and publish telemetry data from the application containers.
2. **Task Definition Example:**

```
{
  "containerDefinitions": [
    {
      "name": "myapp",
      "image": "myapp:1.0.0",
      "essential": true,
      "portMappings": [{ "containerPort": 8080 }]
    },
    {
      "name": "otel-collector",
      "image": "otel/opentelemetry-collector:latest",
      "essential": false,
      "portMappings": [{ "containerPort": 4317 }],
      "environment": [
        { "name": "OTEL_RESOURCE_ATTRIBUTES", "value": "service.name=myapp" }
      ]
    }
  ],
  "family": "my-task"
}
```

**Configuring Multi-Stage Dockerfiles for OpenTelemetry Auto-Instrumentation Libraries (Node.js)**

At the application level, configuring the auto-instrumentation posed a challenge since most applications varied in their build process. By leveraging multi-stage Dockerfiles, we were able to help standardize the way we initialized the auto-instrumentation libraries across microservices. We were primarily a nodejs shop, so below is an example Dockerfile for that.

1. **Multi-Stage Dockerfile:**
    - The Dockerfile is divided into stages to separate the build environment from the final runtime environment, ensuring a clean and efficient image.
    - OpenTelemetry libraries are installed in the build stage and copied to the runtime stage:

```
# Stage 1: Build stage
FROM node:20 AS build
WORKDIR /app
COPY package.json package-lock.json ./
# package.json defines otel libs (ex. @opentelemetry/node @opentelemetry/tracing)
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Runtime stage
FROM node:20
WORKDIR /app
COPY --from=build /app /app
CMD ["node", "dist/index.js"]
```

### Results

By addressing these challenges we were able to reduce build times by **~90%**, which alone dropped our DORA metrics for **Lead time for changes** and **Time to restore** by **~50%.** With the rollback strategy and telemetry changes, we were able to reduce our Mean time to Detect (MTTD) and Mean time to resolve (MTTR) by **~30%**. We believe that it could get to **50-60%** with tuning of alerts and the addition of runbooks (automated and manual).

1. **Enhanced Development Efficiency:** Consistent environments across development, testing, and production stages sped up the development process, and roughly 90% faster build times with the native architecture solution.
2. **Reliable Rollbacks:** Quick and efficient rollbacks minimized downtime and maintained system integrity.
3. **Comprehensive Telemetry:** Sidecar containers enabled detailed monitoring of system health and security without impacting application performance, and was baked right into the infrastructure developers were deploying. Auto-instrumentation of the application code was simplified drastically with the adoption of our Dockerfiles.

### Siimpl: Transforming Enterprises with Cloud-First Solutions

With Docker at the core, Siimpl.io’s solutions demonstrate how teams can build faster, more reliable, and scalable systems. Whether you’re optimizing CI/CD pipelines, enhancing telemetry, or ensuring secure rollbacks, Docker provides the foundation for success. Try Docker today to unlock new levels of developer productivity and operational efficiency.

**Learn more** **from our website** **or contact us at** **_solutions@siimpl.io_**

​Products, CI/CD, Docker Build Cloud, Docker Desktop, Platform Engineering
