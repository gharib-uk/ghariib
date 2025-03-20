---
title: "Oh no! I have a wiped iPhone, now what?"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "amd"
  - "ces"
  - "report"
---

One of the most common questions I got asked during presentations and conferences is: "**During a search and seizure we found a wiped iPhone, what can we do next?**"

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi6xIfngOuvXrGpaenivhwwqQadArE3RoQ9MynGYodzJ1SjdkoYvo1KOqbz23sL8xmL9t7IAxSgrKELQZZ6NEBRYFJu3wWr1ZxBQOW3lBtjyCq5gtrLgBKemMqluauKuC5lvteSiMW7t1Y/w296-h640/image.png)

  

First and foremost: **you cannot recover data stored on the device before wiping occurred**.  
The encryption keys you need to decrypt the data are gone forever.  
Full stop :)

If you are aware of any method, technique, tool or magic box that can do that, please let me know :)

You have **three options** to recover data:

- **Data stored on computer**(s) (Windows or Mac)

- On Windows you can search for

- Lockdown certificates

- C:ProgramDataAppleLockdown

- iOS Backups 

- C:Users<username>AppDataRoamingApple ComputerMobileSyncBackup
- C:Users<username>AppleMobileSyncBackup

- Synced CrashLogs

- C:Users<username>AppDataRoamingApple ComputerLogsCrashReporterMobileDevice

- MediaStream

- C:Users<username>AppDataRoamingApple ComputerMediaStream

- iPodDevices.xml

- C:Users<username>AppDataLocalApple ComputeriTunesiPodDevices.xml

- Data stored on **iCloud**

- iCloud backup
- Synced data (e.g. Contacts, Photos, Messages, and so on)
- Keychain

- Data stored on **synced devices**:

- iPad
- Apple Watch
- Apple TV
- Apple HomePod

In the past, I took some presentations and wrote articles on synced iOS devices. 

Here some references:

- "Apple Watch Forensics: Is It Ever Possible, And What Is The Profit?" at DFRWS EU 2019
- "Apple Watch Forensic Analysis" on Elcomsoft Blog
- "Forensicating the Apple TV" at DFRWS EU 2018
- "Apple TV Forensic Analysis" on Elcomsoft Blog
- "A journey into IoT Forensics - Episode 5 - Analysis of the Apple HomePod and the Apple Home Kit Environment" on our Blog
- "Forensic Analysis of Apple HomePod & Apple HomeKit Environment" at the SANS DFIR Summit 2020

Over the last couple of days, I tried answering those question:

- Is there any general method that I can use to **extract some data** from an iOS device in a reset state ("Hello" screen), before setting it up?
- Which **kind of information can I recover**?

By connecting the device to a computer and by using various tools, you can extract **basic device information**, including:

- Product Type
- Sales Model
- Model Number
- IMEI
- Serial Number
- ECID
- iOS Version
- CPU
- Charge Times
- Battery Life
- Bluetooth Address
- Wi-Fi Address
- Cellular Address
- Disk size
- Disk Usage information

If the device is checkm8-vulnerable, you can try to **obtain a full file system by using a forensic tool or checkra1n**.

  

More in general, also with a non-checkm8-vulnerable device, while the device is in the "Hello" screen, **you can start the generation of a "sysdiagnose" with the physical button combination** (hold the two volume buttons and the side button for 1-1.5 seconds).

  

I wrote the paper "Using Apple Bug Reporting for forensic purposes" with Heather Mahalik and Adrian Leong, also covering sysdiagnose.

  

The generated sysdiagnose file can be extracted by using any tool able to extract crash logs. 

  

In picture, you can see crash logs and the sysdiagnose file extracted with idevicecrashreporter.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQnjXEyv4JghKD6vabXRyYcnPp_EvDZgjR1cAg6lS98FJRKi_FKas1njHxdhXxQjOcpiTPTS_EI_z1pbNwpmFyUYb86hxcWkhwStsdMiWvWXC07NuRiV5JLT7TBfwrya0Esd5WXu2C4DA/w640-h182/image.png)

  

In the specific context of a wiped device, the most interesting files to analyze are:

- **logsMobileInstallationmobile\_installation.log.0 (or mobile\_installation.log.1)**: specifically search for the string "**Did not find last build info; we must be upgrading from pre-8.0 or this is an erase install.**".  
      
    The associated timestamp, in Pacific Timezone (Cupertino) corresponds to the time of wiping

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_CA_4CG8mp501UkVjcMrHkX9QPtw1CeQVkol_ZOZKr-e1DYkZrBFmZ6qjUz6jvK1M_h9BT6BFZee05jY9KfzunWmQeMPyJ5IQ_WY-yUbi1Gg68uTYA-_u0Wic9sx2Z5CiZX7nBz3uZiY/s16000/image.png)

- **logsMobileLockdownlockdown.log**: specifically search for the string "**\_load\_dict: Failed to load /private/var/root/Library/Lockdown/data\_ark.plist.**".  
      
    The associated timestamp, in Pacific Timezone (Cupertino) corresponds to the time of wiping

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiPz6Cov2DAxP1ZOqPuqxqKUz1nbhYCpbjR6Ywxz0yfOQMdBctMlPaJ4al6NdpNEO6lBQOeD7_l8MjYK-AVsNKM9fP2ZU-m_ypNrvr9OKyueXVzSZZyTa6ZMothqCABXkoWQHUirRDzHCw/s16000/image.png)

- **logsMobileLockdownlockdown.log**: specifically search for the string "**\_load\_dict: Failed to load /private/var/root/Library/Lockdown/data\_ark.plist.**".  
      
    The associated timestamp, in Pacific Timezone (Cupertino) corresponds to the time of wiping

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQ3qHYjRjRUwfaV4lM1V8d0cNbJpCaD80H6JKWKRpsaPTDc2XBGoK05LP09azQwUvBjQ0_dnhaxL3JSlVIWO4cZEvJ5LMt1MP00LVqjvJNanoN2l4ZNAYActN1S0CNKHQg7alTXwsLa34/s16000/image.png)

- **logsMobileContainerManagercontainermanagerd.log.0**: specifically search for the string "**containermanagerd performing first boot initialization**".  
      
    The associated timestamp, in Pacific Timezone (Cupertino) corresponds to the time of wiping

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgC62EIUvF0uhvYwPWSTqAIIC_Qvb4oD7mJ4UGuDQO2MIjH2dqh9EDWIp5FX3YKgFtrSa_kScam30I-ccnA58xae_V1AghmuD-uxyyC6xKX2GkaDahAiRD7ItLrGZ33YF5Y63IPRHVT4bY/s16000/image.png)

- **logspowerlogspowerlog\_YYY-MM-HH\_MM\_SS\_XXXXXXXX.PLSQL**, that has the internal structure of a PowerLog file (just rename it as CurrentPowerLog.PLSQL). It can contain, for example, information about Battery Level and so discover if and when the device was on charge

- **WiFiwifi\_scan\_cache.txt**: containing Wi-Fi networks "seen" by the device. It includes SSID and BSSID.

In conclusion, you cannot recover user data, but you can at least understand precisely when the device was wiped and what happened on it before you generate the sysdiagnose. If you are lucky enough you could find SSID and BSSID in the WiFi cache.

Go to Source
