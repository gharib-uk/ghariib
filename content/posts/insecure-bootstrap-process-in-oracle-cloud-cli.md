---
title: "Insecure Bootstrap Process in Oracle Cloud CLI"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "oracle"
---

## Summary

The bootstrap process for Oracle Cloud CLI using the “curl | bash” pattern was insecure since there was no way to verify authenticity of the downloaded binaries. The vendor is now publishing checksums that can be used to verify the downloaded binaries.

## Vulnerability Details

As part of our ongoing research into supply chain attacks, we have been analyzing bash installer scripts using the “curl | basj” pattern. Oracle provides such script used to install the CLI command for interaction with Oracle Cloud. However, there was no way to check whether the files that the script downloads are legitimate, which could potentially open the end-user to supply chain attacks. The installer is run as follows:

```
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

## Vendor Response

The vendor started publishing SHA-256 checksums for the CLI.

## References

Vendor reference # S1456147

Vendor security advisory: Jan 2022

## Timeline

2021-04-21: Initial report to the vendor  
2021-04-21: Vendor acknowledged the report  
2021-05-04: Vendor communicated that a fix is pending  
2021-12-28: Vendor reported that a fix has been implemented and credit will be provided in an advisory  
2022-01-18: Vendor advisory published  
2022-02-06: Public disclosure

Go to Source
