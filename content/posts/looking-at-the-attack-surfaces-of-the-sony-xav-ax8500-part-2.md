---
title: "Looking at the Attack Surfaces of the Sony XAV-AX8500 Part 2"
date: 2025-01-12
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "cybersecurity"
  - "security"
  - "zero_day"
---

In our previous Sony XAV-AX8500 blog post, we detailed the internals of the head unit and provided annotated pictures of each PCB. In this post we aim to outline the threat landscape of the XAV-AX8500 in the hopes of providing inspiration for vulnerability research.

We will cover the main supported technologies that present potential attack surfaces such as USB, Bluetooth, Android Auto, Apple CarPlay, and more. We will also look at some of the open-source components the XAV-AX8500 claims to use.

All information has been obtained through reverse engineering, experimenting, and combing through the following resources:

·      Sony XAV-AX8500 Product Page  
·      Sony XAV-AX8500 Help Guide (PDF)  
·      Sony XAV-AX8500 Operating Instructions (PDF)

 **USB**

The XAV-AX8500 has a single USB-C port that provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes and their associated extensions are:

·      MP3 (.mp3)  
·      WMA (.wma)  
·      AAC (.m4a)  
·      ALAC (.m4a)  
·      WAV (.wav)  
·      FLAC (.flac)  
·      DSD (.dsf, .dff)

As well as audio, a USB flash drive can also be used to playback video files. The supported video filetypes and their associated extensions are:

·      ASF (.asf)  
·      WMV (.wmv)  
·      AVI (.avi)

Robustly parsing and decoding these file formats is notoriously complicated and error-prone, which makes for a potentially rewarding attack surface.

Another point of interest is that the XAV-AX8500 allows users to set the wallpaper by providing JPEG images on a flash drive and then selecting the desired images by navigating to Settings -> Customize -> Wallpaper Select -> Browse. Images must be less than 2048 x 1080 pixels and 6MB in size.

Image handling can be a complex task especially when it comes to image manipulation. Adding to this, the user controls the image filenames which further expands the potential attack surface especially since selected wallpaper images must be stored on disk.

Firmware updates can also be installed via a flash drive by navigating to Settings -> System -> Software Update. The next blog post in this series will cover the XAV-AX8500 firmware in more detail.

USB flash drives must be formatted as either FAT16, FAT32, or exFAT for the head unit to be able to read them.

**Bluetooth**

Bluetooth version 5 is supported by the head unit and is used for making phone calls, receiving phone calls and playing audio from a paired mobile phone. This functionality is offered natively by the head unit and operates using the following Bluetooth profiles:

·      Advanced Audio Distribution Profile (A2DP) v1.3.1  
·      Audio/Video Remote Control Profile (AVRCP) v1.6.1  
·      Hands Free Profile v1.7.1  
·      Phonebook Access Profile v1.2

Android Auto and Apple CarPlay also utilize Bluetooth to varying degrees.

**Wi-Fi**

The head unit provides a Wi-Fi access point that is primarily used for wireless Android Auto and Apple CarPlay. There is no intention for the end user to directly connect to this access point and there is no officially documented way of acquiring the password. However, internal research has discovered a method to obtain the password.

Once connected to the access point a port scan was performed that only detected TCP port 30515 as being open.

There isn't much information about TCP port 30515 relating to automotive other than a vague comment in this Github repository and a slide NCC Group's Revving Up: The Journey to Pwn2Own Automotive 2024 (PDF slide 15).

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party app to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned secured Wi-Fi network to establish a high bandwidth channel for data to be sent and received.

When connecting using a USB cable the Wi-Fi network isn't used by Android Auto or Apple CarPlay but it is still active.

Android Auto and Apple CarPlay have yet to be used to gain control of a head unit at Pwn2Own Automotive, so we are looking forward to any entries that exploit these services!

**Open Source Software**

A list of open-source licenses can be viewed from the head unit by navigating to Settings -> System -> Open Source Licenses. Licenses of particular interest include:

·      Python  
·      Boost (C++ library)  
·      Dropbear  
·      Lame (MP3 encoder)  
·      mpg123 (MP3 decoder)  
·      mpg123 has a fair number of published CVEs

There's no guarantee these open-source projects are actually used, but further information will be provided in the next blog post of this series.

**Summary**

We hope that this blog post has provided enough information about the XAV-AX8500 threat landscape to guide vulnerability research. Not every attack surface has been mentioned, and we encourage researchers to investigate further.

 The next post in this series will cover details of the XAV-AX8500 firmware.

