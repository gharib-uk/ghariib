---
title: "A first look at Android 14 forensics"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "ces"
  - "eu"
  - "intel"
---

Android 14 was released to the public by the Open Handset Alliance on October 4, 2023, and is now available on various smartphones, including the Google Pixel.

This blog post aims to explore a list of the majr oartifacts you can find on this version of the Android OS. 

For testing and review, I set up a Google Pixel 7A and used it for about a month, with a SIM card and various native and third-party apps installed.

The blog post is organized by sections:

- Device information and general settings
- User accounts
- Information on Cellular, Wi-Fi, and Bluetooth connections
- Native Android applications
- Google applications
- Analysis of the use of native and third-party applications
- Other relevant information

As always, I'll try to update this blog post as I test and research.

## Device information and general settings

- **build.prop**

- TXT format
- Stored in the root directory of the system partition 
- Among other things, it contains the device manufacturer, the device model, the operating system version, and the build version
- aLEAPP plugin

- **global\_settings.xml**

- ABX format
- Stored in the userdata partition under "/system/users/0/"
- Contains global settings such as automatic time adjustment, Wi-Fi on/off, and Bluetooth on/off.

- **system\_settings.xml** 

- ABX format
- Stored in the userdata partition under "/system/users/0/"
- Contains system settings such as audio level and display brightness.

- **secure\_settings.xml**

- ABX format
- Stored in the userdata partition under "/system/users/0/"
- Contains secure settings such as the Android ID, the Bluetooth name, and accessibility. 
- aLEAPP plugin

- **googlesettings.db** 

- SQLite format 
- Stored in the userdata partition under "/data/com.android.gsf/databases/" 
- Contains settings for location services

- **gservices.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.gsf/databases/" 
- Contains various settings relating to Google services

- **persistent\_properties** 

- Protobuf format
- Stored in the userdata partition under "/property/" 
- Contains information about the local timezone and language settings

- **factory\_reset** and **factory\_reset\_record\_value**

- Stored in the userdata partition under "/misc/bootstat/" 
- The last modified timestamp corresponds to the timestamp of the factory reset 

- **setup\_wizard\_info.xml**

- XML format
- Stored in the userdata partition under "/data/com.google.android.settings.intelligence/shared\_prefs/"
- It stores the value "suw\_finished\_time\_ms", which corresponds to the timestamp of the device setup
- aLEAPP plugin

- **suggestions.xml**

- XML format
- Stored in the userdata partition under "/data/com.google.android.settings.intelligence/shared\_prefs/"
- It stores the values "com.android.settings.suggested.category.DEFERRED\_SETUP\_setup\_time" and "com.android.settings/com.android.settings.biometrics.fingerprint.FingerprintEnrollSuggestionActivity\_setup\_time", which match the device setup timestamp
- aLEAPP plugin

- **notification\_policy.xml**

- ABX format
- Stored in the userdata partition under "/system/"
- It tracks whether notifications are visible on the lock screen

I have covered the Android settings in a previous blog post "Analysis of Android settings during a forensic investigation". I recommend also reading the two posts by Josh Hickman where he describes how to analyze how an Android device was set up.

- Wipeout! Detecting Android Factory Resets
- Wipeout! Part Deux – Determining How an Android Was Setup

## Accounts information

- **0.xml**

- ABX format
- Stored in the userdata partition under "/system/users/0/"
- Contains information about the local user account, including the last logon date

- **accounts\_ce.db** 

- SQLite format
- Stored in the userdata partition under "/system\_ce/0/"
- Contains information about accounts used on the device, such as Gmail, Facebook, WhatsApp, Telegram, Signal, etc., possibly including passwords/tokens
- For each account

- Account name
- Account type
- Password/token

- aLEAPP plugin
- aLEAPP authtokens plugin

- **accounts\_de.db** 

- SQLite format
- Contains information about accounts used on the device, such as Gmail, Facebook, WhatsApp, Telegram, Signal, etc., possibly including passwords/tokens
- For each account

