---
title: "FIPS mode for Red Hat Go Toolset"
date: 2025-01-23
---

Red Hat Go Toolset includes modifications to allow applications to optionally use OpenSSL as a cryptographic backend instead of the standard Go crypto implementation. This approach replaces upstream Go’s BoringSSL bindings with bindings into Red Hat’s own distribution of OpenSSL. The Go Toolset runtime is designed to be able to detect whether or not the operating system is booted in FIPS mode and to select either the standard crypto or OpenSSL back end accordingly.

In general, running Go applications in FIPS mode requires the following steps:

1. Install Go Toolset with `dnf install -y go-toolset`.
2. Build the application using the toolchain default settings.
3. Ensure the runtime Red Hat Enterprise Linux (RHEL) or Red Hat Enterprise Linux CoreOS (RHCOS) operating system is installed and booted in FIPS mode with OpenSSL available.

## Validating FIPS mode capabilities

Go Toolset's linkage with OpenSSL works via a combination of CGO and dynamic loading of OpenSSL through glibc’s `dlopen`, and requires both of these toolchain features to be accessible to the Go compiler and runtime. 

In addition, the OpenSSL version present on the system at runtime must match the version used to build the application. Lack of availability of any of these features will preclude an application from being able to use the OpenSSL backend. To ensure OpenSSL is used, the following requirements should be met:

- The application must be built with `CGO_ENABLED=1` (which is the toolchain default, but must not be overridden).
- The application must **not** be built with the `-no_openssl` tag.
- The application must link dynamically (not statically) with glibc, due to the usage of dlopen. The `-extldflags “-static”` setting must **not** be used.
- The version of OpenSSL used at build time must be available at runtime.

Newer versions of Go Toolset introduce an experimental startup check, which will attempt to detect some of these build or runtime configuration issues and crash.  To enable this option, build the application using the following GOEXPERIMENT setting:

- `GOEXPERIMENT=strictfipsruntime`

The following article also includes information on verifying that the application is calling into OpenSSL at runtime: Is your Go application FIPS compliant?

The post FIPS mode for Red Hat Go Toolset appeared first on Red Hat Developer.  
  

Go to Source
