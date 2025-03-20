---
title: "Ubuntu 20.04 LTS on Azure: how to stay secure after standard support ends"
date: 2025-03-19
---

As Ubuntu 20.04 LTS (Focal Fossa) approaches its planned May 31, 2025 end of standard support, Azure users must proactively evaluate their options to maintain security and regulatory compliance.

## What this means for you

After May 31, 2025, Ubuntu 20.04 LTS instances on Azure will no longer receive critical security updates through standard channels, potentially affecting compliance with standards like PCI-DSS, SOC 2, or GDPR.

## Your options

### Option 1: Upgrade to a newer Ubuntu LTS Version

Ubuntu 22.04 LTS (Jammy Jellyfish) and 24.04 LTS (Noble Numbat) are currently maintained under standard support by Canonical until 2027 and 2029, respectively. You can upgrade via:

- **Deploying a Fresh Instance**: Launch a new instance from the Azure Marketplace or CLI, then redeploy and test your application. For automation, update your base image.  
    For Ubuntu 24.04 LTS: `az vm create --name vm-name --resource-group myRG --image Canonical:0001-com-ubuntu-server-noble:24_04-lts:latest`
- **Upgrading the OS Version**: Use the \``do-release-upgrade`\` command to update your existing installation. Be aware that there is no rollback option, and packages from external repositories might require manual reinstallation. Given cloud instance flexibility and ease of automation, we strongly recommend deploying a fresh instance for migrations rather than performing in-place upgrades. Learn more in the official documentation.

**Note:** Before proceeding with either upgrade method, ensure compatibility of your applications and dependencies with the target Ubuntu version, as some packages or configurations may have changed or been deprecated.

### Option 2: Extend support with Ubuntu Pro

If you rely on specific application compatibility, hardware stability, or have enterprise constraints that make upgrading difficult, extending your support to Ubuntu Pro may be preferable. 

Ubuntu Pro extends security maintenance for 5 years beyond standard support (until 2030), offering:

- ESM-infra: Security fixes for high and critical CVEs.
- ESM-apps: Patches for over 25,000 open-source applications.
- Kernel Livepatch: Security updates without reboots.
- Ubuntu Security Guide: Hardening for industry standards such as FedRAMP, CIS, STIG, and more
- FIPS Support: Compliance with federal security standards for cryptographic modules.
- Optional phone and ticket support (Contact us)
- Optional Legacy Support: Providing extended security and maintenance through 2032

To learn more about Ubuntu Pro’s offerings, see https://ubuntu.com/azure/pro 

As Andy Parker, Engineering Manager at TIM, explains: 

> _“ESM has given us the space to plan what comes next. It’s helping us get into a position where we can have a more sustainable infrastructure.”_

**How to enable Ubuntu Pro on Azure:**

- **Deploy a Fresh Ubuntu Pro Instance:** Launch a new Ubuntu Pro 20.04 instance and redeploy your application. For automation, switch to the Pro image.  
    `az vm create --name pro-vm --resource-group myRG --image Canonical:0001-com-ubuntu-pro-focal:pro-20_04-lts:latest`
- **Enable Ubuntu Pro on an Existing Instance:** Attach a subscription to your current Ubuntu 20.04 LTS instance:

1. Update the VM license: `az vm update --resource-group myRG --name myVM --license-type UBUNTU_PRO`
2. Install and attach the subscription: `sudo pro auto-attach`
3. Verify activation: `pro status # Should display subscription details` 

## Take action now

Ensure your Azure deployments remain secure by evaluating your migration or upgrade strategy well before May 31, 2025. Whether upgrading to a newer LTS or enabling Ubuntu Pro, early planning ensures a smooth transition. For assistance, contact Ubuntu experts from Canonical at https://ubuntu.com/azure/contact-us.

Go to Source
