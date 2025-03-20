---
title: "<div>Hosted runners for GitLab Dedicated: Now in limited availability</div>"
date: 2025-01-23
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

We are excited to announce that hosted runners for GitLab Dedicated, our single-tenant SaaS solution, have transitioned from beta to limited availability, marking a significant milestone in our commitment to simplifying CI/CD infrastructure management for our customers.

## Streamlined CI/CD infrastructure management

Managing runner infrastructure has traditionally been a complex undertaking, requiring dedicated resources and expertise to maintain optimal performance. Hosted runners for GitLab Dedicated eliminates these challenges by providing a fully managed solution that handles all aspects of runner infrastructure. This allows your teams to focus on what matters most – building and deploying great software.

## Key benefits

### Reduced operational overhead

By choosing hosted runners, you can eliminate the complexity of provisioning, maintaining, and securing your runner infrastructure. Our fully managed service handles all aspects of runner operations, from deployment to updates and security patches.

### Automatic scaling

Hosted runners automatically scale to match your CI/CD demands, ensuring consistent performance during high-traffic periods and for large-scale projects. This dynamic scaling capability means you'll always have runners available to pick up your CI/CD jobs and ensure optimal efficiency of your development teams.

### Cost optimization

With hosted runners, you only pay for the resources you actually use. This consumption-based model eliminates the need to maintain excess capacity for peak loads, potentially reducing your infrastructure costs while ensuring resources are available when needed.

### Enterprise-grade security

Following the same security principles as GitLab Dedicated, hosted runners provide complete isolation from other tenants and are secure by default. Jobs are executed in fully-isolated VMs with no inbound traffic allowed. This means you can maintain the highest security standards without the complexity of implementing and maintaining security measures yourself.

## Introducing native Arm64 support

Our hosted runners now include native Arm64 support in addition to our existing x86-64 runners, offering significant advantages for modern development workflows.

### Enhanced performance for Arm-based development

Native Arm64 runners enable you to build, test, and deploy Arm-based applications in their native environment, ensuring optimal performance and compatibility. Teams developing Docker images or services targeting Arm-based cloud platforms can see build times cut significantly, accelerating their development cycles and deployments.

### Cost-efficient computing

Arm-based runners can significantly reduce your computing costs, due to their efficient processing architecture and lower cost per minute. For compatible jobs, this means more affordable pipeline execution.

### Native building capabilities

With support for both x86-64 and Arm64 architectures, you can:

- build and test applications natively on either architecture
- create multi-architecture container images efficiently
- validate cross-platform compatibility in your CI/CD pipeline
- optimize your delivery pipeline for specific target platforms
- eliminate the performance overhead of emulation when building for Arm targets

This dual-architecture support ensures you have the flexibility to choose the right environment for each specific workload while maintaining a consistent and efficient CI/CD experience across all your projects.

## Available runner sizes

We're expanding our runner offerings to include both x86-64 and Arm64 architectures with a range of configurations. The following sizes are available:

| Size | vCPUs | Memory | Storage |
| --- | --- | --- | --- |
| Small | 2 | 8 GB | 30 GB |
| Medium | 4 | 16 GB | 50 GB |
| Large | 8 | 32 GB | 100 GB |
| X-Large | 16 | 64 GB | 200 GB |
| 2X-Large | 32 | 128 GB | 200 GB |

This expanded size support allows you to optimize your CI/CD pipeline performance based on your application's specific requirements.

## What's next for hosted runners

We plan to release hosted runners in general availability in May 2025. The release includes compute minute visualization to help you better understand and control your CI/CD usage across your organization.

We'll be expanding our hosted runners offering with several new features coming later this year:

- Network controls for enhanced security and compliance
- MacOS runners to support application development for the Apple ecosystem
- Windows runners for .NET and Windows-specific workloads

These additions will provide even more flexibility and coverage for your CI/CD needs, allowing you to consolidate all your build and test workflows on GitLab Dedicated hosted runners.

Ready to simplify your CI/CD infrastructure? Contact your GitLab representative or reach out to our sales team to learn more about hosted runners for GitLab Dedicated.

