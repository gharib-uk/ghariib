---
title: "Enhance build security and reach SLSA Level 3 with GitHub Artifact Attestations"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Learn how GitHub Artifact Attestations can enhance your build security and help your organization achieve SLSA Level 3. This post breaks down the basics of SLSA, explains the importance of artifact attestations, and provides a step-by-step guide to securing your build process.

The post Enhance build security and reach SLSA Level 3 with GitHub Artifact Attestations appeared first on The GitHub Blog.

The need for software build security is more pressing than ever. High-profile software supply chain attacks like SolarWinds, MOVEit, 3CX, and Applied Materials have revealed just how vulnerable the software build process can be. As attackers exploit weaknesses in the build pipeline to inject their malicious components, traditional security measures—like scanning source code, securing production environments and source control allow lists—are no longer enough. To defend against these sophisticated threats, organizations must treat their build systems with the same level of care and security as their production environments.

These supply chain attacks are particularly dangerous because they can undermine trust in your business itself: if an attacker can infiltrate your build process, they can distribute compromised software to your customers, partners, and end-users. So, how can organizations secure their build processes, and ensure that what they ship is exactly what they intended to build?

The Supply-chain Levels for Software Artifacts (SLSA) framework was developed to address these needs. SLSA provides a comprehensive, step-by-step methodology for building integrity and provenance guarantees into your software supply chain. This might sound complicated, but the good news is that GitHub Artifact Attestations simplify the journey to SLSA Level 3!

In this post, we’ll break down what you need to know about SLSA, how Artifact Attestations work, and how they can boost your GitHub Actions build security to the next level.

## Securing your build process: An introduction to SLSA

### What is build security?

When we build software, we convert source code into deployable artifacts—whether those are binaries, container images or packaged libraries. This transformation occurs through multiple stages, such as compilation, packaging and testing, each of which could potentially introduce vulnerabilities or malicious modifications.

A properly secured build process can:

- **Help ensure the integrity of your deployed artifacts** by providing a higher level of assurance that the code has not been tampered with during the build process.
- **Provide transparency into the build process**, allowing you to audit the provenance of your deployed artifacts.
- **Maintain confidentiality** by safeguarding sensitive data and secrets used in the build process.

By securing the build process, organizations can ensure that the software reaching end-users is the intended and unaltered version. **This makes securing the build process just as important as securing the source code and deployment environments**.

### Introducing SLSA: A framework for build security

SLSA is a community-driven framework governed by the Open Source Security Foundation (OpenSSF), designed to help organizations systematically secure their software supply chains through a series of progressively stronger controls and best practices.

The framework is organized into four levels, each representing a higher degree of security maturity:

- **Level 0**: No security guarantees
- **Level 1**: Provenance exists for traceability, but minimal tamper resistance
- **Level 2**: Provenance signed by a managed build platform, deterring simple tampering
- **Level 3**: Provenance from a hardened, tamper-resistant build platform, ensuring high security against compromise

Provenance refers to the cryptographic record generated for each artifact, providing an unforgeable paper trail of its build history. This record allows you to trace artifacts back to their origins, allowing for verification of how, when and by whom the artifact was created.

## Why SLSA Level 3 matters for build security

Achieving SLSA Level 3 is a critical step in building a secure and trustworthy software supply chain. This level requires organizations to implement rigorous standards for provenance and isolation, ensuring that artifacts are produced in a controlled and verifiable manner. **An organization that has achieved SLSA Level 3 is capable of significantly mitigating the most common attack vectors targeting software build pipelines.** Here’s a breakdown of the specific requirements for reaching SLSA Level 3:

- **Provenance generation and availability**: A detailed provenance record must be generated for each build, documenting how, when and by whom each artifact was produced. This provenance must be accessible to users for verification.
- **Managed build system**: Builds must take place on ephemeral build systems—short-lived, on-demand environments that are provisioned for each build in order to isolate builds from one another, reducing the risk of cross-contamination and unauthorized access.
- **Restricted access to signing material**: User-defined build steps should not have access to sensitive signing material to authenticate provenance, keeping signing operations separate and secure.

GitHub Artifact Attestations help simplify your journey to SLSA Level 3 by enabling secure, automated build verification within your GitHub Actions workflows. While generating build provenance records is foundational to SLSA Level 1, the key distinction at SLSA Level 3 is the separation of the signature process from the rest of your build job. At Level 3, the signing happens on dedicated infrastructure, separated from the build workflow itself.

## The importance of verifying signatures

While signing artifacts is a critical step, it becomes meaningless without verification. Simply having attestations does not provide any security advantages if they are not verified. **Verification ensures that the signed artifacts are authentic and have not been tampered with**.

The GitHub CLI makes this process easy, allowing you to verify signatures at any stage of your CI/CD pipeline. For example, you can verify Terraform plans before applying them, ensure that Ansible or Salt configurations are authentic before deployment, validate containers before they are deployed to Kubernetes, or use it as part of a GitOps workflow driven by tools like Flux.

