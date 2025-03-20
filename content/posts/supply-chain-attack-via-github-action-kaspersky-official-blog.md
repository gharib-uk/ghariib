---
title: "Supply chain attack via GitHub Action | Kaspersky official blog"
date: 2025-03-19
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

Immediately update secrets that may have leaked due to the compromise of the changed-files GitHub Action via CVE-2025-30066.

Attacks on open-source mostly start with publishing new malicious packages in repositories. But the attack that occurred on March 14 is in a different league — attackers compromised the popular GitHub Action tj-actions/changed-files, which is used in more than 23,000 repositories. The incident was assigned CVE-2025-30066.  All repositories that used the infected changed-files Action are susceptible to this vulnerability. Although the GitHub administration blocked changed-files Action and then rolled it back to a safe version, everyone who used it should conduct an incident response, and the developer community should draw more general lessons from this incident.

## What are GitHub Actions?

GitHub Actions are workflow patterns that simplify software development by automating common DevOps tasks. They can be triggered when certain events (such as commits) occur at GitHub. GitHub has a kind of app-store where developers can take a ready-made workflow process and apply it to their repository. To integrate such a ready-made GitHub process into your CI/CD development pipeline, you only need one line of code.

## changed-files compromise incident

On March 14, the popular tj-actions/changed-files GitHub Action — used to get any changed files from a project — was infected with malicious code. The attackers modified the process code and updated the version tags to include a malicious commit in all versions of changed-files GitHub Action. This was done on behalf of the Renovate Bot user, but according to current information the bot itself wasn’t compromised; it was just a disguise for an anonymous commit.

The malicious code in changed-files is disguised as the updateFeatures function, which actually runs a malicious Python script and dumps the Runner Worker process memory, then searches it for data that looks like secrets (AWS, Azure and GCP keys, GitHub PAT and NPM tokens, DB accounts, RSA private keys). If something similar is found, it’s written to the repository logs. Both the malicious code and the stolen secrets are written with simple obfuscation — double base64 encoding. If the logs are publicly available, attackers (and not only the operators of the attack, but anyone!) can freely download and decrypt this data. On March 15, a day after the incident was discovered, GitHub deleted the changed-files process, and the CI/CD processes based on it may have not functioned. After another eight hours, the process repository was restored in a “clean version”, and now changed-files is working again without surprises.

## Incident Response

Since logs in public repositories are accessible to outsiders, they’re the most likely to have been affected by the leak. However, in an enterprise environment, relying solely on the assumption that “all our repositories are private” is also not a good idea. Companies often have both public and private repositories, and if their CI/CD pipelines use overlapping secrets, attackers can still use this data to compromise container registries or other resources. Containers or packages built by popular open-source projects can also be compromised in this scenario.

The authors of the ill-fated changed-files recommend analyzing GitHub logs for March 14 and 15. If unusual data is found in the changed-files subsection, it should be decoded to understand what information may have been leaked. Additionally, it’s worth examining GitHub logs for this period for suspicious IP addresses. All changed-files users are advised to replace secrets that could have been used in the build and leaked during this period. First of all, you should pay attention to repositories with public CI logs, and secondly, to private repositories.

In addition to replacing potentially compromised secrets, it’s recommended to download the logs for subsequent analysis, and then clear their public versions.

## Lessons from the incident

The complexity and variety of attacks on the supply chain in software development are growing: we’ve already become accustomed to attacks in the form of malicious repositories, infected packages and container images, and we’ve encountered malicious code in test cases — and now in CI/CD processes. Strict information-security hygiene requirements should extend to the entire life-cycle of an IT project.

In addition to the requirement to strictly select the source code base of your project (open source packages, container images, automation tools), a comprehensive container security solution and a secrets management system are necessary. Importantly, the requirements for special handling of secrets apply not only to the project’s source code, but also to the development processes. GitHub has a detailed guide on securely configuring GitHub Actions — the largest section of which is devoted specifically to handling secrets.

Go to Source
