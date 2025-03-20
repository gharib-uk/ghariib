---
title: "LevelBlue SOC Analysts See Sharp Rise in Cyber Threats: Stay Vigilant"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
---

This holiday season our SOC analysts have observed a sharp uptick in cyber threat activity. Specifically, they’ve seen a rise in attempted ransomware attacks, which started during the American Thanksgiving holiday period (November 25–31, 2024) and are expected to continue throughout the holiday season. We’re sharing details on the threat actors involved, their tactics, as well as recommendations to give you knowledge and tools to proactively strengthen your security against evolving threats.

## Key Threat Groups

### BlackSuit (formerly “Royal”)

Known for targeting critical infrastructure sectors, including healthcare, government, and manufacturing, BlackSuit employs data exfiltration, extortion, and encryption techniques, according to a Cybersecurity and Infrastructure Security Agency (CISA) advisory.

Common attack vectors include:

- Phishing emails and malicious websites
- Exploitation of unsecured virtual private networks (VPNs) lacking multi-factor authentication (MFA)
- Disabling antivirus software to exfiltrate data before encrypting systems

### Black Basta

Operating as a ransomware-as-a-service (RaaS), Black Basta affiliates have targeted over 500 entities in 2024 alone in North America, Europe, and Australia, according to CISA. Key tactics:

- Vishing: Impersonating help desk technicians via phone to access networks
- Using malicious remote management tools to gain entry and escalate attacks

### LevelBlue Observations of Threat Actor TTPs and How to Fortify Security

In recent weeks, our SOC team has observed threat actors using the following tactics to launch attacks:

| Tactic | Recommendations |
| --- | --- |
| Exploitation of a **VPN portal that is not enforcing MFA** to gain initial access |   - Enforce MFA for VPN connections and geo-fence your VPN portal(s)         - Patch VPN devices. Historically we have observed these external-facing network appliances be compromised   |
|   The use of **vishing** (impersonating a “help desk” team member) to gain initial access to end-user workstations, which then gives the attacker access to the larger network (emails and text messages are also being leveraged for credential collection and malware deployment)  **Two numbers LevelBlue has identified to be involved in incidents are 1-844-201-3441 and 304-718-2459**       |   - Provide employees with training and education on vishing attacks and the common lures that may be used         - Implement a process of verification for both help desk employees and employees being called during legitimate IT support scenarios         - Direct employees to report suspicious communications immediately to a supervisor and security leadership           |
| The use of **Rclone, WinSCP, and other file transfer tools to exfiltrate data** from environments |   - Block the installation or execution of common attacker tools that do not have a designated function within your network, or strictly enforce the exceptions for allowing the usage   |
|   **Exploitation of vulnerabilities** across common software/applications to escalate privileges  Vulnerabilities for VMware, Microsoft Exchange, Microsoft SharePoint, and other self-hosted applications are being particularly targeted to gain administrator or even root access within environments   |   - Patch software per vendor recommendations and review your organization’s vulnerability scanning and patching schedule         - Maintain good records of applications and operating systems running within your environment, and enable notifications for when patch notifications, emails, or news updates come out about these applications and operating systems           |
| The use of **Remote Desktop Protocol (RDP), Window Remote Management (WinRM), and Remote Monitoring Management (RMM) tools** for lateral movement |   - Block any external to internal RDP attempts and disable RDP on hosts that do not need it         - Limit RDP and WinRM traffic from segments of the network that do not require that type of west/east traversal. This can also apply to other protocols and overall network traffic as well, stop an attacker’s lateral movement         - Block the installation or execution of RMM tools that are not explicitly used by your organization. **Note that RMM tools have been observed in almost every ransomware-related incident the LevelBlue SOC team has investigated.** Blocking the installation or execution of these tools will significantly decrease the effectiveness of an attack   |

##   
Other Proactive Cybersecurity Measures

### Enhance Employee Awareness

While employees might be enjoying more festivities this time of year, it’s important to communicate the urgency of heightened vigilance during the holiday season. Educate employees on recognizing and reporting suspicious communications. And provide clear guidance on verifying IT support contacts.

### Validate Security Controls and Address Potential Exposures

Stay on top of patching and ensure public-facing assets are secured through MFA. We’re here to help identify potential security gaps and exposures. Take advantage of a 30-day free trial with LevelBlue’s Vulnerability Management service.

### Protect Against Malicious Sites and Emails

If you do not already have email security, secure remote access, or secure web gateway protections in place, consider adding them. LevelBlue provides flexible, managed service delivery options with a choice of leading technologies. These services can help protect employees from phishing attempts and malicious sites as well as help control and manage access to applications.

### Fortify Endpoint Security

More than 75% of organizations say they have experienced at least one cyberattack due to unknown, unmanaged, or poorly managed devices.2 LevelBlue Managed Endpoint Security with SentinelOne protects diverse endpoints, including laptops, servers, desktops, and cloud workloads, from evolving threats. Pair this service with LevelBlue Managed Threat Detection and Response to cover your entire attack surface. We also offer several tiers for an incident response retainer, giving customers access to additional response, forensics, and recovery support. 

Finally, it may be tempting to let tasks linger this time of year, but as we all know, cybercriminals will use that to their advantage. Address security concerns immediately, so they do not compound and grow more severe. The holidays are a busy time for everyone, including threat actors. Use our support services during this season and beyond to fortify your cyber operations and ensure your organization remains safe.

## Contact LevelBlue

### info@levelblue.com

1CISA Alert: Royal Ransomware Actors Rebrand as “BlackSuit,” FBI and CISA Release Update to Advisory. Retrieved Dec. 5, 2024.   
2CISA Alert: CISA and Partners Release Advisory on Black Basta Ransomware. Retrieved Dec. 5, 2024.

