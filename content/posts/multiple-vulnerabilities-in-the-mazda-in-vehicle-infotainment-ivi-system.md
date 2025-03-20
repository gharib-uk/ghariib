---
title: "Multiple Vulnerabilities in the Mazda In-Vehicle Infotainment (IVI) System"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
  - "zeroday"
---

Multiple vulnerabilities have been discovered in the Mazda Connect Connectivity Master Unit (CMU) system installed in multiple car models, such as the Mazda 3 model year 2014-2021. Like in so many cases, these vulnerabilities are caused by insufficient sanitization when handling attacker-supplied input. A physically present attacker could exploit these vulnerabilities by connecting a specially crafted USB device – such as an iPod or mass storage device – to the target system. Successful exploitation of some of these vulnerabilities results in arbitrary code execution with root privileges.

# The Target

The specific CMU unit that was the target of the research was manufactured by Visteon, while the software was initially developed by Johnson Controls Inc (JCI). The research was performed against the latest available software version (74.00.324A). Earlier software versions down to at least 70.x may also be affected by these vulnerabilities.

Notably, the CMU has a very active “modding scene” with the community releasing multiple software “tweaks” to alter the operation of the unit. The installation of these tweaks typically relied on software vulnerabilities. At the time of publication, we are unaware of any publicly known vulnerabilities in the latest firmware version.

**Hardware Design**

The unit exterior looks like this:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d6f79a1c-d7e9-4fd0-b145-cec409a6e16c/IMG_3894.JPG?format=1000w)

There is a sticker on the bottom with some technical information about the unit. The sticker notes the specific CMU model being `MAZDA_GEN_65_CMU`.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ac488631-eff6-411d-b7be-27bb121c3ee1/IMG_3895.jpg?format=1000w)

On the back, the unit provides multiple connectors that carry power, audio input/output, USB and CAN signals, and a serial console.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e8927d4d-01bf-46a6-81d5-a274415c68d5/IMG_E4064.JPG?format=1000w)

Inside, the unit is built on a single PCB which carries an application SoC (Freescale part number SCIMX06DAVT10AC), an assortment of memory ICs (a serial Flash on the back of the board, a NAND Flash, and an eMMC), an auxiliary MCU (Renesas part number R5F35MCEJFF), and a Bluetooth module (Murata part number LBEE6Z2U0C-584).

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/008d1bf6-000c-4b71-9545-337dded74d4a/IMG_E3675.JPG?format=1000w)

The functionality available on the edge connectors was not explored in this research.

Typically, in designs where there is more than one processing unit present, some degree of separation is implemented. User-facing applications handled by a rich OS (i.e., QNX, Linux, Android, etc.) run on the application SoC, while CAN connectivity is handled by an MCU usually running an RTOS. This appears to be the case for this unit as well.

**Software design**

On the software side of things, the unit’s main SoC is running a Linux-based OS image, as evidenced by the following output (abbreviated here) captured via the serial console:

While there is a login prompt, authentication credentials for the latest software version are not publicly known. The console proceeds to serve a vast amount of logging output, which could be useful for testing exploits.

When fully booted, the system has the following file systems mounted:

All the persistent memory storage is accessible from the OS. The serial Flash is present as the `mtdblock`partitions, the NAND is present as the `ffx01` partitions, and the eMMC as the `mmcblk` partitions. Notably, several file systems are mounted read-write as signified by the `rw` string included in parentheses.

**The Software Update Process**

The software on the unit can be updated from the diagnostic menu:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ed197124-dde2-453f-b7cc-966b61a8510a/Picture1.jpg?format=1000w)

The update process expects a file with the `.up` extension to be located on a mass storage device connected via USB. The dealer could then enter this diagnostic menu and start the update process.

Software update files are password-protected ZIP archives containing multiple directories for various steps in the update process, as well as an “instruction” file describing how to perform the update. There are 21 steps in the latest update file. Of special interest is the contents of the following directories:

• `rootfs1upd` containing a `.tar.gz` archive of the entire root file system,  
• `linux1` containing the Linux kernel 3.0.35,  
• `ibc1` and `ibc2` containing a bootloader,  
• `bootstrap` containing yet another bootloader,  
• `passwdupdate` containing the replacement `passwd` file,  
• `vip` containing the update image for the auxiliary MCU.