GitHub offers several native ways to verify Artifact Attestations:

- **GitHub CLI**: This is the easiest way to verify signatures.
- **Kubernetes Admission Controller**: Use GitHub’s distribution of the admission controller for automated verification in Kubernetes environments.
- **Offline verification**: Download the attestations and verify them offline using the GitHub CLI for added security and flexibility in isolated environments.

By verifying signatures during deployment, you can ensure that what you deploy to production is indeed what you built.

## Achieving SLSA Level 3 compliance with GitHub Artifact Attestations

Reaching SLSA Level 3 may seem complex, but GitHub’s Artifact Attestations feature makes it remarkably straightforward. Generating build provenance puts you at SLSA Level 1, and by using GitHub Artifact Attestations on GitHub-hosted runners, you reach SLSA Level 2 by default. From this point, advancing to SLSA Level 3 is a straightforward journey!

The critical difference between SLSA Level 2 and Level 3 lies in using a reusable workflow for provenance generation. This allows you to centrally enforce build security across all projects and enables stronger verification, as you can confirm that a **specific** reusable workflow was used for signing. With just a few lines of YAML added to your workflow, you can gain build provenance without the burden of managing cryptographic key material or setting up additional infrastructure.

### Build provenance made simple

GitHub Artifact Attestations streamline the process of establishing provenance for your builds. By enabling provenance generation directly within GitHub Actions workflows, you ensure that each artifact includes a verifiable record of its build history. This level of transparency is crucial for SLSA Level 3 compliance.

Best of all, you don’t need to worry about the onerous process of handling cryptographic key material. GitHub manages all of the required infrastructure, from running a Sigstore instance to serving as a root signing certificate authority for you.

> Check out our earlier blog to learn more about how to set up Artifact Attestations in your workflow.

### Secure signing with ephemeral machines

GitHub Actions-hosted runners, executing workflows on ephemeral machines, ensure that each build process occurs in a clean and isolated environment. This model is fundamental for SLSA Level 3, which mandates secure and separate handling of key material used in signing.

When you create a reusable workflow for provenance generation, your organization can use it centrally across all projects. This establishes a consistent, trusted source for provenance records. Additionally, signing occurs on dedicated hardware that is separate from the build machine, ensuring that neither the source code nor the developer triggering the build system can influence or alter the build process. With this level of separation, your workflows inherently meet SLSA Level 3 requirements.

Below is an example of a reusable workflow that can be utilized across the organization to sign artifacts:

```
name: Sign Artifact

on:
  workflow_call:
    inputs:
      artifact-path:
            required: true
            type: string

jobs:
  sign-artifact:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      attestations: write
      contents: read

    steps:
    - name: Attest Build Provenance
          uses: actions/attest-build-provenance@<version>
          with:
          subject-name: ${{ inputs.subject-name }}
          subject-digest: ${{ inputs.subject-digest }}
```

When you want to use this reusable workflow for signing in any other workflow, you can call it as follows:

```
name: Sign Artifact Workflow

on:
  push:
    branches:
      - main

jobs:
  sign:
    runs-on: ubuntu-latest

    steps:
    - name: Sign Artifact
      uses: <repository>/.github/workflows/sign-artifact.yml@<version>
      with:
        subject-name: "your-artifact.tar.gz" # Replace with actual artifact name
        subject-digest: "your-artifact-digest" # Replace with SHA-256 digest
```

This architecture of ephemeral environments and centralized provenance generation guarantees that signing operations are isolated from the build process itself, preventing unauthorized access to the signing process. By ensuring that signing occurs in a dedicated, controlled environment, the risk of compromising the signing workflow can be greatly reduced so that malicious actors can not tamper with the signing action’s code and deviate from the intended process. Additionally, provenance is generated consistently across all builds, providing a unified record of build history for the entire organization.

To verify that an artifact was signed using this reusable workflow, you can use the GitHub CLI with the following command:

```
gh artifact verify <file-path> --signer-workflow <owner>/<repository>/.github/workflows/sign-artifact.yml
```

This verification process ensures that the artifact was built and signed using the anticipated pipeline, reinforcing the integrity of your software supply chain.

## A more secure future

GitHub Artifact Attestations bring the assurance and structure of SLSA Level 3 to your builds without having to manage additional security infrastructure. By simply adding a few lines of YAML and moving the provenance generation into a reusable workflow, you’re well on your way to achieving SLSA Level 3 compliance with ease!

**Ready to strengthen your build security and achieve SLSA Level 3?**

Start using GitHub Artifact Attestations today or explore our documentation to learn more.

The post Enhance build security and reach SLSA Level 3 with GitHub Artifact Attestations appeared first on The GitHub Blog.

Go to Source
