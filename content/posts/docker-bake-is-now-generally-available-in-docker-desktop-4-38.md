---
title: "Docker Bake is Now Generally Available in Docker Desktop 4.38!"
date: 2025-02-05
---

We’re excited to announce the General Availability of **Docker Bake** with Docker Desktop 4.38! This powerful build orchestration tool takes the hassle out of managing complex builds and offers simplicity, flexibility, and performance for teams of all sizes.

![2400x1260 docker evergreen logo blog C 1](https://live-docker.pantheonsite.io/app/uploads/2024/12/2400x1260_docker-evergreen_logo_blog_C-1-1110x583.png "- 2400x1260 docker evergreen logo blog C 1")

## What is Docker Bake?

Docker Bake is an orchestration tool that streamlines Docker builds, similar to how Compose simplifies managing runtime environments. With Bake, you can define build stages and deployment environments in a declarative file, making complex builds easier to manage. It also leverages BuildKit’s parallelization and optimization features to speed up build times.

While Dockerfiles are excellent for defining image build steps, teams often need to build multiple images and execute helper tasks like testing, linting, and code generation. Traditionally, this meant juggling numerous `docker build` commands with their own options and arguments – a tedious and error-prone process.

Bake changes the game by introducing a declarative file format that encapsulates all options and image dependencies, referred to as **targets**. Additionally, Bake’s ability to parallelize and deduplicate work ensures faster and more efficient builds.

## Why should you use Bake?

### Challenges with complex Docker Build configuration:

- Managing long, complex build commands filled with countless flags and environment variables.
- Tedious workflows for building multiple images.
- Difficulty declaring builds for specific targets or environments.
- Requires a script or 3rd-party tool to make things manageable

Docker Bake tackles these challenges with a better way to manage complex builds with a simple, declarative approach.

### Key benefits of Docker Bake

- **Simplicity**: Replace complex chains of Docker build commands and scripts with a single `docker buildx bake` command while maintaining clear, version-controlled configuration files that are easy to understand and modify.
- **Flexibility**: Express sophisticated build logic through HCL syntax and matrix builds, enabling dynamic configurations that adapt to different environments and requirements while supporting custom functions for advanced use cases.
- **Consistency**: Maintain standardized build configurations across teams and environments through version-controlled files and inheritance patterns, eliminating environment-specific build issues and reducing configuration drift.
- **Performance**: Automatically parallelize independent builds and eliminate redundant operations through context deduplication and intelligent caching, dramatically reducing build times for complex multi-image workflows.

![Blog Bake After 1068x673 1](https://live-docker.pantheonsite.io/app/uploads/2025/02/Blog-Bake-After-1068x673-1.png "- Blog Bake After 1068x673 1")

**Figure 1**: One simple Docker buildx bake command to replace all the flags and environment variables.

### Use cases for Docker Bake

#### 1\. Monorepo and Image Bakery

Docker Bake can help developers efficiently manage and build multiple related Docker images from a single source repository. Plus, they can leverage shared configurations and automated dependency handling to enforce organizational standards.

- **Development Efficiency:** Teams can maintain consistent build logic across dozens or hundreds of microservices in a single repository, reducing configuration drift and maintenance overhead.
- **Resource Optimization:** Shared base images and contexts are automatically deduplicated, dramatically reducing build times and storage costs.
- **Standardization:** Enforce organizational standards through inherited configurations, ensuring all services follow security, tagging, and testing requirements.
- **Change Management:** A single source of truth for build configurations makes it easier to implement organization-wide changes like base image updates or security patches.

#### 2\. Compose users

Docker Bake provides seamless compatibility with existing docker-compose.yml files, allowing direct use of your current configurations. Existing Compose users are able to get started using Bake with minimal effort.

- **Gradual Adoption:** Teams can incrementally adopt advanced build features while still leveraging their existing compose workflows and knowledge.
- **Development Consistency:** Use the same configuration for both local development (via compose) and production builds (via Bake), eliminating “works on my machine” issues.
- **Enhanced Capabilities**: Access powerful features like matrix builds and HCL expressions while maintaining compatibility with familiar compose syntax.
- **CI/CD Integration**: Seamlessly integrate with existing CI/CD pipelines that already understand compose files while adding Bake’s advanced build capabilities.

#### 3\. Complex build configurations

Developers can use targets, groups, variables, functions, and matrix targets and many more tools in Bake to simplify their build configurations across projects and teams.

- **Cross-Platform Compatibility:** Matrix builds enable teams to efficiently manage builds across multiple architectures, OS versions, and dependency combinations from a single configuration.
- **Dynamic Adaptation:** HCL expressions allow builds to adapt to different environments, git branches, or CI variables without maintaining multiple configurations.
- **Build Optimization:** Custom functions enable sophisticated logic for things like version calculation, tag generation, and conditional builds based on git history.
- **Quality Control:** Variable validation and inheritance ensure consistent configuration across complex build scenarios, reducing errors and maintenance burden.
- **Scale Management:** Groups and targets help organize large-scale build systems with dozens or hundreds of permutations, making them manageable and maintainable.

#### 4\. Docker Build Cloud

With Bake-optimized builds as the foundation, developers can achieve more efficient Docker Build Cloud performance and faster builds.

- **Enhanced Docker Build Cloud Performance:** Instantly parallelize matrix builds across cloud infrastructure, turning hour-long build pipelines into minutes without managing build infrastructure.
- **Resource Optimization:** Leverage Build Cloud’s distributed caching and deduplication to dramatically reduce bandwidth usage and build times, which is especially valuable for remote teams.
- **Cost Management:** Save cost with DBC **—** Bake’s precise target definitions mean you only consume cloud resources for exactly what needs to be built.
- **Developer Experience:** Teams can run complex multi-architecture builds without powerful local machines, enabling development from any device while maintaining build performance.
- **CI/CD Enhancement:** Offload resource-intensive builds from CI runners to Build Cloud, reducing CI costs and queue times while improving reliability.

## What’s New in Bake for GA?

Docker Bake has been an experimental feature for several years, allowing us to refine and improve it based on user feedback. So, there is already a strong set of ingredients that users love, such as targets and groups, variables, HCL Expression Support, inheritance capabilities, matrix targets, and additional contexts. With this GA release, Bake is now ready for production use, and we’ve added several enhancements to make it more efficient, secure, and easier to use:

- **Deduplicated Context Transfers****:** Significantly speeds up build pipelines by eliminating redundant file transfers when multiple targets share the same build context.
- **Entitlements****:** Enhances security and resource management by providing fine-grained control over what capabilities and resources builders can access during the build process.
- **Composable Attributes****:** Simplifies configuration management by allowing teams to define reusable attribute sets that can be mixed, matched, and overridden across different targets.
- **Variable Validation****:** Prevents wasted time and resources by catching configuration errors before the actual build process begins.

### Deduplicate context transfers

When you build targets concurrently using groups, build contexts are loaded independently for each target. If the same context is used by multiple targets in a group, that context is transferred once for each time it’s used. This can significantly impact build time, depending on your build configuration.

Previously, the workaround required users to define a named context that loads the context files and then have each target reference the named context. But with Bake, this will be handled automatically now.

Bake can automatically deduplicate context transfers from targets sharing the same context. When you build targets concurrently using groups, build contexts are loaded independently for each target. If the same context is used by multiple targets in a group, that context is transferred once for each time it’s used. This more efficient approach leads to much faster build time. 

Read more about how to speed up your build time in our docs. 

### Entitlements

Bake now includes entitlements to control access to privileged operations, aligning with Build. This prevents unintended side effects and security risks. If Bake detects a potential issue — like a privileged access request or an attempt to access files outside the current directory — the build will fail unless explicitly allowed.

To be consistent, the Bake command now supports the `--allow=ENTITLEMENT` flag to grant access to additional entitlements. The following entitlements are currently supported for Bake.

- Build equivalents
    - `--allow network.host` **—** Allows executions with host networking.
    - `--allow security.insecure` **—** Allows executions without sandbox. (i.e. —privileged)
- File system: Grant filesystem access for builds that need access files outside the working directory. This will impact `context, output, cache-from, cache-to, dockerfile, secret`
    - `--allow fs=<path|*>` **—** Grant read and write access to files outside the working directory.
    - `--allow fs.read=<path|*>` **—** Grant read access to files outside the working directory.
    - `--allow fs.write=<path|*>` **—** Grant write access to files outside the working directory.
- ssh
    - `--allow ssh` **—**– Allows exposing SSH agent.

### Composable attributes

Several attributes previously had to be defined in CSV (e.g. `type=provenance,mode=min`). These were challenging to read and couldn’t be easily overridden. The following can now be defined as structured objects:

```
target "app" {
		attest = [
			{ type = "provenance", mode = "max" },
			{ type = "sbom", disabled = true}
		]

		cache-from = [
			{ type = "registry", ref = "user/app:cache" },
			{ type = "local", src = "path/to/cache"}
		]

		cache-to = [
			{ type = "local", dest = "path/to/cache" },
		]

		output = [
			{ type = "oci", dest = "../out.tar" },
			{ type = "local", dest="../out"}
		]

		secret = [
			{ id = "mysecret", src = "path/to/secret" },
			{ id = "mysecret2", env = "TOKEN" },
		]

		ssh = [
			{ id = "default" },
			{ id = "key", paths = ["path/to/key"] },
		]
}
```

As such, the attributes are now composable. Teams can mix, match, and override attributes across different targets which simplifies configuration management.

```
 target "app-dev" {
    attest = [
			{ type = "provenance", mode = "min" },
			{ type = "sbom", disabled = true}
		]
  }

  target "app-prod" {
    inherits = ["app-dev"]

    attest = [
			{ type = "provenance", mode = "max" },
		]
  }
```

### Variable validation

Bake now supports validation for variables similar to Terraform to help developers catch and resolve configuration errors early. The GA for Bake also supports the following use cases.

**Basic validation**

To verify that the value of a variable conforms to an expected type, value range, or other condition, you can define custom validation rules using the `validation` block.

```
variable "FOO" {
  validation {
    condition = FOO != ""
    error_message = "FOO is required."
  }
}

target "default" {
  args = {
    FOO = FOO
  }
}
```

**Multiple validations**

To evaluate more than one condition, define multiple `validation` blocks for the variable. All conditions must be true.

```
variable "FOO" {
  validation {
    condition = FOO != ""
    error_message = "FOO is required."
  }
  validation {
    condition = strlen(FOO) > 4
    error_message = "FOO must be longer than 4 characters."
  }
}

target "default" {
  args = {
    FOO = FOO
  }
}
```

**Dependency on other variables**

You can reference other Bake variables in your condition expression, enabling validations that enforce dependencies between variables. This ensures that dependent variables are set correctly before proceeding.

```
variable "FOO" {}
variable "BAR" {
  validation {
    condition = FOO != ""
    error_message = "BAR requires FOO to be set."
  }
}

target "default" {
  args = {
    BAR = BAR
  }
}
```

### New Bake options

In addition to updating the Bake configuration, we’ve added a new –list option. Previously, if you were unfamiliar with a project or wanted a reminder of the supported targets and variables, you would have to read through the file. Now, the list option will allow you to quickly query a list of them. It also supports the JSON format option if you need programmatic access.

**List target**

Quickly get a list of the targets available in your Bake configuration.

- `docker buildx bake --list targets`
- `docker buildx bake --list type=targets,format=json`

**List variables**

Get a list of variables available for your Bake configuration.

- `docker buildx bake --list variables`
- `docker buildx bake --list type=variables,format=json`

These improvements build on a powerful feature set, ensuring Bake is both reliable and future-ready.

## Get started with Docker Bake

Ready to simplify your builds? Update to Docker Desktop 4.38 today and start using Bake. With its declarative syntax and advanced features, Docker Bake is here to help you build faster, more efficiently, and with less effort.

Explore the documentation to learn how to create your first Bake file and experience the benefits of streamlined builds firsthand.

Let’s bake something amazing together!

​Products, build, Docker