The majority of the update process is implemented in several shared objects. All business logic is located in `svcjciupdatea.so` and `svcjciupdates.so`, which use supporting code in `libjcireflashua.so` and `libjcisecurity.so`. The update process was investigated only up to the point of signature verification. Further investigation would assume the ability to produce either properly signed updates or a signature verification bypass. Unfortunately, the former was not available at the time of this research, and the latter was not discovered during this effort.

On a high level, the update verification process is contained in the `svcjciupdates.so:updates_sys_srv_ValidatePackageOperationThread()` function, which calls other functions to perform verification steps in the given order:

• `updates_sys_srv_LoadUP()` parses the specified update file, extracts the `main_instructions.ini` and `versions.ini` files, and stores them in memory for later use.  
• `updates_sys_srv_ValidateUPStructure()` validates whether the internal structure of the update file matches the expected layout. This was not an issue when the format of proper update files was followed to create an exploit.  
• `updates_sys_srv_ExtractCertificates()` extracts the signing certificate chain from the update file to be used for update verification. The chain contains the publisher certificate used to sign the update and an intermediate CA certificate that issued the publisher certificate. The intermediate CA certificate is issued by the JCI root CA.  
• `updates_sys_sec_VerifyUPCertificates()` verifies the extracted certificate chain to ensure the chain properly terminates in the expected root CA stored on the device.  
• `updates_sys_sec_VerifyUPSignature()` verifies the signature of a specified update file against the publisher certificate. The signature in this case is located at the end of the update file and is 256 bytes long.  
• `updates_sys_srv_RemoveExtractedCertificates()` cleans up after the verification process.

Of particular note is the fact that multiple operations are performed on the supplied update file by the `updates_sys_srv_LoadUP()` function and its callees before any integrity checks occur against the user-supplied file.

# The Vulnerabilities

Multiple vulnerabilities were discovered during our research effort, which can be used in conjunction to achieve a complete and persistent compromise of the infotainment system.

# CVE-2024-8355/ZDI-24-1208: DeviceManager iAP Serial Number SQL Injection

This vulnerability was the first bug discovered during research, spurring a deeper look into this unit.

The specific vulnerability exists in the `eInsertDeviceEntry()` function of the `/jci/devicemanager/libdevicemanager.so` library. When attempting to insert a new Apple device into the `DeviceDatabase.db` SQLite database, several values are taken from the device and used in an SQL statement without sanitization. See the following pseudocode implementation of the `eInsertDeviceEntry()` function:

Because of this, when the infotainment system detects an Apple device has been connected and requests, for example, its iAP serial number, a spoofed iPod or other Apple device can instead reply with a string along the lines of:

This leads to the DeviceManager executing the injected SQL statement on the infotainment system with root privileges. This could be used to manipulate the database itself, disclose information, create arbitrary files on the file system, and possibly execute code. Exploitation of this vulnerability is somewhat limited due to an apparent length limitation of 0x36 bytes on the input, but this could potentially be worked around by having several spoofed iPods connect one after the other, each with its own injected SQL statements in place of a serial number.

# CVE-2024-8359/ZDI-24-1191: REFLASH\_DDU\_FindFile Command Injection RCE

One of the multitude of functions supporting the update process is the `REFLASH_DDU_FindFile()` function found in the `libjcireflashua.so` shared object. The pseudocode for this function is reproduced below for clarity:

The function attempts to find a specific file within the update package specified via the provided file path. This is performed by interpolating the provided path into a command line for the `unzip` program. Unfortunately, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system. The attacker can control both the `path` and `file` elements of the overall path.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• REFLASH\_UA\_LoadUP in libjcireflashua.so  
• REFLASH\_DDU\_FindFile

# CVE-2024-8360/ZDI-24-1192: REFLASH\_DDU\_ExtractFile Command Injection RCE

One of the multitude of functions supporting the update process is the `REFLASH_DDU_ExtractFile()` function found in the `libjcireflashua.so` shared object. The pseudocode for this function is reproduced below for clarity:

The function attempts to extract a specific file from the update package specified by the path supplied through the `up_path` argument. This interpolates the provided path into a command line for the `unzip` program. Unfortunately, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. The attacker can control both `path` and `file` elements of the overall path. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• REFLASH\_UA\_LoadUP in libjcireflashua.so  
• REFLASH\_DDU\_ExtractFile

