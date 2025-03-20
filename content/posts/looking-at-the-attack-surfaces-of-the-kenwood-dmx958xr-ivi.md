---
title: "Looking at the Attack Surfaces of the Kenwood DMX958XR IVI"
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

In our previous Kenwood DMX958XR blog post, we detailed the internals of the Kenwood in-vehicle infotainment (IVI) head unit and provided annotated pictures of each PCB. In this post, we aim to outline the attack surface of the DMX958XR in the hopes of providing inspiration for vulnerability research.

We will cover the main supported technologies that present potential attack surfaces, such as USB, Bluetooth, Android Auto, Apple CarPlay, Kenwood apps, and more. We also provide a list of the open-source components the DMX958XR claims to use.

All information has been obtained through reverse engineering, experimenting, and combing through the following resources:

·      Kenwood DMX958XR Product Page  
·      Kenwood DMX958XR Instruction Manual \[PDF\]  
·      Kenwood DMX958XR Quick Start Guide \[PDF\]  
·      Kenwood Portal App  
·      Kenwood Remote S App

**USB**

The DMX958XR is equipped with a single USB-C port that operates at USB 2.0 speeds and provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes and their associated extensions are:

·      MP3 (.mp3)  
·      WMA (.wma)  
·      AAC-LC (.m4a)  
·      WAV (.wav)  
·      FLAC (.flac, .fla)  
·      Vorbis (.ogg)

Beyond just audio, a USB flash drive can also be used to play back video files. The supported video file types and their associated extensions are:

·      MPEG-1 (.mpg, .mpeg)  
·      MPEG-2 (.mpg, .mpeg)  
·      H.264 / MPEG-4 (.mp4, .m4v, .avi, .flv, .f4v)  
·      WMV (.wmv)  
·      MKV (.mkv)

Robustly parsing and decoding these file formats is notoriously complicated and error-prone, which makes for a potentially rewarding attack surface. USB flash drives must be formatted as either FAT16, FAT32, exFAT, or NTFS for the head unit to be able to read them.

**Bluetooth**

Bluetooth version 5 is supported by the head unit and is used for making phone calls, receiving calls, and playing audio from a paired mobile phone. The following Bluetooth profiles are implemented:

·      Hands-Free Profile v1.7  
·      Serial Port Profile  
·      Phonebook Access Profile  
·      Audio/Video Remote Control Profile (AVRCP) v1.6  
·      Advanced Audio Distribution Profile (A2DP)     
·     Supporting codecs: SBC, AAC or LDAC

Android Auto, Apple CarPlay, and the Kenwood apps all utilize Bluetooth in varying capacities. 

**Wi-Fi**

The head unit provides a Wi-Fi access point, which is primarily used for wireless Android Auto and Apple CarPlay. There is no intention for the end user to directly connect to this access point, and there is no officially documented way of acquiring the password. However, internal research has discovered multiple methods to obtain the password. Once connected to the access point the following ports are listening:

·      TCP: 7000, 8086  
·      UDP: 67, 5353, 35917, 50002, 60794

The two TCP ports and UDP port 50002 are of particular interest since they are running non-standard services.

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party application to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned Wi-Fi network to establish a high-bandwidth channel for data to be sent and received. When connecting using a USB cable, the Wi-Fi network isn't used by Android Auto or Apple CarPlay, but it is still active.

Pwn2Own Automotive 2024 didn’t see any entries that leveraged Android Auto or Apple CarPlay functionality to compromise a head unit. We will have to wait and see if Pwn2Own Automotive 2025 does!

**Kenwood Apps**

Kenwood offers two Android/iOS apps to interface with the DMX958XR. The first app is the Kenwood Portal App, which allows users to transfer photos from a mobile phone to the head unit over Bluetooth. The transferred photos can then be viewed as a slideshow on the head unit or be used as wallpaper.

