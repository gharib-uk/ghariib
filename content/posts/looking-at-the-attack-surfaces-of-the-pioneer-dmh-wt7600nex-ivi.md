---
title: "Looking at the Attack Surfaces of the Pioneer DMH-WT7600NEX IVI"
date: 2025-01-22
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

For the upcoming Pwn2Own Automotive contest, a total of four in-vehicle infotainment (IVI) head units have been selected as targets. One of these is the single-DIN Pioneer DMH-WT7600NEX. This unit offers a variety of functionality, such as wired and wireless Android Auto and Apple CarPlay, USB media playback, and more.

This blog post aims to detail some of the attack surfaces of the DMH-WT7600NEX unit as well as provide information on how to extract the software running on this unit for further vulnerability research.

**Software Extraction**

The initial effort to locate a serial console in the hope of easy software extraction bore no fruit. This left only a handful of options:

·      Work with the software update package instead. However, the package was found to be encrypted, making this approach a dead end initially; more on that below.  
·      Attempt to desolder the eMMC chip and dump its contents using a programmer. This necessitates reballing and resoldering the eMMC chip, which is risky without proper SMD rework equipment.  
·      Attempt to extract eMMC contents in-system. This does not require any SMD rework, but the signal locations must be known, and the system must be powered and held in reset while dumping is in progress.

The researchers chose the last option. Connecting to the eMMC chip could be performed via (thankfully labeled) test points on the board. The missing MMC\_CLK signal was probed for using an oscilloscope; here is where it was found after numerous attempts.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/baf98dfe-7ca2-44c5-b34f-c820e1ffd089/Picture1.jpg?format=1000w)

In addition to that, the main SoC was held in reset by pulling the test point labelled RSTN to ground via a 220 Ohm resistor and a switch.

Note that when the SoC is held in reset, the 3.3V power line is cycled periodically by some other component, which powers off the eMMC chip. This is likely some watchdog component attempting to bring the system out of a hung state. Finding that component and persuading it not to do that was deemed too time consuming, and the 3.3V power rail was instead powered directly through a bench power supply.

**Data at Rest**

After the eMMC chip was successfully “backed up,” it was time for Trend ZDI researchers to have a look at the 8 gigabytes of its contents. The image was found to sport a GPT partition table, with the following partitions defined after mounting the image via the loopback interface on a test system:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/01b5e4d7-884f-4bba-922c-eb27284c7e08/Screenshot+2025-01-16+at+10.46.48+PM.png?format=1000w)

There are two sets of bootable images, consisting of the header, boot, system, dtb, hirtos, bootloader, chips, and backup partitions. It is likely this is to safeguard against failed software updates, so there is a known good set of bootable images. Let’s have a closer look at what is contained in each partition:

·      The header partition contains what looks like to be a description of other partitions in the set.  
·      The bootloader partition contains the bootloader as described, which seems to be a version of fastboot.  
·      The boot partition contains the Android/Linux kernel version 3.18.24. 
·      The dtb partition contains the DTB blob as described.  
·      The system partition contains the root file system.  
·      The hirtos partition contains a firmware image with ARM instructions. The exact purpose of this code is not currently known. The image consists of several chunks of code/data; some of it is obvious ARM code while others appear to be bitmap images. The following string was found inside the first chunk: “T-Monitor/triton\_TCC897x Version 2.01.00” This suggests the code is to be executed on the main SoC but likely on a separate core.  
·      The chips partition contains the firmware for the GNSS daughter board.  
·      The backup partition contains some kind of binary data, rather sparsely organized.

Interestingly enough, the system itself appears to be a Linux-based one; none of typical Android infrastructure could be located there. All the custom software is concentrated in /usr/local/ subdirectories.

**Software Updates**

Obtaining an image of the code running on the device allowed a second look at the software update format. The latest update file can be obtained from the manufacturer; unfortunately, they do not seem to list previous versions. This is justified, as downgrading the software is not officially supported anyway—as the team found out firsthand.

The software update package is structured like this:

·      A header of 0x100 bytes describing the file, specifically the header size and the total size of the image, software version in this update, plus which model the update is for.  
·      An RSA signature block of 0x100 bytes, which can be verified by a certain public key hardcoded in the software. The signature covers the described header only.  
·      An RSA signature block of 0x100 bytes, which can be decrypted by the same key, and which carries an AES-256 key instead of the digest.  
·      Update data, encrypted with AES-256-CBC using the all-zero IV. This decrypts into a gzipped “raw” update image.

