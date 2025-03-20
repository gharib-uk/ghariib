---
title: "<div>Secure and publish Python packages: A guide to CI integration</div>"
date: 2025-01-22
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Supply chain security is a critical concern in software development. Organizations need to verify the authenticity and integrity of their software packages. This guide will show you how to implement a secure CI/CD pipeline for Python packages using GitLab CI, incorporating package signing and attestation using Sigstore's Cosign.

You'll learn:

- Why sign and attest your Python packages?
    
- Pipeline overview
    
- Complete pipeline implementation: Setting up the environment
    
    - Environment configuration
    - Configuration breakdown
- The 6 stages
    
    1. Building
    2. Signing
    3. Verification
    4. Publishing
    5. Publishing signatures
    6. Consumer verification

## Why sign and attest your Python packages?

Here are four reasons to sign and attest your Python packages:

- **Supply chain security:** Package signing ensures that the code hasn't been tampered with between build and deployment, protecting against supply chain attacks.
- **Compliance requirements:** Many organizations, especially in regulated industries, require cryptographic signatures and provenance information for all deployed software.
- **Traceability:** Attestations provide a verifiable record of build conditions, including who built the package and under what circumstances.
- **Trust verification:** Consumers of your package can cryptographically verify its authenticity before installation.

## Pipeline overview

Ensuring your code's integrity and authenticity is necessary. Imagine a pipeline that doesn't just compile your code but creates a cryptographically verifiable narrative of how, when, and by whom your package was created. Each stage acts as a guardian, checking and documenting the package's provenance.

Here are six stages of a GitLab pipeline that ensure your package is secure and trustworthy:

- Build: Creates a clean, standard package that can be easily shared and installed.
- Signing: Adds a digital signature that proves the package hasn't been tampered with since it was created.
- Verification: Double-checks that the signature is valid and the package meets all our security requirements.
- Publishing: Uploads the verified package to GitLab's package registry, making it available for others to use.
- Publishing Signatures: Makes signatures available for verification.
- Consumer Verification: Simulates how end users can verify package authenticity.

## Complete pipeline implementation: Setting up the environment

Before we build our package, we need to set up a consistent and secure build environment. This configuration ensures every package is created with the same tools, settings, and security checks.

### Environment configuration

Our pipeline requires specific tools and settings to work correctly.

Primary configurations:

- Python 3.10 for consistent builds
- Cosign 2.2.3 for package signing
- GitLab package registry integration
- Hardcoded package version for reproducibility

**Note about versioning:** We've chosen to use a hardcoded version (`"1.0.0"`) in this example rather than deriving it from git tags or commits. This approach ensures complete reproducibility and makes the pipeline behavior more predictable. In a production environment, you might want to use semantic versioning based on git tags or another versioning strategy that fits your release process.

Tool requirements:

- Basic utilities: `curl`, `wget`
- Cosign for cryptographic signing
- Python packaging tools: `build`, `twine`, `setuptools`, `wheel`

### Configuration breakdown

```yaml
variables:
  PYTHON_VERSION: '3.10'
  PACKAGE_NAME: ${CI_PROJECT_NAME}
  PACKAGE_VERSION: "1.0.0"
  FULCIO_URL: 'https://fulcio.sigstore.dev'
  REKOR_URL: 'https://rekor.sigstore.dev'
  CERTIFICATE_IDENTITY: 'https://gitlab.com/${CI_PROJECT_PATH}//.gitlab-ci.yml@refs/heads/${CI_DEFAULT_BRANCH}'
  CERTIFICATE_OIDC_ISSUER: 'https://gitlab.com'
  PIP_CACHE_DIR: "$CI_PROJECT_DIR/.pip-cache"
  COSIGN_YES: "true"
  GENERIC_PACKAGE_BASE_URL: "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/generic/${PACKAGE_NAME}/${PACKAGE_VERSION}"
```

We use caching to speed up subsequent builds:

```yaml
cache:
  paths:
    - ${PIP_CACHE_DIR}
```

## Building: Crafting the package

Every software journey begins with creation. In our pipeline, the build stage is where raw code transforms into a distributable package, ready to travel across different Python environments.

The build process creates two standardized formats:

