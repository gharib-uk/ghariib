---
title: "Autopilot node compromise via allowlisted workload masquerade"
date: Tue, 08 Mar 2022 00:00:00 GMT
draft: false
type: posts
categories: 
- 
---
# Autopilot node compromise via allowlisted workload masquerade

<br/>

<br/>
Unit 42 researchers disclosed several vulnerabilities and attack techniques in GKE Autopilot to Google, the root cause being insufficient verification of allowlisted workload image names. An attacker with permissions to create a pod could have abused these vulnerabilities to (1) escape their pod and compromise the underlying node, (2) escalate privileges and become full cluster administrators, and (3) covertly persist administrative access through backdoors that are completely invisible to cluster operators.

#### [Source](https://www.cloudvulndb.org/gke-autopilot-allowlist)

<br/>
---