# CVE-2024-8358/ZDI-24-1190: UPDATES\_ExtractFile Command Injection RCE

One of the various functions supporting the update process is the `UPDATES_ExtractFile()` function found in the `svcjciupdates.so` shared object. The pseudocode for this function is partially reproduced below for clarity:

The function attempts to extract a specific file from the update package specified by the path supplied through the `up_path` argument. This interpolates the provided path into a command line for the `unzip` program. As with the other bugs, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. The attacker can control both `path` and `file` elements of the overall path. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• updates\_sys\_srv\_ExtractCertificates  
• updates\_sys\_srv\_ExtractPubCert  
• UPDATES\_ExtractFile("publisher\_cert.pem", "/tmp/reflash/publisher\_cert.pem")

# CVE-2024-8357/ZDI-24-1189: App SoC Missing Root of Trust in Hardware

As previously mentioned, the head unit is partitioned into two relatively independent systems: the application SoC running Linux and the VIP MCU running an unspecified OS. The application SoC uses an onboard SPI Flash chip to boot. The chip is partitioned as follows:

Here, the mtd0 partition stores the bootstrap code, which appears to be the first piece of code to be executed on the system. The application SoC was found to be missing any authentication of this bootstrap code. Additionally, none of the subsequent OS boot steps perform any authentication of the next boot stage before passing control to it. This results in the following opportunities for an attacker who has gained code execution on the Linux system:

• Manipulate the root filesystem, changing arbitrary files and e.g. gaining persistence on the system,  
• Manipulate configuration data residing in the mtd5 partition, e.g. installing arbitrary SSH keys, and  
• Manipulate the bootstrap code, gaining the ability to execute arbitrary code prior to OS boot.

# CVE-2024-8356/ZDI-24-1188: VIP MCU Unsigned Code

As previously mentioned, the head unit is partitioned into two relatively independent systems; the application SoC running Linux and the VIP MCU running an unspecified OS. The MCU appears to support several CMU functions, including CAN/LIN connectivity to the rest of the vehicle. The partitioning is also assumed to represent a security boundary in that the compromise of the application SoC is expected to be contained and not affect the rest of the vehicle in a way that may compromise safety.

The MCU in question is identified as VIP in the strings found in the CMU software. Through the analysis of software update packages, it was discovered the VIP is also updated during the software update process. The tools to do so, as well as the update image, are found in the `vip` subdirectory of the update package.

The `vcm` tool allows performing the following operations against the VIP:

Specifically, the `--write-image` command allows writing an update image to the VIP. This command is normally used by the update script and takes an input file formatted as Motorola S-records. Of particular interest is also querying the versions of VIP software components. The following is reported on the unit running software `cmu150_NA_74.00.324A`:

These strings can be located in the update file `image-vip_appvfbl.mot` and are destined to be programmed at memory address 0xC000A in the VIP MCU memory space and appear to be part of the application image spanning addresses 0xC0000 through 0xFFFFF. By modifying these strings, it is possible to determine whether the image is validated before being programmed into the internal memory.

The original S-record file was manipulated to change one string to produce a visible indication of different firmware, and the resulting image was programmed back to the VIP MCU:

Using the same command for version check, it is possible to confirm the tampered image was accepted:

In a more global sense, this allows an attacker to pivot from a compromised application SoC running Linux to the VIP MCU by installing a crafted firmware version and subsequently gaining direct access to the connected CAN busses of the vehicle.

# Exploitation

Exploitation of the OS command injection vulnerabilities is relatively straightforward. All the attacker needs to do is create a file on a FAT32-formatted USB mass storage device where the name will contain the OS commands to be executed. The filename must end with `.up` for it to be recognized by the software update handling code. While all three command injection vulnerabilities are exploited via the file name, the easiest one to exploit is by far the ZDI-CAN-23420 as there are no specific exploitation requirements such as validity of the crafted update file.

Exploitation of the described command injection vulnerabilities can be further facilitated by the fact that software update installation can be triggered automatically upon connecting a USB mass storage device; this is a “known feature” which is activated by simply placing an empty file named `jci-autoupdate` on the storage device.

