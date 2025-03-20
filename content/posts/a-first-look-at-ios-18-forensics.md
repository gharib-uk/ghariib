---
title: "A first look at iOS 18 forensics"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

This has been a tough year for me: my mom passed away in June, and I'm still slowly recovering from the hard blow. It's time to start again doing what I love: researching and sharing!

It's early September and like every year, that moment is approaching when everyone who deals with mobile forensics starts to tremble at the thought of the arrival of a new version of iOS!

First the good news: the basic and traditional techniques for logical acquisition (or Advanced Logical, if you want to call it that) still work on iOS 18!

A new cross-platform open-source tool has recently become available that enables this type of extraction from various types of Apple devices (iPhone, iPad, Apple TV, Apple Watch). 

The UFADE tool https://github.com/prosch88/UFADE), developed and maintained by Christian Peter, fully supports iOS 18 and was used to perform the tests described in this article.

The tests were performed on an iPhone 12 with **iOS 18** beta operating system, which was **reinstalled after a reset and without restoring from a previous backup**. After configuration, an existing iCloud account was logged in, with subsequent synchronization of the existing data (e.g. photos, contacts, etc.).

![](https://blogger.googleusercontent.com/img/a/AVvXsEidtDWABqZoMyGu9BEkrkV8xwpqWtxZkuBtULS-faBWGgzfLmkiSKupv-OKU4KYCU3l5FP9FXCBQspck9Ws_vflnIRI9aSgPVjhapZQIwuQo6iTHQSSCBNPTpQqzWkX_155ntO2swy2GtZGPw2xrnuANaUw0V8X5xfdr7MjUwYLqfaFVZ1afsPl8NIdsyk=s16000)

After installation, the device was only used with native applications, and various acquisitions were performed:

- **iTunes backup**
- **AFC**
- **Crash logs and Sysdiagnose**

While we wait for one of the gray/green/black/blue/orange tools that will allow us to obtain a full file system, we need to optimize the data we can extract by combining the traditional techniques.

  

Among the different techniques, the **iTunes backup** (encrypted, of course!) is still the one that can extract the most data. Below is a short list of the most important files already present in previous versions of iOS and available in an iTunes backup with iOS 18.

#### Basic device information and configuration

- **Device Model Number and Device Name**

- /private/var/preferences/SystemConfiguration/preferences.plist

- **Serial Number**

- /private/var/root/Library/Caches/locationd/consolidated.db

- **Last Known ICCID**

- /private/var/wireless/Library/Preferences/com.apple.commcenter.plist

- **SIM Card information**

- /private/var/wireless/Library/Databases/CellularUsage.db
- /private/var/wireless/Library/Preferences/com.apple.commcenter.data.plist

- **Control Center**

- /private/var/wireless/Library/ControlCenter/ModuleConfiguration.plist

- **Timezone**

- /private/var/mobile/Library/Preferences/com.apple.AppStore.plist

- **Location Services setting**

- /private/var/mobile/Library/Preferences/com.apple.locationd.plist

- **Find my iPhone settings**

- /private/var/mobile/Library/Preferences/com.apple.icloud.findmydeviced.FMIPAccounts.plist

- **Backup information**

- /private/var/mobile/Library/Preferences/com.apple.MobileBackup.plist
- /private/var/mobile/Library/Preferences/com.apple.mobile.ldbackup.plist

#### Native Apps

- **Accounts**

- /private/var/mobile/Library/Accounts/Accounts3.sqlite

- **Address Book**

- /private/var/mobile/Library/AddressBook/AddressBook.sqlitedb
- /private/var/mobile/Library/AddressBook/AddressBookImages.sqlitedb

- **Calendar**

- /private/var/mobile/Library/Calendar/Calendar.sqlitedb
- /private/var/mobile/Library/Calendar/Extras.db

- **Call History (Phone and FaceTime)**

- /private/var/mobile/Library/CallHistoryDB/CallHistory.storedata

- **Files**

- /private/var/mobile/Library/Application Support/CloudDocs/session/db/client.db
- /private/var/mobile/Library/Application Support/CloudDocs/session/db/server.db

- **Maps**

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Maps/MapsSync\_0.0.1

- **Media (Pictures/Videos)**

- /private/var/mobile/Media/DCIM/\*
- /private/var/mobile/Media/PhotoData/Photos.sqlite
- /private/var/mobile/Media/PhotoData/CPL/\*
- /private/var/mobile/Media/PhotoData/Mutations/\*
- /private/var/mobile/Media/PhotoData/Thumbnails/\*

- **Notes**

- /private/var/mobile/Library/Notes/notes.sqlite

- **Safari**

- /private/var/mobile/Library/Safari/Bookmarks.db
- /private/var/mobile/Library/Safari/History.db
- /private/var/mobile/Library/Safari/SafariTabs.db

- **SMS/MMS/iMessage**

- /private/var/mobile/Library/SMS/sms.db
- /private/var/mobile/Library/Preferences/com.apple.MobileSMS.plist

- **Shortcuts**

- /private/var/mobile/Library/Shortcuts/Shortcuts.sqlite

- **Voicemail**

- /private/var/mobile/Library/Voicemail/voicemail.db

- **Weather**

- /private/var/mobile/Containers/Shared/AppGroup/<GUID>/Library/Preferences/group.com.apple.weather.plist

#### Networks and connected devices

- **Bluetooth**

- /private/var/containers/Shared/SystemGroup/<GUID>/Library/Database/com.apple.MobileBluetooth.ledevices.paired.db
- /private/var/containers/Shared/SystemGroup/<GUID>/Library/Database/com.apple.MobileBluetooth.ledevices.other.db

- **Wi-Fi Networks**

- /private/var/preferences/com.apple.wifi.known-networks.plist
- /private/var/preferences/SystemConfiguration/com.apple.wifi-private-mac-networks.plist

- **Home Kit (IoT Devices)**

- /private/var/mobile/homed/datastore3.sqlite

#### Application usage

- **Application State (Installed applications)**

- /private/var/mobile/FrontBoard/applicationState.db

- **iOS Screen Layout**

- /private/var/mobile/Library/SpringBoard/IconState.plist

- **TCC (Transparency, Consent and Control)**

- /private/var/mobile/Library/TCC/TCC.db

- **Data Usage**

- /private/var/wireless/Library/Databases/DataUsage.sqlite

#### Pattern of life

- **Health**

- /private/var/mobile/Library/Health/healthdb.sqlite
- /private/var/mobile/Library/Health/healthdb\_secure.sqlite

- **Keyboard Usage Stats**

- /private/var/mobile/Library/Keyboard/user\_model\_database.sqlite

- **InteractionC**

- /private/var/mobile/Library/CoreDuet/People/interactionC.db

- **Personalization Portrait**

- /private/var/mobile/Library/PersonalizationPortrait/PPSQLDatabase.db

- **Recents**

- /private/var/mobile/Library/Recents/Recents

The **AFC protocol** can be used to extract multimedia files (even if an iTunes backup password is already set on the device). In combination with the iTunes backup (so-called "Advanced Logical" or "Logical+"), additional files can be extracted, e.g. the **Media Library** of the iCloud account (/private/var/mobile/Media/iTunes\_Control/iTunes/MediaLibrary.sqlitedb).

  

The **Crash Reporter** protocol can be used to extract crash logs as well as generated "sysdiagnose" stored in "/private/var/mobile/Library/Logs/CrashReporter/".

If you do not know what a "sysdiagnose" is, I recommend reading my 2019 paper available at https://www.for585.com/sysdiagnose. UFADE also supports the direct creation and extraction of a "sysdiagnosis". In my personal opinion, **the extraction of sysdiagnose should always be included in an extraction workflow**, especially if an exploit to obtain a FFS is not available. sysdiagnose also contains Apple Unified Logs, which can be crucial in certain types of investigations (e.g. a car accident).

  

When analyzing the sysdiagnose created under iOS 18, we can find the following relevant files and folders that were extracted from the iOS file system but are not included in an iTunes backup:

- **Mobile Installation Logs** (useful to track app installation and uninstall)

- /private/var/installd/Libary/Logs/MobileInstallation/

- **Mobile Activation Logs** (useful to track device power on)

- /private/var/mobile/Library/Logs/mobileactivationd/\*

- **Lockdownd Logs** (useful to track passcode changes)

- /private/var/logs/lockdownd.log

- **Shutdown Logs** (useful to track device power off/shutdown)

- /private/var/db/diagnostics/shutdown.log

- **Logd Log** (useful to track device power off/shutdown)

- /private/var/db/diagnostics/logd.N.log

- **PowerLog.PLSQL** (partial)

- privatevarcontainersSharedSystemGroup<GUID>LibraryBatteryLifeCurrentPowerlog.PLSQL

- **Wi-Fi Manager Logs**

A sysdiagnose also contains files that are typically included in an iTunes backup, such as the TCC database and the plist file for saving Wi-Fi networks. R**emember that if a backup password is already set on the device and you reset it, one of the files affected is the list of known Wi-Fi networks!** Another reason why extracting a sysdiagnose is a good choice!

  

The logs are reorganized with different paths within a sysdiagnose file. If you want to process them with conventional forensic tools, you need to restore the correct file system (at the moment the only option is... manually). In the picture you can see the output generated by iLEAPP after path reordering.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiOv0uYk2P-I1MvnHMKf4Zwt_N2m9u1PVVxVct3Whs8LBFEVouzU5XTgwRaiNtsfxFJ1qbBFGqT-TC0-jof6g0PX0MTHbpracPkXeZYTICvViIcNhQRAE490i3b-3nlZL4EAHchePpvU5FaCfXuy03ygj-diiL5mZlG1acHt_7njvdvOskUOFKPR3a249w=s16000)

  

By processing test acquisitions with different tools I found that they all failed in processing DataUsage.sqlite, so possibly something has changed in iOS 18 with this file. 

  

Let us finish with the bad news: what are you missing out on without a FFS extraction?

  

Short answer: **a lot!** Not only in terms of third-party applications (Telegram or Signal, to name but a few) but also for native databases and logs.

  

Detailed answer: Here is a list of the most important files that **cannot be extracted** with Advanced Logical (Logical+ in the UFADE GUI):

- **Emails**
- **Location-related artifacts**

- Cached locations (Cache.sqlite)
- Wi-Fi/Cell towers locations (cache\_encryptedB.db)
- Motion History (cache\_encryptedC.db)
- Significant Location (local.sqlite/cloud-V2.sqlite)
- FindMy Devices

- **Device usage-related artifacts**

- Notifications (DuetExpertCenter folder)
- CurrentPowerLog.PLSQL (complete)
- Biome
- knowledgec.db
- Network Usage (netusage.sqlite)
- Aggregated Dictionary (ADDataStore.sqlitedb)
- Screen Time (RMAdminStore-Local.sqlite / RMAdminStore-Cloud.sqlite)

  

Go to Source