We are looking forward to Pwn2Own Automotive again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

In our previous Sony XAV-AX8500 blog post, we detailed the internals of the head unit and provided annotated pictures of each PCB. In this post we aim to outline the threat landscape of the XAV-AX8500 in the hopes of providing inspiration for vulnerability research.

We will cover the main supported technologies that present potential attack surfaces such as USB, Bluetooth, Android Auto, Apple CarPlay, and more. We will also look at some of the open-source components the XAV-AX8500 claims to use.

All information has been obtained through reverse engineering, experimenting, and combing through the following resources:

·      Sony XAV-AX8500 Product Page  
·      Sony XAV-AX8500 Help Guide (PDF)  
·      Sony XAV-AX8500 Operating Instructions (PDF)

 **USB**

The XAV-AX8500 has a single USB-C port that provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes and their associated extensions are:

·      MP3 (.mp3)  
·      WMA (.wma)  
·      AAC (.m4a)  
·      ALAC (.m4a)  
·      WAV (.wav)  
·      FLAC (.flac)  
·      DSD (.dsf, .dff)

As well as audio, a USB flash drive can also be used to playback video files. The supported video filetypes and their associated extensions are:

·      ASF (.asf)  
·      WMV (.wmv)  
·      AVI (.avi)

Robustly parsing and decoding these file formats is notoriously complicated and error-prone, which makes for a potentially rewarding attack surface.

Another point of interest is that the XAV-AX8500 allows users to set the wallpaper by providing JPEG images on a flash drive and then selecting the desired images by navigating to Settings -> Customize -> Wallpaper Select -> Browse. Images must be less than 2048 x 1080 pixels and 6MB in size.

Image handling can be a complex task especially when it comes to image manipulation. Adding to this, the user controls the image filenames which further expands the potential attack surface especially since selected wallpaper images must be stored on disk.

Firmware updates can also be installed via a flash drive by navigating to Settings -> System -> Software Update. The next blog post in this series will cover the XAV-AX8500 firmware in more detail.

USB flash drives must be formatted as either FAT16, FAT32, or exFAT for the head unit to be able to read them.

**Bluetooth**

Bluetooth version 5 is supported by the head unit and is used for making phone calls, receiving phone calls and playing audio from a paired mobile phone. This functionality is offered natively by the head unit and operates using the following Bluetooth profiles:

·      Advanced Audio Distribution Profile (A2DP) v1.3.1  
·      Audio/Video Remote Control Profile (AVRCP) v1.6.1  
·      Hands Free Profile v1.7.1  
·      Phonebook Access Profile v1.2

Android Auto and Apple CarPlay also utilize Bluetooth to varying degrees.

**Wi-Fi**

The head unit provides a Wi-Fi access point that is primarily used for wireless Android Auto and Apple CarPlay. There is no intention for the end user to directly connect to this access point and there is no officially documented way of acquiring the password. However, internal research has discovered a method to obtain the password.

Once connected to the access point a port scan was performed that only detected TCP port 30515 as being open.

There isn't much information about TCP port 30515 relating to automotive other than a vague comment in this Github repository and a slide NCC Group's Revving Up: The Journey to Pwn2Own Automotive 2024 (PDF slide 15).

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party app to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned secured Wi-Fi network to establish a high bandwidth channel for data to be sent and received.

When connecting using a USB cable the Wi-Fi network isn't used by Android Auto or Apple CarPlay but it is still active.

Android Auto and Apple CarPlay have yet to be used to gain control of a head unit at Pwn2Own Automotive, so we are looking forward to any entries that exploit these services!

**Open Source Software**

A list of open-source licenses can be viewed from the head unit by navigating to Settings -> System -> Open Source Licenses. Licenses of particular interest include:

·      Python  
·      Boost (C++ library)  
·      Dropbear  
·      Lame (MP3 encoder)  
·      mpg123 (MP3 decoder)  
·      mpg123 has a fair number of published CVEs

There's no guarantee these open-source projects are actually used, but further information will be provided in the next blog post of this series.

**Summary**

We hope that this blog post has provided enough information about the XAV-AX8500 threat landscape to guide vulnerability research. Not every attack surface has been mentioned, and we encourage researchers to investigate further.

 The next post in this series will cover details of the XAV-AX8500 firmware.

We are looking forward to Pwn2Own Automotive again in Tokyo in January 2025 at Automotive World, and we will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions and register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

Go to Source