- a wheel package (.whl) for quick, efficient installation
- a source distribution (.tar.gz) that carries the complete code

Here's the build stage implementation:

```yaml
build:
  extends: .python-job
  stage: build
  script:
    - git init
    - git config --global init.defaultBranch main
    - git config --global user.email "ci@example.com"
    - git config --global user.name "CI"
    - git add .
    - git commit -m "Initial commit"
    - export NORMALIZED_NAME=$(echo "${CI_PROJECT_NAME}" | tr '-' '_')
    - sed -i "s/name = ".*"/name = "${NORMALIZED_NAME}"/" pyproject.toml
    - sed -i "s|"Homepage" = ".*"|"Homepage" = "https://gitlab.com/${CI_PROJECT_PATH}"|" pyproject.toml
    - python -m build
  artifacts:
    paths:
      - dist/
      - pyproject.toml
```

Let's break down what this build stage does:

1. Initializes a Git repository (`git init`) and configures it with basic settings
2. Normalizes the package name by converting hyphens to underscores, which is required for Python packaging
3. Updates the package metadata in `pyproject.toml` to match our project settings
4. Builds both wheel and source distribution packages using `python -m build`
5. Preserves the built packages and configuration as artifacts for subsequent stages

## Signing: The digital notarization

If attestation is the package's biography, signing is its cryptographic seal of authenticity. This is where we transform our package from a mere collection of files into a verified, tamper-evident artifact.

The signing stage uses Cosign to apply a digital signature as an unbreakable seal. This isn't just a stamp — it's a complex cryptographic handshake that proves the package's integrity and origin.

```yaml
sign:
  extends: .python+cosign-job
  stage: sign
  id_tokens:
    SIGSTORE_ID_TOKEN:
      aud: sigstore
  script:
    - |
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          cosign sign-blob --yes 
            --fulcio-url=${FULCIO_URL} 
            --rekor-url=${REKOR_URL} 
            --oidc-issuer $CI_SERVER_URL 
            --identity-token $SIGSTORE_ID_TOKEN 
            --output-signature "dist/${filename}.sig" 
            --output-certificate "dist/${filename}.crt" 
            "$file"
        fi
      done
  artifacts:
    paths:
      - dist/
```

This signing stage performs several crucial operations:

1. Obtains an OIDC token from GitLab for authentication with Sigstore services
2. Processes each built package (both wheel and source distribution)
3. Uses Cosign to create a cryptographic signature (`.sig`) for each package
4. Generates a certificate (`.crt`) that proves the signature's authenticity
5. Stores both signatures and certificates alongside the packages as artifacts

## Verification: The security checkpoint

Verification is our final quality control gate. It's not just a check — it's a security interrogation where every aspect of the package is scrutinized.

```yaml
verify:
  extends: .python+cosign-job
  stage: verify
  script:
    - |
      failed=0
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          if ! cosign verify-blob 
            --signature "dist/${filename}.sig" 
            --certificate "dist/${filename}.crt" 
            --certificate-identity "${CERTIFICATE_IDENTITY}" 
            --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" 
            "$file"; then
            failed=1
          fi
        fi
      done
      if [ $failed -eq 1 ]; then
        exit 1
      fi
```

The verification stage implements several security checks:

1. Examines each package file in the `dist` directory
2. Uses Cosign to verify the signature matches the package content
3. Confirms the certificate's identity matches our expected GitLab pipeline identity
4. Validates our trusted OIDC provider issued the certificate
5. Fails the entire pipeline if any verification check fails, ensuring only verified packages proceed

## Publishing: The controlled release

Publishing is where we make our verified packages available through GitLab's package registry. It's a carefully choreographed release that ensures only verified, authenticated packages reach their destination.

```yaml
publish:
  extends: .python-job
  stage: publish
  script:
    - |
      cat << EOF > ~/.pypirc
      [distutils]
      index-servers = gitlab
      [gitlab]
      repository = ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi
      username = gitlab-ci-token
      password = ${CI_JOB_TOKEN}
      EOF
      TWINE_PASSWORD=${CI_JOB_TOKEN} TWINE_USERNAME=gitlab-ci-token 
        twine upload --repository-url ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi 
        dist/*.whl dist/*.tar.gz
```

The publishing stage handles several important tasks:

