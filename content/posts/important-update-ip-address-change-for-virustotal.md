---
title: "Important Update: IP Address Change for VirusTotal"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

We're making a change to the IP address for www.virustotal.com. If you're currently whitelisting our IP address in your firewall or proxy, you'll need to update your rules to maintain access to VirusTotal.

Starting November 25th, we'll be gradually transitioning the resolution of www.virustotal.com to a new IP address: **34.54.88.138**. If you have hardcoded the previous IP address (74.125.34.46) in your firewall or proxy, you'll need to update your configuration to include the new IP address. This will ensure continued access to VirusTotal.

**TLS Certificate provider change:**

We're also updating our TLS certificate provider, moving from a DigiCert wildcard certificate to Google Trust Services single-host certificate. While this change should be seamless for most users, you'll need to update your configuration if you validate the certificate's signer or subject.

**Note for Big Files API Users:**

If you use the Big Files endpoint (https://docs.virustotal.com/reference/files-upload-url) for submitting files larger than 32MB, remember that it provides a URL pointing to the bigfiles.virustotal.com domain.

This domain is managed by a ghs.googlehosted.com load balancer, which uses dynamic IP address resolution. Please ensure your firewall rules can accommodate this.

We'll be implementing this change gradually starting on November 25th to minimize any potential disruption.

We understand that this change may require adjustments to your systems, and we appreciate your prompt attention to this matter. If you have any questions or concerns, please don't hesitate to contact us.

We're making a change to the IP address for www.virustotal.com. If you're currently whitelisting our IP address in your firewall or proxy, you'll need to update your rules to maintain access to VirusTotal.

Starting November 25th, we'll be gradually transitioning the resolution of www.virustotal.com to a new IP address: **34.54.88.138**. If you have hardcoded the previous IP address (74.125.34.46) in your firewall or proxy, you'll need to update your configuration to include the new IP address. This will ensure continued access to VirusTotal.

**TLS Certificate provider change:**

We're also updating our TLS certificate provider, moving from a DigiCert wildcard certificate to Google Trust Services single-host certificate. While this change should be seamless for most users, you'll need to update your configuration if you validate the certificate's signer or subject.

**Note for Big Files API Users:**

If you use the Big Files endpoint (https://docs.virustotal.com/reference/files-upload-url) for submitting files larger than 32MB, remember that it provides a URL pointing to the bigfiles.virustotal.com domain.

This domain is managed by a ghs.googlehosted.com load balancer, which uses dynamic IP address resolution. Please ensure your firewall rules can accommodate this.

We'll be implementing this change gradually starting on November 25th to minimize any potential disruption.

We understand that this change may require adjustments to your systems, and we appreciate your prompt attention to this matter. If you have any questions or concerns, please don't hesitate to contact us.

Go to Source