Once the initial compromise is achieved, the attacker can gain persistence through manipulation of the root file system, such as installing backdoored system components which will persist across system restarts.

Furthermore, the attacker can move laterally and install a specially crafted VIP microcontroller software allowing unfettered access to vehicle networks, potentially impacting vehicle operation and safety. Specific impacts (what could be controlled and how) were not investigated during this research effort.

# Associated Risks for Users/Owners

In the lab environment, the entire attack chain – from plugging in a USB drive to installing a crafted update on the VIP – takes only a few minutes. Currently, there is no reason to believe a similar attack will take significantly more time against a unit installed in a car. This means that the vehicle can be compromised while, for example, it is being handled by a valet, during a ride share, or via USB malware. Naturally, the same can be done in a shop environment where there are no significant time pressures.

The CMU can then be compromised and “enhanced” to, for example, attempt to compromise any connected device in targeted attacks that can result in DoS, bricking, ransomware, safety compromise, etc.

# Conclusions

This research effort and its output highlighted the fact that high-impact vulnerabilities can be found even in a very mature automotive product that has been on the market for a number of years and with a long history of security fixes. To date, these vulnerabilities remain unpatched by the vendor.

There has been a lot of focus and discussion recently in the security community about the importance of language security and “memory safe” languages. This set of real-world vulnerabilities is a good reminder that focusing on only one bug class or employing one set of security tools (static code analysis, for instance) does not result in deployed systems being secure. In this case, C was used to develop the firmware, but it is an irrelevant fact considering the nature of the discovered vulnerabilities. Any ‘memory-safe’ firmware would have also been compromised in this instance.

While the SQL and OS command injection issues represent programming errors, the remaining vulnerabilities are security design flaws. It is paramount that system integrity is ensured at all runtime stages and for all components. Only then is it possible to guarantee the downstream security properties of languages and their compilers.

In summary, it is important to consider the whole system security and security-test complete production systems in all their operational modes.

You can find me on Mastodon at @infosecdj, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Multiple vulnerabilities have been discovered in the Mazda Connect Connectivity Master Unit (CMU) system installed in multiple car models, such as the Mazda 3 model year 2014-2021. Like in so many cases, these vulnerabilities are caused by insufficient sanitization when handling attacker-supplied input. A physically present attacker could exploit these vulnerabilities by connecting a specially crafted USB device – such as an iPod or mass storage device – to the target system. Successful exploitation of some of these vulnerabilities results in arbitrary code execution with root privileges.

# The Target

The specific CMU unit that was the target of the research was manufactured by Visteon, while the software was initially developed by Johnson Controls Inc (JCI). The research was performed against the latest available software version (74.00.324A). Earlier software versions down to at least 70.x may also be affected by these vulnerabilities.

Notably, the CMU has a very active “modding scene” with the community releasing multiple software “tweaks” to alter the operation of the unit. The installation of these tweaks typically relied on software vulnerabilities. At the time of publication, we are unaware of any publicly known vulnerabilities in the latest firmware version.

**Hardware Design**

The unit exterior looks like this:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d6f79a1c-d7e9-4fd0-b145-cec409a6e16c/IMG_3894.JPG?format=1000w)

There is a sticker on the bottom with some technical information about the unit. The sticker notes the specific CMU model being `MAZDA_GEN_65_CMU`.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ac488631-eff6-411d-b7be-27bb121c3ee1/IMG_3895.jpg?format=1000w)

On the back, the unit provides multiple connectors that carry power, audio input/output, USB and CAN signals, and a serial console.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e8927d4d-01bf-46a6-81d5-a274415c68d5/IMG_E4064.JPG?format=1000w)

Inside, the unit is built on a single PCB which carries an application SoC (Freescale part number SCIMX06DAVT10AC), an assortment of memory ICs (a serial Flash on the back of the board, a NAND Flash, and an eMMC), an auxiliary MCU (Renesas part number R5F35MCEJFF), and a Bluetooth module (Murata part number LBEE6Z2U0C-584).

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/008d1bf6-000c-4b71-9545-337dded74d4a/IMG_E3675.JPG?format=1000w)

The functionality available on the edge connectors was not explored in this research.