1. Creates a `.pypirc` configuration file with GitLab package registry credentials
2. Uses the GitLab CI job token for secure authentication
3. Uploads both wheel and source distribution packages to the GitLab PyPI registry
4. Makes the packages available for installation via pip

## Publishing signatures: Making verification possible

After publishing the packages, we must make their signatures and certificates available for verification. We store these in GitLab's generic package registry, making them easily accessible to users who want to verify package authenticity.

```yaml
publish_signatures:
  extends: .python+cosign-job
  stage: publish_signatures
  script:
    - |
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          curl --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --fail 
               --upload-file "dist/${filename}.sig" 
               "${GENERIC_PACKAGE_BASE_URL}/${filename}.sig"

          curl --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --fail 
               --upload-file "dist/${filename}.crt" 
               "${GENERIC_PACKAGE_BASE_URL}/${filename}.crt"
        fi
      done
```

The signature publishing stage performs these key operations:

1. Processes each built package to find its corresponding signature files
2. Uses the GitLab API to upload the signature (`.sig`) file to the generic package registry
3. Uploads the corresponding certificate (`.crt`) file
4. Makes these verification artifacts available for downstream package consumers
5. Uses the same version and package name to maintain the connection between packages and signatures

## Consumer verification: Testing the user experience

The final stage simulates how end users will verify your package's authenticity. This stage acts as a final check and a practical example of the verification process.

```yaml
consumer_verification:
  extends: .python+cosign-job
  stage: consumer_verification
  script:
    - |
      git init
      git config --global init.defaultBranch main
      mkdir -p pkg signatures

      pip download --index-url "https://gitlab-ci-token:${CI_JOB_TOKEN}@gitlab.com/api/v4/projects/${CI_PROJECT_ID}/packages/pypi/simple" 
          "${NORMALIZED_NAME}==${PACKAGE_VERSION}" --no-deps -d ./pkg

      pip download --no-binary :all: 
          --index-url "https://gitlab-ci-token:${CI_JOB_TOKEN}@gitlab.com/api/v4/projects/${CI_PROJECT_ID}/packages/pypi/simple" 
          "${NORMALIZED_NAME}==${PACKAGE_VERSION}" --no-deps -d ./pkg

      failed=0
      for file in pkg/*.whl pkg/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          sig_url="${GENERIC_PACKAGE_BASE_URL}/${filename}.sig"
          cert_url="${GENERIC_PACKAGE_BASE_URL}/${filename}.crt"

          curl --fail --silent --show-error 
               --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --output "signatures/${filename}.sig" 
               "$sig_url"

          curl --fail --silent --show-error 
               --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --output "signatures/${filename}.crt" 
               "$cert_url"

          if ! cosign verify-blob 
            --signature "signatures/${filename}.sig" 
            --certificate "signatures/${filename}.crt" 
            --certificate-identity "${CERTIFICATE_IDENTITY}" 
            --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" 
            "$file"; then
            failed=1
          fi
        fi
      done

      if [ $failed -eq 1 ]; then
        exit 1
      fi
```

This consumer verification stage simulates the end-user experience by:

1. Creating a clean environment to test package installation
2. Downloading the published packages from the GitLab PyPI registry
3. Retrieving the corresponding signatures and certificates from the generic package registry
4. Performing the same verification steps that end users would perform
5. Ensuring the entire process works from a consumer's perspective
6. Failing the pipeline if any verification step fails, providing an early warning of any issues

## Summary

This comprehensive pipeline provides a secure and reliable way to build, sign, and publish Python packages to GitLab's package registry. By following these practices and implementing the suggested security measures, you can ensure your packages are appropriately verified and safely distributed to your users.

The pipeline combines modern security practices with efficient automation to create a robust software supply chain. Using Sigstore's Cosign for signing and attestation, along with GitLab's built-in security features, you can provide users with trustworthy cryptographically verified packages.

> #### Get started on your security journey today with a free 60-day trial of GitLab Ultimate.

## Learn more

- Documentation: Use Sigstore for keyless signing and verification
- Streamline security with keyless signing and verification in GitLab
- Annotate container images with build provenance using Cosign in GitLab CI/CD

