---
title: "Detailing the Attack Surfaces of the WolfBox E40 EV Charger"
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

The WolfBox E40 is a Level 2 electric vehicle charge station designed for residential home use. Its hardware has a minimal user interface, providing a Bluetooth Low Energy (BLE) interface for configuration and an NFC reader for user authentication. Typical for this class of devices, the appliance employs a mobile application for the owner's installation and regular operation of the equipment.

At the moment of writing, the software versions were reported as follows:

·      Main module: 3.1.17  
·      MCU module: 1.2.6  
·      Mobile application: 1.0.3

# WolfBox EV Mobile Application

The manufacturer distributes one application to configure and maintain the device. The application, named WolfBox EV, is available for both Android and iOS users.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/66eeafbe-13b1-48bc-866b-a6b3a402248f/Picture1.png?format=1000w)

<figcaption>

_Figure 1 - Application Start Screen_

</figcaption>

</figure>

Interestingly, the application apparently allows pairing and managing with more than the expected EV charger devices; when attempting to add a new device, a list is presented that includes several other ostensibly known and accepted device types, such as lamps, switches, doorbells, mosquito repellent heaters, irrigators and battery packs, to name a few.

While Trend Micro Zero Day Initiative (ZDI) did not thoroughly investigate this application for vulnerabilities or other bugs, problems in mobile applications have been used by threat actors in the past and represent a significant attack surface. Even though the mobile application itself is out of scope for the Pwn2Own Automotive contest, the research community ought to thoroughly review it.

# WolfBox E40 Hardware Analysis

Trend ZDI researchers have analyzed the discrete hardware components found in the device. The device itself is comprised of several printed circuit boards (PCBs) hosting the system; the “main PCB” contains power and communications electronics, while the human-machine interface is spread over three more PCBs, carrying correspondingly the LCD display, the NFC reader, and the LEDs.

Below is a list of notable parts on the main PCB:

·      GigaDevice GD32F307VET6 (U12), ARM Cortex-M4 with 512K Flash  
·      Tuya CBU-IPEX WiFi/BLE radio (U11)  
·      Atmel ATMLH242 04CM (U13), likely to be an AT24C04 serial EEPROM  
·      Winbond 25Q64JVSIQ (U14), 64MB serial Flash

On the board itself, the two terminal blocks on the left couple the device to both the power inlet from the grid and to the charging cable. The two relay units control the flow of power, with current transformers inserted inline. On the right-hand side at the top, the board houses a low-current switching power supply for its finer electronics. Below it, there is a 1.0F supercap, the MCU, and the Wi-Fi/BLE communications module. Finally, there is an undocumented DIP switch and a host of connectors with nothing connected to them except the last one; the single white wire goes to the CP line of the charging plug.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/22d7a38d-2da4-46f7-b881-7a6bbb5c1af8/Picture2.jpg?format=1000w)

<figcaption>

_Figure 2 - WolfBox power/processing board—top_

</figcaption>

</figure>

Of special note is the CN17 connector located right beside the MCU, which is used for SWD debugging.

On the bottom side, the only component of note is the serial Flash chip.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/9628bc30-e321-4eea-8021-c105fcf164b8/Picture3.jpg?format=1000w)

<figcaption>

_Figure 3 - WolfBox power/processing board—bottom_

</figcaption>

</figure>

Finally, the main PCB is covered in conformal coating. Trend Micro researchers discovered the coating to be soluble in acetone, allowing for easy removal where needed to facilitate probing.

As a final note, Tuya provides some documentation regarding the wire protocol between their modules and system MCUs.

The display and LED boards do not present much in terms of interesting components. The NFC board, however, carries a SmartLink SL2823 NFC interface chip coupled to an anonymous part ostensibly used to translate between the SmartLink chip and the rest of the system.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4f567115-8510-4734-93c2-a04eecbc4bfd/Picture4.jpg?format=1000w)

<figcaption>

_Figure 4 - WolfBox NFC board_

</figcaption>

</figure>

In terms of potential attack surface and vectors, the outermost exposed interfaces are handled by the communications module, which then passes messages to the main MCU that, in turn, controls the power and human interfaces. As such, compromising the communications module may be a required step.

# Firmware Extraction

The manufacturer, WolfBox, does not appear to provide any way to download firmware images or update packages for the device.

