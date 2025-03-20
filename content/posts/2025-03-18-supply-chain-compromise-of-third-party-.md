---
title: "Supply Chain Compromise of Third-Party GitHub Action CVE-2025-30066"
date: Tue, 18 Mar 25 12:00:00 +0000
draft: false
type: posts
categories: 
- 
---
# Supply Chain Compromise of Third-Party GitHub Action CVE-2025-30066

<br/>

<br/>
A popular third-party GitHub Action, tj-actions/changed-files (tracked as [CVE-2025-30066](https://www.cve.org/CVERecord?id=CVE-2025-30066)), was compromised. This GitHub Action is designed to detect which files have changed in a pull request or commit. The supply chain compromise allows for information disclosure of secrets including, but not limited to, valid access keys, GitHub Personal Access Tokens (PATs), npm tokens, and private RSA keys. This has been patched in v46.0.1. 

_**(Updated March 19, 2025)**_ The compromise of tj-actions/changed-files was potentially due to a similar compromise of another GitHub Action, reviewdog/action-setup@v1 (tracked as [CVE-2025-30154](https://www.cve.org/CVERecord?id=CVE-2025-30154)), which occurred around the same time. The following Actions may also be affected:  

-   reviewdog/action-shellcheck 

-   reviewdog/action-composite-template 

-   reviewdog/action-staticcheck 

-   reviewdog/action-ast-grep 

-   reviewdog/action-typos

  
CISA added CVE-2025-30066 to its [Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog). 

CISA strongly urges users to implement the recommendations to mitigate this compromise and strengthen security when using third-party actions.  

See the following resources for more guidance: 

-   GitHub: [tj-actions changed-files through 45.0.7 allows remote attackers to discover secrets by reading actions logs](https://github.com/advisories/GHSA-mrrh-fwg8-r2c3)  

-   GitHub: [Security hardening for GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions#using-third-party-actions) 

-   GitHub: [tj-actions/changed-files: :octocat: Github action to retrieve all (added, copied, modified, deleted, renamed, type changed, unmerged, unknown) files and directories](https://github.com/tj-actions/changed-files?tab=readme-ov-file#changed-files) 

-   StepSecurity: [Harden-Runner detection: tj-actions/changed-files action is compromised](https://www.stepsecurity.io/blog/harden-runner-detection-tj-actions-changed-files-action-is-compromised#recovery-steps) 

-   Wiz: [GitHub Action tj-actions/changed-files supply chain attack](https://www.wiz.io/blog/github-action-tj-actions-changed-files-supply-chain-attack-cve-2025-30066)
-   _**(Updated March 19, 2025)**_ GitHub: [Multiple Reviewdog actions were compromised during a specific time period · CVE-2025-30154 · GitHub Advisory Database](https://github.com/advisories/GHSA-qmg3-hpqr-gqvc) 

  
Organizations should report incidents and anomalous activity to CISA’s 24/7 Operations Center at [Report@cisa.gov](mailto:Report@cisa.gov) or (888) 282-0870.  

_This alert is provided “as is” for informational purposes only. CISA does not provide any warranties of any kind regarding any information within. CISA does not endorse any commercial product, entity, or service referenced in this alert or otherwise._

#### [Source](https://www.cisa.gov/news-events/alerts/2025/03/18/supply-chain-compromise-third-party-github-action-cve-2025-30066)

<br/>
---