Supply chain security is a critical concern in software development. Organizations need to verify the authenticity and integrity of their software packages. This guide will show you how to implement a secure CI/CD pipeline for Python packages using GitLab CI, incorporating package signing and attestation using Sigstore's Cosign.

You'll learn:

- Why sign and attest your Python packages?
    
- Pipeline overview
    
- Complete pipeline implementation: Setting up the environment
    
    - Environment configuration
    - Configuration breakdown
- The 6 stages
    
    1. Building
    2. Signing
    3. Verification
    4. Publishing
    5. Publishing signatures
    6. Consumer verification

## Why sign and attest your Python packages?

Here are four reasons to sign and attest your Python packages:

- **Supply chain security:** Package signing ensures that the code hasn't been tampered with between build and deployment, protecting against supply chain attacks.
- **Compliance requirements:** Many organizations, especially in regulated industries, require cryptographic signatures and provenance information for all deployed software.
- **Traceability:** Attestations provide a verifiable record of build conditions, including who built the package and under what circumstances.
- **Trust verification:** Consumers of your package can cryptographically verify its authenticity before installation.

## Pipeline overview

Ensuring your code's integrity and authenticity is necessary. Imagine a pipeline that doesn't just compile your code but creates a cryptographically verifiable narrative of how, when, and by whom your package was created. Each stage acts as a guardian, checking and documenting the package's provenance.

Here are six stages of a GitLab pipeline that ensure your package is secure and trustworthy:

- Build: Creates a clean, standard package that can be easily shared and installed.
- Signing: Adds a digital signature that proves the package hasn't been tampered with since it was created.
- Verification: Double-checks that the signature is valid and the package meets all our security requirements.
- Publishing: Uploads the verified package to GitLab's package registry, making it available for others to use.
- Publishing Signatures: Makes signatures available for verification.
- Consumer Verification: Simulates how end users can verify package authenticity.

## Complete pipeline implementation: Setting up the environment

Before we build our package, we need to set up a consistent and secure build environment. This configuration ensures every package is created with the same tools, settings, and security checks.

### Environment configuration

Our pipeline requires specific tools and settings to work correctly.

Primary configurations:

- Python 3.10 for consistent builds
- Cosign 2.2.3 for package signing
- GitLab package registry integration
- Hardcoded package version for reproducibility

**Note about versioning:** We've chosen to use a hardcoded version (`"1.0.0"`) in this example rather than deriving it from git tags or commits. This approach ensures complete reproducibility and makes the pipeline behavior more predictable. In a production environment, you might want to use semantic versioning based on git tags or another versioning strategy that fits your release process.

Tool requirements:

- Basic utilities: `curl`, `wget`
- Cosign for cryptographic signing
- Python packaging tools: `build`, `twine`, `setuptools`, `wheel`

### Configuration breakdown

```yaml
variables:
  PYTHON_VERSION: '3.10'
  PACKAGE_NAME: ${CI_PROJECT_NAME}
  PACKAGE_VERSION: "1.0.0"
  FULCIO_URL: 'https://fulcio.sigstore.dev'
  REKOR_URL: 'https://rekor.sigstore.dev'
  CERTIFICATE_IDENTITY: 'https://gitlab.com/${CI_PROJECT_PATH}//.gitlab-ci.yml@refs/heads/${CI_DEFAULT_BRANCH}'
  CERTIFICATE_OIDC_ISSUER: 'https://gitlab.com'
  PIP_CACHE_DIR: "$CI_PROJECT_DIR/.pip-cache"
  COSIGN_YES: "true"
  GENERIC_PACKAGE_BASE_URL: "${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/generic/${PACKAGE_NAME}/${PACKAGE_VERSION}"
```

We use caching to speed up subsequent builds:

```yaml
cache:
  paths:
    - ${PIP_CACHE_DIR}
```

## Building: Crafting the package

Every software journey begins with creation. In our pipeline, the build stage is where raw code transforms into a distributable package, ready to travel across different Python environments.

The build process creates two standardized formats:

- a wheel package (.whl) for quick, efficient installation
- a source distribution (.tar.gz) that carries the complete code

Here's the build stage implementation:

