---
title: "Reviewing the Attack Surface of the Autel MaxiCharger Part One"
date: Wed, 15 Jan 2025 14:45:00 +0000
draft: false
type: posts
categories: 
- Blog post
---
# Reviewing the Attack Surface of the Autel MaxiCharger Part One
https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/1736922850679-TSO9KHRY8QJRX6OWJO7N/computer-technology-guitar-toy-electronic-electronics-851978-pxhere.com.jpg?format=1500w
<br/>

<br/>
For the upcoming Pwn2Own Automotive contest a total of 7 electric vehicle chargers have been selected. One of these is the Autel MaxiCharger AC Wallbox Commercial (MAXI US AC W12-L-4G) which also made an appearance at the inaugural Pwn2Own Automotive last January. 

We have previously posted [internal photos of the MaxiCharger](https://www.zerodayinitiative.com/blog/2023/11/28/a-detailed-look-at-pwn2own-automotive-ev-charger-hardware) in 2023 so the goal of this blog post is to present up to date internal photos of the main boards and provide additional information.

**Internals**

Opening the MaxiCharger is easy and involves removing a few Torx T10 screws and then prying open the edges of the housing.

The metrology board is mounted on the back part of the housing and is responsible for power monitoring, handling the input mains power and providing power to the charging cable. Most of the components mounted on the lower voltage part of the board (towards the top) are covered in conformal coating.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/00a2e71a-7246-4da7-a5c9-3349d152cab6/Picture1.png?format=1000w)

_Figure 1: Power board_

