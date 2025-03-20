---
title: "iOS 15 Image Forensics Analysis and Tools Comparison  - Processing details and general device information"
date: 2025-01-15
categories: 
  - "general"
---

As explained in the first blog post, I would like to start discussing the acquisition and processing details.

The acquisition was done by Josh Hickman using the Cellebrite Premium tool and the result is a Full File System capture in the traditional file format created by UFED.

If you open the file EXTRACTION \_FFS.zip ZIP you will see that UFED organizes the extracted full file system into 5 subfolders;

- **filesystem1**: extraction of system partition, without the mount point “/private/var/mobile/” (“user data")
- **filesystem2:** extraction of the mount point for user data (“/private/var/mobile”)
- **metadata1:** metadata for files stored in “filesystem1”
- **metadata1:** metadata for files stored in “filesystem2”
- **extra:** iOS Keychain

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiG-mJRk-eDWqHunSP05m0r4r5fxh2x6RYBI5j-31XBOHCqIBAalhgOJ3IRyiqzagdNZrBIPxh-tUn5cl8W4neBa9opUl3rb2ILlWgEICU18EyYQ7uiNfLVxlJEUKi36-P63eopuOOoBZJVonQ6Rby4lcSY6JHLWHhKr1xt3Zn6172TLxWgAWWX_k2FJko=w640-h228)

  

  

As always, when an acquisition is made with UFED, the ZIP file is accompanied by a UFD file that contains all the details of the acquisition process.

  

UFED PA can load an acquisition created with UFED 4PC/Premium by simply loading the UFD file into PA GUI. The user has just a few options to select, including hash sets and recovering data from archives. Some tools, namely XAMN and Oxygen, support a direct import of the UFD file and thus a natural import of the Full File System extraction. Some tools do not support direct import of the UFD file. To ensure that as much data as possible was processed, I used two different approaches to get the best results: the first approach was to load the EXTRACTION \_FFS.zip directly, providing the keychain file if supported by the tool (for example, AXIOM); the second approach was to extract the contents of “filesystem2” and manually recreate the expected path “/private/var/”. Most tools were able to parse the data correctly both ways. A single tool, APOLLO, processes only extracted files, so the contents of “filesystem2” were extracted on an analysis PC, and the folder was processed.

As for tool versions, I tried to use the latest version available for each tool, and whenever possible I reprocessed the acquisition with different versions, including the most recent ones. I am sure there could be an updated version that I haven’t checked yet that parses a bit more. I provide here the last version I used for each tool:

- AXIOM 7.5
- UFED Physical Analyzer 7.63.0.126
- XAMN 7.7 (Beta version, directly provided by MSAB)
- Oxygen Forensics Detective 16.0
- Belkasoft Evidence Center X
- ArtEx 2.7.4
- APOLLO (update with latest plugins as of September 26th 2023)
- iLEAPP 1.18.7 (update with latest plugins as of September 26th 2023)

After processing, I divided the parsed data into 4 different categories to make it easier to understand and reproduce the comparison: 

- General device information
- Native application configuration and data
- Third party application configuration and data
- Native logs tracking specific activities (interaction with the device, geolocation, traces of deleted/uninstalled apps, etc.)

It is important to clarify one aspect of the research: **I only considered “parsed data”**. Using a tool that has in-content search capabilities or special file viewers, you could manually search for the information. But that's outside the scope of this investigation: I want to check which information is parsed by which tool, and whether it was parsed as expected. To check and validate the non-parsed values, I searched for the expected value among the parsed data (a kind of backward search).

It is possible that I missed something, so I ask people and vendors to check my results. I am happy to correct and update the following tables.

Let’s start with the **general information** about the device. I have organised this section into 6 subsections:

- Hardware information
- iOS and SIM card information
- iOS wipe and setup
- iOS basic settings
- Apple Account(s)
- Installed applications

As for the **hardware information**, I identified that once in the table as relevant. The first column contains a description of the content, the second column contains the expected value, and the third column contains the source of the information, that is, the file containing the information.

