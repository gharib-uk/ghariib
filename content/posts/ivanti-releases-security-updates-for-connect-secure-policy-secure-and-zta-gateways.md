---
title: "Ivanti Releases Security Updates for Connect Secure, Policy Secure, and ZTA Gateways"
date: 2025-01-10
categories: 
  - "cisa"
  - "cybersecurity"
  - "security"
---

Ivanti released security updates to address vulnerabilities (CVE-2025-0282, CVE-2025-0283) in Ivanti Connect Secure, Policy Secure, and ZTA Gateways. A cyber threat actor could exploit CVE-2025-0282 to take control of an affected system.  
  
CISA has added CVE-2025-0282 to its Known Exploited Vulnerabilities Catalog, based on evidence of active exploitation.

CISA urges organizations to hunt for any malicious activity, report any positive findings to CISA, and review the following for more information:

- Ivanti: Security Advisory Ivanti Connect Secure, Policy Secure & ZTA Gateways (CVE-2025-0282, CVE-2025-0283)
- Mandiant: Ivanti Connect Secure VPN Targeted in New Zero-Day Exploitation

  
For all instances of Ivanti Connect Secure, Policy Secure, and ZTA Gateways, see the following steps for general hunting guidance:

1. Conduct threat hunting actions:  
    1. Run the In-Build Integrity Checker Tool (ICT). Instructions can be found here. 
    2. Conduct threat hunt actions on any systems connected to—or recently connected to—the affected Ivanti device.  
2. If threat hunting actions determine no compromise: 
    1. Factory reset the device and apply the patch described in Security Advisory Ivanti Connect Secure, Policy Secure & ZTA Gateways (CVE-2025-0282, CVE-2025-0283). 
    2. Monitor the authentication or identity management services that could be exposed. 
    3. Continue to audit privilege level access accounts. 
3. If threat hunting actions determine compromise: 
    1. Report to CISA and Ivanti immediately to start forensic investigation and incident response activities.  
    2. Disconnect instances of affected Ivanti Connect Secure products.  
    3. Isolate the systems from any enterprise resources to the greatest degree possible. 
    4. Revoke and reissue any connected or exposed certificates, keys, and passwords, to include the following: 
        1. Reset the admin enable password. 
        2. Reset stored application programming interface (API) keys. 
        3. Reset the password of any local user defined on the gateway, including service accounts used for auth server configuration(s).  
    5. If domain accounts associated with the affected products have been compromised: 
        1. Reset passwords twice for on premise accounts, revoke Kerberos tickets, and then revoke tokens for cloud accounts in hybrid deployments. 
        2. For cloud joined/registered devices, disable devices in the cloud to revoke the device tokens.
    6. After investigation, fully patch and restore system prior to returning any device to service.

Organizations should report incidents and anomalous activity to CISA’s 24/7 Operations Center at Report@cisa.gov or (888) 282-0870. When available, please include the following information regarding the incident: date, time, and location of the incident; type of activity; number of people affected; type of equipment used for the activity; the name of the submitting company or organization; and a designated point of contact.

Go to Source