This holiday season our SOC analysts have observed a sharp uptick in cyber threat activity. Specifically, they’ve seen a rise in attempted ransomware attacks, which started during the American Thanksgiving holiday period (November 25–31, 2024) and are expected to continue throughout the holiday season. We’re sharing details on the threat actors involved, their tactics, as well as recommendations to give you knowledge and tools to proactively strengthen your security against evolving threats.

## Key Threat Groups

### BlackSuit (formerly “Royal”)

Known for targeting critical infrastructure sectors, including healthcare, government, and manufacturing, BlackSuit employs data exfiltration, extortion, and encryption techniques, according to a Cybersecurity and Infrastructure Security Agency (CISA) advisory.

Common attack vectors include:

- Phishing emails and malicious websites
- Exploitation of unsecured virtual private networks (VPNs) lacking multi-factor authentication (MFA)
- Disabling antivirus software to exfiltrate data before encrypting systems

### Black Basta

Operating as a ransomware-as-a-service (RaaS), Black Basta affiliates have targeted over 500 entities in 2024 alone in North America, Europe, and Australia, according to CISA. Key tactics:

- Vishing: Impersonating help desk technicians via phone to access networks
- Using malicious remote management tools to gain entry and escalate attacks

### LevelBlue Observations of Threat Actor TTPs and How to Fortify Security

In recent weeks, our SOC team has observed threat actors using the following tactics to launch attacks:

| Tactic | Recommendations |
| --- | --- |
| Exploitation of a **VPN portal that is not enforcing MFA** to gain initial access |   - Enforce MFA for VPN connections and geo-fence your VPN portal(s)         - Patch VPN devices. Historically we have observed these external-facing network appliances be compromised   |
|   The use of **vishing** (impersonating a “help desk” team member) to gain initial access to end-user workstations, which then gives the attacker access to the larger network (emails and text messages are also being leveraged for credential collection and malware deployment)  **Two numbers LevelBlue has identified to be involved in incidents are 1-844-201-3441 and 304-718-2459**       |   - Provide employees with training and education on vishing attacks and the common lures that may be used         - Implement a process of verification for both help desk employees and employees being called during legitimate IT support scenarios         - Direct employees to report suspicious communications immediately to a supervisor and security leadership           |
| The use of **Rclone, WinSCP, and other file transfer tools to exfiltrate data** from environments |   - Block the installation or execution of common attacker tools that do not have a designated function within your network, or strictly enforce the exceptions for allowing the usage   |
|   **Exploitation of vulnerabilities** across common software/applications to escalate privileges  Vulnerabilities for VMware, Microsoft Exchange, Microsoft SharePoint, and other self-hosted applications are being particularly targeted to gain administrator or even root access within environments   |   - Patch software per vendor recommendations and review your organization’s vulnerability scanning and patching schedule         - Maintain good records of applications and operating systems running within your environment, and enable notifications for when patch notifications, emails, or news updates come out about these applications and operating systems           |
| The use of **Remote Desktop Protocol (RDP), Window Remote Management (WinRM), and Remote Monitoring Management (RMM) tools** for lateral movement |   - Block any external to internal RDP attempts and disable RDP on hosts that do not need it         - Limit RDP and WinRM traffic from segments of the network that do not require that type of west/east traversal. This can also apply to other protocols and overall network traffic as well, stop an attacker’s lateral movement         - Block the installation or execution of RMM tools that are not explicitly used by your organization. **Note that RMM tools have been observed in almost every ransomware-related incident the LevelBlue SOC team has investigated.** Blocking the installation or execution of these tools will significantly decrease the effectiveness of an attack   |

##   
Other Proactive Cybersecurity Measures

### Enhance Employee Awareness

While employees might be enjoying more festivities this time of year, it’s important to communicate the urgency of heightened vigilance during the holiday season. Educate employees on recognizing and reporting suspicious communications. And provide clear guidance on verifying IT support contacts.

### Validate Security Controls and Address Potential Exposures

Stay on top of patching and ensure public-facing assets are secured through MFA. We’re here to help identify potential security gaps and exposures. Take advantage of a 30-day free trial with LevelBlue’s Vulnerability Management service.

### Protect Against Malicious Sites and Emails

If you do not already have email security, secure remote access, or secure web gateway protections in place, consider adding them. LevelBlue provides flexible, managed service delivery options with a choice of leading technologies. These services can help protect employees from phishing attempts and malicious sites as well as help control and manage access to applications.

### Fortify Endpoint Security

More than 75% of organizations say they have experienced at least one cyberattack due to unknown, unmanaged, or poorly managed devices.2 LevelBlue Managed Endpoint Security with SentinelOne protects diverse endpoints, including laptops, servers, desktops, and cloud workloads, from evolving threats. Pair this service with LevelBlue Managed Threat Detection and Response to cover your entire attack surface. We also offer several tiers for an incident response retainer, giving customers access to additional response, forensics, and recovery support. 

Finally, it may be tempting to let tasks linger this time of year, but as we all know, cybercriminals will use that to their advantage. Address security concerns immediately, so they do not compound and grow more severe. The holidays are a busy time for everyone, including threat actors. Use our support services during this season and beyond to fortify your cyber operations and ensure your organization remains safe.

## Contact LevelBlue

### info@levelblue.com

1CISA Alert: Royal Ransomware Actors Rebrand as “BlackSuit,” FBI and CISA Release Update to Advisory. Retrieved Dec. 5, 2024.   
2CISA Alert: CISA and Partners Release Advisory on Black Basta Ransomware. Retrieved Dec. 5, 2024.

Go to Source