This presents an interesting attack surface – especially if the DMX958XR itself performs any complex image handling tasks on the received images, such as resizing or converting between different image formats. The user-supplied images also need to be persisted in the head unit's filesystem, further expanding the attack surface.

The second app is the Kenwood Remote S app, which connects to the head unit over Bluetooth and allows for multimedia control, such as selecting a radio station, skipping a track, and more. The Bluetooth Audio/Video Remote Control Profile (AVRCP) is designed for this task. However, no research was performed to confirm if the Remote S app takes advantage of AVRCP. There are a few other Kenwood apps available, but they are not listed as supported on the DMX958XR product page and therefore have not been explored.

**Open Source Software**

A list of open-source licenses can be viewed from the head unit by navigating to Menu -> Settings -> Special -> Open Source Licenses. There is no guarantee these open-source projects are actually used, but a complete list of the projects is provided at the end of this blog post. Where available, the versions of the projects have also been included.

**Summary**

We hope that this blog post has provided enough information about the DMX958XR attack surface to guide vulnerability research. Not every attack surface has been mentioned and we encourage researchers to investigate further. The next post in this series will cover details of the DMX958XR firmware.

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

**Open Source Software List**

Below is a complete list of all the open-source software the head unit claims to use:

·      OpenSSL (2011)  
·      SSLeay (1998)  
·      ALSA  
·      BusyBox  
·      Cairo  
·      D-Bus  
·      dnsmasq (2014)  
·      e2fsprogs (2007)  
·      Freeware Advanced Audio Coder v1.36 (2009)  
·      flac (2014)  
·      fontconfig (2012)  
·      GLIB (1997)  
·      bashline (1993)  
·      iconv (2011)  
·      GNU MP (2007)  
·      GNU readline (2005)  
·      GNU tar (2006)  
·      gstreamer (2000)  
·      GdkPixbuf (1999)  
·      GnuTLS (2012)  
·      HarfBuzz (2012)  
·      ICU (2015)  
·      ImageMagick (2016)  
·      iperf (2007)  
·      libpng (2019)  
·      libusb (2015)  
·      xiph (2015)  
·      libxml2 (2012)  
·      libxslt (2002)  
·      Naver fonts (2007)  
·      GIO (2010)  
·      OpenSSH  
·      OpenSSL (2011)  
·      PCI Utilities v3.3.1 (2015)  
·      Qt (2013)  
·      Bluetooth SBC library (2013)  
·      Sysvinit (2004)  
·      Info-ZIP (2007)  
·      bzip2 v1.0.6 (2010)  
·      cURL (2015)  
·      dpkg (1995)  
·      libffi (2014)  
·      libjpeg v9a (2014)  
·      XFree86 (2000)  
·      libproxy (2006)  
·      libX11 (2006)  
·      soup-cache (2010)  
·      nettle (2002)  
·      libdpkg (1995)  
·      pango (1999)  
·      sysctl v1.0.1 (1999)  
·      alloc (2002)  
·      pslash (2006)  
·      tslib (2001)  
·      libudev (2011)  
·      usbmisc (2003)  
·      zlib v1.2.8  
·      s-bios (2011)  
·      devmem2 (2000)  
·      hostapd (2015)  
·      hidapi (2010)  
·      wpa-supplicant (2015)  
·      OpenMax (2008)  
·      oRTP (2015)  
·      unzip v1.1 (2010)  
·      hts\_engine (2011)  
·      google-breakpad (2006)  
·      boost v1.0 (2003)  
·      SQLite (2001)  
·      PCRE (2019)  
·      OpenGL (2012)  
·      base64 (2001)  
·      mDNSResponder  
·      RapidJSON (2015)  
·      crc32 (2005)  
·      zconf (2005)

In our previous Kenwood DMX958XR blog post, we detailed the internals of the Kenwood in-vehicle infotainment (IVI) head unit and provided annotated pictures of each PCB. In this post, we aim to outline the attack surface of the DMX958XR in the hopes of providing inspiration for vulnerability research.