The raw update image in turn consists of headers very similar to what can be found in the header partitions followed by a series of images for each partition mentioned in the headers. The image(s) can be processed further to extract the content of interest like the root file system.

**Serial Console**

Armed with some knowledge of the unit’s software, it was time to revisit the search for the serial console.

By studying the contents of the bootloader partitions, Trend ZDI researchers discovered the bootloader may use values from the backup partitions to decide which values to pass via the \`console\` and \`login\` kernel parameters, among other things. Specifically, the sector at byte offset 0x800800 contains that data. The format which this data is in can be reverse engineered both from the bootloader and the NPSystemDebug class implementation. 

Notably, it appears that manipulation of these values could be performed via the UI as the code flow can be traced all the way to the \`UI\_UIEB\_MM\_99\_018\` class which implements two buttons changing the state of the values. However, at the moment of writing it was unknown how to reach that specific UI screen.

Thus, the direct manipulation of the flags was chosen instead. The contents of the backup partition were altered to enable both serial console and the login prompt. After probing the board connectors for any semblance of serial data, it was discovered on CN3603 pin 7.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d6f04dd7-d541-4586-97c1-f6cfd67c6abf/Picture2.jpg?format=1000w)

Connecting a UART-to-USB dongle to that pin confirmed that indeed, console output is present, as well as the login prompt. Only three signals are routed to that connector; however, the RX signal was not immediately identified among those.

Studying the bottom layer of the board showed a single installed passive among several missing ones; this was one resistor pulling up a line otherwise not connected to any connector pin. Probing that line for being the missing RX line resulted in a success. Likely, one of the missing passives should connect that line to a connector pin.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c409d9a0-31cc-4290-b87f-0330b245d067/Picture3.jpg?format=1000w)

Now it was possible to communicate with the device—and log in locally. Having console access is always a big boon in vulnerability research.

**Bluetooth**

The vendor lists the following supported Bluetooth profiles:

·      Advanced Audio Distribution Profile (A2DP)  
·      Hands-Free Profile  
·      Serial Port Profile  
·      Audio/Video Remote Control Profile (AVRCP) v1.6

Given the rich history of bugs in Bluetooth-related functionality, this could be an interesting attack vector 

**Wi-Fi**

The unit can be set up in both the client and access point modes for Wi-Fi.

When in the AP mode, the unit allows using the WPS setup in addition to entering the PSK. This could potentially be an interesting attack angle as WPS flows were historically weak to attacks.

After connecting to the unit in AP mode and running a network scan, the following TCP ports were found to be open: 5000, 38000, 38001, 42000, 43000, and 60000. Nmap script scan only showed that port 5000 uses TLS with a self-signed certificate; other services were not recognized. Using the console access, it is possible to map out the open ports to the corresponding processes (only ports allowed through the iptables are shown here for brevity):

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c9a0a26b-0891-40fa-8711-8c6526370d34/Screenshot+2025-01-16+at+10.45.57+PM.png?format=1000w)

Given the abundance of what looks like non-standard services, Wi-Fi connectivity presents a potenially rewarding target for vulnerability research.

**USB**

The unit is equipped with a single USB-C port that provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes are 

·      MP3  
·      WMA  
·      WAV  
·      AAC  
·      FLAC  
·      DSD

The unit also supports video playback with the following formats listed as supported:

·      AVI  
·      MPEG  
·      DivX  
·      MP4  
·      3GP  
·      MKV  
·      FLV  
·      WMV/ASF  
·      M4V  
·      H.263, H.264

In addition, it is also possible to view images in BMP, JPEG, and PNG formats. Parsing complex file formats is error-prone and has been a rich source of exploitable bugs since time immemorial.

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party application to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned Wi-Fi network to establish a high-bandwidth channel for data to be sent and received. When connecting using a USB cable, the Wi-Fi network isn't used by Android Auto or Apple CarPlay and can be disabled in Settings. As evidenced above, the \`Media\` process is likely responsible for handling both.

Pwn2Own Automotive 2024 didn’t see any entries that leveraged Android Auto or Apple CarPlay functionality to compromise a head unit. We will have to wait and see if Pwn2Own Automotive 2025 does!

**Summary**

We hope that this blog post has provided enough information about the Pioneer DMH-WT7600NEX attack surface to guide vulnerability research. Not every attack surface has been mentioned, and we encourage researchers to investigate further. 

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Mastodon at @InfoSecDJ, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

For the upcoming Pwn2Own Automotive contest, a total of four in-vehicle infotainment (IVI) head units have been selected as targets. One of these is the single-DIN Pioneer DMH-WT7600NEX. This unit offers a variety of functionality, such as wired and wireless Android Auto and Apple CarPlay, USB media playback, and more.

This blog post aims to detail some of the attack surfaces of the DMH-WT7600NEX unit as well as provide information on how to extract the software running on this unit for further vulnerability research.

**Software Extraction**

The initial effort to locate a serial console in the hope of easy software extraction bore no fruit. This left only a handful of options:

·      Work with the software update package instead. However, the package was found to be encrypted, making this approach a dead end initially; more on that below.  
·      Attempt to desolder the eMMC chip and dump its contents using a programmer. This necessitates reballing and resoldering the eMMC chip, which is risky without proper SMD rework equipment.  
·      Attempt to extract eMMC contents in-system. This does not require any SMD rework, but the signal locations must be known, and the system must be powered and held in reset while dumping is in progress.

The researchers chose the last option. Connecting to the eMMC chip could be performed via (thankfully labeled) test points on the board. The missing MMC\_CLK signal was probed for using an oscilloscope; here is where it was found after numerous attempts.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/baf98dfe-7ca2-44c5-b34f-c820e1ffd089/Picture1.jpg?format=1000w)

In addition to that, the main SoC was held in reset by pulling the test point labelled RSTN to ground via a 220 Ohm resistor and a switch.

Note that when the SoC is held in reset, the 3.3V power line is cycled periodically by some other component, which powers off the eMMC chip. This is likely some watchdog component attempting to bring the system out of a hung state. Finding that component and persuading it not to do that was deemed too time consuming, and the 3.3V power rail was instead powered directly through a bench power supply.

**Data at Rest**

After the eMMC chip was successfully “backed up,” it was time for Trend ZDI researchers to have a look at the 8 gigabytes of its contents. The image was found to sport a GPT partition table, with the following partitions defined after mounting the image via the loopback interface on a test system:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/01b5e4d7-884f-4bba-922c-eb27284c7e08/Screenshot+2025-01-16+at+10.46.48+PM.png?format=1000w)

There are two sets of bootable images, consisting of the header, boot, system, dtb, hirtos, bootloader, chips, and backup partitions. It is likely this is to safeguard against failed software updates, so there is a known good set of bootable images. Let’s have a closer look at what is contained in each partition:

·      The header partition contains what looks like to be a description of other partitions in the set.  
·      The bootloader partition contains the bootloader as described, which seems to be a version of fastboot.  
·      The boot partition contains the Android/Linux kernel version 3.18.24. 
·      The dtb partition contains the DTB blob as described.  
·      The system partition contains the root file system.  
·      The hirtos partition contains a firmware image with ARM instructions. The exact purpose of this code is not currently known. The image consists of several chunks of code/data; some of it is obvious ARM code while others appear to be bitmap images. The following string was found inside the first chunk: “T-Monitor/triton\_TCC897x Version 2.01.00” This suggests the code is to be executed on the main SoC but likely on a separate core.  
·      The chips partition contains the firmware for the GNSS daughter board.  
·      The backup partition contains some kind of binary data, rather sparsely organized.

Interestingly enough, the system itself appears to be a Linux-based one; none of typical Android infrastructure could be located there. All the custom software is concentrated in /usr/local/ subdirectories.

**Software Updates**

Obtaining an image of the code running on the device allowed a second look at the software update format. The latest update file can be obtained from the manufacturer; unfortunately, they do not seem to list previous versions. This is justified, as downgrading the software is not officially supported anyway—as the team found out firsthand.

The software update package is structured like this:

·      A header of 0x100 bytes describing the file, specifically the header size and the total size of the image, software version in this update, plus which model the update is for.  
·      An RSA signature block of 0x100 bytes, which can be verified by a certain public key hardcoded in the software. The signature covers the described header only.  
·      An RSA signature block of 0x100 bytes, which can be decrypted by the same key, and which carries an AES-256 key instead of the digest.  
·      Update data, encrypted with AES-256-CBC using the all-zero IV. This decrypts into a gzipped “raw” update image.

The raw update image in turn consists of headers very similar to what can be found in the header partitions followed by a series of images for each partition mentioned in the headers. The image(s) can be processed further to extract the content of interest like the root file system.

**Serial Console**

Armed with some knowledge of the unit’s software, it was time to revisit the search for the serial console.

By studying the contents of the bootloader partitions, Trend ZDI researchers discovered the bootloader may use values from the backup partitions to decide which values to pass via the \`console\` and \`login\` kernel parameters, among other things. Specifically, the sector at byte offset 0x800800 contains that data. The format which this data is in can be reverse engineered both from the bootloader and the NPSystemDebug class implementation. 

Notably, it appears that manipulation of these values could be performed via the UI as the code flow can be traced all the way to the \`UI\_UIEB\_MM\_99\_018\` class which implements two buttons changing the state of the values. However, at the moment of writing it was unknown how to reach that specific UI screen.

Thus, the direct manipulation of the flags was chosen instead. The contents of the backup partition were altered to enable both serial console and the login prompt. After probing the board connectors for any semblance of serial data, it was discovered on CN3603 pin 7.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d6f04dd7-d541-4586-97c1-f6cfd67c6abf/Picture2.jpg?format=1000w)

Connecting a UART-to-USB dongle to that pin confirmed that indeed, console output is present, as well as the login prompt. Only three signals are routed to that connector; however, the RX signal was not immediately identified among those.

Studying the bottom layer of the board showed a single installed passive among several missing ones; this was one resistor pulling up a line otherwise not connected to any connector pin. Probing that line for being the missing RX line resulted in a success. Likely, one of the missing passives should connect that line to a connector pin.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c409d9a0-31cc-4290-b87f-0330b245d067/Picture3.jpg?format=1000w)

Now it was possible to communicate with the device—and log in locally. Having console access is always a big boon in vulnerability research.

**Bluetooth**

The vendor lists the following supported Bluetooth profiles:

·      Advanced Audio Distribution Profile (A2DP)  
·      Hands-Free Profile  
·      Serial Port Profile  
·      Audio/Video Remote Control Profile (AVRCP) v1.6

Given the rich history of bugs in Bluetooth-related functionality, this could be an interesting attack vector 

**Wi-Fi**

The unit can be set up in both the client and access point modes for Wi-Fi.

When in the AP mode, the unit allows using the WPS setup in addition to entering the PSK. This could potentially be an interesting attack angle as WPS flows were historically weak to attacks.

After connecting to the unit in AP mode and running a network scan, the following TCP ports were found to be open: 5000, 38000, 38001, 42000, 43000, and 60000. Nmap script scan only showed that port 5000 uses TLS with a self-signed certificate; other services were not recognized. Using the console access, it is possible to map out the open ports to the corresponding processes (only ports allowed through the iptables are shown here for brevity):

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c9a0a26b-0891-40fa-8711-8c6526370d34/Screenshot+2025-01-16+at+10.45.57+PM.png?format=1000w)

Given the abundance of what looks like non-standard services, Wi-Fi connectivity presents a potenially rewarding target for vulnerability research.

**USB**

The unit is equipped with a single USB-C port that provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes are 

·      MP3  
·      WMA  
·      WAV  
·      AAC  
·      FLAC  
·      DSD

The unit also supports video playback with the following formats listed as supported:

·      AVI  
·      MPEG  
·      DivX  
·      MP4  
·      3GP  
·      MKV  
·      FLV  
·      WMV/ASF  
·      M4V  
·      H.263, H.264

In addition, it is also possible to view images in BMP, JPEG, and PNG formats. Parsing complex file formats is error-prone and has been a rich source of exploitable bugs since time immemorial.

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party application to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned Wi-Fi network to establish a high-bandwidth channel for data to be sent and received. When connecting using a USB cable, the Wi-Fi network isn't used by Android Auto or Apple CarPlay and can be disabled in Settings. As evidenced above, the \`Media\` process is likely responsible for handling both.

Pwn2Own Automotive 2024 didn’t see any entries that leveraged Android Auto or Apple CarPlay functionality to compromise a head unit. We will have to wait and see if Pwn2Own Automotive 2025 does!

**Summary**

We hope that this blog post has provided enough information about the Pioneer DMH-WT7600NEX attack surface to guide vulnerability research. Not every attack surface has been mentioned, and we encourage researchers to investigate further. 

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Mastodon at @InfoSecDJ, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
