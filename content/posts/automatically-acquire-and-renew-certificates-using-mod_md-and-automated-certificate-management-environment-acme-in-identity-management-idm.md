---
title: "Automatically acquire and renew certificates using mod_md and Automated Certificate Management Environment (ACME) in Identity Management (IdM)"
date: 2025-01-02
categories: 
  - "general"
---

IntroductionIn a previous article, I demonstrated how to configure the Automatic Certificate Management Environment (ACME) feature included in the Identity Management (IdM) Dogtag Certificate Authority (CA). Specifically, I covered installation of IdM with random serial numbers, and how to enable the ACME service and expired certificate pruning. This article explains the management of ACME (currently a technology preview) with IdM and Red Hat Enterprise Linux (RHEL) clients.Currently, mod\_md is the only ACME client implementation completely supported and provided by Red Hat. For this article,

Go to Source