We will cover the main supported technologies that present potential attack surfaces, such as USB, Bluetooth, Android Auto, Apple CarPlay, Kenwood apps, and more. We also provide a list of the open-source components the DMX958XR claims to use.

All information has been obtained through reverse engineering, experimenting, and combing through the following resources:

·      Kenwood DMX958XR Product Page  
·      Kenwood DMX958XR Instruction Manual \[PDF\]  
·      Kenwood DMX958XR Quick Start Guide \[PDF\]  
·      Kenwood Portal App  
·      Kenwood Remote S App

**USB**

The DMX958XR is equipped with a single USB-C port that operates at USB 2.0 speeds and provides the necessary interface for wired Android Auto and Apple CarPlay. The USB port also supports playback of audio files from a USB flash drive. The supported audio filetypes and their associated extensions are:

·      MP3 (.mp3)  
·      WMA (.wma)  
·      AAC-LC (.m4a)  
·      WAV (.wav)  
·      FLAC (.flac, .fla)  
·      Vorbis (.ogg)

Beyond just audio, a USB flash drive can also be used to play back video files. The supported video file types and their associated extensions are:

·      MPEG-1 (.mpg, .mpeg)  
·      MPEG-2 (.mpg, .mpeg)  
·      H.264 / MPEG-4 (.mp4, .m4v, .avi, .flv, .f4v)  
·      WMV (.wmv)  
·      MKV (.mkv)

Robustly parsing and decoding these file formats is notoriously complicated and error-prone, which makes for a potentially rewarding attack surface. USB flash drives must be formatted as either FAT16, FAT32, exFAT, or NTFS for the head unit to be able to read them.

**Bluetooth**

Bluetooth version 5 is supported by the head unit and is used for making phone calls, receiving calls, and playing audio from a paired mobile phone. The following Bluetooth profiles are implemented:

·      Hands-Free Profile v1.7  
·      Serial Port Profile  
·      Phonebook Access Profile  
·      Audio/Video Remote Control Profile (AVRCP) v1.6  
·      Advanced Audio Distribution Profile (A2DP)     
·     Supporting codecs: SBC, AAC or LDAC

Android Auto, Apple CarPlay, and the Kenwood apps all utilize Bluetooth in varying capacities. 

**Wi-Fi**

The head unit provides a Wi-Fi access point, which is primarily used for wireless Android Auto and Apple CarPlay. There is no intention for the end user to directly connect to this access point, and there is no officially documented way of acquiring the password. However, internal research has discovered multiple methods to obtain the password. Once connected to the access point the following ports are listening:

·      TCP: 7000, 8086  
·      UDP: 67, 5353, 35917, 50002, 60794

The two TCP ports and UDP port 50002 are of particular interest since they are running non-standard services.

**Android Auto and Apple CarPlay**

Both wired and wireless Android Auto and Apple CarPlay are supported without the need for a third-party application to be installed on the paired mobile phone. When using the wireless versions, the paired phone connects to the aforementioned Wi-Fi network to establish a high-bandwidth channel for data to be sent and received. When connecting using a USB cable, the Wi-Fi network isn't used by Android Auto or Apple CarPlay, but it is still active.

Pwn2Own Automotive 2024 didn’t see any entries that leveraged Android Auto or Apple CarPlay functionality to compromise a head unit. We will have to wait and see if Pwn2Own Automotive 2025 does!

**Kenwood Apps**

Kenwood offers two Android/iOS apps to interface with the DMX958XR. The first app is the Kenwood Portal App, which allows users to transfer photos from a mobile phone to the head unit over Bluetooth. The transferred photos can then be viewed as a slideshow on the head unit or be used as wallpaper.

This presents an interesting attack surface – especially if the DMX958XR itself performs any complex image handling tasks on the received images, such as resizing or converting between different image formats. The user-supplied images also need to be persisted in the head unit's filesystem, further expanding the attack surface.

