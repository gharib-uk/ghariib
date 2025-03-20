---
title: "Ubuntu available in Microsoft’s new WSL distribution format"
date: 2025-02-06
---

We are happy to announce that Ubuntu on Windows Subsystem for Linux (WSL) is now available in Microsoft’s new tar-based distribution architecture. 

Ubuntu has been a widely used Linux distribution on WSL, offering a familiar development environment for many users. This new distribution architecture for WSL will make adoption easier in enterprise environments by enabling image customization and deployments at scale. The new tar-based WSL distro format allows developers and system administrators to distribute, install, and manage Ubuntu WSL instances from tar files without relying on the Microsoft Store.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1919,h_1079/https://ubuntu.com/wp-content/uploads/2e4b/Ubuntu-on-WSL-New-image-format.png)

## **What’s new**

With this new format, Ubuntu on WSL gains several key benefits:

- **Easier Deployment**: Ubuntu can be installed directly from a tar file, eliminating the need for Windows-specific packaging or the use of the Microsoft Store.
- **Enterprise-Ready:** Organizations can now self-host — on a network share, for instance — and centrally control which WSL images are available, ensuring compliance with security and IT policies.
- **Customization:** Developers and administrators can fully tailor Ubuntu installations by modifying the image itself. Additionally, native cloud-init support on Ubuntu allows for more advanced configuration and automation during initial setup.

## **Getting Started**

The new WSL distribution format is available now. To install Ubuntu using this format, grab the latest version of WSL (2.4.8 or higher). Then you can install Ubuntu from the web with the following command:

`wsl --install ubuntu`

You can also download the image and run the command:

`wsl --install --from-file ubuntu.tar.wsl`

Alternatively, you can double-click on the `.wsl` file to begin the installation.

For more details on configuring and deploying Ubuntu with this new format, refer to our documentation:

- Windows Subsystem for Linux (WSL) | Ubuntu 
- Ubuntu on WSL documentation 
- Build a Custom Linux Distribution for WSL – Windows
- Automatic setup with cloud-init – Ubuntu on WSL documentation 

#### Looking for enterprise support?

Contact our team

Go to Source
