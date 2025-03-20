---
title: "Bare Metal Provisioning with PXE and iPXE A Step-by-Step Guide"
date: 2025-03-10T14:48:00.143Z
draft: false
type: posts
categories: 
- gpu
---
# Bare Metal Provisioning with PXE and iPXE A Step-by-Step Guide

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Physical server provisioning and maintenance are fundamental elements in modern IT infrastructure. Organizations running large-scale web services or specialized AI/ML workloads that require high performance benefit from server deployment automation to enhance operational efficiency. Network booting through PXE and enhanced iPXE boot are popular approaches in these scenarios.

This guide explains PXE and iPXE technologies, including their operational mechanism and essential role in bare metal provisioning. This article will further explain the differences between PXE and iPXE while exploring configuration and scripting aspects. We will address security concerns and provide step-by-step setup instructions for automated OS deployments. After reading this article, you will understand how to implement PXE or iPXE booting within your infrastructure.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Understanding IP addressing, DHCP, and TFTP protocols is essential to understanding PXE boot and iPXE boot operations in bare metal provisioning.
-   A foundational knowledge of [BIOS](https://www.techtarget.com/whatis/definition/BIOS-basic-input-output-system#:~:text=BIOS%20\(basic%20input%2Foutput%20system\)%20is%20the%20program%20a,%2C%20keyboard%2C%20mouse%20and%20printer.), [UEFI](https://www.techtarget.com/whatis/definition/Unified-Extensible-Firmware-Interface-UEFI#:~:text=Unified%20Extensible%20Firmware%20Interface%20\(UEFI\)%20is%20a%20specification%20for%20a,its%20operating%20system%20\(OS\).) boot mechanisms, and network boot protocols for remote operating system deployments should be familiar to readers.
-   Understanding PXE and iPXE differences and PXE server-client interactions will provide essential insights for mastering the configuration process.
-   Knowledge of [bare metal dedicated servers](/resources/articles/bare-metal-gpus), automated OS deployment, and their role in large-scale infrastructure management will be beneficial.
-   Proficiency in iPXE commands, the Linux shell, and networking tools like [Wireshark](https://www.wireshark.org/) will help resolve PXE/iPXE configuration issues.

[What is Network Booting](#what-is-network-booting)[](#what-is-network-booting)
-------------------------------------------------------------------------------

Network booting enables a computer to load its operating system and essential components from a network instead of local storage devices. Companies operating extensive server fleets, especially bare metal dedicated servers, can use network booting to deploy or reconfigure operating systems.

[What is PXE](#what-is-pxe)[](#what-is-pxe)
-------------------------------------------

**PXE stands for Preboot eXecution Environment**. This network boot protocol operates in a client-server model, enabling clients to fetch their boot loaders and additional files over the network. Intel introduced [PXE](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/os-deployment/understand-pxe-boot) as an industry-standard technology in 1998\*\*.\*\* It allows a computer to start its boot process through the network card before accessing local storage devices where operating systems are installed. This capability benefits data centers and bare metal server environments by allowing server provisioning without manual CD or USB drive deployment.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Shaoni/Adrien/10-mar/Screenshot%202025-03-10%20at%205.39.01%E2%80%AFPM.png)

[What is iPXE](#what-is-ipxe)[](#what-is-ipxe)
----------------------------------------------

iPXE is an open-source network boot firmware solution that delivers substantial improvements beyond standard PXE capabilities. It evolved from gPXE, which originated as a fork of Etherboot. iPXE offers advanced features that are lacking in traditional PXE implementations. The [iPXE GitHub](https://github.com/ipxe/ipxe) repository contains its source code and documentation. Its high configurability allows developers to compile it into multiple formats, including EFI(Extensible Firmware Interface) applications and PXE images. They can also embed it within network interface card (NIC) ROMs.

**Note**: **NIC ROM(Network Interface Card Read-Only Memory)** consists of a small memory chip on the network interface card, which holds firmware that drives the card’s operation. It is an important component during the system boot process and proves especially important for network boot operations such as PXE.

[Core Components of PXE/iPXE Boot](#core-components-of-pxe-ipxe-boot)[](#core-components-of-pxe-ipxe-boot)
----------------------------------------------------------------------------------------------------------

Both PXE and iPXE share some fundamental components. Let’s consider some of them:

-   **Dynamic Host Configuration Protocol(DHCP)**: The [DHCP Server](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top) assigns IP addresses and indicates where to find the boot files.
-   **Boot Loader**: The initial piece of software that the client loads when booting. It can include _pxelinux.0_ for PXE systems and either _ipxe.lkrn_ or _undionly.kpxe_ for iPXE systems.
-   **Boot Configuration Files**: These configuration files specify the kernel and _initramfs_ (initial RAM disk) to load along with necessary boot options and environmental variables.
-   **OS Images / Installation Media**: These are essential files required for operating system installation on bare metal hardware.
-   **PXE Server**: The PXE Server provides the boot configuration file to the client.
-   **Trivial File Transfer Protocol (TFTP)**: [TFTP](https://www.ibm.com/docs/el/i/7.4?topic=services-trivial-file-transfer-protocol) represents a minimal file transfer protocol designed for booting purposes.
-   **Network Boot Program (NBP)**: The client-side software initiates booting.

With these components established, you can manage complex provisioning processes that require minimal human interaction. Hence, it allows you to maintain efficient, reliable, bare metal dedicated server configurations.

[How PXE Works](#how-pxe-works)[](#how-pxe-works)
-------------------------------------------------

PXE booting requires synchronized collaboration between client firmware, a DHCP server, a TFTP server, and other servers for the actual OS files. Here’s the typical PXE boot workflow step-by-step:

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Shaoni/Adrien/10-mar/two.png)

**Client PXE Request**  
When the network boot-configured machine powers on, its [NIC firmware](https://www.dell.com/support/home/en-in/drivers/driversdetails?driverid=gxj5g) (PXE ROM) takes over. The NIC transmits a DHCP DISCOVER broadcast message that shows it supports PXE boot capability( this is done by embedding PXE-specific DHCP options to indicate a PXE request). The client requests network configuration services and intends to initiate PXE boot.

**DHCP Offer + PXE Info**  
Upon receiving a request, the network DHCP server (or proxy DHCP service) replies with a DHCP OFFER message containing network details and an allocated IP address. It also includes PXE boot information: The network details include the PXE boot server name (which usually matches the TFTP server address) and bootfile identification (commonly known as NBP or Network Bootstrap Program). For example, the DHCP reply says: “IP address 11.1.1.5 is assigned with boot file _pxelinux.0_ from server 11.1.1.1.”

**Client Downloads NBP via TFTP** Once the client determines the correct boot file location, it retrieves the file using the [Trivial File Transfer Protocol (TFTP)](https://www.sciencedirect.com/topics/computer-science/trivial-file-transfer-protocol#:~:text=Trivial%20File%20Transfer%20Protocol%20\(TFTP,diskless%20workstations%20to%20bootstrap%20themselves.) from the boot server. The NBP file may be a lightweight bootloader such as PXELINUX(from Syslinux), an iPXE image, or act as a Windows WDS(Windows Deployment Services), etc.

**Execute NBP** The PXE firmware hands off control to the NBP after the client downloads it into memory. The NBP progresses by loading the actual operating system or installer. If [PXELINUX](https://ltsp.org/guides/pxelinux/) serves as the NBP, it will retrieve a PXELINUX configuration file through TFTP to display a boot menu or directly start loading the Linux kernel using an _initrd_ image.

**OS Load** The specifics of this ultimate stage depend entirely on the nature of the [Network Boot Program](https://networkboot.org/fundamentals/) (NBP). The primary goal is to load the target operating system (OS) kernel into memory for execution, along with an initial RAM disk (_initrd_) if necessary. The PXE boot process typically requires the NBP, or a second-stage bootloader, to download the OS kernel and _initrd_ files via TFTP. Once the operating system takes over, the network boot process is considered complete.

[How iPXE Improves on PXE](#how-ipxe-improves-on-pxe)[](#how-ipxe-improves-on-pxe)
----------------------------------------------------------------------------------

iPXE enhances the basic PXE concept, providing a stronger and more user-friendly solution. There are two primary ways to use iPXE.

![image](https://doimages.nyc3.cdn.digitaloceanspaces.com/010AI-ML/2025/Shaoni/Adrien/10-mar/three.png)

The native iPXE configuration involves replacing the firmware or ROM of your network interface card (NIC) with iPXE. In contrast, [chain-loading](https://docs.nvidia.com/networking/display/flexbootv37300uefiv143310/flexboot+ipxe-pxe+chainloading) iPXE utilizes the original PXE firmware to load iPXE as a secondary stage. Once iPXE begins operating on the client device, it offers several capabilities:

-   **Expanded Protocol Support**: Besides TFTP, iPXE supports protocols like HTTP, FTP, [iSCSI](https://www.techtarget.com/searchstorage/definition/iSCSI), [FCoE](https://en.wikipedia.org/wiki/Fibre_Channel_over_Ethernet), etc.
-   **Advanced Scripting**: PXE’s scripting engine allows users to write boot scripts for complex tasks like conditional booting, menu systems, and dynamic configuration file retrieval.
-   **Embedded Boot Scripts**: Embedding a script within the iPXE binary allows you to implement fully automated boot procedures without relying on additional configuration files.
-   **Security and Authentication**: The combination of HTTPS support, 802.1x authentication, and cryptographic check allows for secure boot operations in high-security environments.
-   **Direct Boot from Cloud Storage:** iPXE enables kernel loading or OS image from remote resources through various communication protocols.
-   **Enhanced Debugging and Commands:** Built-in commands (like _chain_, _imgfetch_, _autoboot_, etc.) enable advanced control over the booting process.

In short, iPXE is a modern, more powerful replacement for PXE when you need advanced network boot options.

Example: To boot a custom Linux environment, you can utilize iPXE.

It allows the creation of a script as shown below:

```
#!ipxe
dhcp # get network config
kernel http://192.168.1.10/boot/vmlinuz initrd=initrd.img ro console=ttyS0
initrd http://192.168.1.10/boot/initrd.img
boot
```

The script instructs iPXE to get a DHCP address and download kernel and _initramfs_ files through HTTP from a server before initiating the boot process. The process works like PXE boot but uses HTTP instead with enhanced control capabilities and allows easy inclusion of kernel command line arguments such as \*console=ttyS0 (\*The console output directs to the serial port._ttyS0_)

[PXE vs. iPXE – What’s the Difference?](#pxe-vs-ipxe-what-s-the-difference)[](#pxe-vs-ipxe-what-s-the-difference)
-----------------------------------------------------------------------------------------------------------------

While PXE and iPXE serve similar goals (network-based booting), there are significant differences in features and capabilities. Below is a comparison:

Feature

PXE (Traditional)

iPXE

**Source**

Firmware (usually closed-source vendor NIC ROM)

Open-source (can replace or chain from PXE)

**Protocols for Boot**

DHCP + TFTP only

DHCP + TFTP, HTTP, HTTPS, iSCSI, FTP, NFS, etc.

**Speed of File Transfer**

Uses TFTP (UDP-based, can be slow for large files)

Can use HTTP/HTTPS (TCP-based, faster transfers)

**Boot Media Support**

Requires PXE-capable NIC firmware

It can boot via NIC (PXE or native), USB/CD (as .iso), etc.

**Scripting & Logic**

None (Since the boot process is a fixed procedure)

Yes, it supports scripting, conditional logic, menus, etc.

**Extended Features**

Limited to basic network boot

The system supports Wi-Fi, VLAN, IPv6, and authentication features.

**UEFI Compatibility**

UEFI PXE supported (with EFI-spec network boot)

UEFI supported (via ipxe.efi), including HTTP Boot integration

**Maintainers**

Vendor-specific (Intel, etc.), spec is open

Community-driven open-source project

PXE works well for straightforward PXE boot processes involving small images. iPXE stands out for large-scale deployment or cloud integration needs and users dependent on advanced scripting features.  
Using iPXE scripting\*\*,\*\* you can automate boot decisions by integrating scripts to detect hardware types\*\*,\*\* select the correct OS installer\*\*,\*\* or display boot menus.

[Interaction with Modern Hardware and UEFI](#interaction-with-modern-hardware-and-uefi)[](#interaction-with-modern-hardware-and-uefi)
-------------------------------------------------------------------------------------------------------------------------------------

The [**transition**](https://www.techtarget.com/whatis/definition/Unified-Extensible-Firmware-Interface-UEFI) from traditional BIOS to UEFI enhances security and modular design. While both PXE (Preboot Execution Environment) and iPXE are compatible with UEFI, there are a few key differences to consider:

-   **UEFI PXE Boot:** Unlike legacy BIOS\*\*,\*\* which uses _.pxe_ or _.kpxe_ files for network booting\*\*,\*\* UEFI requires a .efi application.
-   **Chainloading:** Some configurations enable PXE boot through UEFI and then proceed to chainload iPXE for extended functionality.
-   **Native iPXE EFI Binaries:** Users can boot iPXE directly in UEFI environments because it provides UEFI-compatible binaries such as _ipxe.efi,_ eliminating the need for chain loading.

It is essential to check that hardware systems support UEFI PXE or iPXE boot. In GPU-accelerated bare metal server environments, running specialized AI/ML workloads and optimized booting solutions enhances provisioning efficiency. Organizations that allocate investments to bare metal hardware for GPU-intensive tasks must prioritize UEFI support to access all modern server motherboard features.

DigitalOcean provides a quick start guide for organizations that want to provision bare metal GPUs. [Explore what Bare Metal GPUs](/resources/articles/bare-metal-gpus) are to discover products featuring bare metal GPUs, learn about [bare metal GPU offerings,](/products/bare-metal-gpu) and gain further [GPU-related insights](/blog/digitalocean-bare-metal-gpus).

[Setting Up PXE and iPXE for Bare Metal Servers](#setting-up-pxe-and-ipxe-for-bare-metal-servers)[](#setting-up-pxe-and-ipxe-for-bare-metal-servers)
----------------------------------------------------------------------------------------------------------------------------------------------------

Now that we understand how PXE and iPXE work, it’s time to configure the PXE/iPXE environment for bare metal provisioning. We will configure a PXE server followed by iPXE integration for advanced network booting.

[Prerequisites to set up PXE/iPXE environment](#prerequisites-to-set-up-pxe-ipxe-environment)[](#prerequisites-to-set-up-pxe-ipxe-environment)
----------------------------------------------------------------------------------------------------------------------------------------------

-   To set up a PXE/iPXE environment, start with a solid network infrastructure. Keep all your servers on the same LAN, or set up a DHCP relay if you’re booting across different subnets. Using VLANs can help with security.
-   Check that bare metal servers can handle PXE in the BIOS/UEFI settings and that network booting is enabled.
-   Configure a DHCP server to assign IP addresses and point to the boot file location, while a TFTP server will transfer those boot files.
-   For iPXE, an HTTP server will efficiently serve scripts and OS images.
-   Gather the necessary boot files, such as _pxelinux.0_, _bootx64.ef_i, and _ipxe.efi_, along with your OS installation images.
-   To keep things running smoothly, use a dedicated PXE server machine with a static IP to host the DHCP, TFTP, and HTTP services for network booting.

**Best practice:** Document your network environment (like IP ranges and server IPs). Using a test machine to experiment with the PXE setup before deploying it to the production servers is also a good idea.

### [Setting Up a PXE Server](#setting-up-a-pxe-server)[](#setting-up-a-pxe-server)

Setting up a PXE server involves two key players:

-   DHCP responds to boot requests with the necessary network details and specifies the boot file.
-   TFTP is responsible for sending that boot file to the client.

Here’s a straightforward guide on how to get these pieces in place:

#### Install and Configure DHCP with PXE Options

**Install a DHCP Server** If you don’t have a DHCP service configured for PXE, it’s time to install one. For instance, if you’re using a Debian or Ubuntu Linux server, you can go ahead and install the ISC DHCP server:

```
sudo apt update && sudo apt install isc-dhcp-server
```

On Red Hat-based systems, _dhcp-server_ can be installed via _dnf_ or y_um_. Ensure the DHCP service is enabled and will start on boot.

**Basic DHCP Configuration:** Start by editing the configuration file—e.g., _/etc/DHCP/dhcpd.conf_ for ISC DHCP. You must define the network’s subnet, the IP range, the gateway, DNS settings, and anything specific to your configuration. To support booting via Bootstrap Protocol (BOOTP) or PXE, check if the DHCP server requires lines like _allow booting_; _allow bootp_; to be added. For example:

```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.100;
    option routers 192.168.1.1;
    option domain-name-servers 192.168.1.1;
    ...
}
```

**Configure PXE-specific Options**  
In the DHCP configuration for your subnet, include the PXE boot file and the TFTP server.

-   **Option 66** (Next-Server): The TFTP server’s IP address is specified here. It is often the same as the PXE server. With ISC DHCP, you can do this using _next-server_ 192.168.1.10 (Just replace it with your TFTP server’s IP).
-   **Option 67** (Boot File Name): Here, you’ll provide the file name the client will download from the TFTP server. For PXELINUX clients using BIOS, you might use _pxelinux.0_. For UEFI clients, it could be an EFI loader (such as _bootx64.efi_ for x86\_64 UEFI). In your ISC DHCP config. Use the filename _pxelinux.0_; it is defined within the subnet or host group.

If you have clients using BIOS and UEFI, the best thing to do is to differentiate them based on the DHCP architecture type. For instance, you might implement conditional statements for this purpose.

```
if option arch = 00:07 {
    filename "bootx64.efi"; # UEFI x86-64
} else {
    filename "pxelinux.0"; # BIOS
}
```

This ensures that each client gets the correct bootloader. (For architecture codes, _00:07_ refers to x64 UEFI, _00:06_ is for x86 UEFI, and _00:00_ or _00:09_ indicates BIOS, etc.)

**Start the DHCP Service** After saving the configuration, go ahead and start the DHCP service. For example, you can use _sudo systemctl to start isc-dhcp-server_. Then, check the system logs (like _/var/log/syslog_ or _/var/log/messages_) to confirm it started smoothly without errors.

### [Install and Configure the TFTP Server](#install-and-configure-the-tftp-server)[](#install-and-configure-the-tftp-server)

**Install TFTP Service** You’ll want to install a TFTP service on the PXE server (or whatever you’ve set up as a TFTP server). If using [Ubuntu](/community/tutorials/initial-server-setup-with-ubuntu) or [Debian](/community/tutorials/initial-server-setup-with-debian-11), just run this command:

```
sudo apt install tftpd-hpa
```

If you’re on RHEL or CentOS, try this instead:

```
sudo dnf install tftp-server
```

Ensure the TFTP service (or _xinetd_ if using xinetd) is enabled. Modify your firewall settings to allow TFTP traffic through (UDP port 69, just in case!).

**TFTP Root Directory** **Now, you must identify the TFTP root directory.**

Depending on the system configuration, you’ll usually find the TFTP root directory at _/var/lib/tftpboot_ or _/tftpboot_. You can easily create it if it’s missing by running the command:

```
sudo mkdir -p /var/lib/tftpboot.
```

Just remember to set the correct permissions so everyone can read it. TFTP clients need to access the files without specific user accounts. You can handle the permissions using the command:

```
sudo chmod -R 755 /var/lib/tftpboot.
```

This way, the TFTP server, which often operates as the user _nobody_, will have access to the directory.

**Obtain PXE Boot Files** Next, copy the bootloader files into the TFTP root. If you’re working with PXELINUX from the Syslinux project, you’ll need a few essentials:

-   _pxelinux.0_ – this is the primary PXE bootloader for BIOS clients.
-   Other important files include _ldlinux.c32_, _libcom32.c32_, _libutil.c32_, and several menu modules such as _menu.c32_ or _vesamenu.c32_ if you prefer a menu interface. These files typically come with the Syslinux package. When you install the syslinux or syslinux-common package on most Linux distributions, these files are usually located in the _/usr/lib/PXELINUX/_ directory. Simply copy them to the _tftpboot_ directory. If you’re using Red Hat, you can extract them directly from the Syslinux RPM.
-   If you need a UEFI bootloader, get _syslinux.efi_ or an iPXE EFI binary, especially if you plan to use iPXE for UEFI from the beginning.
-   Optionally, other tools like _undionly.kpxe_ (a BIOS binary for chainloading) or [wimboot](https://www.ventoy.net/en/doc_wimboot.html) for Windows PE booting. This will depend on the use case.

**Set Up PXELINUX Configuration:**

PXELINUX looks for a configuration in the _pxelinux.cfg_ directory under the TFTP root. let’s create that directory:

```
mkdir -p /var/lib/tftpboot/pxelinux.cfg
```

Next, create a default config file, like this: _/var/lib/tftpboot/pxelinux.cfg/default_. In this file, you’ll lay out the PXE boot menu or any necessary settings. For example:

```
DEFAULT menu.c32
PROMPT 0
TIMEOUT 600
MENU TITLE PXE Boot Menu

LABEL linux
  MENU LABEL Install Linux OS
  KERNEL images/linux/vmlinuz
  APPEND initrd=images/linux/initrd.img ip=dhcp inst.repo=http://192.168.1.10/os_repo/

LABEL memtest
  MENU LABEL Run Memtest86+
  KERNEL images/memtest86+-5.31.bin

LABEL local
  MENU LABEL Boot from local disk
  LOCALBOOT 0
```

The PXE boot menu setup to boot from the network with multiple options using Syslinux (menu.c32). You’ll have a 600-second countdown before it automatically selects the default choice. The menu features three options:

-   Installing Linux, which will load the kernel (_vmlinuz_) and the initial RAM disk (_initrd.img_) while downloading installation files from an HTTP repository (_inst.repo_).
-   Memtest86+ for checking memory using memtest86±5.31.bin.
-   Booting from the local disk (_LOCALBOOT 0_) exits PXE and launches the operating system already installed on your machine.

Make sure to adjust the paths according to your environment (for instance, you might store the kernel and _initr_d in the images folder under _tftpboot_ while keeping an HTTP repo for the OS files). The KERNEL line points to the network boot kernel. Additionally, the APPEND line contains the _initrd_ and any necessary kernel parameters, such as the location for the installer or the Kickstart file.

**Note:** The paths for KERNEL and INITRD (using APPEND) are based on TFTP root unless you’ve specified a different protocol like http://. Just ensure those files are in the TFTP directory or accessible as indicated. For example, _images/linux/vmlinuz_, actually point to _/var/lib/tftpboot/images/linux/vmlinuz_ on the server.

**NFS or HTTP Setup for OS Files (if required)** If your OS installer requires a repository—like a complete [distro](https://distrowatch.com/dwres.php?resource=family-tree) tree or an ISO, you should serve it through NFS or HTTP. For example, you can mount an ISO and share it over HTTP, allowing the installer to access and retrieve the necessary packages.

**Start/Restart TFTP Service** Once you’ve uploaded files, restart the TFTP service to ensure it recognizes the new files. For instance, you can run:

```
sudo systemctl restart tftpd-hpa
```

Typically, TFTP does not require a restart for new files since it retrieves them directly from the disk with each request. However, it is wise to confirm that the service is operational.

At this point, you have a basic PXE server configuration. When a client starts using PXE, the DHCP server will hand out an IP address and specify the boot file (like _pxelinux.0_). The client will fetch this file from the TFTP server and load the PXELINUX bootloader. From there, it will read the configuration, show the menu options, and proceed with the OS installation.

### [Installing and Configuring iPXE](#installing-and-configuring-ipxe)[](#installing-and-configuring-ipxe)

We’ll examine chain loading iPXE via PXE, which works well with the setup we discussed earlier.

#### Download or Compile iPXE

You can get iPXE binaries through these methods\*\*:\*\*

-   **Precompiled Binaries:** For quick experimentation, the iPXE project offers a pre-built ISO image that can be easily accessed. Additionally, binaries like undionly.kpxe and ipxe.efi can be built from the source code.
-   **Compile from Source:** To get the latest features or adhere to the customization process(like embedding scripts or enabling HTTPS), compile iPXE from the source. First, you’ll need to install some build tools (like _make_, _gcc_, Perl, etc.).

For example, on a Debian/Ubuntu system:

```
sudo apt install -y git make gcc binutils perl liblzma-dev mtools
git clone https://github.com/ipxe/ipxe.git
cd ipxe/src
# Build BIOS PXE binary:
make bin/undionly.kpxe
# Build UEFI PXE binary (x86_64):
make bin-x86_64-efi/ipxe.efi
```

After a successful build, you’ll find the binaries in the _bin/_ or _bin-x86\_64-efi/ directories_. If you want to include a startup script in the binary, add _EMBED=script.ipxe_ to your _make_ command.

#### Configure DHCP for Chainloading iPXE

Now, to use iPXE during the boot process, you’ll configure the DHCP settings as explained in the section above:

-   **BIOS** clients will first download _undionly.kpxe_ (that’s the iPXE firmware for BIOS) through TFTP instead of jumping straight to PXELINUX. For **UEFI** clients, they can directly load _ipxe.efi_.
-   Update the DHCP filename and related options:
    -   For BIOS, set the filename to “_undionly.kpxe_” (ensure the _next server_ still directs to the TFTP server hosting this file).
    -   For UEFI, set the filename to “_ipxe.efi_”; for x86\_64 UEFI (you can change the architecture according to your need).

**Avoiding Loops** One of the challenges is to avoid getting stuck in a loop where iPXE keeps loading itself repeatedly. When iPXE initiates, it sends its own DHCP request. Suppose the DHCP server responds with _undionly.kpxe_ or _ipxe.efi_, you’ll be trapped in a never-ending cycle.

Configure the DHCP server to recognize an iPXE client to break this loop.

By default, iPXE uses the user-class identifier “iPXE.” If you’re using ISC DHCP, use this to differentiate the second-stage requests. For example:

```
if exists user-class and option user-class = "iPXE" {
    filename "http://192.168.1.10/";
} else {
    filename "undionly.kpxe";
}
```

In the above piece, we’re dealing with a client called “iPXE.” Instead of getting another boot file through TFTP, it will be sent straight to an HTTP URL, specifically \*\*, a custom iPXE script you’ll set up. Clients that haven’t switched to iPXE yet (the initial BIOS PXE boot) will receive the _undionly.kpxe_ for chain loading. If you’re working with UEFI, follow a similar method: serve _ipxe.efi_ and configure it to load the script via HTTP.

Make sure to update the DHCP settings and restart the service. Don’t forget to place the new _undionly.kpxe_ and _ipxe.efi_ files into the TFTP root folder. The process now goes like this:

-   BIOS client initiates PXE boot.
-   It downloads _undionly.kpxe_ from TFTP.
-   iPXE takes it from here.
-   A second DHCP request that supplies the \*\* URL is made by iPXE.
-   iPXE performs an HTTP GET on that URL to fetch the boot script.

#### Set Up an HTTP Server for iPXE

Now that we’re getting iPXE to fetch a boot script (and maybe some other files) over HTTP let’s configure an HTTP server:

**Install HTTP Server**  
You can do this on the PXE server or any other machine, but we’ll stick with the PXE server for simplicity. Install a web server like Apache or Nginx. For instance, when using Ubuntu, you can run:

```
sudo apt install apache2
```

Ensure the server is running and reachable from your target network. If necessary, open port 80 in the firewall.

**Create iPXE Script File** This will be the file you reference in the DHCP configuration. Create a file, such as _/var/www/html/_, where you’ll write the iPXE commands. At a minimum, this file can chain to another script or load an OS installer directly. A basic configuration might prompt iPXE to display a menu or start an installer.

```
#!ipxe
kernel http://192.168.1.10/os_install/vmlinuz initrd=initrd.img nomodeset ro
initrd http://192.168.1.10/os_install/initrd.img
boot
```

This example (not interactive) tells iPXE to download a kernel and _initrd_ over HTTP and boot them.

**Host OS Installation Files (if needed)** Before proceeding, ensure that iPXE can access the necessary OS installation files, such as the kernel, _initrd_, or the complete ISO repository. iPXE can retrieve both the kernel and _initrd_ files. These essential files should be in the main directory or the web server’s subdirectory.  
For example, copy your Linux distribution installer files _vmlinuz_ and _initrd.img_ to the directory _/var/www/html/os\_install/_. iPXE can retrieve large files such as Windows PE images or Linux ISOs through HTTP to load them into memory or pass them to the bootloader.

**Test HTTP Access**  
Run a retrieval test for the \*\* file from another machine or by using the _curl_ command. This helps verify that the web server is functioning correctly.

[Creating iPXE Boot Scripts](#creating-ipxe-boot-scripts)[](#creating-ipxe-boot-scripts)
----------------------------------------------------------------------------------------

The scripting capability stands out as one of iPXE’s most powerful features. The iPXE scripting process involves writing a text file that contains a sequence of iPXE commands that begins with the [shebang](https://www.geeksforgeeks.org/using-shebang-in-linux/) line _#!ipxe_ Using scripts, you can create menus, automate operating system selection, and pass kernel parameters to completely automate deployment processes.

**Basics of iPXE Scripting**  
The following commands can be implemented within an iPXE script:

-   _kernel_ – Select a kernel or boot loader that should be downloaded and executed.
-   _initrd_ – It allows you to define the initial ramdisk for download purposes.
-   _boot_ – Use it to launch the loaded kernel using the specified _initrd_.
-   _chain_ – It allows a script to transfer boot control to another script or bootloader.
-   Flow control commands such as _goto_, _ifopen_, and menu-related commands line _menu_, _item_, and _choose to_ enable the creation of interactive decisions.

An embedded iPXE script can use the `chain` command to load _boot.ipxe_ by specifying the appropriate server URL.

**Example: Interactive Boot Menu Script** Create a script file such as _menu.ipxe_ on your web server or directly embed it into iPXE. For example:

```
#!ipxe
console --x 1024 --y 768  # set console resolution (optional)
menu iPXE Boot Menu
item --key u ubuntu Install Ubuntu 22.04
item --key m memtest Run Memtest86+
choose --timeout 5000 target || goto cancel

:ubuntu
kernel http://192.168.1.10/boot/ubuntu/vmlinuz initrd=initrd.img autoinstall ds=nocloud-net;s=http://192.168.1.10/ubuntu-seed/
initrd http://192.168.1.10/boot/ubuntu/initrd.img
boot
:memtest
kernel http://192.168.1.10/boot/memtest86+
boot
:cancel
echo Boot canceled or timed out. Rebooting...
reboot
```

The iPXE script builds a boot menu that allows users to start an Ubuntu installation or perform diagnostics using [Memtest](https://www.memtest86.com/).  
The script configures console resolution while displaying options that enable the user to initiate Ubuntu installation by pressing “_u_” and start Memtest by pressing “_m_.”

The system will cancel the process and restart if the user does not make a selection within 5 seconds. The Ubuntu boot option initiates kernel and _initrd_ loading through HTTP, enabling automated installation through [_cloud-init_](https://canonical-subiquity.readthedocs-hosted.com/en/latest/explanation/cloudinit-autoinstall-interaction.html#cloudinit-autoinstall-interaction) using _nocloud-net_. The Memtest option loads a tool for memory testing from the same server. This entire networking operation runs remotely without physical interaction with the machine. This is the essence of bare metal provisioning.

[Security Implications of Network Booting and Mitigations](#security-implications-of-network-booting-and-mitigations)[](#security-implications-of-network-booting-and-mitigations)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Improper configuration of network booting can create potential security vulnerabilities. Attackers might fake DHCP or TFTP server identities to send harmful images to clients. Here are some best practices:

-   **Use Separate VLANs:** Ensure your production network remains isolated from your provisioning network.
-   **Use iPXE with HTTPS:** iPXE enables network booting from servers that use HTTPS to maintain traffic encryption.
-   **Enable Client Authentication:** iPXE scripts allow integration with security protocols such as username/password authentication or certificate-based validation.
-   **Configure Secure Boot:** Enable secure boot on systems using UEFI firmware. It creates additional challenges but blocks unauthorized boot loaders from running.
-   **DHCP Snooping:** In some environments, network switches that implement [DHCP snooping](https://www.fs.com/blog/what-is-dhcp-snooping-and-how-it-works-1108.html) can block unauthorized DHCP servers from distributing addresses and harmful boot instructions.

[Common Issues and Troubleshooting Tips](#common-issues-and-troubleshooting-tips)[](#common-issues-and-troubleshooting-tips)
----------------------------------------------------------------------------------------------------------------------------

When using PXE/iPXE, you will experience typical issues. The table below displays issues and tips to troubleshoot them:

Issue

Troubleshooting Tip

PXE Boot Not Triggering

Check BIOS/UEFI settings. Ensure the network adapter is enabled for booting and is higher in boot order than the disk.

No DHCP Offer (PXE-E51)

Ensure the DHCP server is running and reachable. If on different subnets, configure a DHCP relay (IP Helper).

PXE Download Fails / TFTP Timeout (PXE-E32)

Check that the TFTP server is running, file paths are correct, and firewall rules allow TFTP traffic.

Infinite Boot Loop (iPXE chainloading)

Ensure DHCP provides a different filename to iPXE or use an embedded iPXE script to chain to an HTTP URL.

iPXE Command Failed

Verify the URL or file name. Test HTTP links in a browser and TFTP files using a TFTP client.

UEFI Boot Issues

Ensure the correct binary is used (ipxe.efi for UEFI). Check Secure Boot settings and compatibility with boot files.

TFTP Access Denied

Ensure TFTP files have the correct permissions and are in the server’s TFTP directory.

Slow PXE Boot

To improve speed, use iPXE to load boot files via HTTP/HTTPS instead of TFTP.

PXE VLAN Misconfiguration

Check that PXE VLANs are properly tagged and the DHCP relay is configured to pass packets.

Client Architecture Mismatch

Ensure BIOS clients receive BIOS boot files and UEFI clients receive UEFI-compatible ones.

Troubleshooting PXE is complex because it relies on several interconnected systems, including the client, DHCP server, and TFTP server. You can usually isolate the issue by checking each stage (Did I get an IP? Did I download the file? Did it execute?).  
[Microsoft’s documentation](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/os-deployment/advanced-troubleshooting-pxe-boot) on debugging PXE issues and tools such as [Wireshark](https://www.wireshark.org/docs/wsug_html_chunked/) for capturing DHCP/TFTP exchanges prove useful for advanced troubleshooting.

[FAQ SECTION](#faq-section)[](#faq-section)
-------------------------------------------

**How to install a bare metal server?**  
You need to set up a PXE server or iPXE boot environment and set the server’s BIOS or UEFI to network boot while pulling the operating system installer from network resources. The operating system installation continues with either full automation or a combination of manual and automatic approaches.

**What is iPXE vs PXE?**  
PXE is the traditional network booting standard PXE that operates under the TFTP protocol. As an enhanced open-source network boot solution, iPXE supports modern protocols such as HTTP/HTTPS, iSCSI, and scripting capabilities.

**What is an iPXE file?**  
The iPXE file is a script or the compiled boot loader binary from the iPXE GitHub repository. The script represents a set of instructions for fetching and booting images from the network. In binary form, iPXE usually functions as either a standalone or chainloadable version of iPXE.

**What does PXE boot stand for?**  
Preboot eXecution Environment (PXE) represents a booting technique that allows computers to boot through their network interface without depending on local storage or installed operating systems.

**What is the difference between PXE and gPXE?**  
gPXE emerged from Etherboot to enhance PXE through additional network boot protocols. iPXE is a further evolution of gPXE. It has replaced gPXE while maintaining ongoing development work.

**What does PXE stand for in IT?**  
The acronym PXE in IT refers to the Preboot eXecution Environment specification created by Intel to enable network-based booting.

**Is iPXE a bootloader?**  
iPXE operates as a bootloader. It can load kernels\*, initrds\*, and disk images through multiple network protocols.

**What is the iPXE command line?**  
Once iPXE is launched, it gives you access to a command line interface, enabling you to execute commands such as _dhcp_, _ifstat_, _chain_, _boot, etc_. The interface delivers extensive capabilities for real-time debugging purposes and advanced provisioning tasks.

**What is the difference between PXE and UEFI?**  
PXE represents a network booting protocol, and UEFI stands for Universal Extensible Firmware Interface, which serves as modern firmware replacing legacy BIOS. UEFI incorporates built-in network stack capabilities to support network boots through PXE and HTTP protocols.

**What is PXE, and how does it work?**  
PXE is a network boot protocol that enables clients to load their operating system from a server using DHCP to assign IP addresses and TFTP to download the bootloader. The boot process begins with a DHCP broadcast, which guides the request to a TFTP server, which then downloads the boot loader to proceed with OS installation or loading.

**How is iPXE different from PXE?**  
iPXE extends PXE capabilities by supporting new protocols (HTTP, HTTPS, iSCSI, etc.), a scripting engine, and better performance.

**What are the benefits of using iPXE scripts?**

-   **Automation**: Complex logic can be implemented to reduce the need for manual intervention.
-   **Dynamic Boot Choices**: The system can automatically select which OS images to load based on the detected hardware specifications, MAC address information, or user input.
-   **Security**: Use HTTPS integration to ensure secure downloads of images and authenticate users.

**Can PXE and iPXE be used in hybrid cloud environments?** PXE and iPXE technologies are utilized in hybrid cloud and multi-cloud environments. Organizations commonly use them to quickly deploy on-premise bare metal servers and manage cloud-based auto-scaling groups that address temporary workload spikes.

**How do I troubleshoot PXE boot issues?**

-   **Check DHCP**: The DHCP server should be operational and properly assigning IP addresses.
-   **Review TFTP Logs**: TFTP requires careful configuration as it needs accurate file paths and appropriate permissions to operate correctly.
-   **Firewall Configuration**: Ensure necessary TFTP and DHCP ports remain open.
-   **UEFI vs BIOS**: Before proceeding, check that the boot files correspond to the firmware type.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

Network booting is fundamental to modern IT systems, especially when provisioning bare metal servers and deploying dedicated infrastructure. Using PXE and iPXE boot, organizations can achieve automated operating system deployments, optimize server provisioning, and efficiently manage their infrastructures.

Network booting for bare metal servers requires IT professionals to understand the differences between PXE and iPXE, PXE server configuration, and automated OS deployment implementation. This guide details setup procedures and security implications while providing troubleshooting methods to help organizations implement scalable and secure network boot solutions. Through proper PXE/iPXE configuration, businesses can achieve efficient bare metal provisioning while improving their IT operational performance.

[References and Resources](#references-and-resources)[](#references-and-resources)
----------------------------------------------------------------------------------

-   [Preparing a PXE installation source](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/automatically_installing_rhel/preparing-for-a-network-install_rhel-installer#preparing-for-a-network-install_rhel-installer)
-   [PXE Booting and Kickstart Technology](https://docs.oracle.com/cd/E17559_01/em.111/e16599/appdx_pxeboot.htm#:~:text=computers%20can%20boot%20over%20a,pdf)
-   [Network-Based Deployment Using Preboot Execution Environment](https://www.theseus.fi/bitstream/handle/10024/512485/Cameron%20Stewart%20-%20Thesis%20-%20Network-Based%20Deployment%20using%20PXE.pdf?sequence=2#:~:text=The%20PXE%20framework%20is%20implemented,This%20firmware%20includes%20a%20minimal)
-   [Understand PXE boot in Configuration Manager](https://learn.microsoft.com/en-us/troubleshoot/mem/configmgr/os-deployment/understand-pxe-boot)
-   [iPXE - open-source boot firmware](https://ipxe.org/start)
-   [iPXE - open-source boot firmware](https://ipxe.org/docs#:~:text=)
-   [What are the most significant security concerns on PXE?](https://security.stackexchange.com/questions/64915/what-are-the-biggest-security-concerns-on-pxe)
-   [Security and privacy for OS deployment in Configuration Manager](https://learn.microsoft.com/en-us/mem/configmgr/osd/plan-design/security-and-privacy-for-operating-system-deployment)
-   [Attacks Against Windows PXE Boot Images](https://www.netspi.com/blog/technical-blog/network-pentesting/attacks-against-windows-pxe-boot-images/#:~:text=Attacks%20Against%20Windows%20PXE%20Boot,authorization%20of%20any%20kind)

#### [Source](https://www.digitalocean.com/community/tutorials/bare-metal-provisioning-with-pxe-and-ipxe)

<br/>
---