Typically, in designs where there is more than one processing unit present, some degree of separation is implemented. User-facing applications handled by a rich OS (i.e., QNX, Linux, Android, etc.) run on the application SoC, while CAN connectivity is handled by an MCU usually running an RTOS. This appears to be the case for this unit as well.

**Software design**

On the software side of things, the unit’s main SoC is running a Linux-based OS image, as evidenced by the following output (abbreviated here) captured via the serial console:

While there is a login prompt, authentication credentials for the latest software version are not publicly known. The console proceeds to serve a vast amount of logging output, which could be useful for testing exploits.

When fully booted, the system has the following file systems mounted:

All the persistent memory storage is accessible from the OS. The serial Flash is present as the `mtdblock`partitions, the NAND is present as the `ffx01` partitions, and the eMMC as the `mmcblk` partitions. Notably, several file systems are mounted read-write as signified by the `rw` string included in parentheses.

**The Software Update Process**

The software on the unit can be updated from the diagnostic menu:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ed197124-dde2-453f-b7cc-966b61a8510a/Picture1.jpg?format=1000w)

The update process expects a file with the `.up` extension to be located on a mass storage device connected via USB. The dealer could then enter this diagnostic menu and start the update process.

Software update files are password-protected ZIP archives containing multiple directories for various steps in the update process, as well as an “instruction” file describing how to perform the update. There are 21 steps in the latest update file. Of special interest is the contents of the following directories:

• `rootfs1upd` containing a `.tar.gz` archive of the entire root file system,  
• `linux1` containing the Linux kernel 3.0.35,  
• `ibc1` and `ibc2` containing a bootloader,  
• `bootstrap` containing yet another bootloader,  
• `passwdupdate` containing the replacement `passwd` file,  
• `vip` containing the update image for the auxiliary MCU.

The majority of the update process is implemented in several shared objects. All business logic is located in `svcjciupdatea.so` and `svcjciupdates.so`, which use supporting code in `libjcireflashua.so` and `libjcisecurity.so`. The update process was investigated only up to the point of signature verification. Further investigation would assume the ability to produce either properly signed updates or a signature verification bypass. Unfortunately, the former was not available at the time of this research, and the latter was not discovered during this effort.

On a high level, the update verification process is contained in the `svcjciupdates.so:updates_sys_srv_ValidatePackageOperationThread()` function, which calls other functions to perform verification steps in the given order:

• `updates_sys_srv_LoadUP()` parses the specified update file, extracts the `main_instructions.ini` and `versions.ini` files, and stores them in memory for later use.  
• `updates_sys_srv_ValidateUPStructure()` validates whether the internal structure of the update file matches the expected layout. This was not an issue when the format of proper update files was followed to create an exploit.  
• `updates_sys_srv_ExtractCertificates()` extracts the signing certificate chain from the update file to be used for update verification. The chain contains the publisher certificate used to sign the update and an intermediate CA certificate that issued the publisher certificate. The intermediate CA certificate is issued by the JCI root CA.  
• `updates_sys_sec_VerifyUPCertificates()` verifies the extracted certificate chain to ensure the chain properly terminates in the expected root CA stored on the device.  
• `updates_sys_sec_VerifyUPSignature()` verifies the signature of a specified update file against the publisher certificate. The signature in this case is located at the end of the update file and is 256 bytes long.  
• `updates_sys_srv_RemoveExtractedCertificates()` cleans up after the verification process.

Of particular note is the fact that multiple operations are performed on the supplied update file by the `updates_sys_srv_LoadUP()` function and its callees before any integrity checks occur against the user-supplied file.

# The Vulnerabilities

Multiple vulnerabilities were discovered during our research effort, which can be used in conjunction to achieve a complete and persistent compromise of the infotainment system.

# CVE-2024-8355/ZDI-24-1208: DeviceManager iAP Serial Number SQL Injection

This vulnerability was the first bug discovered during research, spurring a deeper look into this unit.

The specific vulnerability exists in the `eInsertDeviceEntry()` function of the `/jci/devicemanager/libdevicemanager.so` library. When attempting to insert a new Apple device into the `DeviceDatabase.db` SQLite database, several values are taken from the device and used in an SQL statement without sanitization. See the following pseudocode implementation of the `eInsertDeviceEntry()` function:

