---
title: "Detailing the Attack Surfaces of the Tesla Wall Connector EV Charger"
date: Tue, 17 Dec 2024 16:49:57 +0000
draft: false
type: posts
categories: 
- Blog post
---
# Detailing the Attack Surfaces of the Tesla Wall Connector EV Charger
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1734386127850-6ZI0GH3CC9IS3FLO34LU/technology-car-wheel-cable-vehicle-auto-884025-pxhere.com.jpg?format=1500w
<br/>

<br/>
The Tesla Wall Connector is a Level 2 electric vehicle charge station designed for use by residential home users. The device has a minimal user interface in its hardware, providing a Wi-Fi based interface for configuration and an NFC reader for user authentication. The device does not come with a dedicated mobile application out of the box; the initial configuration—which is rather minimal—is performed via the web interface. After that, the device suggests the Tesla One app is to be used for further configuration. [The Tesla app](https://www.tesla.com/en_gb/support/charging/wall-connector/tesla-app) allows connecting to the device but apparently can only be used to perform a few functions such as scheduled charging and viewing information about the device. It does not offer options for further configuration.

At the moment of writing, the latest software version was reported as 24.36.3.

Hardware Analysis
-----------------

Trend ZDI researchers have performed an analysis of the discrete hardware components found in the device. The device itself is comprised of several printed circuit boards (PCBs) hosting the system; the “main” PCB contains power and communications electronics while the smaller flex core PCB carries the minimalistic human interface in the form of several LEDs and the NFC reader circuitry.

Below is a list of notable parts on the main PCB:

·      U1 AD [ADE7854AACPZ](https://www.analog.com/en/products/ade7854a.html), a 3-phase energy measurement IC,  
·      U7 ST [STM32L431RCT6](https://www.st.com/en/microcontrollers-microprocessors/stm32l431rc.html), an ARM MCU with 256KB of Flash memory,  
·      U20 NXP 7102 3105, an unknown device, but likely to be [an I2C secure element](https://www.nxp.com/docs/en/data-sheet/A71CH.pdf?pspll=1),  
·      U21 AzureWave [AW-CU300](http://www.azurewave.com/img/wireless-modules/AW-CU300%20V2_DS_DF_V01_STD.pdf), a WLAN module,  
·      U27 GigaDevice 25Q128E, a 128MB serial Flash.

On the board itself, about half of the area is occupied by the power electronics, with low-current electronics being concentrated on the left in this photo. Of most interest, the STM32 MCU is at the top left in Figure one (below) and the comms module is at the middle left. The number of external connectors is kept at a minimum, with J3 in the middle providing connection for non-power signals in the charging cable.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/95a0a5b5-3e35-4166-8a81-adfd78a326a5/Picture1.jpg?format=1000w)

_Figure 1 - Tesla Wall Connector main PCB top_

The bottom side of this board carries a variety of small components and the energy-metering IC. The green connector on the bottom left has an unknown purpose.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4ec56b9b-b6ef-4a8f-a7a4-3be177e9b00b/Picture2.jpg?format=1000w)

_Figure 2 - Tesla Wall Connector main PCB bottom_

Notably, many signals appear to be broken out on unpopulated pin headers. Once the varnish is removed, connecting to any MCU pin should be easy.

There are multiple unpopulated footprints on the top side of the board. Of these, J1 and J7 immediately stand out. Trend ZDI researchers have determined that J1 is wired to the STM32 MCU and has a standard ARM JTAG/SWD 10-pin connector pinout. Dealing with J7 was considerably more complicated, but it seems reasonable to assume a similar pinout, if not identical.

Unfortunately, rather little information was found regarding the communications module used in this device. It appears to be produced by AzureWave Technologies, Inc. On their website, the AW-CU300 modules are listed as carrying a MW320 SoC which appears to reference [88MW320](https://www.nxp.com/docs/en/data-sheet/88MW320-88W322.pdf), a SoC designed by Marvell and then acquired by NXP. In terms of software, however, the only mention made is that the module uses “Marvell Smart Connect”. No downloads of binary images or sources are provided by AzureWave.

Trend ZDI researchers look forward to discussing with the contestants the role the U20 IC plays in the overall system security.

Firmware Extraction
-------------------

Extracting the STM32 firmware proceeded without many obstacles. As the J1 connector pinout is known, just connecting an ST-Link V2 dongle proved to be sufficient to dump out the whole contents of the on-chip flash.

Extracting the firmware of the communication module, however, turned out to be more challenging. After attempting multiple approaches, Trend ZDI researchers were unable to interact with the interface over the J7 connector to get any meaningful results; only once, the device returned an IDCODE of all zeros in the JTAG mode when using a pinout corresponding to the ARM 10-pin connector. The unavailability of JTAG seems to be consistent with documented behavior when the MW320 SoC is in its secured mode: in this case, the JTAG interface is expected to be disabled by default and will not be enabled by the boot ROM.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/ae4747b1-4744-40cf-b375-3f6f50e927de/Picture3.jpg?format=1000w)

_Figure 3 - Tesla Wall Connector—detail of low-power electronics; both debugging headers installed_

Another avenue of attack would be extracting contents of the GigaDevice serial flash device U26—after all, this is all the storage the communications module has available. The challenge here is two-fold: first, the Flash device is inconveniently positioned in a tight space between the module and the flat cable connector J6, posing considerable risk of damaging the connector while attempting to remove the Flash chip from the board; second, the Flash contents may be encrypted at rest with an unknown key, hindering further analysis. That said, Trend ZDI researchers found a way to dump the contents of this IC in-system.

Looking closely, all the signal pins of U26 are broken out through small vias and can be accessed on the bottom side; since those vias are not covered with solder mask, they probably double up as test points. By soldering to these points, it is possible to build an “adapter” good enough to connect the chip to a flash programmer such as the TL866+. Here is how vias correspond to the IC pins:

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/c4802bcc-8050-46c8-991d-0a297d36cc04/Picture4.jpg?format=1000w)

_Figure 4 - Tesla Wall Connector—detail of the vias connection_

Pin 8, VDD, is not directly accessible here but can be tapped off the debugging connector. The only issue is that the communications module CPU is simultaneously executing code, driving all the signals. This can be overcome by keeping the CPU in reset; experimentally, placing a 1 kΩ resistor between pins 3 and 10 of J7 achieved exactly that.

As a bonus, this interface enables experimenting on the system without having to resolder the flash chip every time.

Firmware Analysis
-----------------

The STM32 flash image is clearly partitioned into two parts, the bootloader and the application, with the latter starting at offset 0x5000.

The extracted STM32 firmware was found to be completely stripped of any useful ASCII strings. This made it very challenging for Trend Micro researchers to identify any specific software components in use. What could be said, however, is that the MCU does not appear to be in charge of handling any internet-facing communications. It does seem to use some kind of an RTOS, due to the presence of what looked like thread names in the few strings the image had: wifirx, wifitx, wifireltx, cantx, canrx, metertrx, leds, and IDLE. Based on that, we conclude that the MCU mostly deals with CAN, metering, and LEDs, while the communications module then handles the rest of the communications.

The communications module firmware is somewhat more involved, as code signing is supported and used. The image was inspected in a hex editor; the contents were not encrypted. The following interesting fragments were detected:

·      0x00000000: A secure boot header corresponding to what is described in SoC documentation is found there.  
·      0x000002BA: A short code fragment, apparently a bootloader of sorts.  
·      0x00004000: A short header marked with WMPT, which looks like a partition table for the rest of the flash.  
·      0x00005000: A copy of the above.  
·      0x00006000: The psm partition. This seems to contain some (encrypted?) configuration data.  
·      0x00008000: The mcufw partition, storing the actual application firmware. The format of this image [has been investigated before](https://akrutsinger.github.io/2023/10/08/tesla-wall-connector-firmware-file-structure.html) by others.  
·      0x00288000: The second mcufw partition.  
·      0x00508000: The fs partition, which is littlefs-formatted. This appears to contain mostly log data.

As a side note, the mcufw image appears to include the full STM32 image inside; likely, the comms module can reprogram the STM32 MCU directly.

Network Traffic Analysis
------------------------

The device provides its own Wi-Fi access point for configuration and management with the SSID starting with `TeslaWallConnector` and the passphrase comprised of 12 upper-case letters. Once connected to an AP, it is possible to interact with the web-based interface and configure the unit to connect itself to an internet-facing Wi-Fi network. This allows the user to monitor the unit, and the unit to communicate with remote endpoints and update itself when a new software version becomes available.

A network scan was performed with nmap, which discovered TCP ports 80 and 34578 as well as UDP ports 67 and 5353 open on the device. This set of ports remained the same independent of whether the device was configured to connect to a Wi-Fi access point or remained standalone; the same TCP ports were exposed over the client side when connected to the Wi-Fi AP.

When connected, the device communicated to the following servers:

·      hermes-prd.sn.tesla.services  
·      wc-maestro-prd.sn.tesla.services  
·      s3-us-west-2.amazonaws.com

Communications with the first two services were TLS-protected, and both client and server certificates were requested and provided by both sides. The server certificates were issued by Tesla Energy Services CA, while the client certificate came from NXP Plug Trust CA. MitM attacks were not attempted by Trend Micro researchers at this point.

The third endpoint, however, used plain HTTP. This allowed to snoop on the requested data, which turned out to be the software update bundle. The bundle was found to be encrypted; a cursory search on the web did not reveal any hints on the algorithm or the key.

Research on TCP port 34758 [seems to indicate](https://community.home-assistant.io/t/tesla-wall-connector-gen-3-restful/311670/55) that this is an endpoint for load-sharing setup, requiring mutual TLS authentication before further access can be gained.

Summary
-------

While these may not be the only attack surfaces available on the Tesla Wall Connector unit, they represent the most likely avenues a threat actor may use to exploit the device. We’re excited to see what research is displayed in Tokyo during the Pwn2Own Automotive event. Stay tuned to the blog for attack surface reviews for other devices, and if you’re curious, you can see [all the devices](https://www.zerodayinitiative.com/blog/2024/9/23/announcing-pwn2own-automotive-for-2025) included in the contest.

Until then, you can find me on Mastodon at [@infosecdj](https://infosec.exchange/@infosecdj), and follow the team on [Twitter](https://www.twitter.com/thezdi), [Mastodon](https://infosec.exchange/@thezdi), [LinkedIn](https://www.linkedin.com/company/zerodayinitiative), or [Bluesky](https://bsky.app/profile/thezdi.bsky.social) for the latest in exploit techniques and security patches.

#### [Source](https://www.thezdi.com/blog/2024/12/16/detailing-the-attack-surfaces-of-the-tesla-wall-connector-ev-charger)

<br/>
---