- Account name
- Account type
- Action type (action\_account\_add, action\_set\_password, and so on)
- Debug time

- aLEAPP plugin

- **accounts.xml**

- ABX format
- Stored in the userdata partition under "/system/sync/"
- Stores information about data synchronization via the Google Account

- **google\_account\_history.db**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gms/databases/"
- Contains a single table of interest ("AccountHistory")
- Stores the history of all Google accounts set up and used on the device

It's important to note this:

1. There can be more than one local user. The first user is user 0, and if you examine the "/system/users/" folder, you may find more local users. A fairly common example on a Samsung device is the local user that manages the "Secret Folder" and is usually numbered 150
2. Third-party applications aren't required to store information in the accounts\_ce.db or accounts\_en.db. These files must therefore be considered as a starting point, which must be expanded by analyzing all sandboxes of all relevant applications

## Cellular, Wi-Fi and Bluetooth configuration

## 

- telephony.db

- SQLite database
- Stored in the userdata partition under "/user\_de/0/com.android.providers.telephony/databases/" 
- Contains information about the SIM card(s) used on the device
- Contains a single table of interest ("siminfo")
- For each SIM card:

- ICCID ("icc\_id")
- IMSI ("imsi")
- Phone number ("phone\_number\_source\_ims")
- Display name ("display\_name")
- Name of the network operator ("carrier\_name")
- MCC ("mcc\_string")
- MNC ("mnc\_string")
- Country code ("iso\_country\_code")

- aLEAPP plugin

- MdpSimBasedDatabase

- SQLite database
- Stored in the userdata partition under  "/data/com.google.android.gms/databases/" 
- Contains information about the SIM card(s) used on the device

- Checkin.xml

- XML format
- Stored in the userdata partition under "/data/com.google.android.gms/shared\_prefs/"
- Contains the ICCID and the IMSI of the last used SIM Card, including timestamp

- constellation.db

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gms/databases/"
- Contains the IMSI of the last used SIM card
- Contains a single table of interest ("sim\_verifications")

- WifiConfigStore.xml

- XML format
- Stored in the userdata partition under "/misc/apexdata/com.android.wifi/" 
- Contains the list of connected Wi-Fi networks
- aLEAPP plugin

- WifiConfigStoreSoftAp.xml 

- XML format
- Stored in the userdata partition under "/misc/apexdata/com.android.wifi/" 
- Contains information about the Wi-Fi hotspot of the device
- aLEAPP plugin

- bt\_config.conf and bt\_config.bak 

- TXT format
- Stored in the userdata partition under "/misc/bluedroid/bt\_config.conf"
- Contains information about the Bluetooth configuration of the device and the history of connected Bluetooth devices
- Covered in a blog post by Kevin Pagano
- aLEAPP plugin

For SIM card related artifacts I recommend this blog post by Cellebrite "Hex Diving — The Easy Way to Uncover Hidden Forensic Artifacts".

## Installed applications and permissions

- **packages.list** and **package-usage.list**

- TXT Format
- Stored in the userdata partition under "/system/"
- Both files contain the list of installed packages, including, the package name, the package UID and the package sandbox path
- aLEAPP plugin

- **packages.xml**

- ABX format
- Stored in the userdata partition under "/system/"
- Contains information about installed packages, including, the package UID, the package installer (i.e. "com.android.vending" for apps that are installed via the Google Play Store or "com.google.android.packageinstaller" for apps that are installed via the package installer), and the permissions (not including run-time permissions)
- aLEAPP plugin

- **package-cstats.list**

- TXT format
- Stored in the userdata partition under "/system/"
- Contains statistics about the package manager compiler
- This file is useful as it usually contains references to an application, even after it has been uninstalled

- **roles.xml** 

- XML format
- Stored in the userdata partition under "/misc\_de/0/apexdata/com.android.permission/"
- Contains the list of applications set by default for certain functions in Android
- aLEAPP plugin

- **runtime-permissions.xml**