Because of this, when the infotainment system detects an Apple device has been connected and requests, for example, its iAP serial number, a spoofed iPod or other Apple device can instead reply with a string along the lines of:

This leads to the DeviceManager executing the injected SQL statement on the infotainment system with root privileges. This could be used to manipulate the database itself, disclose information, create arbitrary files on the file system, and possibly execute code. Exploitation of this vulnerability is somewhat limited due to an apparent length limitation of 0x36 bytes on the input, but this could potentially be worked around by having several spoofed iPods connect one after the other, each with its own injected SQL statements in place of a serial number.

# CVE-2024-8359/ZDI-24-1191: REFLASH\_DDU\_FindFile Command Injection RCE

One of the multitude of functions supporting the update process is the `REFLASH_DDU_FindFile()` function found in the `libjcireflashua.so` shared object. The pseudocode for this function is reproduced below for clarity:

The function attempts to find a specific file within the update package specified via the provided file path. This is performed by interpolating the provided path into a command line for the `unzip` program. Unfortunately, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system. The attacker can control both the `path` and `file` elements of the overall path.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• REFLASH\_UA\_LoadUP in libjcireflashua.so  
• REFLASH\_DDU\_FindFile

# CVE-2024-8360/ZDI-24-1192: REFLASH\_DDU\_ExtractFile Command Injection RCE

One of the multitude of functions supporting the update process is the `REFLASH_DDU_ExtractFile()` function found in the `libjcireflashua.so` shared object. The pseudocode for this function is reproduced below for clarity:

The function attempts to extract a specific file from the update package specified by the path supplied through the `up_path` argument. This interpolates the provided path into a command line for the `unzip` program. Unfortunately, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. The attacker can control both `path` and `file` elements of the overall path. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• REFLASH\_UA\_LoadUP in libjcireflashua.so  
• REFLASH\_DDU\_ExtractFile

# CVE-2024-8358/ZDI-24-1190: UPDATES\_ExtractFile Command Injection RCE

One of the various functions supporting the update process is the `UPDATES_ExtractFile()` function found in the `svcjciupdates.so` shared object. The pseudocode for this function is partially reproduced below for clarity:

The function attempts to extract a specific file from the update package specified by the path supplied through the `up_path` argument. This interpolates the provided path into a command line for the `unzip` program. As with the other bugs, no sanitization is performed before that occurs, and attacker-controlled input (e.g., `/dev/sda1/path/file.up`) is passed to the `system()` function. The attacker can control both `path` and `file` elements of the overall path. This allows the attacker to inject arbitrary OS commands that will be executed by the head unit OS shell, allowing for a full compromise of the system.

The affected function can be reached via the following call graph:

• UPDATES\_SYS\_IPC\_INTERFACE\_GetPackageInfo\_svc in svcjciupdates.so  
• UPDATES\_SYS\_SRV\_GetPackageInfo  
• updates\_sys\_srv\_GetPackageInfoOperationThread  
• updates\_sys\_srv\_LoadUP  
• updates\_sys\_srv\_ExtractCertificates  
• updates\_sys\_srv\_ExtractPubCert  
• UPDATES\_ExtractFile("publisher\_cert.pem", "/tmp/reflash/publisher\_cert.pem")

# CVE-2024-8357/ZDI-24-1189: App SoC Missing Root of Trust in Hardware

As previously mentioned, the head unit is partitioned into two relatively independent systems: the application SoC running Linux and the VIP MCU running an unspecified OS. The application SoC uses an onboard SPI Flash chip to boot. The chip is partitioned as follows:

Here, the mtd0 partition stores the bootstrap code, which appears to be the first piece of code to be executed on the system. The application SoC was found to be missing any authentication of this bootstrap code. Additionally, none of the subsequent OS boot steps perform any authentication of the next boot stage before passing control to it. This results in the following opportunities for an attacker who has gained code execution on the Linux system:

• Manipulate the root filesystem, changing arbitrary files and e.g. gaining persistence on the system,  
• Manipulate configuration data residing in the mtd5 partition, e.g. installing arbitrary SSH keys, and  
• Manipulate the bootstrap code, gaining the ability to execute arbitrary code prior to OS boot.

# CVE-2024-8356/ZDI-24-1188: VIP MCU Unsigned Code