Towards the top right of the power board is the [STM32F407ZGT6](https://www.st.com/en/microcontrollers-microprocessors/stm32f407zg.html), which is a general purpose ARM Cortex-M4 microcontroller. Many of the pins are broken out surrounding the STM32 and are not covered in conformal coating allowing for easy probing.

UART output can be viewed using the broken-out pins above the STM32 with a baud rate of 921600bps. SWD pins are also broken out which allows for full access to the STM32 including dumping the internal flash. A cursory glance of the flash dump shows references to FreeRTOS.

The power board connects to the main board which is mounted on the top part of the housing. The main board is responsible for most of the heavy lifting, including handling Bluetooth, Wi-Fi, ethernet and more. This board isn't covered in conformal coating but quite a few of the components are under metal shielding that was removed for the following photos.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/e252cd69-ea64-4af2-bdb3-7be181f3ffa3/Picture2.png?format=1000w)

_Figure 2: Main board (top)_

The main board contains many labelled test points that make for easy probing.

In the top left is the Barrot BR8041A01 bluetooth module. There isn't much publicly available information about this module. The test points nearby suggest that this module is operated over UART (BT\_RX, BT\_TX) however sniffing these points doesn't show much other than very basic initialization even when attempting to pair a new device.

Towards the center of the board is a [IS65WV10248EBLL](https://www.issi.com/WW/pdf/62-65WV10248EALL-BLL.pdf) (PDF) SRAM chip.

Flipping the main board over reveals the main processor and an ESP32. Again, there are many labelled test points.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/64f7903b-e9c1-4a8d-b49b-2caab0579997/Picture3.png?format=1000w)

_Figure 3: Main board (underside)_

The MCU is the [GD32F407ZGT6](https://www.gigadevice.com.cn/Public/Uploads/uploadfile/files/20241008/GD32F407xxDatasheetRev2.8.pdf) (PDF) ARM Cortex-M4. Interestingly it has been noted that Autel occasionally swap out the GD32F407ZGT6 for the STM32F407ZGT6 for unknown reasons, presumably due to supply. To the left of the battery is the broken out SWD pins for the MCU. Connecting to these pins shows that the MCU has been configured with readout protection level 1 (or "Security Protection Code low" to use GigaDevice's terminology).

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4bc14af1-5b78-4ede-8036-4bc2715f2314/Picture4.png?format=1000w)

_Figure 4: Secured GD32 device detected_

This prevents tools such as ST-Link and J-Link from dumping the internal flash, however Jonathan Andersson and Thanos Kaliyanakis [Blackhat EU talk](https://www.blackhat.com/eu-24/briefings/schedule/index.html#decoding-em-fi-attacks-lessons-learned-from-glitching-the-gigadevice-gdf-42681) details a few very interesting bypasses that circumvent this readout protection! One such method doesn’t require glitching.

To the right of the MCU is a [Winbond W25Q128JV](https://www.winbond.com/resource-files/w25q128jv%20revf%2003272018%20plus.pdf) (PDF) serial flash chip. Below is an RJ45 jack for ethernet communications. This is one of the ways the charger can connect to the internet, the other methods are Wi-Fi and over a mobile network.

There is a mysterious USB C port to the left of the MCU which doesn't have a documented use. This isn't the only USB port on the Autel that has an unknown use, but it is the only one that can be accessed without dismantling the charger.

The ESP32 module in the top left is the [ESP32 WROOM 32D](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32d_esp32-wroom-32u_datasheet_en.pdf) (PDF) which has Bluetooth and Wi-Fi capabilities. Internally the module uses the ESP32-D0WD dual core Xtensa MCU and a 4MB SPI flash chip.

Directly above the USB C port one of the GD32 UARTs is broken out. Connecting with a baud rate of 921600bps shows a lot of debugging information. Interestingly, during initialization the string "UART\_WIFI\_BT" is printed alongside AT commands that are sent to the ESP32 from the GD32.  When pairing a new device, many of these messages are logged.

Combining this information with the very little traffic sniffed to/from the Barrot bluetooth module over the test points hints towards the Barrot bluetooth module being redundant. It seems as though the ESP32 is used for the bluetooth operations.

To the right of the ESP32 are more broken out pins, this time for one of the ESP32 UARTs and the IO0 pin which is used for the ESP32 boot mode selection. Connecting to the UART header at 115200 baud will show the usual ESP32 boot log.

Stacked underneath the main board is the 4G board that has a SIM card tray and a mobile communications module.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/4459876d-044c-402e-9e9b-366200e45168/Picture5.png?format=1000w)

_Figure 5: 4G mobile communications board_

The 4G module is the [Quectel EC25AFXDGA](https://www.quectel.com/content/uploads/2024/03/Quectel_EC25_Series_LTE_Standard_Specification_V2.7-1.pdf?wpId=114164) (PDF) that internally uses the Qualcomm MDM9207 LTE modem which itself contains an ARM Cortex-A7 core. The SIM card tray sits to the left of the 4G module. Connecting the Autel to a mobile network is optional.

The top right of the 4G board has a micro USB port with no known use. The charger must be disassembled to access this port. Presumably this is for some kind of debugging of the 4G module.

A few pins are broken out from the 4G module along the left side of the board. Connecting to RXD and TXD at 115200 baud prints a Linux boot log which ultimately drops to a login prompt for the Quectel module.

Above the UART connection is what's labelled as "BOOT". Shorting these unpopulated headers together and then booting the charger changes the behavior of the 4G module. Notably, the UART connection doesn’t print out the Linux boot log anymore. This behavior may be linked to the aforementioned USB port but this wasn't investigated further.

Interestingly, under the 4G board is yet another unused micro USB port. This is attached to the back of the LCD board and is likely used for debugging LCD related functionality. The silkscreen also shows SWD related text.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/3bc15ff5-ed31-4e7b-bdc0-b3341ca3e860/Picture6.png?format=1000w)

_Figure 6: Unused USB port_

The final board of interest is the NFC and LED board which is connected to the main board. There is an unused 4 pin connector on the back of the board which is likely for debugging purposes.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/d98ac585-c27a-4753-aaae-f841ead85326/Picture7.png?format=1000w)

_Figure 7: NFC and LED board (top)_

Flipping the board over reveals the NFC chip.

![](https://images.squarespace-cdn.com/content/v1/5894c269e4fcb5e65a1ed623/bf19c92f-3b70-49cf-a1e1-6b31ff6eb893/Picture8.png?format=1000w)

_Figure 8: Multi-protocol contactless transceiver_

The NFC chip is a Fudan Microelectronics [FM17660](https://www.scribd.com/document/794131877/NFC-tag-FM17660K-Ds-Eng-1) multi-protocol contactless transceiver IC.

**Summary**

Overall, all the main components are the same as the previous MaxiCharger we tore down last year which is good news for any contestants who previously bought the MaxiCharger.

Hopefully this blog post provides enough information to kickstart vulnerability research against the Autel MaxiCharger. Keep an eye out for future posts that will cover the threat landscape of the MaxiCharger.

We are looking forward to [Pwn2Own Automotive](https://www.zerodayinitiative.com/blog/2024/9/23/announcing-pwn2own-automotive-for-2025) again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at [@ByteInsight](https://www.x.com/ByteInsight), and follow the team on [Twitter](https://www.twitter.com/thezdi), [Mastodon](https://infosec.exchange/@thezdi), [LinkedIn](https://www.linkedin.com/company/zerodayinitiative), or [Bluesky](https://bsky.app/profile/thezdi.bsky.social) for the latest in exploit techniques and security patches.

#### [Source](https://www.thezdi.com/blog/2025/1/15/reviewing-the-attack-surface-of-the-autel-maxicharger-part-one)

<br/>
---