- XML format
- Stored in the userdata partition under "/misc\_de/0/apexdata/com.android.permission/"
- Contains the permissions classified as "dangerous" by Android
- For each package and each dangerous permission

- Granted (yes/no)
- Flag

- aLEAPP plugin

Some interesting blog posts you should read about these files are:

- Android’s “Dangerous” Permissions by Josh Hickman
- Android - Roles and Permissions (Android 10/11) by Scott Vance
- Mobile Forensics: Discovering the Undiscovered

Once you have identified the installed applications, you should analyze:

1. The "**/app/**" folder in the userdata partition, to find the APK for a specific app
2. The "**/data/**" folder in the userdata partition, to find an application sandbox
3. The "**/media/**" folder (also known as "Emulated SD-Card"), to find files saved there by an application

Other relevant databases for tracking installed applications are:

- **library.db** 

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Contains the library of applications for the Google user in use on the device. It is important to emphasize that this database is not related to the applications installed on the device: you can find here applications being installed by the same Google account on another device or on a previous installation of the device you are analyzing
- Contains a single table of interest ("ownership")
- For each application:

- Gmail account ("account")
- Package name ("doc\_id")
- Purchase date ("purchase\_time")

- aLEAPP plugin

- **localappstate.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Tracks applications installed on the device via the Google Play Store (not apps installed by other means)
- Contains a single table of interest ("appstate")
- For each application:

- Package name ("package\_name")
- Delivery data timestamp ("delivery\_data\_timestamp\_ms")
- Timestamp of the first download ("first\_download\_ms")
- Google account ("account")
- Application common name ("title")
- Last notified version ("last\_notified\_version")
- Timestamp of the last update ("last\_update\_timestamp\_ms")
- Timestamp of the installation request ("install\_request\_timestamp\_ms")

- The information is not removed when an application is uninstalled
- aLEAPP plugin

- **frosting.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Tracks the installation and update of applications regardless of how they were installed
- Contains a single table of interest ("frosting")
- For each application:

- APK Path ("apk\_path")
- Last Updated ("last\_updated")
- Package name ("pk")

- The information is removed when an application is uninstalled
- aLEAPP plugin

- **gass.db**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gms/databases/"
- Tracks applications installed on the device, no matter how they were installed
- Contains a single table of interest ("app\_info")
- For each application:

- Package name ("package\_name")
- Version ("version\_code")
- APK SHA-256 ("digest\_sha256")

- The information is not removed when an application is uninstalled
- aLEAPP plugin

- **app\_icons.db**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.nexuslauncher/databases/"
- Tracks information about applications' icons 
- Contains a single table of interest ("icons")
- For each application:

- Label ("label")
- Icon Color ("icon\_color")
- Last Updated ("last\_updated")
- Version ("version")

- The information is removed when an application is uninstalled
- aLEAPP plugin

- **launcher\_4\_by\_5.db**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.nexuslauncher/databases/"
- Tracks information about the application icons 
- Contains a single table of interest ("favorites")
- For each application:

- Application name ("title")
- Application intent ("intent")
- Screen ("screen")
- PNG Icon ("icon")
- Modified timestamp ("modified")

- **install\_queue.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Tracks installation activities
- Contains a single table of interest ("install\_requests")
- For each application:

- Reason ("reason"): contains values like "single\_install", "webapk\_install", "auto\_update", etc.
- Package name ("pk")

- The information is not removed when an application is uninstalled

- **auto\_update.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Tracks the automatic update for applications
- Contains a single table of interest ("auto\_update")
- The information is not removed when an application is uninstalled

- **xternal\_referrer\_status.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/databases/"
- Contains a single table of interest ("xternal\_referrer\_status\_store")
- The information is not removed when the app is uninstalled

## Native applications

- **Calendar (calendar.db)**: 

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.calendar/databases/"
- aLEAPP plugin