```yaml
build:
  extends: .python-job
  stage: build
  script:
    - git init
    - git config --global init.defaultBranch main
    - git config --global user.email "ci@example.com"
    - git config --global user.name "CI"
    - git add .
    - git commit -m "Initial commit"
    - export NORMALIZED_NAME=$(echo "${CI_PROJECT_NAME}" | tr '-' '_')
    - sed -i "s/name = ".*"/name = "${NORMALIZED_NAME}"/" pyproject.toml
    - sed -i "s|"Homepage" = ".*"|"Homepage" = "https://gitlab.com/${CI_PROJECT_PATH}"|" pyproject.toml
    - python -m build
  artifacts:
    paths:
      - dist/
      - pyproject.toml
```

Let's break down what this build stage does:

1. Initializes a Git repository (`git init`) and configures it with basic settings
2. Normalizes the package name by converting hyphens to underscores, which is required for Python packaging
3. Updates the package metadata in `pyproject.toml` to match our project settings
4. Builds both wheel and source distribution packages using `python -m build`
5. Preserves the built packages and configuration as artifacts for subsequent stages

## Signing: The digital notarization

If attestation is the package's biography, signing is its cryptographic seal of authenticity. This is where we transform our package from a mere collection of files into a verified, tamper-evident artifact.

The signing stage uses Cosign to apply a digital signature as an unbreakable seal. This isn't just a stamp — it's a complex cryptographic handshake that proves the package's integrity and origin.

```yaml
sign:
  extends: .python+cosign-job
  stage: sign
  id_tokens:
    SIGSTORE_ID_TOKEN:
      aud: sigstore
  script:
    - |
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          cosign sign-blob --yes 
            --fulcio-url=${FULCIO_URL} 
            --rekor-url=${REKOR_URL} 
            --oidc-issuer $CI_SERVER_URL 
            --identity-token $SIGSTORE_ID_TOKEN 
            --output-signature "dist/${filename}.sig" 
            --output-certificate "dist/${filename}.crt" 
            "$file"
        fi
      done
  artifacts:
    paths:
      - dist/
```

This signing stage performs several crucial operations:

1. Obtains an OIDC token from GitLab for authentication with Sigstore services
2. Processes each built package (both wheel and source distribution)
3. Uses Cosign to create a cryptographic signature (`.sig`) for each package
4. Generates a certificate (`.crt`) that proves the signature's authenticity
5. Stores both signatures and certificates alongside the packages as artifacts

## Verification: The security checkpoint

Verification is our final quality control gate. It's not just a check — it's a security interrogation where every aspect of the package is scrutinized.

```yaml
verify:
  extends: .python+cosign-job
  stage: verify
  script:
    - |
      failed=0
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          if ! cosign verify-blob 
            --signature "dist/${filename}.sig" 
            --certificate "dist/${filename}.crt" 
            --certificate-identity "${CERTIFICATE_IDENTITY}" 
            --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" 
            "$file"; then
            failed=1
          fi
        fi
      done
      if [ $failed -eq 1 ]; then
        exit 1
      fi
```

The verification stage implements several security checks:

1. Examines each package file in the `dist` directory
2. Uses Cosign to verify the signature matches the package content
3. Confirms the certificate's identity matches our expected GitLab pipeline identity
4. Validates our trusted OIDC provider issued the certificate
5. Fails the entire pipeline if any verification check fails, ensuring only verified packages proceed

## Publishing: The controlled release

Publishing is where we make our verified packages available through GitLab's package registry. It's a carefully choreographed release that ensures only verified, authenticated packages reach their destination.

```yaml
publish:
  extends: .python-job
  stage: publish
  script:
    - |
      cat << EOF > ~/.pypirc
      [distutils]
      index-servers = gitlab
      [gitlab]
      repository = ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi
      username = gitlab-ci-token
      password = ${CI_JOB_TOKEN}
      EOF
      TWINE_PASSWORD=${CI_JOB_TOKEN} TWINE_USERNAME=gitlab-ci-token 
        twine upload --repository-url ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi 
        dist/*.whl dist/*.tar.gz
```

The publishing stage handles several important tasks:

1. Creates a `.pypirc` configuration file with GitLab package registry credentials
2. Uses the GitLab CI job token for secure authentication
3. Uploads both wheel and source distribution packages to the GitLab PyPI registry
4. Makes the packages available for installation via pip

