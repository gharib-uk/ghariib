---
title: "The benefits of native FIPS support in Go 1.24"
date: 2025-03-19
---

The Go programming language has reached another significant milestone with the release of version 1.24. We at Red Hat are particularly excited about one of its standout features, native FIPS 140-3 support. This addition represents a major step forward for Go's adoption in enterprise and government environments where security compliance is paramount.

## Native FIPS support

The introduction of the FIPS Cryptographic Module in Go 1.24 marks a watershed moment for the language's security capabilities. This new module provides FIPS 140-3-compliant implementations of cryptographic algorithms, seamlessly integrated into the standard library. What makes this particularly noteworthy is its transparent implementation. Existing Go applications can leverage FIPS-compliant cryptography without requiring code changes.

## Red Hat's contribution to Go's FIPS

At Red Hat, we've long understood the importance of FIPS compliance for our enterprise customers, particularly those in regulated industries and government sectors. Our engineers have worked closely with the Go team to help make this feature a reality, contributing our expertise in FIPS implementation and certification processes gained from our extensive experience with Red Hat Enterprise Linux (RHEL).

The collaboration between Red Hat and the Go team has focused on ensuring that the FIPS implementation meets both the rigorous standards required for certification and the practical needs of real-world applications and RHEL customers and the overall Go community. This partnership demonstrates our commitment to not just using open source software, but actively contributing to its advancement.

## Implementation details

The new FIPS support in Go 1.24 introduces two key mechanisms for enabling FIPS compliance:

1. Build-time configuration through the `GOFIPS140` environment variable, allowing developers to select specific versions of the Go Cryptographic Module.
    
2. Runtime control via the `fips140` GODEBUG setting, enabling dynamic FIPS mode activation.
    

The initial release includes Go Cryptographic Module version v1.0.0, which is currently undergoing validation with a CMVP-accredited laboratory. This thorough validation process ensures that the implementation meets all FIPS 140-3 requirements.

## Benefits for enterprise users

This native FIPS support brings several significant advantages:

- **Simplified compliance:** Organizations can more easily meet FIPS requirements without additional third-party modules.
    
- **Improved maintainability:** Direct integration with Go's standard library means fewer external dependencies.
    
- **Better performance:** Native implementation allows for optimized cryptographic operations without the overhead of CGO calling into OpenSSL.
    
- **Reduced development overhead:** Transparent integration means no code changes for existing applications.
    

## Red Hat's commitment

We at Red Hat will continue to maintain our existing FIPS solution for older Go versions. This will ensure that customers using RHEL, Red Hat OpenShift, or other Red Hat products in their FIPS environments will still continue to work and benefit from security updates. Going forward, we are committed to moving over to the pure Go solution, dropping a significant amount of downstream modifications. This is in line with Red Hat's dedication to open source and our upstream first mindset.

Looking ahead, Red Hat plans to integrate Go's native FIPS module into our product ecosystem. This will allow us to provide a more streamlined and maintainable FIPS-compliant environment for Go applications across our platforms. We're committed to the following practices:

- Contributing to the upstream maintenance and enhancement of the FIPS Cryptographic Module.
    
- Providing feedback and real-world usage patterns to guide future development.
    
- Ensuring smooth integration with Red Hat's existing FIPS-compliant infrastructure.
    

## Next steps

The addition of native FIPS support in Go 1.24 represents a significant maturation of the language's enterprise readiness. Red Hat is proud to have contributed to this achievement and remains committed to supporting and enhancing this capability both upstream and within our product portfolio.

As the Go ecosystem continues to evolve, Red Hat will maintain our active involvement in the community, ensuring that critical features like FIPS compliance receive the attention and support they deserve. We look forward to seeing how this new capability enables our customers to build more secure and compliant applications with Go.

The post The benefits of native FIPS support in Go 1.24 appeared first on Red Hat Developer.  
  

Go to Source