- **Contacts (contacts2.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.contacts/databases/"
- aLEAPP plugin

- **Contacts (icing\_contacts.db)**
    - SQLite format
    - Stored in the userdata partition under "/data/com.google.android.gms/databases/"
- **Call history (calllog.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.contacts/databases/"
- aLEAPP plugin

- **Downloads (download.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.downloads/databases/"
- aLEAPP plugin

- **SMS/MMS (mmssms.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.telephony/databases/"
- aLEAPP plugin

- **User dictionary (user\_dict.db)**: 

- SQLite format
- Stored in the userdata partition under "/data/com.android.providers.userdictionary/databases/"
- aLEAPP plugin

## Google applications

- **Google Android messaging (bugle\_db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.messaging/databases"
- aLEAPP plugin

- **Phone by Google (dialer.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.dialer/databases/"

- **Phone by Google (annotated\_call\_log.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.dialer/databases/"

- **Phone by Google (phone\_lookup\_history.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.dialer/databases/"

- **Google Mail (bigTopDataDB.-<User\_ID>)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gm/databases/"
- aLEAPP plugin

- **Files by Google (files\_master\_database)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.nbu.files/databases"
- Covered in a blog post by Kevin Pagano
- aLEAPP plugin

- **Google Photos (**gphotos-0.db /** gphotos-1.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.photos/databases/"ù
- aLEAPP plugin

- **Google Maps** 

- Various files of interest
- Stored in the userdata partition under "/data/com.google.android.apps.maps/databases"
- The most important artifacts are covered in a blog post by Josh Hickman and in blog post by Kibaffo33
- aLEAPP plugin

- **Google Docs (cello.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.docs.editors.docs/app\_cello/<email\_address>/"
- Covered in a blog post by Kevin Pagano

- **Google Docs (DocList.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.docs/databases/"
- Covered in a blog post by Kevin Pagano
- aLEAPP plugin

- **Google Keep (keep.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.keep/databases/"
- Covered in this blog post

- **GBoard - Google Keyboard (trainingcachev3.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.inputmethod.latin/databases/"
- Covered in a blog post by Kibaffo33 and in a blog post by Yogesh Khatri
- aLEAPP plugin

- **Google Meet (tachyon.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.tachyon/databases/"
- Covered in a blog post by Kevin Pagano
- aLEAPP plugin

- **Google Calendar (cal\_v2a)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.calendar/databases/"

## Native and third-party applications usage analysis

- **recent\_tasks** folder and **snapshots** folder

- Tasks in ABX format, Snapshots in JPG format
- Stored in the userdata partition under "/system\_ce/0/"
- Tracks the recent activities by application, including the last snapshot
- For each application activity:

- Package name
- Last moved date
- Application activity
- Application data
- Application screenshot (JPG file)

- The information is removed when an application is closed and/or it is uninstalled
- Covered in a blog post byAlexis Brignoni
- aLEAPP plugin

- **netstats** folder

- Proprietary format
- Tracks network usage statistics
- Stored in the userdata partition under "/system/netstats"
- The information is retained for a certain period of time (in my experience, up to 60/90 days)
- The Information for a certain application is removed when the application is uninstalled

- **usagestats** folder

- Tracks statistics about application usage
- Daily, weekly, monthly, and yearly statistics
- Stored in the userdata partition under "/system\_ce/0/usagestats"
- The information is retained even after an application has been uninstalled, usually up to one year later
- Covered in two blog posts by Alexis Brignoni and Yogesh Khatri
- aLEAPP plugin

- **batterystats** 

- Various artifacts related to battery usage and battery statistics
- Stored in various files and format

- /system/batterystats-daily.xml

- ABX format

- /system/batterystats.bin

- Proprietary/unknown format

- /system/battery-history.bin

- Proprietary/unknown format

- /system/battery-usage-stats\*.bus

- ABX format

- /system/batterystats-checkin.bin

- Proprietary/unknown format

- One of the least explored artifacts on Android
- It may contain information about application usage, even after the application has been uninstalled

- **Battery Usage (battery-usage-db-v9)**

- SQLite format
- Stored in the userdata partition under "/user\_de/0/com.android.settings/databases/"
- It has a different name than in previous Android versions
- The previous version is described in a blog post by Kevin Pagano
- aLEAPP plugin for older database

- **jobs**

- ABX Format
- Stored in the userdata partition under "/system/job/"
- Various files, that track "jobs" for each package
- Unexplored artifact

- **Privacy Dashboard**

- ABX format
- Stored in the userdata partition under "/system/appops/discrete"
- Tracks the use of dangerous permissions
- Covered in a blog post by Josh Hickman
- aLEAPP plugin

- **Digital Wellbeing (app\_usage**)

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.wellbeing/databases/"
- Covered in a blog post by Josh Hickman
- aLEAPP plugin

- **Firebase Cloud Messaging**

- LevelDB format
- Stored in the userdata partition under "/data/com.google.android.gms/files/fcm\_queued\_messages.ldb
- Used by applications to send/receive messages via a shared conduit
- Covered in a blog post by Alex Caithness
- aLEAPP plugin

- **Devices Health Services (Turbo) Application Usage (app\_usage\_stats.xml)**

- XML Format
- Stored in the userdata partition under "/data/com.google.android.apps.turbo/shared\_prefs/"
- Device Health Service monitors the battery consumption
- Covered on DFRWS Review by Kevin Pagano
- aLEAPP plugin

- **Android System Intelligence (SimpleStorage)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.as/databases/"
- It tracks the launch of applications
- Briefly covered in a blog post by Cellebrite
- aLEAPP plugin

- **Graphic Stats**

- Proprietary/unknown
- Stored in the userdata partition under "/system/graphicsstats/"
- Can be useful to track application usage
- Only persists for a short time

- **Notification History**

- TXT format
- Stored in the userdata partition under "/system\_ce/0/notification\_history/history/"
- Stores notification history, including the content, for 24 hours
- Must be activated by the user (not active by default)

- **Google Play Protect (odad\_datastore.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.odad/databases/"

- **appops.xml**

- ABX format
- Stored in the userdata partition under "/system/"
- Packages permissions usage
- aLEAPP plugin

- **appops\_accesses-xml**

- ABX format
- Stored in the userdata partition under "/system/"
- Packages permissions use

- **data\_usage.db**

- SQLite format
- Stored in the userdata partition under "/data/com.android.vending/"
- Packages data usage

## Other relevant information

- **Devices Health Services (Turbo) (turbo.db) - Battery Level and Timezone**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.apps.turbo/databases/"
- Tracks the battery level of the phone
- Contains a single table of interest ("battery\_event")
- Each row in the table contains

- Timestamp in millisecond ("timestamp\_millis")
- Battery level ("battery\_level")
- Charge type ("charge\_type")
- Timezone ("timezone")

- aLEAPP plugin

- **Google Play Searches (suggestions.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.vending/databases/"
- Stores the keywords searched in the Google Play Store
- Contains a single table of interest ("suggestions")
- For each search:

- Date ("date")
- Searched term ("query")

- Covered in a blog post by Forensafe
- aLEAPP plugin

- **Emulated SD Card (external.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.providers.media.module/databases/"
- Contains details about files stored in the emulated SD card
- Various tables of interest
- Covered in a blog post by Josh Hickman
- aLEAPP plugin

- **Shutdown Checkpoints**

- TXT format
- Stored in the userdata partition under "/system/shutdown-checkpoints/"
- Tracks device shutdown
- Covered in a blog post by Kevin Pagano
- aLEAPP plugin

- **Google Cast Devices (cast.db)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gms/databases"
- Tracks Google devices that are connected to the smartphone (such as Google Home Hub and Google Home Speaker)
- Covered in a blog post by deagler
- aLEAPP plugin

- **Android Pay (android\_pay)**

- SQLite format
- Stored in the userdata partition under "/data/com.google.android.gms/databases"

- **ADB Keys**

- TXT format
- Stored in the userdata partition under "/misc/adb/adb\_keys"
- Hosts connected via ADB
- aLEAPP plugin

Go to Source
