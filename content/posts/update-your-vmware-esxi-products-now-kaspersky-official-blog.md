---
title: "Update your VMware ESXi products now | Kaspersky official blog"
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

Why VM escape threatens large organizations, and how you can defend against the threat.

On March 4, Broadcom released emergency updates to address three vulnerabilities — CVE-2025-22224, CVE-2025-22225 and CVE-2025-22226 — that affect several VMware products, including ESXi, Workstation, and Fusion. A note in the Broadcom advisory stated that at least one of these — CVE-2025-22224 — has been exploited in real-world attacks. The vulnerabilities allow for virtual machine escape — enabling attackers to execute code directly on the ESX hypervisor. Information available on VMware’s GitHub suggests that the Microsoft Threat Intelligence Center was the first to detect the exploit in the wild and notify Broadcom. Neither company has named the attacker or the victim.

Broadcom reports that the vulnerabilities affect VMware ESXi 7.0–8.0, Workstation 17.x, vSphere 6.5–8, Fusion 13.x, Cloud Foundation 4.5–5.x, Telco Cloud Platform 2.x–5.x, and Telco Cloud Infrastructure 2.x–3.x. However, some experts suggest that the range of impacted products is potentially wider. In particular, older versions of ESXi, such as 5.5, should be vulnerable as well, but these unsupported versions are not getting patched. According to some assessments, more than 41,000 ESXi servers had been affected across the globe (mainly in China, France, the U.S., Germany, Iran and Brazil) as at the end of last week.

## What issues VMware has fixed

The most severe vulnerability in VMware ESXi and Workstation — CVE-2025-22224 — received a CVSS rating of 9.3. It’s related to a heap overflow in VMCI, and allows an attacker with local administrative privileges on the virtual machine to execute code as the VMX process on the host — the hypervisor.

The CVE-2025-22225 vulnerability in VMware ESXi (CVSS 8.2) allows an attacker to perform an arbitrary kernel write, which also implies sandbox escape. CVE-2025-22226 — an HGFS information disclosure vulnerability (CVSS 7.1) — permits an attacker with guest VM administrative access to extract the contents of the VMX process memory. VMware ESXi, Workstation, and Fusion are affected by this vulnerability.

## Dangerous exploitation scenarios

The vulnerability descriptions indicate that exploitation requires an attacker to have already compromised the virtual machine and possess administrative privileges on it. This seems like a relatively high entry barrier, but in reality such a scenario can materialize quite easily. The primary danger of these vulnerabilities is that they drastically reduce the steps an attacker needs to take from compromising a single virtual machine to completely seizing control of the computing cluster. The trio of vulnerabilities allows the attacker to reach hypervisor level without conducting “noisy” network environment scans for servers, or having to circumvent network security measures. The following are typical enterprise scenarios where this could occur:

- VMware-based VDI workstations. A single employee makes a mistake by launching a malicious attachment on their virtual workstation. Instead of just one workstation being compromised, this leads to a large-scale incident.
- VMware-based hybrid and private clouds. A successful compromise of any server via a publicly accessible application vulnerability allows an attacker to rapidly propagate the attack across the entire network.
- Leasing virtual servers and workstations (prebuilt VMs) from an MSP. A client’s error leading to infection on a rented host will result in compromise of all MSP clients sharing resources within the same cluster.

Some features of VMware clusters create further complexities in detecting and remediating such incidents. Once an attacker compromises the hypervisor level, they automatically gain access to all storage connected to the cluster. The attacker can then move freely throughout the VMware environment, and the configuration files available from the hypervisor permit their conducting extensive reconnaissance without raising security alerts.

The hypervisor lacks an EDR agent, and security tools have very limited visibility into what’s happening at the cluster level. Hackers can sneak in and grab important information, such as Active Directory databases, without security teams noticing. All of these factors make the three VMware vulnerabilities a veritable goldmine for malicious actors — particularly ransomware groups. They’ve repeatedly conducted attacks on ESXi environments in the past: RansomExx, ESXiargs, Clop, and so on.

## Recommendations for organizational security

Luckily for businesses, proof-of-concept (PoC) code for exploiting these vulnerabilities has not yet been published, so widespread exploitation of the flaw has not begun. Nevertheless, such code could surface at any moment, so VMware products need to be updated quickly as a top priority. Since patching VMware environments can be complex, especially in high-availability infrastructures, organizations should leverage tools like vMotion to deploy patches without downtime.

Patching is the only mitigation for these vulnerabilities. However, Broadcom also recommends reviewing your settings according to the vSphere Security Configuration & Hardening guide. Among other things, you need to ensure that your VMware infrastructure is properly segmented to restrict access to the hypervisor management network.

Be sure to use cloud security tools, including having an EDR agent properly installed and running on your virtual machines. This will allow for the detection and prevention of the initial infection stage — blocking attackers from obtaining the administrative access required to exploit the vulnerabilities.

Go to Source
