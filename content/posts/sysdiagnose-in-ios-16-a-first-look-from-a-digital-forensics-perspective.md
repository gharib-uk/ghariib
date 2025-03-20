---
title: "Sysdiagnose in iOS 16: a first look from a Digital Forensics perspective"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

Back in May 2019, along with my colleagues Heather Mahalik and Adrian Leong, we wrote the paper "**Using Apple “Bug Reporting” for forensic purposes**" and some scripts to parse data stored in Sysdiagnose logs.

The paper is still available for download and, for the most part, is still accurate. But time goes on, and new iOS versions have come on the market in recent years. I took a first look at a sysdiagnose generated on a freshly wiped iPhone with iOS 16 natively installed.

For sysdiagnose generation and extraction, nothing has changed since our paper. You can still generate it in a hardware or software way, and you can extract it with forensic tools (i.e. Elcomsoft iOS Forensic Toolkit) or with iOS device manager tools (i.e. 3uTools).

Once extracted, the sysdiagnose is a TAR file that contains various files in the root folder and different subfolders.

![](https://blogger.googleusercontent.com/img/a/AVvXsEiN4dKDma_nUYUXt_AvSOEN8DgYZDRZ_AtnLBwUVeszle_l4s6qDFdh-GQXOjHXbzsB_XTMhRwXsFOOVYdfvLwtlN5JQXrzt7NiXMS_mNUNlzZ8IJ5xAjiDG2SUbuFTmpu8aqIzs4IoCaYEkzGzvWDkmCsp9LsByNBnowG82idB1sfAtlfei4gVM6rA=s16000)

  

At first look, most of the files seem coherent with what we wrote in the paper. You can in fact find:

- sysdiagnose.log
- tasksummary.csv
- disks.txt
- mount.txt
- ckksctl\_status.txt
- apfs\_stats.txt
- error\_log.txt
- pcstatus.txt
- smcDiagnose.txt
- hidutil.plist
- vm\_stat.txt
- microstackshots
- kbdebug.txt
- taskinfo.txt
- spindump-nosymbols.txt
- ps.txt
- ps\_thread.txt
- tailspin-info.txt

There are some new files, not seen in 2019. Probably they were added during the time in different iOS versions:

- codecctl.txt
- night-shift.log
- otctl\_status.txt
- remotectl\_dumpstate.txt, containing a lot of details about the device including model, iOS version and build, UDID, language, timezone, etc.
- security-sysdiagnose.txt
- swcutil\_show.txt, related to iOS Universal Links

Some of the folders have the same content ("ASPSnapshots", "brctl", "crashes\_and\_spins" and "errors"), while others contain additional files or folders.

  

The "**ioreg**" folder contains 2 additional files:

- IOPort.txt
- IOService.txt

The "**Preferences**" folder contains 6 additional files:

- Accessibility\_Preferences.txt, containing Accessibility configuration info
- CaptureSourceInfo\_CurrentUser.txt
- com.apple.avfoundation\_CurrentUser.txt
- com.apple.camera\_CurrentUser.txt
- com.apple.coremedia\_CurrentUser.txt
- ScreenTimeEnabled\_CurrentUser.txt, containing info about ScreenTime (enabled or not)

The new "**Personalization**" folder is related in some way to the Personalization Portrait and the new 

"**RunningBoard**" folder is related in some way to the RunningBoard service introduced in MacOS Catalina.

  

The "**system\_logs.logarchive**" still contains Apple logs that can be easily opened with the Mac OX C Console application.

  

The most interesting changes are in the "**logs**" and in the "**WiFi**" folders.

  

In the "**logs**" folder we can find some folders already available in 2019 and covered in our paper:

- AccessiblityPrefs
- appinstallation
- AWD
- itunesstored
- keyboards
- MobileActivation
- MobileBackup
- MobileContainerManager
- MobileInstallation
- MobileLockdown
- Networking
- olddsc
- powerlogs
- suggest\_tool
- SystemVersion

Also, some folders seem not available anymore:

- AppConduit
- AVConference
- taispindb

Last, I found these new subfolders:

- Accessibility, containing the TCC.db (Transparency, Consent and Control) database
- ACLogs
- AFK
- AppSupport
- Baseband
- BatteryBDC
- BatteryHealth
- BatteryUIPlist
- Bluetooth
- CalendarPreferences
- DCP
- FDR
- MCState
- MemoryExceptions
- MSU
- NetworkRelay
- OTAUpdateLogs
- parseced
- ProactiveInputPredictions
- SensorKit
- Sentry
- SiriAnalytics, containing the SiriAnalytics.db
- Splat
- Trial
- UserManagement, containing the "usermanagerd.log.0" file

In the "**WiFi**" folder we can find some files and folders already available in 2019 and covered in our paper:

- awdl\_status.txt
- bluetooth\_status.txt
- debug-log.txt
- leaky\_ap\_stats.txt
- network\_status.txt
- wifi\_scan.txt
- wifi\_scan\_cache.txt
- wifi\_status.txt

The main difference is the new file format for Wi-Fi networks: the old com.apple.wifi.plist has now changed to **com.apple.wifi.known-networks.plist**. This file is also available in a traditional iTunes backup. Last, Wi-Fi logs are now stored in a file named "**wifimanager.log.tgz**"

  

Overall, the most interesting files you can find in a Sysdiagnose, to the best of my knowledge, are:

- Mobile Installation Logs
- Mobile Activation Logs
- Mobile Container Manager Logs
- Lockdownd Logs
- WiFi Manager Logs
- User Manager Logs
- CurrentPowerLog.PLSQL (partial)
- com.apple.MobileBackup.plist (also available in an iTunes backup)
- TCC.db (also available in an iTunes backup)
- com.apple.wifi.known-networks.plist (also available in an iTunes backup)
- AppUpdates.sqlitedb (also available in an iTunes backup)

More research is needed on "User Manager Logs" as they seems containing useful timestamps to track device boot and application usage.

  

  

Go to Source
