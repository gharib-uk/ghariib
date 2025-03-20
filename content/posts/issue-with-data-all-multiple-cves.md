---
title: "Issue with data.all (Multiple CVEs)"
date: 2025-01-02
categories: 
  - "security"
---

**Publication Date: 2024/11/8 4:00 PM PDT  
**

Data.all is an open source development framework to help customers build a data marketplace on AWS.

We have identified the following issues within data.all version 1.0.0 through 2.6.0. On November 8, 2024, we released a fix and recommend customers upgrade to version 2.6.1 or later and ensure any forked or derivative code are patched to incorporate the new fixes.

- CVE-2024-52311 relates to an issue where data.all does not invalidate authentication token upon user logout.
- CVE-2024-52312 relates to an issue where data.all authenticated users can perform restricted operations against DataSets and Environments.
- CVE-2024-52313 relates to an issue where data.all authenticated users can obtain incorrect object level authorizations.
- CVE-2024-52314 relates to an issue where data.all admin user may access potentially sensitive data stored by producers via logs.
- CVE-2024-10953 relates to an issue where data.all authenticated users can perform mutating update operations on persisted notification records.

References:

- CVE-2024-52311 GitHub Security Advisory  
    
- CVE-2024-52312 GitHub Security Advisory  
    
- CVE-2024-52313 GitHub Security Advisory  
    
- CVE-2024-52314 GitHub Security Advisory  
    
- CVE-2024-10953 GitHub Security Advisory  

Please email aws-security@amazon.com with any security questions or concerns.

Go to Source