We are excited to announce that hosted runners for GitLab Dedicated, our single-tenant SaaS solution, have transitioned from beta to limited availability, marking a significant milestone in our commitment to simplifying CI/CD infrastructure management for our customers.

## Streamlined CI/CD infrastructure management

Managing runner infrastructure has traditionally been a complex undertaking, requiring dedicated resources and expertise to maintain optimal performance. Hosted runners for GitLab Dedicated eliminates these challenges by providing a fully managed solution that handles all aspects of runner infrastructure. This allows your teams to focus on what matters most – building and deploying great software.

## Key benefits

### Reduced operational overhead

By choosing hosted runners, you can eliminate the complexity of provisioning, maintaining, and securing your runner infrastructure. Our fully managed service handles all aspects of runner operations, from deployment to updates and security patches.

### Automatic scaling

Hosted runners automatically scale to match your CI/CD demands, ensuring consistent performance during high-traffic periods and for large-scale projects. This dynamic scaling capability means you'll always have runners available to pick up your CI/CD jobs and ensure optimal efficiency of your development teams.

### Cost optimization

With hosted runners, you only pay for the resources you actually use. This consumption-based model eliminates the need to maintain excess capacity for peak loads, potentially reducing your infrastructure costs while ensuring resources are available when needed.

### Enterprise-grade security

Following the same security principles as GitLab Dedicated, hosted runners provide complete isolation from other tenants and are secure by default. Jobs are executed in fully-isolated VMs with no inbound traffic allowed. This means you can maintain the highest security standards without the complexity of implementing and maintaining security measures yourself.

## Introducing native Arm64 support

Our hosted runners now include native Arm64 support in addition to our existing x86-64 runners, offering significant advantages for modern development workflows.

### Enhanced performance for Arm-based development

Native Arm64 runners enable you to build, test, and deploy Arm-based applications in their native environment, ensuring optimal performance and compatibility. Teams developing Docker images or services targeting Arm-based cloud platforms can see build times cut significantly, accelerating their development cycles and deployments.

### Cost-efficient computing

Arm-based runners can significantly reduce your computing costs, due to their efficient processing architecture and lower cost per minute. For compatible jobs, this means more affordable pipeline execution.

### Native building capabilities

With support for both x86-64 and Arm64 architectures, you can:

- build and test applications natively on either architecture
- create multi-architecture container images efficiently
- validate cross-platform compatibility in your CI/CD pipeline
- optimize your delivery pipeline for specific target platforms
- eliminate the performance overhead of emulation when building for Arm targets

This dual-architecture support ensures you have the flexibility to choose the right environment for each specific workload while maintaining a consistent and efficient CI/CD experience across all your projects.

## Available runner sizes

We're expanding our runner offerings to include both x86-64 and Arm64 architectures with a range of configurations. The following sizes are available:

| Size | vCPUs | Memory | Storage |
| --- | --- | --- | --- |
| Small | 2 | 8 GB | 30 GB |
| Medium | 4 | 16 GB | 50 GB |
| Large | 8 | 32 GB | 100 GB |
| X-Large | 16 | 64 GB | 200 GB |
| 2X-Large | 32 | 128 GB | 200 GB |

This expanded size support allows you to optimize your CI/CD pipeline performance based on your application's specific requirements.

## What's next for hosted runners

We plan to release hosted runners in general availability in May 2025. The release includes compute minute visualization to help you better understand and control your CI/CD usage across your organization.

We'll be expanding our hosted runners offering with several new features coming later this year:

- Network controls for enhanced security and compliance
- MacOS runners to support application development for the Apple ecosystem
- Windows runners for .NET and Windows-specific workloads

These additions will provide even more flexibility and coverage for your CI/CD needs, allowing you to consolidate all your build and test workflows on GitLab Dedicated hosted runners.

Ready to simplify your CI/CD infrastructure? Contact your GitLab representative or reach out to our sales team to learn more about hosted runners for GitLab Dedicated.

Go to Source