|   **Description**   |   **Value**   |   **Sources**   |
| --- | --- | --- |
|   Model (Internal Name)   |   D201AP   |   privatevarpreferencesSystemConfigurationpreferences.plist   |
|   Retail Name / Model   |   Apple iPhone 8   (GSM+CDMA)   |   privatevarpreferencesSystemConfigurationpreferences.plist   privatevarmobileLibraryCachescom.apple.findmy.fmipcoreDevices.data   privatevarcontainesDataSystem<GUID>Libraryactivation\_recordsactivation\_record.plist   |
|   Model number   |   A1905   |   privatevarpreferencesSystemConfigurationpreferences.plist   |
|   Model Identifier   |   iPhone10,4   |   privatevarpreferencesSystemConfigurationpreferences.plist   |
|   IMEI   |   356763089830742   |   privatevarwirelessLibraryPreferencescom.apple.commcenter.device\_specific\_nobackup.plist   privatevarmobileLibraryLogsmobileactivationdcollection\_oob\_request.txt   privatevarmobileLibraryLogsmobileactivationd   |
|   Serial Number   |   FFMW4BH5JC67   |   privatevarrootLibraryCacheslocationdconsolidated.db   privatevarcontainesDataSystem<GUID>Libraryactivation\_recordsactivation\_record.plist   privatevarmobileLibraryLogsmobileactivationd   |
|   UDID   |   c50d35ac85f428883e3b6   fa3893599da85f708ea   |   privatevarrootLibraryCacheslocationdcache.plist   privatevarrootLibrarylocationduser.plist   privatevarcontainesDataSystem<GUID>Libraryactivation\_recordsactivation\_record.plist   privatevarmobileLibraryLogsmobileactivationd   |
|   Wi-Fi Mac Address   |   F0:98:9D:35:40:00   |   privatevarpreferencesSystemConfigurationNetworkInterfaces.plist   |
|   MAC Address EN1   |   F2:98:9D:35:40:02   |   privatevarpreferencesSystemConfigurationNetworkInterfaces.plist   |
|   MAC Address EN1   |   F2:98:9D:35:40:FD   |   privatevarpreferencesSystemConfigurationNetworkInterfaces.plist   |
|   Bluetooth Mac Address   |   F0:98:9D:35:40:01   |   backup\_keychain\_v2.plist   |

When analyzing the data parsed by the tools, I found that none of the tools did incorrect parsing, but then again, none of the tools parsed everything either. APOLLO, which is intended more as a tool for advanced analysis, does not have parsers for basic device information. It is important to highlight that some values are in some way redundant (model number and internal name) and are equivalent for their scope, while other values are specific and peculiar to the device and therefore should rather be extracted (e.g. serial number or UDID). It's relevant to clarify that, through testing, I verified that MSAB XRY/XAMN can extract and parse also Model Internal Name and Bluetooth Mac Address. The results for each value and tool are shown in the figure, where YES means that the value was parsed and parsed correctly, and NO means that the value was not parsed.

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|   Model (Internal Name)   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |   NO   |
|   Retail Name / Model   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |
|   Model number   |   NO   |   NO   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Model Identifier   |   YES   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   IMEI   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |   NO   |
|   Serial Number   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   UDID   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Wi-Fi Mac Address   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   MAC Address EN1   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   MAC Address EN1   |   NO   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Bluetooth Mac Address   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |

As for **software and SIM card information**, I identified the once in the table as relevant. The first column contains a description of the content, the second column contains the expected value, and the third column contains the source of the information, i.e., the file containing the information.

|   **Description**   |   **Value**   |   **Sources**   |
| --- | --- | --- |
|   iOS Version   |   iPhone OS 15.3.1   |   privatevarinstalldLibraryMobileInstallationLastBuildInfo.plist   privatevarrootLibraryLockdowndata\_ark.plist   SystemLibraryCoreServicesSystemVersion.plist   |
|   Build Version   |   19D52   |   privatevarinstalldLibraryMobileInstallationLastBuildInfo.plist   |
|   Device Phone Name   |   This Is’s iPhone   |   privatevarrootLibraryLockdowndata\_ark.plist   |
|   Device Phone Number   (MSISDN)   |   19195794674   |   privatevarwirelessLibraryPreferencescom.apple.commcenter.plist   |
|   DSID   |   17193901029   |   privatevarmobileLibraryPreferencescom.apple.itunescloud.plist   privatevarmobileMediaiTunes\_ControliTunesMediaLibrary.sqlitedb   |
|   ICCID   |   8901260971148676693   |   privatevarwirelessLibraryPreferencescom.apple.commcenter.plist   privatevarwirelessLibraryDatabasesCellularUsage.db   |
|   IMSI   |   310260974867669   |   privatevarwirelessLibraryPreferencescom.apple.commcenter.plist   |
|   Advertising ID   |   72345DE-4105-4A87- A51F-561FD4F2AF3D   |   privatevarcontainersSharedSystemGroup<GUID>LibraryCachescom.apple.lsdidentifiers.plist   |
|   Airdrop ID   |   be241deee67a   |   privatevarmobileLibraryPreferencescom.apple.sharingd.plist   |