## Publishing signatures: Making verification possible

After publishing the packages, we must make their signatures and certificates available for verification. We store these in GitLab's generic package registry, making them easily accessible to users who want to verify package authenticity.

```yaml
publish_signatures:
  extends: .python+cosign-job
  stage: publish_signatures
  script:
    - |
      for file in dist/*.whl dist/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          curl --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --fail 
               --upload-file "dist/${filename}.sig" 
               "${GENERIC_PACKAGE_BASE_URL}/${filename}.sig"

          curl --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --fail 
               --upload-file "dist/${filename}.crt" 
               "${GENERIC_PACKAGE_BASE_URL}/${filename}.crt"
        fi
      done
```

The signature publishing stage performs these key operations:

1. Processes each built package to find its corresponding signature files
2. Uses the GitLab API to upload the signature (`.sig`) file to the generic package registry
3. Uploads the corresponding certificate (`.crt`) file
4. Makes these verification artifacts available for downstream package consumers
5. Uses the same version and package name to maintain the connection between packages and signatures

## Consumer verification: Testing the user experience

The final stage simulates how end users will verify your package's authenticity. This stage acts as a final check and a practical example of the verification process.

```yaml
consumer_verification:
  extends: .python+cosign-job
  stage: consumer_verification
  script:
    - |
      git init
      git config --global init.defaultBranch main
      mkdir -p pkg signatures

      pip download --index-url "https://gitlab-ci-token:${CI_JOB_TOKEN}@gitlab.com/api/v4/projects/${CI_PROJECT_ID}/packages/pypi/simple" 
          "${NORMALIZED_NAME}==${PACKAGE_VERSION}" --no-deps -d ./pkg

      pip download --no-binary :all: 
          --index-url "https://gitlab-ci-token:${CI_JOB_TOKEN}@gitlab.com/api/v4/projects/${CI_PROJECT_ID}/packages/pypi/simple" 
          "${NORMALIZED_NAME}==${PACKAGE_VERSION}" --no-deps -d ./pkg

      failed=0
      for file in pkg/*.whl pkg/*.tar.gz; do
        if [ -f "$file" ]; then
          filename=$(basename "$file")
          sig_url="${GENERIC_PACKAGE_BASE_URL}/${filename}.sig"
          cert_url="${GENERIC_PACKAGE_BASE_URL}/${filename}.crt"

          curl --fail --silent --show-error 
               --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --output "signatures/${filename}.sig" 
               "$sig_url"

          curl --fail --silent --show-error 
               --header "JOB-TOKEN: ${CI_JOB_TOKEN}" 
               --output "signatures/${filename}.crt" 
               "$cert_url"

          if ! cosign verify-blob 
            --signature "signatures/${filename}.sig" 
            --certificate "signatures/${filename}.crt" 
            --certificate-identity "${CERTIFICATE_IDENTITY}" 
            --certificate-oidc-issuer "${CERTIFICATE_OIDC_ISSUER}" 
            "$file"; then
            failed=1
          fi
        fi
      done

      if [ $failed -eq 1 ]; then
        exit 1
      fi
```

This consumer verification stage simulates the end-user experience by:

1. Creating a clean environment to test package installation
2. Downloading the published packages from the GitLab PyPI registry
3. Retrieving the corresponding signatures and certificates from the generic package registry
4. Performing the same verification steps that end users would perform
5. Ensuring the entire process works from a consumer's perspective
6. Failing the pipeline if any verification step fails, providing an early warning of any issues

## Summary

This comprehensive pipeline provides a secure and reliable way to build, sign, and publish Python packages to GitLab's package registry. By following these practices and implementing the suggested security measures, you can ensure your packages are appropriately verified and safely distributed to your users.

The pipeline combines modern security practices with efficient automation to create a robust software supply chain. Using Sigstore's Cosign for signing and attestation, along with GitLab's built-in security features, you can provide users with trustworthy cryptographically verified packages.

> #### Get started on your security journey today with a free 60-day trial of GitLab Ultimate.

## Learn more

- Documentation: Use Sigstore for keyless signing and verification
- Streamline security with keyless signing and verification in GitLab
- Annotate container images with build provenance using Cosign in GitLab CI/CD

Go to Source