Thus, upon the discovery of CN17, an attempt was made to extract the firmware from the GigaDevice MCU. By connecting a ST-Link V2 dongle, the MCU was detected successfully by the ST-Link software, allowing its on-chip Flash memory to be read. This brought an unexpected end to the MCU firmware extraction journey.

Extracting firmware from the communications module proved to be a little more involved. The documentation for the module SoC – BK7231N – could be found on the Tuya website. It is said the chip has either 2MB or 4MB of Flash memory. Further research uncovered a tool used by the open-source community to program custom firmware to Tuya devices, among others. Trend ZDI researchers used the tool to read a Flash image out from the module SoC. This required some soldering: a USB-to-UART dongle was used to communicate with the module with the following connections:

·      Module TX on pin 15 -> UART bridge RX (white)  
·      Module RX on pin 16 -> UART bridge TX (green)  
·      Module ground on pin 16 -> UART bridge ground (black)

In addition, the module CEN (reset) signal on pin 18 was also used, and power was supplied from a bench power supply via CN17.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/85719bfc-5720-4b4f-b1dc-a198c951739c/Picture5.jpg?format=1000w)

<figcaption>

_Figure 5 - A view of the USB-to-UART dongle soldered in place_

</figcaption>

</figure>

After all connections were made, the following procedure was used to extract the flash contents:

1. Press and hold the SW2 button; this will hold the main MCU in reset, preventing it from interfering with communications.
2. Supply power to the board.
3. Start the `uartprogram` downloader script. The script will get stuck at `Read Getting Bus...`.
4. Short the CEN signal to ground for a bit. The RF shield on the module can be used for that. This will apparently reboot the module into a state where the script can interact with it.
5. Check if readout has started on the console. If not, retry step 4 until successful. The script may need to be restarted.
6. Power off the board after the download has been completed.

Here is what it should look like on the console, along with the command line parameters that were used:

# Firmware Analysis

Without going too deep, Trend ZDI researchers performed an initial analysis of the GigaDevice firmware.

As is typical for MCU-based devices, the firmware is one solid blob without any discernible structure such as a filesystem. Upon analyzing ASCII strings present in the dumped firmware image, it became obvious the firmware is built on top of uC/OS II, an embedded real-time operating system (RTOS). Multiple references to mBedTLS version 2.16.4 are also visible. Alongside that, there are multiple AT commands. This suggests the MCU handles communications via a missing wireless module. However, in the present configuration, it would seem the bulk of handling Wi-Fi, BLE, TCP/IP, and TLS falls upon the communications module.

Unfortunately, there were not too many useful strings encountered in the communications module firmware. Most were JSON data which looked like logging records of some sort. The image itself appears to be partitioned as follows, with spans of unprogrammed memory in between:

·      `0x000000`: medium entropy; machine code. This is the bootloader.  
·      `0x011000`: medium entropy; machine code. This is the application.  
·      `0x12B000`: high entropy; unknown. This appears to be encrypted in ECB mode.  
·      `0x1CF000`: high entropy; unknown. This appears to be encrypted in ECB mode.  
·      `0x1D0000`: low entropy; marked as “TLV”.  
·      `0x1D2000`: low entropy; this could be some kind of filesystem. This is where log-like JSON data was identified.  
·      `0x1ED000`: high entropy; unknown. This appears to be encrypted in ECB mode.

Worth noting: Tuya provides SDK source code access for BK7231N, which may prove useful during reverse engineering and vulnerability research efforts. That said, Trend ZDI did not verify whether there were any changes made for WolfBox products.

# Bluetooth Low Energy (BLE) Analysis

Using a BLE scanning tool, the Trend ZDI researchers observed the following Bluetooth LE endpoints on the WolfBox E40.