As previously mentioned, the head unit is partitioned into two relatively independent systems; the application SoC running Linux and the VIP MCU running an unspecified OS. The MCU appears to support several CMU functions, including CAN/LIN connectivity to the rest of the vehicle. The partitioning is also assumed to represent a security boundary in that the compromise of the application SoC is expected to be contained and not affect the rest of the vehicle in a way that may compromise safety.

The MCU in question is identified as VIP in the strings found in the CMU software. Through the analysis of software update packages, it was discovered the VIP is also updated during the software update process. The tools to do so, as well as the update image, are found in the `vip` subdirectory of the update package.

The `vcm` tool allows performing the following operations against the VIP:

Specifically, the `--write-image` command allows writing an update image to the VIP. This command is normally used by the update script and takes an input file formatted as Motorola S-records. Of particular interest is also querying the versions of VIP software components. The following is reported on the unit running software `cmu150_NA_74.00.324A`:

These strings can be located in the update file `image-vip_appvfbl.mot` and are destined to be programmed at memory address 0xC000A in the VIP MCU memory space and appear to be part of the application image spanning addresses 0xC0000 through 0xFFFFF. By modifying these strings, it is possible to determine whether the image is validated before being programmed into the internal memory.

The original S-record file was manipulated to change one string to produce a visible indication of different firmware, and the resulting image was programmed back to the VIP MCU:

Using the same command for version check, it is possible to confirm the tampered image was accepted:

In a more global sense, this allows an attacker to pivot from a compromised application SoC running Linux to the VIP MCU by installing a crafted firmware version and subsequently gaining direct access to the connected CAN busses of the vehicle.

# Exploitation

Exploitation of the OS command injection vulnerabilities is relatively straightforward. All the attacker needs to do is create a file on a FAT32-formatted USB mass storage device where the name will contain the OS commands to be executed. The filename must end with `.up` for it to be recognized by the software update handling code. While all three command injection vulnerabilities are exploited via the file name, the easiest one to exploit is by far the ZDI-CAN-23420 as there are no specific exploitation requirements such as validity of the crafted update file.

Exploitation of the described command injection vulnerabilities can be further facilitated by the fact that software update installation can be triggered automatically upon connecting a USB mass storage device; this is a “known feature” which is activated by simply placing an empty file named `jci-autoupdate` on the storage device.

Once the initial compromise is achieved, the attacker can gain persistence through manipulation of the root file system, such as installing backdoored system components which will persist across system restarts.

Furthermore, the attacker can move laterally and install a specially crafted VIP microcontroller software allowing unfettered access to vehicle networks, potentially impacting vehicle operation and safety. Specific impacts (what could be controlled and how) were not investigated during this research effort.

# Associated Risks for Users/Owners

In the lab environment, the entire attack chain – from plugging in a USB drive to installing a crafted update on the VIP – takes only a few minutes. Currently, there is no reason to believe a similar attack will take significantly more time against a unit installed in a car. This means that the vehicle can be compromised while, for example, it is being handled by a valet, during a ride share, or via USB malware. Naturally, the same can be done in a shop environment where there are no significant time pressures.

The CMU can then be compromised and “enhanced” to, for example, attempt to compromise any connected device in targeted attacks that can result in DoS, bricking, ransomware, safety compromise, etc.

# Conclusions

This research effort and its output highlighted the fact that high-impact vulnerabilities can be found even in a very mature automotive product that has been on the market for a number of years and with a long history of security fixes. To date, these vulnerabilities remain unpatched by the vendor.

There has been a lot of focus and discussion recently in the security community about the importance of language security and “memory safe” languages. This set of real-world vulnerabilities is a good reminder that focusing on only one bug class or employing one set of security tools (static code analysis, for instance) does not result in deployed systems being secure. In this case, C was used to develop the firmware, but it is an irrelevant fact considering the nature of the discovered vulnerabilities. Any ‘memory-safe’ firmware would have also been compromised in this instance.

While the SQL and OS command injection issues represent programming errors, the remaining vulnerabilities are security design flaws. It is paramount that system integrity is ensured at all runtime stages and for all components. Only then is it possible to guarantee the downstream security properties of languages and their compilers.

In summary, it is important to consider the whole system security and security-test complete production systems in all their operational modes.

You can find me on Mastodon at @infosecdj, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