The parsing results for each tool are shown in the figure. When it comes to the ICCID it’s import to highlight that iLEAPP only parse the **com.apple.commcenter.plist**, while others parse also the **CellularUsage.db**, containing historical information about SIM card used on the device. An interesting value that is parsed only by AXIOM and iLEAPP is the DSID (https://www.theiphonewiki.com/wiki/DSID ) while the Airdrop ID is parsed only Cellebrite, Oxygen and ArtEx.

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|   iOS Version   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   Build Version   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Device Phone Name   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |
|   Device Phone Number (MSISDN)   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   DSID   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |
|   ICCID   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |
|   IMSI   |   NO   |   YES   |   NO   |   YES   |   NO   |   NO   |   YES   |
|   Advertising ID   |   NO   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |
|   Airdrop ID   |   NO   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |

As for **device wiping and setup**, I identified the once in the table as relevant. The first column contains a description of the content and the second column contains the source of the information, i.e., the file containing the information.

|   **Description**   |   **Sources**   |
| --- | --- |
|   Device Wipe   |   privatevarrootLibraryLogsMobileContainerManagercontainermanagerd.log.0   privatevarroot.obliterated   |
|   Setup Date   |   privatevarmobileLibraryPreferencescom.apple.purplebuddy.plist   |
|   Setup Type   |   privatevarmobileLibraryPreferencescom.apple.purplebuddy.plist   |
|   Backup Restore   |   privatevarrootLibraryPreferencescom.apple.MobileBackup.plist   |

Two tools (Cellebrite PA and ArtEx) analyzed and reported the **wiping time** as April 11th, 2023 14:44:31 UTC using the timestamp of the **.obliterated** file (the traditional old method), while ArtEx also reports April 11th, 2023 14:44:50 UTC using the **containermanager.log.0** file as confirmation. XAMN reports April 11th 023 14:44:54 UTC from the **general.log** file.

The traditional method and manual analysis confirmed and validated the timestamps:

- **Keybagd.log.3** has the first entry on Tue Apr 11 14:44:32 2023
- **Mobile\_installation.log.1** has this entry “Tue Apr 11 07:45:45 2023 \[206\] (0x16f72b000) MIIsBuildUpgrade: The latest build information was not found; we need to upgrade from before 8.0 or this is a delete installation", where the timestamp is in the Cupertino time zone
- **Lockdownd.log** has this entry “04/11/23 07:44:51.388024 pid=76 \_load\_dict: Failed to load /private/var/root/Library/Lockdown/data\_ark.plist.”, where the timestamp is in the Cupertino time zone

AXIOM did not report a wiping time, but analyzed and reported the **device setup timestamp** (April 15th 2023 13:59:16), while iLEAPP did not report a deletion time, but analyzed the **device backup restore timestamp** (April 15rh 2023 14:08:21). Belkasoft did not analyze this information.

As for **device settings**, I identified the ones in the table as relevant. The first column contains a description of the content, the second column contains the expected value, and the third column contains the source of the information, i.e., the file containing the information.

|   **Description**   |   **Value**   |   **Sources**   |
| --- | --- | --- |
|   Location Service   |   Enabled   |   varmobileLibraryPreferencescom.apple.locationd.plist   |
|   Last Backup Date/Time   |   20/05/2023 01:40:54   |   privatevarmobileLibraryPreferencescom.apple.ldbackup.plist   |
|   Find My iPhone   |   Enabled   |   privatevarmobileLibraryPreferencescom.apple.icloud.findmydeviced.FMIPAccounts.plist   |
|   Timezone   |   America/New\_York   |   privatevarmobileLibraryPreferencescom.apple.AppStore.plist   privatevardbtimezonelocaltime   |
|   Timezone Setting   |   Automatic   |   privatevardbtimedLibraryPreferencescom.apple.preferences.datetime.plist   |
|   Message Retention Duration   |   Forever   |   privatevarmobileLibraryPreferencescom.apple.MobileSMS.plist   |
|   Last Hotspot Activity   |   17/5/2023 12:26:25   |    |

Some of the relevant settings were parsed only by some tools, as indicated in the table. Among others, the time zone setting is important to match timestamps, and the date of the last backup is important to decide whether it is worth looking for this backup (locally or in the iCloud). It's relevant to clarify that, through testing, I verified that MSAB XRY/XAMN can extract and parse Timezone and Last Backup Date/Time.  

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|   Location Service   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Last Backup Date/Time   |   YES   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |
|   Find My iPhone   |   YES   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Timezone   |   NO   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |
|   Timezone Setting   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Message Retention Duration   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |
|   Last Hotspot Activity   |   NO   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |

All the tools correctly identified the **Apple account** on the device, and all of them parsed the **Accounts3.sqlite** file at some level, providing different amounts of information. The following table lists the information parsed by each of the tools for Apple accounts.

|   **Description**   |   **AXIOM**   |   **PA**   |   **OXYGEN**   |   **XAMN**   |   **BELKASOFT**   |   **ARTEX**   |   **APOLLO**   |   **ILEAPP**   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   Timestamp   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |   NO   |   YES   |
|   Account type   |   YES   |   YES   |   NO   |   NO   |   YES   |   NO   |   NO   |   YES   |
|   Username   |   YES   |   YES   |   YES   |   YES   |   YES   |   YES   |   NO   |   YES   |
|   Description   |   YES   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |   YES   |
|   Identifier   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |
|   Bundle ID   |   YES   |   NO   |   NO   |   NO   |   YES   |   NO   |   NO   |   YES   |
|   Credential Type   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |
|   Parent Account ID   |   YES   |   NO   |   NO   |   NO   |   NO   |   NO   |   NO   |   YES   |

The approach used by tools to search for **installed applications** is quite different. Most of them rely on the **applicationstate.db** file to generate the list of installed applications, while some of them integrates this analysis with additional sources, such as:

- the **IconState.plist**, available at privatevarmobileLibrarySpringBoardIconState.plist and containing iOS Home Screen Item
- the **iTunesMetadata.plist** and **BundleMetadata.plist**, both available in each Bundle folder.
- the **.com.apple.mobile\_container\_manager.metdata.plist**, available in the root folder of each App Container

All tested tools have a function to identify application information and provide relevant information for analysis.

An important aspect of analyzing apps is identifying **permissions**. If you know what permissions are granted, you can prioritize your investigation. For example, if an application has access to geolocation, it is worth investigating that app for traces of location; on the other hand, if an app does not have access to geolocation, you will investigate first other apps.

The **TCC.db** is parsed by AXIOM, XAMN, APOLLO and iLEAPP. Apparently, it is not parsed by Cellebrite PA and Oxygen. All tools report the bundle ID, the service (es. AddressBook, Calendar, Camera, Liverpool, Microphone, Motion, Photos, PhotosAdd, Siri, Ubiquity, etc.) and whether the permission was granted or not.

APOLLO and iLEAPP also reports the timestamp when the user has granted or not granted permission, while AXIOM does not display the timestamp, but does display the “prompt count," which records the number of times the user is prompted for permission on the user screen. XAMN does not evaluate the timestamp and prompt count.

An interesting artifact related to applications is the **mobile\_installation\_log**: It could be used to identify installation and uninstallation activities and to define a time frame for the usage of each application (see here for a detailed reference https://dfir.pubpub.org/pub/e5xlbw88/release/2). This artifact is analyzed by AXIOM, BELKASOFT and iLEAPP: BELKASOFT analyzes only the entries for successful installation, while AXIOM analyzes both installation and uninstallation, and iLEAPP also analyzes the entries related to a reboot of the device. In the figure you can see an excerpt from the mobile installation logs parsed by iLEAPP.

![](https://blogger.googleusercontent.com/img/a/AVvXsEjHi7QNDIkWoh_IKZOnQHluFxuYX0OGGS3xZ50r9MXdrY2qUcD7JdijyxO5ZBd29XKKs8RaJ2BPEWcbVpWzh-7wngBHIveMp4IZl_7PlLbY1meRPT_ayxwC-AHu73-6u8PbTBu4eNuCPePeCVDRsc7tRc4zRD7fWIwumpkv5u1D1gLR3IWLMhrc0fC_fBg=s16000)

As a last consideration, based on available research and papers, what is still missing that apparently is not parsed by any tool? I have at least two papers in mind, by Scott Koening.

- **iOS Setting Display Auto-Lock and Require Password**

- https://theforensicscooter.com/2021/09/05/ios-settings-display-auto-lock-require-passcode/
- https://dfir.pubpub.org/pub/khnqi0ff/release/1

- **iOS Location Services and System Services are they ON or OFF**

- https://theforensicscooter.com/2021/09/20/ios-location-services-and-system-services-on-or-off/
- https://dfir.pubpub.org/pub/4sv4kxyh/release/2

That’s all for this episode: see you soon with the next one about native applications configuration and data!

Go to Source