When powered on, the device is visible under the name \`TUYA\_\`. The device exposes a single service with three characteristics.

It is reasonable to assume this is what the mobile application uses to discover the device. Additional information can be likely obtained by reverse engineering the mobile application.

# Network Traffic Analysis

The charger can connect to a local Wi-Fi network. Trend ZDI researchers connected one device to investigate the network-side attack surface exposed by the device.

During the initial setup, the charger reached out to `h3-eu.iot-dns.com` (alias of `a3480a15a4710bb9d.awsglobalaccelerator.com`, IP address 15.197.184.59) via HTTPS. This was followed by TCP port 443 connections to IP address 18.195.249.137, which had no prior DNS queries, and then again followed by a connection to 18.185.146.33 (AWS), TCP port 8886.

UDP communication over port 7000 was also observed. Initially, the phone sent out packets to this port to broadcast addresses and received a response from the device’s address. The packet contents are, unfortunately, encrypted in some way.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/17f089ca-e5fc-4fbb-8516-c86a37f7a95d/Picture6.png?format=1000w)

<figcaption>

_Figure 6 - Observed network communications_

</figcaption>

</figure>

The phone also reached out to TCP port 6668 on the device. The observed traffic followed the same format with mostly encrypted payloads.

After the app checked for software updates and was allowed to update to the latest version, the device queried DNS for `fireware-ttls.tuyaeu.com` (alias of `tuya-fireware-update-8b783aab86e29e3a.elb.eu-central-1.amazonaws.com`, IP 18.197.251.244) and connected to TCP port 2443 (protected by TLS) and proceeded to download the update. The device rebooted immediately after the download.

Having completed the update, the device queried DNS for `m3.tuyaeu.com` (alias of `out-mqtt-tytls-43c09399a95b9ab8.elb.eu-central-1.amazonaws.com`, IP 18.185.146.33 – matching the prior IP) and connected to TCP port 8886 there. This was followed by a query for `a3.tuyaeu.com` (IP 18.195.249.137 – matching the prior IP) and a connection to TCP port 443 there as well.

The device kept TCP port 6668 open for connections over Wi-Fi. Of note, in-app actions related to device configuration (e.g., LED setup) resulted in communications over that connection.

Further research into the encrypted local communications pointed to this being a Tuya-specific protocol. Open-source implementations exist for it as well, such as the tuyapi library. This could be used as a good starting point in building a fuzzer for this protocol.

# Summary

While these may not be the only attack surfaces available on the WolfBox E40 unit, they represent the most likely avenues a threat actor may use to exploit the device. We’re excited to see what research is displayed in Tokyo during the Pwn2Own Automotive event. Stay tuned to the blog for attack surface reviews for other devices, and if you’re curious, you can see all the devices included in the contest.

Until then, you can find me on Mastodon at @infosecdj, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

The WolfBox E40 is a Level 2 electric vehicle charge station designed for residential home use. Its hardware has a minimal user interface, providing a Bluetooth Low Energy (BLE) interface for configuration and an NFC reader for user authentication. Typical for this class of devices, the appliance employs a mobile application for the owner's installation and regular operation of the equipment.

At the moment of writing, the software versions were reported as follows:

·      Main module: 3.1.17  
·      MCU module: 1.2.6  
·      Mobile application: 1.0.3

# WolfBox EV Mobile Application

The manufacturer distributes one application to configure and maintain the device. The application, named WolfBox EV, is available for both Android and iOS users.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/66eeafbe-13b1-48bc-866b-a6b3a402248f/Picture1.png?format=1000w)

<figcaption>

_Figure 1 - Application Start Screen_

</figcaption>

</figure>

Interestingly, the application apparently allows pairing and managing with more than the expected EV charger devices; when attempting to add a new device, a list is presented that includes several other ostensibly known and accepted device types, such as lamps, switches, doorbells, mosquito repellent heaters, irrigators and battery packs, to name a few.

While Trend Micro Zero Day Initiative (ZDI) did not thoroughly investigate this application for vulnerabilities or other bugs, problems in mobile applications have been used by threat actors in the past and represent a significant attack surface. Even though the mobile application itself is out of scope for the Pwn2Own Automotive contest, the research community ought to thoroughly review it.

# WolfBox E40 Hardware Analysis

Trend ZDI researchers have analyzed the discrete hardware components found in the device. The device itself is comprised of several printed circuit boards (PCBs) hosting the system; the “main PCB” contains power and communications electronics, while the human-machine interface is spread over three more PCBs, carrying correspondingly the LCD display, the NFC reader, and the LEDs.

Below is a list of notable parts on the main PCB:

·      GigaDevice GD32F307VET6 (U12), ARM Cortex-M4 with 512K Flash  
·      Tuya CBU-IPEX WiFi/BLE radio (U11)  
·      Atmel ATMLH242 04CM (U13), likely to be an AT24C04 serial EEPROM  
·      Winbond 25Q64JVSIQ (U14), 64MB serial Flash

On the board itself, the two terminal blocks on the left couple the device to both the power inlet from the grid and to the charging cable. The two relay units control the flow of power, with current transformers inserted inline. On the right-hand side at the top, the board houses a low-current switching power supply for its finer electronics. Below it, there is a 1.0F supercap, the MCU, and the Wi-Fi/BLE communications module. Finally, there is an undocumented DIP switch and a host of connectors with nothing connected to them except the last one; the single white wire goes to the CP line of the charging plug.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/22d7a38d-2da4-46f7-b881-7a6bbb5c1af8/Picture2.jpg?format=1000w)

<figcaption>

_Figure 2 - WolfBox power/processing board—top_

</figcaption>

</figure>

Of special note is the CN17 connector located right beside the MCU, which is used for SWD debugging.

On the bottom side, the only component of note is the serial Flash chip.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/9628bc30-e321-4eea-8021-c105fcf164b8/Picture3.jpg?format=1000w)

<figcaption>

_Figure 3 - WolfBox power/processing board—bottom_

</figcaption>

</figure>

Finally, the main PCB is covered in conformal coating. Trend Micro researchers discovered the coating to be soluble in acetone, allowing for easy removal where needed to facilitate probing.

As a final note, Tuya provides some documentation regarding the wire protocol between their modules and system MCUs.

The display and LED boards do not present much in terms of interesting components. The NFC board, however, carries a SmartLink SL2823 NFC interface chip coupled to an anonymous part ostensibly used to translate between the SmartLink chip and the rest of the system.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4f567115-8510-4734-93c2-a04eecbc4bfd/Picture4.jpg?format=1000w)

<figcaption>

_Figure 4 - WolfBox NFC board_

</figcaption>

</figure>

In terms of potential attack surface and vectors, the outermost exposed interfaces are handled by the communications module, which then passes messages to the main MCU that, in turn, controls the power and human interfaces. As such, compromising the communications module may be a required step.

# Firmware Extraction

The manufacturer, WolfBox, does not appear to provide any way to download firmware images or update packages for the device.

Thus, upon the discovery of CN17, an attempt was made to extract the firmware from the GigaDevice MCU. By connecting a ST-Link V2 dongle, the MCU was detected successfully by the ST-Link software, allowing its on-chip Flash memory to be read. This brought an unexpected end to the MCU firmware extraction journey.

Extracting firmware from the communications module proved to be a little more involved. The documentation for the module SoC – BK7231N – could be found on the Tuya website. It is said the chip has either 2MB or 4MB of Flash memory. Further research uncovered a tool used by the open-source community to program custom firmware to Tuya devices, among others. Trend ZDI researchers used the tool to read a Flash image out from the module SoC. This required some soldering: a USB-to-UART dongle was used to communicate with the module with the following connections:

·      Module TX on pin 15 -> UART bridge RX (white)  
·      Module RX on pin 16 -> UART bridge TX (green)  
·      Module ground on pin 16 -> UART bridge ground (black)

In addition, the module CEN (reset) signal on pin 18 was also used, and power was supplied from a bench power supply via CN17.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/85719bfc-5720-4b4f-b1dc-a198c951739c/Picture5.jpg?format=1000w)

<figcaption>

_Figure 5 - A view of the USB-to-UART dongle soldered in place_

</figcaption>

</figure>

After all connections were made, the following procedure was used to extract the flash contents:

1. Press and hold the SW2 button; this will hold the main MCU in reset, preventing it from interfering with communications.
2. Supply power to the board.
3. Start the `uartprogram` downloader script. The script will get stuck at `Read Getting Bus...`.
4. Short the CEN signal to ground for a bit. The RF shield on the module can be used for that. This will apparently reboot the module into a state where the script can interact with it.
5. Check if readout has started on the console. If not, retry step 4 until successful. The script may need to be restarted.
6. Power off the board after the download has been completed.

Here is what it should look like on the console, along with the command line parameters that were used:

# Firmware Analysis

Without going too deep, Trend ZDI researchers performed an initial analysis of the GigaDevice firmware.

As is typical for MCU-based devices, the firmware is one solid blob without any discernible structure such as a filesystem. Upon analyzing ASCII strings present in the dumped firmware image, it became obvious the firmware is built on top of uC/OS II, an embedded real-time operating system (RTOS). Multiple references to mBedTLS version 2.16.4 are also visible. Alongside that, there are multiple AT commands. This suggests the MCU handles communications via a missing wireless module. However, in the present configuration, it would seem the bulk of handling Wi-Fi, BLE, TCP/IP, and TLS falls upon the communications module.

Unfortunately, there were not too many useful strings encountered in the communications module firmware. Most were JSON data which looked like logging records of some sort. The image itself appears to be partitioned as follows, with spans of unprogrammed memory in between:

·      `0x000000`: medium entropy; machine code. This is the bootloader.  
·      `0x011000`: medium entropy; machine code. This is the application.  
·      `0x12B000`: high entropy; unknown. This appears to be encrypted in ECB mode.  
·      `0x1CF000`: high entropy; unknown. This appears to be encrypted in ECB mode.  
·      `0x1D0000`: low entropy; marked as “TLV”.  
·      `0x1D2000`: low entropy; this could be some kind of filesystem. This is where log-like JSON data was identified.  
·      `0x1ED000`: high entropy; unknown. This appears to be encrypted in ECB mode.

Worth noting: Tuya provides SDK source code access for BK7231N, which may prove useful during reverse engineering and vulnerability research efforts. That said, Trend ZDI did not verify whether there were any changes made for WolfBox products.

# Bluetooth Low Energy (BLE) Analysis

Using a BLE scanning tool, the Trend ZDI researchers observed the following Bluetooth LE endpoints on the WolfBox E40.

When powered on, the device is visible under the name \`TUYA\_\`. The device exposes a single service with three characteristics.

It is reasonable to assume this is what the mobile application uses to discover the device. Additional information can be likely obtained by reverse engineering the mobile application.

# Network Traffic Analysis

The charger can connect to a local Wi-Fi network. Trend ZDI researchers connected one device to investigate the network-side attack surface exposed by the device.

During the initial setup, the charger reached out to `h3-eu.iot-dns.com` (alias of `a3480a15a4710bb9d.awsglobalaccelerator.com`, IP address 15.197.184.59) via HTTPS. This was followed by TCP port 443 connections to IP address 18.195.249.137, which had no prior DNS queries, and then again followed by a connection to 18.185.146.33 (AWS), TCP port 8886.

UDP communication over port 7000 was also observed. Initially, the phone sent out packets to this port to broadcast addresses and received a response from the device’s address. The packet contents are, unfortunately, encrypted in some way.

<figure>

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/17f089ca-e5fc-4fbb-8516-c86a37f7a95d/Picture6.png?format=1000w)

<figcaption>

_Figure 6 - Observed network communications_

</figcaption>

</figure>

The phone also reached out to TCP port 6668 on the device. The observed traffic followed the same format with mostly encrypted payloads.

After the app checked for software updates and was allowed to update to the latest version, the device queried DNS for `fireware-ttls.tuyaeu.com` (alias of `tuya-fireware-update-8b783aab86e29e3a.elb.eu-central-1.amazonaws.com`, IP 18.197.251.244) and connected to TCP port 2443 (protected by TLS) and proceeded to download the update. The device rebooted immediately after the download.

Having completed the update, the device queried DNS for `m3.tuyaeu.com` (alias of `out-mqtt-tytls-43c09399a95b9ab8.elb.eu-central-1.amazonaws.com`, IP 18.185.146.33 – matching the prior IP) and connected to TCP port 8886 there. This was followed by a query for `a3.tuyaeu.com` (IP 18.195.249.137 – matching the prior IP) and a connection to TCP port 443 there as well.

The device kept TCP port 6668 open for connections over Wi-Fi. Of note, in-app actions related to device configuration (e.g., LED setup) resulted in communications over that connection.

Further research into the encrypted local communications pointed to this being a Tuya-specific protocol. Open-source implementations exist for it as well, such as the tuyapi library. This could be used as a good starting point in building a fuzzer for this protocol.

# Summary

While these may not be the only attack surfaces available on the WolfBox E40 unit, they represent the most likely avenues a threat actor may use to exploit the device. We’re excited to see what research is displayed in Tokyo during the Pwn2Own Automotive event. Stay tuned to the blog for attack surface reviews for other devices, and if you’re curious, you can see all the devices included in the contest.

Until then, you can find me on Mastodon at @infosecdj, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
