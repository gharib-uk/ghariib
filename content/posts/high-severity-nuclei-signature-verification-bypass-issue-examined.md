---
title: "High-severity Nuclei signature verification bypass issue examined"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "development"
  - "devops"
tags: 
  - "vulnerability-management"
---

Such a flaw stems from Nuclei's template signature verification process, with the simultaneous usage of regular expressions, or regex, and YAML parser potentially resulting in the introduction of a "r" character read as a line break and leading to the circumvention of regex-based signature verification.

Go to Source