The second app is the Kenwood Remote S app, which connects to the head unit over Bluetooth and allows for multimedia control, such as selecting a radio station, skipping a track, and more. The Bluetooth Audio/Video Remote Control Profile (AVRCP) is designed for this task. However, no research was performed to confirm if the Remote S app takes advantage of AVRCP. There are a few other Kenwood apps available, but they are not listed as supported on the DMX958XR product page and therefore have not been explored.

**Open Source Software**

A list of open-source licenses can be viewed from the head unit by navigating to Menu -> Settings -> Special -> Open Source Licenses. There is no guarantee these open-source projects are actually used, but a complete list of the projects is provided at the end of this blog post. Where available, the versions of the projects have also been included.

**Summary**

We hope that this blog post has provided enough information about the DMX958XR attack surface to guide vulnerability research. Not every attack surface has been mentioned and we encourage researchers to investigate further. The next post in this series will cover details of the DMX958XR firmware.

We are looking forward to Automotive Pwn2Own, again to be held in January 2025 at the Automotive World conference in Tokyo. We will see if IVI vendors have improved their product security. Don’t wait until the last minute to ask questions or register! We hope to see you there.

You can find me on Twitter at @ByteInsight, and follow the team on Twitter, Mastodon, LinkedIn, or Bluesky for the latest in exploit techniques and security patches.

**Open Source Software List**

Below is a complete list of all the open-source software the head unit claims to use:

·      OpenSSL (2011)  
·      SSLeay (1998)  
·      ALSA  
·      BusyBox  
·      Cairo  
·      D-Bus  
·      dnsmasq (2014)  
·      e2fsprogs (2007)  
·      Freeware Advanced Audio Coder v1.36 (2009)  
·      flac (2014)  
·      fontconfig (2012)  
·      GLIB (1997)  
·      bashline (1993)  
·      iconv (2011)  
·      GNU MP (2007)  
·      GNU readline (2005)  
·      GNU tar (2006)  
·      gstreamer (2000)  
·      GdkPixbuf (1999)  
·      GnuTLS (2012)  
·      HarfBuzz (2012)  
·      ICU (2015)  
·      ImageMagick (2016)  
·      iperf (2007)  
·      libpng (2019)  
·      libusb (2015)  
·      xiph (2015)  
·      libxml2 (2012)  
·      libxslt (2002)  
·      Naver fonts (2007)  
·      GIO (2010)  
·      OpenSSH  
·      OpenSSL (2011)  
·      PCI Utilities v3.3.1 (2015)  
·      Qt (2013)  
·      Bluetooth SBC library (2013)  
·      Sysvinit (2004)  
·      Info-ZIP (2007)  
·      bzip2 v1.0.6 (2010)  
·      cURL (2015)  
·      dpkg (1995)  
·      libffi (2014)  
·      libjpeg v9a (2014)  
·      XFree86 (2000)  
·      libproxy (2006)  
·      libX11 (2006)  
·      soup-cache (2010)  
·      nettle (2002)  
·      libdpkg (1995)  
·      pango (1999)  
·      sysctl v1.0.1 (1999)  
·      alloc (2002)  
·      pslash (2006)  
·      tslib (2001)  
·      libudev (2011)  
·      usbmisc (2003)  
·      zlib v1.2.8  
·      s-bios (2011)  
·      devmem2 (2000)  
·      hostapd (2015)  
·      hidapi (2010)  
·      wpa-supplicant (2015)  
·      OpenMax (2008)  
·      oRTP (2015)  
·      unzip v1.1 (2010)  
·      hts\_engine (2011)  
·      google-breakpad (2006)  
·      boost v1.0 (2003)  
·      SQLite (2001)  
·      PCRE (2019)  
·      OpenGL (2012)  
·      base64 (2001)  
·      mDNSResponder  
·      RapidJSON (2015)  
·      crc32 (2005)  
·      zconf (2005)

Go to Source
