---
title: "Insecure Bootstrap Process in Google’s Cloud SQL Proxy"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
tags: 
  - "google"
---

## Summary

The bootstrap process for Google’s cloud SQL Proxy CLI uses the “curl | bash” pattern and didn’t document a way to verify authenticity of the downloaded binaries. The vendor updated documentation with information on how to use checksums to verify the downloaded binaries.

## Vulnerability Details

As part of our ongoing research into supply chain attacks, we have been analyzing bash installer scripts using the “curl | bash” pattern. Google provides such script used to install the Cloud SQL proxy. However, the documentation doesn’t indicate how to verify downloaded files prior to execution.

## Vendor Response

The vendor updated their documentation with information on how to verify downloaded binaries via checksums.

## References

Vendor issue tracker # 244384166

## Timeline

2022-08-30: Initial report to the vendor  
2022-08-30: Vendor acknowledged the report  
2022-09-27: Vendor rejected the report as a security issue  
2023-03-03: Vendor reported that a fix has been implemented  
2023-03-19: Public disclosure

Go to Source
