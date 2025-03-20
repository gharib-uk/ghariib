---
title: "Has the user ever used the XYZ application? aka traces of application execution on mobile devices"
date: 2025-03-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "forensics"
  - "security"
  - "security-awareness"
  - "security-awareness-3-0"
---

A common question during a forensic investigation of a digital device is: "**Has the user ever used the XYZ application?**".

As always when answering this question, it is important to create and follow a solid process.

In this blog post, I want to share a possible process that everyone should customize based on their needs and roles.

The ability to answer the question depends on the acquisition that was obtained from the device. In this blog post, I will address the scenario where you have a full file system acquisition (or physical, for older devices).

# Check whether the XYZ application is installed

First, **check whether the XYZ application is still installed on the device at the time of the acquisition**.

On an iOS device, there are several easy ways to check this. To name just a few of the most common:

- Check if the XYZ bundle is available under privatevarcontainersBundleApplication<GUID>
- Check whether the XYZ App Sandbox folder is available under privatevarmobileContainersDataApplication<GUID>
- Check whether the XYZ App Shared folder is available under privatevarmobileContainersSharedAppGroup<GUID>
- Analyze:

- Mobile installation logs
- ApplicationState.db
- containers3.sqlite

This can also be checked in various ways on an Android device:

- Analyze the "/app" and "/data" folders in the "Userdata" partition and look for folders and APKs related to the package of interest.
- Analyze:

- packages.list
- packages.xml
- frosting.db
- localappstate.db
- gass.db

# If the XYZ application is installed

If the application is still installed, the next step is to **analyze all relevant files in the application's sandbox**. 

  

The first stage will probably be your tools, after that, you will need to dig deeper, analyze the files manually, and create your own queries/parsers.

  

On iOS devices, you can analyze various native artifacts to find and confirm traces of app usage. Some of the most commonly analyzed artifacts are:

- iOS notifications (useful for finding potentially deleted messages)
- Power logs (and backups)
- InteractionC
- KnowledgeC
- Biome 
- Screen time
- Net usage 
- Data usage
- TCC (permissions)
- Crash logs
- Store.db (historical use of the app by the iCloud user)

There are usually fewer traces on Android devices, but still a lot of information:

- Application permissions (packages.xml)
- Application run-time permissions (runtime-permissions.xml)
- Digital Wellbeing
- App ops
- Privacy Dashboard (Android 12 and higher)
- Firebase Cloud Messaging
- PkgPredictions.db (Samsung devices only)
- notification\_log.db (Samsung devices only, up to Android 10)
- Application activity/tasks
- Usage stats
- Network stats
- Battery stats
- Graphic stats
- library.db

It is also a good practice to search for:

- **App screenshots stored on the device**: common file formats on iOS are KTX (for automatically generated screenshots) and PNG (for user-generated screenshots), while on Android the common file formats are PNG and JPG.
- **Analysis of the images and videos saved by the app on the device**: for iOS devices this typically involves analyzing the files in the camera roll and analyzing the Photos.sqlite database; for Android devices, this means analyzing the emulated SD card used by apps to store files (i.e. multimedia)

You should also analyze **received SMS messages** to look for messages that the user received after installing the specific app or due to 2FA authentication.

  

Last but not least, especially for chat/messaging and browser apps, you can **analyze the main database and search for missing rows** to look for possibly deleted messages and the approximate timestamp.

# If the XYZ application is not installed

If the application is not installed, you should normally do so: 

- Check if the application has ever been used on this system
- Possibly recover some data from the application

It is important to emphasize that on modern devices that use file-based encryption, once an app is uninstalled, **the contents of the app sandbox are deleted and cannot be recovered**.

  

Therefore, it is important to understand and verify which artifacts are retained after uninstalling an app.

  

This topic has already been covered in various presentations and blog posts. The most interesting are:

- Mobile Forensics: Discovering the Undiscovered, 2020 by Scott Vance
- Tracking Traces of Deleted Applications, SANS DFIR Summit 2019 by Alexis Brignoni and Scott Vance
- iOS - Tracking Traces of Deleted Applications 2019 by Scott Vance
- Analysis of application installation logs on Android systems, 2019
- Every Step You Take: Application and Network Usage in Android, SANS DFIR Summit 2018 by Jessica Hyde

For iOS devices, based on my experience and previous testing, I suggest the following list as a starting point:

1. Analyze the "Mobile Installation Logs" and check which apps have been uninstalled and compare them to the list of actually installed apps
2. Search for SMS messages related to app usage (e.g. activation and 2FA messages)
3. iOS notifications
4. Screenshots created manually by the user that relate to the specific app
5. Photos.sqlite
6. InteractionC  
7. Power logs (and backups)
8. KnowledgeC
9. Biome
10. Screen time
11. Net usage 
12. Data usage
13. TCC (Permissions)
14. Store.db
15. Raw keyword search for the app bundle and the app common name
16. Syslog archive from a Sysdiagnose

On Android devices, I recommend this as a starting list:

1. Analyze "localappstate.db" and "gass.db", and check which apps were installed via the Google Play Store and compare them to the list of actual installed apps
2. Search for SMS messages related to app usage (e.g. activation and 2FA messages)
3. Files stored on the emulated SD card
4. Screenshots created manually by the user that relate to the specific app
5. Digital Wellbeing
6. Firebase Cloud Messaging
7. PkgPredictions.db (Samsung devices only)
8. notification\_log.db (Samsung devices only, up to Android 10)
9. Usage stats
10. Battery stats
11. Graphic stats
12. Library.db
13. Raw keyword search for the app bundle and the app common name

# Testing and validating the process

To test and validate the proposed procedures, I conducted an experiment. This is only a single test on two specific devices, so do not assume that you will find similar results in all real-world cases. This is especially true for Android devices, for which each individual manufacturer may have their own customizations and packages.

  

I had an iPhone 7, running iOS 15.7, and an Android device from Samsung, running Android 10 updated from Android 9 with Full Disk Encryption.

  

I installed two messaging apps on both devices: **Telegram** and **Signal**. 

  

The apps were used for a few days and exchanged messages between the two devices. 

  

I gave both apps all permissions on both devices and I enabled incoming notifications.

  

After a few days of use, I performed a full file system acquisition of the iPhone and a physical dump of the Samsung device with both apps still installed.

  

I then rebooted both devices and uninstalled both apps. I turned the devices off and back on and then I acquired them again. 

  

After a week of using both devices with other apps, I did another full file system acquisition of the iPhone and a physical dump of the Android device.

  

I plan further acquisitions after 14 days and after 30 days: this post will be updated as soon as the acquisitions are available.

  

I parsed all the acquisitions with various tools and did a thorough keyword search using the common names of the apps (Telegram and Signal) and the package/bundle names (for iOS "ph.telegra.Telegraph" and "org.whispersystems.signal", for Android "org.telegram.messenger" and "org.thoughtcrime.securesms").

## Analysis of the iOS device

First, I did a keyword search on the results generated by the tools, just to see the differences. Following are some comparison images with AXIOM and PA in the three different acquisitions: App still installed, App just installed, App uninstalled 6 days ago.

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **ph.telegra.Telegraph**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEht9lEZymJIflebjr50GK9i0-KIrOAOsUKNBslpCUsKAa5V7c-_5bAg1BVR5KlP_zha7rDHaErDuDmRCI1b5I74VZ2Zou6OweusBVkKZc_wkxjgJnR4VLQUR-y3Q0A23HAJ5yth_DTcmLBH7AkKjCHEQip-VrVZR6qD5ScnsElXa7Zxvg5dmlWeUvJB2ts=s16000)

  
Tool: **Cellebrite Physical Analyzer**

Keyword: **org.whispersystems.signal**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEggujfO8vH6kVaP7o-BAW52JZ_AavOdJxVr0WIxW4BwBLjzvcslwLwbG5dy4MEzo4zG1ioYIY05sEL7YNE73rqycrzMJbnjJ6iCfQLVWLvtgciW2TAnKxychBbDxmMxZlCARxv7ybT6gjfkVTfjqQS4TPRolaOn6iadvMhoB_O7c3CR5mk2iS5gJARqtrM=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **Telegram**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEidJrH_4bJiklw2FRUcCoT6o828G-oJZSY6IZvmB6rVgd2jxTGiqHNVqtc6Yqp0fjKQMWM8kfxi6mkL-JQ4tZbs-efQ-BZjIQ37KT_p1kFMXFD54e069KXkL-5HZNjAqxbOMLoONMgVwlvIuR5NDIeHTf5SC7cfweogovo743cEt2iMsWY-9RihTrxluXw=s16000)

  

Tool: **AXIOM**  
Keyword: **ph.telegra.Telegraph**  
Comparison between the 3 acquisitions. 

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhnHcaf4CRdfyMcHJlu_FY9m2amEvemPoRgHhKbS6cDZXHMFrNw3Oncw2SN_VyyiC-uifa7atwFKayEiULd_Ln5bAH7p-KXxfRbFje_iUro4Paapx9N8X3zK9BTxARv3NQtuulWubSm0MV7RXxKKgAZiuqRvSCrO8_8efsbPkbk-ndYr2wJIGa6ClIwG0g=s16000)

  
Tool: **AXIOM**  
Keyword: **org.whispersystems.signal  
**Comparison between the 3 acquisitions. 

![](https://blogger.googleusercontent.com/img/a/AVvXsEiYtPOSMsMuStcCbvEzg__siDrXH5mxpLi1k1-FtIFzlo6WuhE4R9NY3gmrEafWoN6cyutRXf5gLCzRyp2lIsORyuA9z-Ow0OyDULNeNMyjUTYwlxOnSY-wcY9a_TE3PFPUHL40kLHdY4YnMNycBh7YVk4a14Os6BhEzZICFBnjtVOl0OV6EG4UgtwJ7Cw=s16000)

  
Tool: **AXIOM**  
Keyword: **Signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEg81_8tZdWtjHy8Xyas5WeBCnSK6BodCy-81atTjysegywzs-o_sZMgzLihDF-DL1ZpLRUmNCIqSyctMTqeLMwUQnikZT3ITZBJXa-NcLDZaweZy1gS05Q7iqbsDXkHHQc8Gqbf_TeGP6EwX4FuncArnHvVyewk9lmCcYdfEnCNZ15pEXG7l5NXQETMK8I=w496-h640)

![](https://blogger.googleusercontent.com/img/a/AVvXsEijGNNUhzUbqSHIcTuxNyS52EIU5PEnYMziqL6mzAgArW2ysNt-aFFJrVmp614AQIhA5L-qGfDliYK3pwhrQjnV5rEfx0S5IxZCyJsI_2uhR55ntMavzNhrUTscS-op-q377mmjBlcZkh7J2UNrm79a6QHin-n9utSGoCpNuMOTIDKMFTTjbr6UdU03CkA=w640-h274)

  

Tool: **AXIOM**  
Keyword: **Telegram**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhnKbx0ItXYrYq42eHnwIgEn4TCqRT84TGcjOgLLyROCpCLK9AKj3OOJMcvJZEKSH6T4bY5rq4WbEkL9MI-z0w7acYPid8lxg2IqwCjfvYDDkJiuE3enLXs3F_GPBI7VBNOoDE0p9kqa5RbY75W7g5dyOa2wiz87f8gIzwAjx0VxBN15x_PWJxjqWHh7vo=s16000)

![](https://blogger.googleusercontent.com/img/a/AVvXsEimuyGc6DJXw8X3Jpu1azq8HCfzKddruKbSA2Lf1iag3_qZeYdgKUwiajdEYp91p4Ypj30V_LykYxSHMImTO_4XY8pMaQl5C0GzObQb1S5sC7RgVhH-pG8z5Kmgnhvp_1DBeyihueYQWyZ2yTo8Dv0dFOM0x22KZPUQ0_CwYA1gdQh1S37UbQ5spDkEyAE=s16000)

#### Mobile Installation Logs

The first step was to analyze **mobile installation logs** and search for traces of uninstalled applications. This information was available in both acquisitions after the applications were uninstalled.

  

Tool: **AXIOM**  
Artifact: **Mobile Installation Logs - Signal Installed and Uninstalled**

  
![](https://blogger.googleusercontent.com/img/a/AVvXsEgEuS1MrwX1mpEvKVyUbMmOJr0iK-TiSm-MjsryLwnOLU0s7l1WdZtd7IgDp_rBLD1IMTF548Fv97tAMRBepo25X5Po68cQ_MABvId52e-siVJAZDYyrXCSMI6uqO5yfexRsG1kr-YrrUd4cY1eIDWgLhECJBXfA_NX07XhIXH7sXIGF-BZd2jlqMPVaLI=s16000)

  

  

Tool: **iLEAPP**  
Artifact: **Mobile Installation Logs - Telegram Installed and Uninstalled**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhDTLZKM-ONLLIfav3PX8-Dzwn76wLiJNU9VmODsbbNBwswA6M7t2xTa8v6NmYhyIAgKPeg8tYSMR4IbDjkcqt0Bx7Y8I2PEsCjUYarAPUc08x0joA7mj4xlPXspjbNDZ-4JJsLeuPqkToRo7jElZxke6ZpjHqcM0HpFVFijYjxNfVhtJjhwnto1o4u4jc=s16000)

  

Tool: **iLEAPP**  
Artifact: **Mobile Installation Logs - Signal Installed and Uninstalled**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgbf9xY9jmM3MOMt69DfRkzfmZU2TBfi4nrU4MnhfmP4Tk4_zFDAtTNCyCHNU53422RutfsUv_3QuW0sv0E6Ic8wdXOjv_YVcheCL4JdRoI5Lhl020lx2aEb4--zgYTbTkMQ1XaXk2as8cUDK9LR62yhpvtFMYgyMeiEzvkpI7tg2XnWk_S_ni01RL6aTY=s16000)

#### SMS Messages

Then I checked the **SMS messages received on the phone** that are related to the activation or use of the apps. Below are some examples of the tested scenario.

  

Tool: **OXYGEN**  
Artifact: **SMS Message - Telegram**

![](https://blogger.googleusercontent.com/img/a/AVvXsEj7p4cautRDK8KUhgY6Y6bhZx9GL_NbS-9vUTfSN1cCvFnyWaLObcOKprfhZGkuDJuVdRw_nbXXf1RpPJCv2regfTHuDj1D9JTiMKfIOCM8nvgt3TlDKqTdT3lv2FkitC95JZHVXuU-bssfh8lCjw0CNcwkF7AF93yAfyjGjherwiMpl5jX28qaIa7hhrk=w400-h362)

  

Tool: **OXYGEN**  
Artifact: **SMS Message - Signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhgFYFqewKcx3ITY7MEYBsX2G-KFXUgrh9Vv64vI0eO8ZjERNOF54F43I_VvDrCcUcnmmiS2dClQmRlD5OH8LJyzjf726Vzro1UxpsAx6hYDwz95VKSM9dLSO3acURsixZFpqeEMFmaFuIfCS_iv1BsbPNyPHGwCcZ-fb8SxZWfRXiJFgCo5lUHFONIUO8=w400-h268)

#### iOS Notifications

Then I analyzed **iOS notifications** as they contain metadata (timestamp and sender of the message) and data (message text). Notifications, if enabled, are only available for received messages. In the tested scenario, the notifications were available for both apps and in both acquisitions, after the apps had been uninstalled.

  

Tool: **Cellebrite Physical Analyzer**  
Artifact: **iOS notifications -** **ph.telegra.Telegraph**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgLOjTF4rQxmhVVeqwwObyp02tyJdrS73IYGiniEStuBgLB2VA1B3Cwel1X1JJwTifdInIAbxtUgg5zAgF6wU5ZvUyuC6Ks2rf4mJ4vNF8CWs_OoeECKkfdkoOiQZL_llwtxd46SB9Cld82OkT8MkRHBWJqSYrh_2vfL7VhzJAkx2pitGXuXtTnzq_7k50=s16000)

  

Tool: **Cellebrite Physical Analyzer**  
Artifact: **iOS notifications -** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjBQvcfcIxPHSmA77LvxUL7QS4XY9atQDZAgWPLiudWI56uNX_YRk16Odk933bnNj6xgKSGzGXgZIiyl1G6B_b6GsMZnuDdTdt5JTWqjeh6zzaaRBe5gpIUs010IuhdY35oqTPDJwlGE5qS1_8ddnpUyorqCNNLsFahLhH0goC5n1y4u3AdHVXnIk8o1as=s16000)

  

Tool: **AXIOM**

Artifact: **iOS notifications -** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEi4E7OIwLyoeDJRHkyf8iA8bYpo6ug4MzVcmkKGlVytbwUBHVYdWLgF7Zc_qrmtEelPqtIO4upK2nKe2CKdL4I_JJHWlRy1jQkfnQ7ecLUM6__tpn0da_a0T9ezdcm4C9kMdCi8V3z_DBWoaj8OJVJ92py-YXgKQAnc5bLBb9x13_KBq37APiO-3MMP1dU=s16000)

  

Tool: **AXIOM**  
Artifact: **iOS notifications -** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEiIhyP3MZStVLJeupTlBqpSMkMsYutHZKuul_a8Hvy16-OE9xXsXhtrhA1m94MY4l9Zl25CjIBjdgKiSP4PMUOV_m-BNs1TRrYkn33PzFu_TrkWLsC7KrhWKZ2XdGhtYp9BXVJUWJasANYU-9UjUu4QwUGYOWvx3p9961hRHFwjtCZ0gMHVwjV96A6y4rQ=s16000)

  

  

Tool: **iLEAPP**  
Artifact: **iOS notifications -** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEifboHFseqLE9nR0HRameyxK2KHo3_WdDRofIqFyaU2tcl6AR9t9KAFBwFS_Yl_W1V8gxU4ENW51_etjHCKCaTWtAh2yOzAC_LKbmG4MhEUrb09xJKkAKzjOiKYUz6gTz56YXTAtigMe7LkhFmpPKRxjT9TKxVr3RGiA3738ye3YE-lqaDvvmyPVL6Kjhk=s16000)

  

Tool: **iLEAPP**  
Artifact: **iOS notifications -** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEi5TH1U9WQNhfBjq9OYD3hp6tIIcILuJ74mPQgMfaR51QrY5vU1tfvj_XM-0_dtnTs-zRR42VoJj4NvC009Pyms187G91H2JIevTR-bhK6BIYAbSu0tcUENXxnTlo2X4-q860V5mQP-8VAktJL_SjgA49e_Rk5bzyAHqyXyRUmIORvkNEZv-sQjBdIfZb8=s16000)

  

I did not take any screenshots or save any images or videos during the test, so I could not check the camera roll and photos.sqlite.

#### interactionC.db

When analyzing the **interactionc.db** database, I found differences between the two apps tested: entries related to interactions on Telegram were deleted when the app was uninstalled, while entries related to interactions on Signal were not deleted when the app was uninstalled. Deleted Telegram entries were still recoverable after uninstalling the app in both cases. In previous tests, for example, I found that when using the "Clear all chats" function in WhatsApp, the entries in the interactionc.db file are not deleted. So I would say that each app can have different effects on the interactionc.db.

  

Tool: **OXYGEN**  
Artifact: **Deleted interactionC entries -** **ph.telegra.Telegraph**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEieerWuUGUmuTgu8qZRJBd9qGN3c4l3FMwUOrxC2nPSTaw8_H-CyINHT0XqQZ-_sQOsqcSR5b43JjDj6bK9-3kYk6iOXQHxga0-XHQdkDOvFSNDbCNaU04eqYhFC85H-G9lZL7zHq5JI4ukNKsni6wKjl5nphhTeShyNe68fTbsWEhyabuoRYWEJMEkm48=s16000)

  

Tool: **OXYGEN**

Artifact: **interactionC entries -** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgrPX-Guodv0ACeWZwNlHffnYMtnSbGJSyYtM6zwyiiNo8breBjBDIdtt6ZZk_xlNA5JvKIFl61O-o_Qhq3c2ZPrT-D2cKSIrho5kivJVVn9KXcxt2N6bms0yCvOIsqDNlTDpJPenWP8wqjVwsmcwubtXdQLTMxdhFiipiSEjFr1NkIXRAQagGwe-fzNAU=s16000)

Tool: **Cellebrite Physical Analyzer**

Artifact: **interactionC entries - org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjRRjYmowOVpGS8AB4f7YkjydrKar0ON17TMAcfMEyQtPqvgLIobMg26vEiqqs48-znRWVUR96Ll-ELSDvygae0QILnpj3d0Erpe79bReWX3tqOOkEr7SjAKEmUqh6Uz2QG0SsmkUUapyndkDf-z4TZXdVmRgiVu-lIOMJZy84HPyAfpOuF-QJrlQppysE=s16000)

  
Tool: **iLEAPP**

Artifact: **interactionC entries -** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEh-fxQWs698Z15fpD7n1ISU-_lRoFqB3qRngDnOJtTTi5n4FQKOtKpuP6VUXKPgLBDOYNcWA1Mf8kgCS6QMkMU2PcCVl6kqzE8qfqKCTmmfTmo1TPuzNsjgvxWdT5OcnTWLWJFDetaLKlTy3GJYku7xzp7aH84Zkwo_oT8WMMTXZinVP0umXo2Z0I5gTLg=s16000)

#### Current Power Log

To check the use of the application, the **Current Power Log** database is the most interesting. This database stores data for a few days (usually 3-5), but backups are created by iOS automatically and during iOS updates. When testing, I found several references in the acquisition done immediately after uninstalling the app, and even more (duplicated, because of the backups) in the acquisition after 6 days.

  

Tool: **AXIOM**  
Artifact: **Power Log - Application State -** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgqMb2jkH9zvgj5BIskZiDEkPkiyf278T3LrHENpBN6mUdd8wXslUZoloI4V1yK8aFP7QQNpKb36ykY_0WkZdUAWKgDzryxjpTyeRoWvTTSIO2IoeUW5k1fSqXJR9bah-oa6-4XcY_OsX6dmvEtR1NjTnZOf6bHK8Ucw7GsQ4zLXugH3x0JlzYtUQlZAds=s16000)

  

Tool: **AXIOM**  
Artifact: **Power Log - Application State -** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiNOU2vASGH8Z9yedftFY7QsOuYg16JJiETGIom5x7TyHKpz9rbnH8YaqKfUpm2mswUaGEaDqRoikAZLL43Ii0ZcAwcNEpCzSjob4KJmEf8H8lZRBAPHxliTtKN-1e39DBPhuZy9vtqylbBpT1ZNdLc8aR-DTezMGk4dY0ux6ZfLEdtIyj2lOoF1LeWuvQ=s16000)

  

Tool: **AXIOM**  
Artifact: **Power Log - Process Data Usage -** **ph.telegra.Telegraph**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjS8fYrV82m9ccpsrSG2akxp-_2V991cqHLimmCmZ2zL8sb_Ag1-tPwcL2vebeBkfMTOMSKlsTw8MW9mGVUsW7ggBujPGNrzpnLGvafAhLumwyJTfzGUVje9w0o4NokPqyd82ISa_ENGbQSgGXvhzRLbR1nO1AJhekE-nE3L7gZUrC4LqOSCN1t14k8aY0=s16000)

  

Tool: **AXIOM**  
Artifact: **Power Log - Process Data Usage -** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhq2AU-HwjBKD_Kal_YEXOxpBD1vAZfiFKblgnZwJcWYW_PCfcUtfba6ksuKc4tNIHQ_hddOOAIviG-K3igmLgjuGdgyiSX4CfK2iybBjEn98rSnq2dihLNf-FrIr3ztz9Z0u6geWyNP3UbwZ6F5A34lwmkNt3Zp2xftMOFAMxdSFEK_s5vYx_S2NyunfI=s16000)

  

Tool: **AXIOM**  
Artifact: **Power Log - App Usage -** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEiHtzLGAD-8cyjRK0PCoS1yPK0lmPnLRejd_ll1Cpmp48MuWZ7IusdnB6vYgXelKVE11rPKf38oogNkFpNf_uLbLOGdTJaFHU6aZ78uRUuTuk4rhPLy3Spn7RvAQQc--xC6Yo8AI3VRwYld5yAIOINgLG9WkuRr3tUNehpYQUgl_9Rcq4Jb6AWBLv0XDDI=s16000)

  

  

Tool: **AXIOM**  
Artifact: **Power Log - App Usage -** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgdYk_V_64HiRhOwl_8dl3hSkFP6WpZ-4IBBQSIq2f4e_kYhbX5dCihEXLzxfXAg0lj-7OOk-uaF4aEoWbCu1FG9Lbr6yqRDQ3c3RWwSj8gs606oo8GMG0gwxrNXAasyDnha1FMHNdKIMDZtfVFhtKGvQIlUfia9IgA3L2cBZXVc9_pf-4cLz-9dGL0r0A=s16000)

  

  

  

Tool: **AXIOM**  
Artifact: **Power Log - Camera State -** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgTLN_xok6mqwPkEqPj-VNJb4QxxZOk2j9t-_WA0a-GRtmoZb8sH49GZY5q_PBOkSl0ne0pgoJmmE2_gsD43DpTV_mJS9mbMOrzbU4ZmdWjvvqwF1FdBQ67xHvX0u43-ya4swVfbOOSal9nN5LU6bZ3rVw2lgaG8epkdt4lMCGf53D-WZtPeChM60YAxq0=s16000)

#### Biome

Most of the data stored in **Biome** was removed when the applications were uninstalled, but "**Notification**" (with metadata only and not text) and "**Text Input Sessions**" were still available after the applications were uninstalled, even after 6 days.

  

Tool: **iLEAPP**  
Artifact: **Biome - Notifications** **-** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgUuieK4xndQNx0bqYkiS6YTJ9ucJOG209Ma1nSkbRI6Hyu3qRDCnRQSBN1NOpA2sKqR9Rqnb63uWd-8bGwEAwjz3B9osW33o2k33HMBOm_NG9S3RXAPwFUabrRZwkz2XUypsTeS2126Copc2hJ3or8C3jJ777N-XQjosU8KZnoEv5W_YAq5atUuYK8czA=w640-h494)

  

Tool: **iLEAPP**  
Artifact: **Biome - Text Input Sessions** **-** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEjvmQajMnr_O2KzmJuMPbAMAiFUQriYZJlLzXROyxsz_K7N0VzwHfmYaI2m_cZyYjH8bx0jCMtFobjwGtOLnPvFKCiP6FehwFKo38aJ9rfQczB_kvXqf4o_pc_vmJ8mrY1L_i5I12NJAA6KkG0FerNh70XsR63HUXHL6xlip36xZ26vPdIEWHDdlFe1NNE=s16000)

Tool: **iLEAPP**  
Artifact: **Biome - Text Input Sessions** **-** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEihqfu5vx9aw6cC-FjBq0_chirQpe5qNIH74p3Sqn6BZMQXZfPVFpoePvG7x2S7D7wGph-F2qRpGeo_Hsv4-3X-ARj35hk0zCXERk-ov3DsuBP9Cy0vv3fwFNBD4izD1bZ05NRYohRjJOxmfVSgL7tWp5POEuA1-o1hrkA3VENT1KEyLTKKbM1YihgKp4A=s16000)

#### KnowledgeC

Entries related to a specific bundle are deleted from the **knowledgeC.db** database when the application is uninstalled. You may still be able to find information about installed and uninstalled applications.

  

Tool: **OXYGEN**  
Artifact: **KnowledgeC** **-** **ph.telegra.Telegraph**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhMlf0rv7jkY9qcWkLn8biUebhbkcR10JG-rOAtd7ubThciKoVg8S7OvaxFX6P2AYC_OmTByWkEFLQL8kaL8m8BKWB3sU4i0LUhgCMu4v8d4v5kEY2-owU8ixWOU1WjVMOeirVcdeEWQFYbo3AN7xCa56hW5DmrVsk6ysbP9Ep2yZT_Fchcbb0f3BF3GPc=s16000)

  
Tool: **OXYGEN**  
Artifact: **KnowledgeC** **-** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEi1anfRs5BXbowrJBBohZfhaneKB8rfdcA4ObQdJC53rYIh4QFoUu8E92LumXy6YtAki7tBIVh9qlDKn1ZrJNuxQz3y9JPKJmZ4e868hHr-x0zHc_BmWihDE6YPtKA9vlgPXkbfqxe_ltsOs2MNGJumMtaViOKh2ocyUIZ616P54vKbBdXBunAkW-N3rjs=s16000)

#### Screen Time

Similarly, entries in the **Screen Time database** (RMAdminStore-Local-sqlite) are deleted when an app is uninstalled. Deleted rows were recovered in the acquisition made just after uninstalling apps, but not in the acquisition made after 6 days.

  

Tool: **OXYGEN**  
Artifact: **Screen Time** **-** **ph.telegra.Telegraph**

![](https://blogger.googleusercontent.com/img/a/AVvXsEiCa9oDEm8uBu6MJgrOBw8Q_fXrdmTkAD94ajVZK5F77oxhwjIstCfnm1Yip2NHB2dlwgfab_zSCsnTRp_hbDSL65UNFJCSUHe68wiMN8t_W_yQ9qRp4Y7hLjRLMYf2AluuyYqR2lzf6Ja8W4HfvfsFqJAYdqZRhWn6LNxeuiFeYmzPrF9Lv0YTbpFsYog=s16000)

  

Tool: **OXYGEN**  
Artifact: **Screen Time** **-** **org.whispersystems.signal**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjSeDGDtuEx3KoF4CN9iY1XwpMsFoq-6fQr9kr8bcNbRKE7Y3AKTJyektLgVqlK-32iVMbq4bsEuOBGTM5o41baFL-ESS_FjIAPsAwE5ETwZH029VxTmZRM-icPeNnrWdv61NBy3p9Co1pmTzEkHl59TLZZ8zSdu5PlOjwkFsR_wkJMSRH7R6V8t3W9jDA=s16000)

#### Net Usage

The **Net Usage** **database** contained information about the uninstalled apps in the acquisition done immediately after the reboot, but the information was not available in the acquisition done after 6 days.

  

Tool: **iLEAPP**  
Artifact: **Net Usage** **-** **ph.telegra.Telegraph**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjc1JJj4-osbpEQgbAzStw_F8A6dan6RvDK-aQXfKyfawP0BHpguFaxGKOpT5V9ED7BcssZwV4VEPjkSzBDkV7BCzRnRfM3biDrzY2amTVXXEXPcyFka03PK6t-9p8sGSfguggf_UfLN6ZT-wI6lhAXHF3AawM2pv1oE112dHazES3h2iv9q2FLE5C9ftI=s16000)

  

Tool: **iLEAPP**  
Artifact: **Net Usage** **-** **org.whispersystems.signal**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgOzyNXlDxpNbDQ592rNWotPZGpuv5N_5Il_33aKcaBW7LmF_iUfMtF7JhTpIU6a5PlnUYqHcWwJ05Y_cCJ3ntHi2IlH-T4W4TdOYm7J-_Oopu_4Dj4C8wqy1ts1-e8hCiTB1TOc-lFFpwLJmvqNmfrQGHQx0xZw2fSoskf4EjrQKlZuEQNuqarUBbBt0Q=s16000)

#### TCC

The **TCC database**, which contains information about the application's permissions, is normally deleted (entries are removed) when an application is uninstalled. However, in the tested scenario, the kTCCServiceLiverpool permission granted to the Telegram application was still present after the application was uninstalled.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhUvO4aeV1Y-rm6I7gkT4aGPaBZkMtMeAADc-hOuhThXtEhLMwRNkR_yK2GjN6dgJ0_qnQ2DcNBq5gFqUem1lRWdlcK2EAG4JhuHkCmtX0Hh_5oVExLonHP7xSas3XFvY42ArD7zTfPAAQKAt5SQORQ63MSZv5AdI8DAD00FxYAAhw1zZFG_h7tEIZw5qQ=s16000)

**

#### **storeUser.db**

**

The database **storeUser.db**, which is available in the App Store cache, contains two interesting tables:

- **current\_apps**, which lists the currently installed apps including a timestamp that appears to be related to the app installation time
- **purchase\_history\_apps**, which lists all the apps purchased by the Apple account set up on the device

These tables can be used to prove that a user has used an app, even on a different device or a previous installation of the device under investigation. You can compare the contents of the two tables, to see which apps were purchased by the user but are not currently installed on the device being analyzed. Be careful: the fact that an app was purchased and is not installed does not mean that it was installed and then uninstalled on the current device you are analyzing. The app could have been used on another device with the same Apple account.

  

**Raw Keyword Search with bundle/app name**

It's also a good idea to do a raw keyword search using the bundle and application name to find other potentially relevant information.

In my tests, I found traces of the bundle name in these files and folders as well:

- /private/var/db/diagnostics/
- /private/var/db/uuidtext/
- /private/var/Library/Logs/CrashReporter/\*.ips
- /private/var/mobile/Library/AggregateDictionary/ADDataStore.sqlitedb
- /private/var/mobile/Library/Application Support/CloudDocs/session/containers/<bundle\_name>
- /private/var/mobile/Library/Application Support/CloudDocs/session/db/server.db
- /private/var/mobile/Library/Application Support/CloudDocs/session/db/client.db
- /private/var/mobile/Library/Caches/CloudKit/CloudKitMetadata
- /private/var/mobile/Library/Caches/CloudKit/<bundle\_name>/
- /private/var/mobile/Library/Caches/com.apple.appstored/Cache.db
- /private/var/mobile/Library/Caches/com.apple.appstored/fsCachedData/
- /private/var/mobile/Library/Caches/com.apple.ctcategories.service/Cache.db
- /private/var/mobile/Library/Caches/com.apple.ctcategories.service/fsCachedData/
- /private/var/mobile/Library/Caches/com.apple.parsecd/installed\_app\_whitelist\_url
- /private/var/mobile/Library/com.apple.siri.interference/siriremembers2.sqlite3
- /private/var/mobile/Library/DuetExpertCenter/\_ATXDataStore.db
- /private/var/mobile/Library/DuetExpertCenter/\_ATXNotificationStore.db  
    
- /private/var/mobile/Library/DuetExpertCenter/notificationsAndSuggestionDB.db
- /private/var/mobile/Library/Spotlight/applications.mdplist
- /private/var/mobile/Library/SyncedPreferences/com.apple.kvs/com.apple.KeyValueService-Production.sqlite

#### \*\*\*\*\*\[UPDATE 14 DAYS AFTER UNINSTALL \]\*\*\*\*\*

#### 

14 days after uninstalling I was still able to find traces in:

- Mobile Installation Logs
- SMS Messages
- iOS Notifications
- InteractionC (Signal still allocated, Telegram recovered)
- Power Logs (fewer traces than in previous acquisition)
- KnowledgeC (app install/uninstall)
- Biome (Notifications and Text Input Sessions)
- TCC (Telegram only)
- storeUser.db
- Most of the files mentioned in the "Raw keyword search for the app bundle and the app common name" section 

## Analysis of the Android device

Similar to the iOS device, I searched the results of the various tools by keyword. Here are some comparison images with AXIOM and PA in the three different acquisitions: App still installed, App just installed, App uninstalled 6 days ago.

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **org.telegram.messenger**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiz22fqHR4o6LC_BiJwCmO8EmW1zIPx8X6jkI9bPKS2cJVyDNtyAmd6jakDX0bTK-HJdu0HtMAzSxFUYGuFtbMI3Bdv3Gt_vcWc9Cw8JM7wJs3OATBeqqmFI2mp0-prH3KcFg0TNsL84UaVfh10kgm5waXWZiy0bBaX2Yn2Of1WkKOKOqGgJRwUNICrQec=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **Telegram**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiufmr_fUKUMvxNt7P19N7RJ10FHZptG2rgHOY1RYK_H0g8pfYY5eciz-G1YD3M-n3wDi9PSvM6LqpNpev_uUdVe28AgGaNOIQIWxbvFa4dxUsvspMhfm4oTrQoWdyrfgWq8lzlqNWwGR7AHQwnA2pJ1O2mv8i6iZTwURbcsxdqGY72qw-D7HxiVETl6mE=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **org.thoughtcrime.securesms**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgFQjIOZwLKS9dm89kxZl6nDp6CnAXmzqwINeDhUF0QByaJUJ15mVnINlCeDkX45cRjKj9GAm9GBfz0MsWreVxB5hZx2hUW2WZS9EvAgoM33IDre2tdGM4czjmg9RRG1obycDzYziMYdxstYQUGPjqUyxGrmXx2dERf5mPy1khwTGfUQcBdnQhhrRPodJA=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Keyword: **Signal**  
Comparison between the 3 acquisitions.

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEinib3PnmW3Kgk3ymi3Q0zxU81rX6I-U6jg4RaTuRnKnWV15VkGn9ewVxnyPH8xZzspWcrh-xaz6gohpVxl0zKSypm5YR3mV-GmEZKTw94zuQdQ4UfR2TYvVFdtqmR86WhVEjutZ-xVongalA1I5Ln90US8H4-rBEIy6GZor9qYafv9a3WKBupUU6Z5QKE=s16000)

  
  

Tool: **AXIOM**

Keyword: **org.telegram.messenger**  
Comparison between app installed and app just uninstalled

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEiiM2pBSKQvUh-TgnMbFadyltneRa5Fc4ThgX9nVXFbbNuOjbrAJUYwA6QvHXjKDTaja6Z1sWb89hAb_B_XlcOVa2pH42la3fX2l2P_qP9qXvmcLvmZO2jf9HWKl2mjLOEZGjom4AK5P6byTvGOnN4fHiQk6bIhXKb9E0-WvPQXWVVK5Lg5683TMlG57hM=w640-h524)

  

Tool: **AXIOM**  
Keyword: **Telegram**  
Comparison between app installed and app just uninstalled

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjimsYs7pQl3zL1-xbDz1ucGN2ScwVQa3eM3DuZLsuALDXEoNlmlpmtKFjmfrzrr7HZpfWK9_D3BzRQJqeS_MXZxv9ClMGR6WgyVMlauhc7oPjkVUx9MgMyZskz_YPE5B5FQz8OCtbuPYrG3rXT8MgpNfsgI2igVm_nbJMUU8ZiCfEkTjlvVYJFSbwgblc=w531-h640)

  

Tool: **AXIOM  
**Keyword: **org.thoughtcrime.securesms  
**Comparison between app installed and app just uninstalled

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEh0VOzXVfITchL16OynFd_ig11ooCRZXVxGC1O8WQ9BQrvgs5vWP_I14tnkzrvInBzMqkketWSy-P0rJ5rlOAXdyqT8mLjCm-rZ7c08JeNSG_w5YanC_RtBshITDRXbPXPYePjwM0E9jm1rMLE1ILakweixRIQ16SUAi3aucH-VxRl81PwSPsFl9r5Oz0k=w640-h330)

  

#### Localappstate.db and Gass.db

The first step is to analyze the **localappstate.db** and the **gass.db** and search for traces of uninstalled applications by comparing the list of actually installed apps with the list of applications available in these databases. The localappstate.db tracks the Google account used to download the application, the first install date and time, and the last updated date and time, while the gass.db tracks the version and the SHA-256 hash of the last installed APK.

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Localappstate.db - Gass.db -** **org.telegram.messenger**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjZrL3YswdsxH8f5JgvjE3BHaT3uOHVhhlFEmN8tV8g3fuaPOn5gbs443d6K3F-C4DH-5e0VPLcDh7e1AXijtF4yPrxRtY5pKz5i-3iWvg0Elzf0wdJ5IMc1nfxf16COtuXl4N4tMiH1I8hLpBwKIvdQxINkU1kh4WnhgqlU3imOQKoqVsAUtyN_WQz5zw=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Localappstate.db - Gass.db -** **org.thoughtcrime.securesms**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhtJS1uLGsxGt4LHHgICCxWemuI5CvZycYuGYx5ZDfx5RxlEK-fkVEpHseIMz9wiY3jdTlLHDX1O6xbhp47xr8NlXNzXj6WXqtyOhzmaYMbh1IQTOCr2YmY4Q1fal8DBadXTPX8o95zUzKj1iQKC_7jIBZttQ-X3sWTxZwDM2UV54NuGuHH8w2t4ouNAm4=s16000)

  

Tool: **AXIOM**

Artifact: **localappstate.db - org.telegram.messenger**

![](https://blogger.googleusercontent.com/img/a/AVvXsEjyG0crP0zo1NMt-_arkdpYKtlSkYhxTo8xvR9c-f3hEAxXo-oaIt-_I8S1keyXNvXW1Q--tZjcbysZ3xJdRRqfVAKC95-2L9ah5giM4RKEkGjmJAuh_zQBqE8y3EXh2JmYgUgGY1gdJfW6qpd7SAPNtDH8Yw1nnzuFmisG1qjqgl-S6YEs3uhoYy5vIm8=s16000)

  

  

Tool: **AXIOM**

Artifact: **localappstate.db -** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEi-uiKAND9gOGX8wUvXH3y4-Et-lIAlclNZnMoLg4n9L67Rx2B3m6xlPtcGAIEoTpq0Vs_nCzXXIBZkrGjIYTsYBLiIfnesXeN9PHotffdrWk62U_YSpFV6UNf7B8mGB4d6UsPP7-S0U9ZX8gKa_sHHAT7YXEoGsK4qSJxtKEahm3pxrSqXUzi38AAijUk=s16000)

  

Tool: **aLEAPP**  
Artifact: **localappstate.db - org.telegram.messenger**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhhgZvM0Wm33h0qc6QIe9R1Jo7byGN-E2OAA68MsAzU-adRvEswavYnm3blLq7oqsmWC1lSu5ga2y93_-stKRMJAUN5ysIdtP-xLwZ2cJnF0ZsjVfFs2HuhOSfN2HtV3jpQENmpqVAH-OkanRVHXS_g8Q3aQi4JJnvUi4I3TgVC8_EXqtVe_EfghpAniYA=s16000)

  

Tool: **aLEAPP**

Artifact: **localappstate.db -** **org.thoughtcrime.securesms**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEi8jgVj3CHX3nQBJuu5vhRGshbuyNHtPBNuM0W03_zneb11DXtXEUB_XK9rT1E9fo3a1wmqfNHf9_2TCq5r2k7N3TJzb_EPIr1O6Mf26-ecIy1PzuNbOlBL11XtqML1V0cpa2J2nXw5J2MKaWuZg76eyN4Us-DzDCatC3TjjcAdEI6xLpnIOJYdgYKCink=s16000)

  

Tool: **aLEAPP**  
Artifact: **gass.db - org.telegram.messenger**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgIiVSHFnhed3y8Kyh87P4Qah3BLzb-Twq7DuBuRLzR4zxYF5kCARS4VlBySgjz8t3MtPtyDUp9eVdcrToKp0uxgAaLvf0uXawRxJb-vHBo5-F1gpo0zyF7Q-qaJFH18s9UtQh3hRlFdxQzsWwM0A4sTsuMB7pfJ5FDQ5QtCzL-3XEID7WqyvperKPwMp0=s16000)

  
Tool: **aLEAPP**

Artifact: **gass.db -** **org.thoughtcrime.securesms**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhKlfnVHW6qntcAQwKKGYndknFSEogptjw8tVhsH3osfmJ82sxXMF8TgXV8PyclZIphVwuRhz-TSggv6IqKHnvIHPyRp1CDMCdRf5NqPyjWPLBB0W1Nz00XT1OsSBWIpnuvkxp3cCYJ9_AwlX3XBycpW_4wZ98ySqfkFMMCV760aOXw2tLnDWlPutAsvKs=s16000)

  
**

#### SMS Messages

Then you can check the **SMS messages received on the phone** that are related to the activation or use of the apps. Below are some examples of the tested scenario.

  

Tool: **AXIOM**

Artifact: **SMS - Telegram Code - mmssms.db**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjP9p2ACndrd6MnErYZR5TMxkC8kPaLJ57ns5vS7aNznqd4WbFyON79VEQDp5MusVLWkm612Rm7-8LGEfunDO0dD3uEZroGXuh3ncMU9Q_-HRHCimpbFXQnCySln7SjDADO05fakbRQCp-AIi4ikafKaIsZZM4mpYP8okwmodJGBVjZDqcMzzRgUoM7G0w=s16000)

  

Tool: **AXIOM**

Artifact: **SMS - Telegram Code - message\_content.db**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEh2KJ1gzTWIPC74H34PWM8iBhjbnvtsfxSdBdCX53_QpHRVqb-afa2ya3rAOJvskUIPs0kP2IAKQltOjQf1XPallImJAI96NuGPkfjvCl77fJ_EDH6CaR0WZfx2FzG7oB3fAKAegUWI3rWiNvDEMaSXgNwRCwGiIMvG9eC9oYpuwKQpcageEt-QNK3nF3A=s16000)

  

Tool: **AXIOM**

Artifact: **SMS - Signal Code - mmssms.db**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgOq4KBKdT2VzXAz7snc7-rpuWIgHxEjEL4DJKH0CljLN6FX7OztG5iJhSWjrwkS74ZprufPdZn-O7GqzELrncVPmWeSVPn8pEhZEsZM5ZymF4YFN2ykmLa6gA7UwjEqOHmShneGsGonE117nVW0IDLu-QVCOiFoYnimflhBlMcN7yutu_Aq2--dV1UOb0=s16000)

#### Files stored in the emulated SD Card

**Files saved by an application on the emulated SD card are not deleted when the application is uninstalled**. In the test scenario, the images sent/received by Telegram were still available.

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Multimedia files stored in the emulated SD Card - Telegam**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEhdcqfCTra35R86eS8JLPVDOjO4WpPuFgM21Qbr95LQVBBACNHCPUKRBn-pL12-03t9OArQdaPv8TtfClJEP3K7AbnvC_US75zUzFheXAAFH5Ihz4A3wLKyT1jorAXmHoQHhDtfPzTFgmbS7m_lOdmwUUzIZejDS7MCPV0-XE6E38hrb3t8WFMfO0kRvxU=s16000)

  
Tool: **AXIOM**

Artifact: **Multimedia files stored in the emulated SD Card - Telegam**

![](https://blogger.googleusercontent.com/img/a/AVvXsEg_gPwGi7odIoDHc4JGF5l6F8Gu2hBiLy1pVoGpYJCUK5oBdj9SjWdiu6ChQooL1kYPlzoE1GVZlUbDVydZyYyuXj1jQKbMoQdHJ7C3updjhgMFb0XYF5ccrd6nHrzImQ-VINecz50G7I-mtZQ35eM8PD3MfBcCWlo1ItgH7vf9aAthKa3j5zP3If2U0DM=s16000)

  

#### Digital Wellbeing

#### Digital Wellbeing tracks application usage and application notifications and may contain traces even after an application has been uninstalled. In the case tested, Digital Wellbeing was stored in the dwbCommon.db database, but different manufacturers may implement this Android feature differently.

#### 

Tool: **Cellebrite Physical Analyzer**

Artifact: **Digital Wellbeing - Application Usage -** **org.telegram.messenger**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEia7vDJSMFWqXTLCnZ9FR1RRthoRJSXFInJxyw59SNzxuw6ONJSBjSfd3QJQL8_D9DeOKvgOiJYJQ-0vm3juC_VPqllYemZ7ZiT4wmP5d17vRqCqKxF4palhqFtjeyT1n6icWcdvkk-9DbtTA8-Y2DiplYYcBz8rNN93SovUpfOo6RaF1gGZmmdlbiIyyI=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Digital Wellbeing - Notifications -** **org.telegram.messenger**

  

![](https://blogger.googleusercontent.com/img/a/AVvXsEgQTtw8UXVv4mzRLep3T5ksPTl1EyA5ZHWVlJASqQDmdRpcnL0LVyBlOti01YjRlcJqKCMYNwUKJEA0_Cw0IAyjkMpM_PPVoNYD2cmGF2f6e-iTNQPbVI89ITwjfXNQTjbvh2IssVefHgQGt___8Zqt3pRTRsl29bq0LPmo1kBiBmuXEhhnzECtiF2D4Nw=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Digital Wellbeing - Application Usage -** **org.thoughtcrime.securesms**

#### 

![](https://blogger.googleusercontent.com/img/a/AVvXsEiVXRDox_UzJrOuY4_MM9LNAqEqNgpYrOcbByI8xt0o33UKPa5a2vitWi2RBItuJZg76t-5EqAX4ZCvoMGTN4tYZYRuzXDbBtdQ6KCfg3Xe4nnN6BbkHEzhr5BiDjAqvpy236Y-Sa0Vuq-o9ymeZSNxlAbHM_r4Q2DJtHpKXHab1OGS7fvTYqxI14R2hOk=s16000)

  

Tool: **Cellebrite Physical Analyzer**

Artifact: **Digital Wellbeing - Notifications -** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEiAzt0t1HqPZkfsrsO4uBO0T-KNP2vYgBdERzEMC5_EpfM5j5YhLpq-TqUsgGbnNIxwLe6N5CPSjJ3pEgUvLZBSsTDeYP7aThdCcEn11Xki_Xl7IQm16UbW3D6FYw7T6LnEUgceuYnIpEhm0rqH9DWg3919xFurPXqh5HFCQ4NaMlQhnsVfqPW2gzxsbRA=s16000)

  
Tool: **aLEAPP**

Artifact: **Digital Wellbeing - Application Usage - org.telegram.messenger**

![](https://blogger.googleusercontent.com/img/a/AVvXsEiLoIZ7tTiZF-MU-tEKv9uL0QR2ckKptBb0PW9DCA5WbzaaxXjzkFQeBpUHyb3VpDu2N5Vfjt4uZvSuuyvTzAiNnShOqmYODOCUMM7mjpPEQ4zfRb-AkFUEPd0nmpDVZr4hYUUg2AwJfe-7R59UCwY7Md-Xr-xBKFGms-k_x4h3npIfPMyRU90nxUjojvI=s16000)

#### 

Tool: **aLEAPP**

Artifact: **Digital Wellbeing - Application Usage -** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhO4AG5xmgqSxtbLluMKuL79A9Y3VETKgy4N0JXG31VP9KZeNRKBPn6OVQJPHGamcrQd2Pbztwx7XFD4Mt5uXLyixEv8kWbEKsnMoJlJocZvbRpKjRYGPLmT0VLX1qeYzfhJm8RN9fqvUvuEjEzowtf17Q1WHUlhhrFFbjIjyE148OT-qjt0ParybUxLpE=s16000)

#### Firebase Cloud Messaging

Firebase Cloud Messaging is a cross-platform messaging solution that lets a developer send messages at no cost. As described by CCL Solutions, it "presents a golden second opportunity to recover data that might otherwise be unrecoverable".

  

Tool: **aLEAPP**

Artifact: **Firebase Cloud Messaging Queued Messages Dump - org.telegram.messenger**  
  

![](https://blogger.googleusercontent.com/img/a/AVvXsEj3dwQjBJxTd5qGQCAKXTrgrJi8jSttQLNNj_180pL4bOSxa91UTVnbAL95OtMXLrXmxeoj2QHiIBti0rb2soO27VSCAu5z_atvn19lwh6bkMIip8LeAb-kbX98XO1156MeVDJdWLY-IO4lwsVXRrpHSLuu1gBY1QmQCdhc3bU4bkQ9-jdPAsw9tmbggd4=s16000)

  
Tool: **aLEAPP**

Artifact: **Firebase Cloud Messaging Queued Messages Dump -** **org.thoughtcrime.securesms**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgu2Spb6G555qqKpjTPpa6kO6mCwhZhVbmDcSUWeNxIH20oSOwnhjvElAfymnuRVMaETUFEMbp0XxDl9cISFDYCQTvBLns5diSeYGDRqnOg-nXsQIqJNmw8xuGczT22ZCye_xyqcsv0ScWCBl0aRERTrPrrKFN1iAOvyVEIltA-JwQFDwwD7WNcIkPFdbw=s16000)

**

#### PkgPredictions.db

On Samsung devices, the PkgPredictions.db file contains timestamps for the start execution of each package, including the activity name. In my experience, this database typically persists for 7-14 days, so it's always worth checking the name of each package.

  

**Tool: **aLEAPP**

Artifact: **Package Predictions - org.telegram.messenger**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhNNjDkMK-ndblKlWCYPYORgNB9DIIpCiAGRaetFg4RfAfBJbSoqcIsNPWWUi-yWGb_CHkGNfls3gMLB1d1VtX-ioRyemDj4X7vsLpjZ-9yhVREwp_F8V6v7eOtnTvkEUTd7CHP9ETA8MIrgEeIBSxPlLi_QwcHYzR3KVE0V44eUecb94688qoCFwPhzBQ=s16000)

****

Tool: **aLEAPP**

Artifact: **Package Predictions -** **org.thoughtcrime.securesms**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhhDsB2ypibvjlOleGMeRyfQ87JTziGgrOuH2K-PZSv98-qL3MLqPM7Johg8h1in6IBnWD32g2f0WtqriK_agL2_QQ4g_rQG8j725Ytzmx4CiQt_KihTGUKUKnGgK_EYrG4-mzK64IEEmBDojwXYK8VP7kOlihrnLnPlAhSsEtjs8gQeHGHkKFFcL5Wv-Q=s16000)

**

**

  

#### notification\_log.db

On older Samsung devices, the **notification\_log.db** file contains a "log" table that records application usage including the event type (i.e. message) and associated timestamp.

#### 

Tool: Cellebrite Physical Analyzer

Artifact: **notification\_log.db database** **- org.telegram.messenger**

#### 

![](https://blogger.googleusercontent.com/img/a/AVvXsEjKF2HRCnVEyrjEnGftjrG40zYTDnmaNAS4_OrlGir8k5oVr6n_6UHXhL-7CGkkpUr3ImbvKrN1L-7OmMfWIdtudDsdZLs5EADCRzb51sa6EvcdSiig15h7fsGF5e1OQJEPJn800y30FuQ538KNFQ9HP7vIUMGwsPj-_SGNzK7PBYUe8Zb9oP1Tbj5mG_U=s16000)

  

#### 

Tool: Cellebrite Physical Analyzer

Artifact: **notification\_log.db database** **-** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgsqzAwFhroAfF_g1YKjX3nUR4uTZ-igAhcYVLMBUWaSVzJ12nIAh7RisnTQuKeahNv9dtMqM0EFyk6VCT3NSUK--DxMr0tcZmVh4Uk-15iOfssGrN7W3AS-_5eM8gYDP0HStcq8BegYEjDH3SE4MBL6qIHL7_ReK2BIQfueElG1UnyO4GvwISJ24LW-xg=s16000)

#### Usage Stats

#### 

Usage stats can provide information about application usage (application events) and package usage (last active time and total usage in seconds over the last day/week/month/year), The information is not removed when the application is uninstalled and is proven to be the most persistent usage artifact.

  

Tool: **AXIOM**

Artifact: **Usage Stats - org.telegram.messenger**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEjKXFDGNM6FLvZdd6jdjtimTPk9dOIfA5sw8K4QSefWPMjwIjlyaUIUHb5mpH2D4rRce3SuCWg-ekv5MbCtOyFdnuT1eL0MUxVcr-FWFP4GlPMyJHT4YpTmEGOkTn4eOKXNO_LgkuEeqq4GLNqKhLFilCIhkzFv8ewhuP3sxWDoqx7ySWFribR5U6VUXB4=s16000)

  
**Tool: **AXIOM**

Artifact: **Usage Stats -** **org.thoughtcrime.securesms**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEirIBJZebB0Y0jFy3Vlia0beRJWlubMCPEGB53kAkISKzop_Sv8HLpKsfX5PzabxKPpY_MRzJB3hHHU0GtjfFQ-UXAZBDPK2xtLoVdv8LFOq-SuFQ5zRQMsy2ByWl9kTa0fsO03ag43W5dAh8uvxeOoa71Wv6eLeC3cpJccqns5rzoT4QdwecCcz0lw5eI=s16000)

**

#### Battery Stats

**Battery stats** are probably the most underrated and least analyzed artifact on Android devices, even though they contain a large amount of information about system status (on, off, battery level), system usage and application execution. Battery stats can be stored in different paths depending on the manufacturer, but can easily be found by searching for "batterystats" in files and folder names. The best way I know of to analyze battery stats is to extract them from the system via a bug report and analyze them with Google Battery Historian. I also suggest you reading this interesting blog post titled "At Your Service: Timelining Android Usage with System Services"

#### 

Tool: OXYGEN

Artifact: **Battery Stats**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgDstyTVicljE4dY6h_dSVrbX3G2TiXucesD0jDBvXr8EammyWx6pzHLGDGgRJpHzIX1LWZPF2zFhZtB1dt-MjTnltdhTIsK0ZPxmW8sLsjqBpB3tG0xczjb7TFbprc_MGTdD-xHN21olzqG92udW2IY0AnogLv50YioNhWgfCtcwKPxROB7X4WE0ft-bE=s16000)

![](https://blogger.googleusercontent.com/img/a/AVvXsEiuOAF2grBsDgHfcJHZx8AI1L8JO_3O08WE_k0pteQSozAPR5cNUwImdWkj6sv2ZhFCgizaUoKU8-lgsDohb8zs3Uw4Yq2RQ4nE9I0qvge2f3AGUeJKjlDHhzvClKA08IYkLVOIYpK0cHD7_rj-Ug-u9GAa6U4xTDCspSLv-pB7KYeBUhEdi-_dBJ2gU08=s16000)

#### Graphic Stats

#### 

Graphic Stats can be used to track application usage by analyzing each subfolder and searching for the desired package. This information does not seem to be really persistent.

  

Tool: OXYGEN

Artifact: **Graphic Stats**

**

![](https://blogger.googleusercontent.com/img/a/AVvXsEhpv-BdxmyxMpTP6pQJwpguAZv-vC-Oe0vbSAzz-vIyHNmPypuWGGbneTWTSCEPbaAR0xkjZPzH1EtfASLUwedFso9W9vs4H65B44-hRc9IGf1HMkc1DSbKQKvgPGsOmPZD4C1ixtqhV2_EuAKenCL0Y25sk_dcQ7lqEri13wxZIl1hgQhSqIG5LwsqPOc=s16000)

**

#### Library.db

The library.db stores the history of applications purchased by the Google user on the device. This database is not linked to the specific device, but to the Google account. As soon as the user logs in for the first time, this database is populated, even with applications that have never been installed on the device. In testing, I set up a freshly reset Google Pixel device with the same user account and the database was immediately populated with data, including Telegram and Signal, even if these apps were never installed.

So this database is important to understand what apps the Google user has used over time, but be careful about attributing usage to the specific device you are analyzing.

  

Tool: **AXIOM**

Artifact: **library.db - org.telegram.messenger**  
  

![](https://blogger.googleusercontent.com/img/a/AVvXsEjduqpv4SfN_4qaz3AnwxqiEbea-maMW_jZHMn9bCk97_AmumwEIwvO1y5AvaDUGswK78xhKZt_5_eS8EhIJgEEDlolrd2bxqKvTfb4iseLmbsrYfrfTu42mHFHz7VKwwWYfniT3avbmMXhXvNMKQx8RSRjMeZZuGj4WNFqHse_9A4nwIcr7cW4r4VV7Dc=s16000)

  

Tool: **AXIOM**  
Artifact: **library.db -** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgwUvCmR9xOQmCW2U5IGyQH6nEP6FaZPN6bWW5YEl8tTa3ydx9g321amaqqHI1Mj85vI5tqa8NqlJ7vJQMbZG6hyiAOP1gBeNWii4U7tToc3gdtiBD7pTBdR2DGsDb4kRL5Dd8ipvjDw_aZkjySJ-UxBe-hIGZVX2mbAlDk34IKkg-Sf6hbRrKrd09rPVU=s16000)

  

Tool: **aLEAPP**

Artifact: **library.db - org.telegram.messenger**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgyhz7EAoSjeu50iSRdtMpODs6XOIsax7KUYv0JhdrlT-BdZiKUbY6iptvB8dqkry4vU5hPC88njm9fzY1SvrFRIycWm1O3TTtshU4LPeIQM4GutAxBRMID5Us_MRbpgiQr_PbZloaLyljCFnKXQUU_xGvEL0QpNJVVMx1mAttHdfYJ8XN-Lb8oexR8yYk=s16000)

  

Tool: **aLEAPP**

Artifact: **library.db** **\-** **org.thoughtcrime.securesms**

![](https://blogger.googleusercontent.com/img/a/AVvXsEgTOH8CUz8ht_2Sri-C__Ifp7hLBKqYanu7DdfVM5dlz_YxgnYHa5Dzi9g8BWAw60rOShznXva8gGmq2AetoSLSwLVOVDp43sgksXu5T5z2oU06pFlg1XbYe2s8UFcSb5xv8nNoqqYFdmakbtkFo86rxxmAh24tnQemeBOntOzlBLPD7Dlm1SKOT_NtQQs=s16000)

  

**Raw Keyword Search with package/app name**

It's also good practice to do a raw keyword search using the package and the common name of the application to find other potentially relevant information. In my tests, I found traces of the package name in these files and folders as well:

- datacom.google.android.googlequicksearchboxdatabasesportable\_geller\_<Gmail\_username>.db
- datacom.google.android.googlequicksearchboxdatabasesportable\_providers\_<Gmail\_username>.db
- datacom.google.android.gmsfilesreportsstate\_dump.txt
- datacom.android.vendingcachesstreamdatastore.db
- datacom.android.vendingdatabasesauto\_update.db
- datacom.android.vendingdatabasesdata\_usage.db
- datacom.android.vendingdatabasesinstall\_queue.db
- datacom.android.vendingdatabasesinstall\_source.db
- datacom.android.vendingdatabasesprefetch\_recommendation\_store.db
- datacom.android.vendingdatabasesxternal\_referrer\_status.db
- datacom.android.vendingapp\_commerce\_acquire\_cache
- datacom.samsung.android.sm.devicesecuritydatabasesAPP\_INFO\_TBL
- datacom.samsung.android.sm.devicesecuritydatabasesdevice\_security.db
- datacom.sec.android.provider.badgedatabasesbadge.db
- logpackage\_dump.txt
- logdumpstate\_lowbattery\_NN.log
- logdumpstate\_app\_native.zip
- logdumpstate\_sys\_watchdog.zip
- logacore\_dumpcore\_all.txt
- miscstats-data
- systembatterystats-daily.xml
- systempackage-cstats.list
- systemipm\_input\_data.txt

#### \*\*\*\*\*\[UPDATE 14 DAYS AFTER UNINSTALL \]\*\*\*\*\*

#### 

14 days after uninstalling I was still able to find traces in:

- localappstate.db 
- gass.db
- SMS Messages
- Files stored on the emulated SD card
- Firebase Cloud Messaging
- Digital Wellbeing
- PkgPredictions.db
- Usage stats (only one reference per package)
- Battery stats
- Library.db
- Some of the files mentioned in the "Raw keyword search for the app bundle and the app common name" section

# Conclusions

Based on my personal experience and on testing, here is a starting checklist for the analysis of application usage. I'll be happy to improve this list based on future tests or suggestions from readers.

  

**Suggested steps on iOS**

1. Verify if the app is still installed or not

- Check if the XYZ bundle is available under privatevarcontainersBundleApplication<GUID>
- Check whether the XYZ App Sandbox folder is available under privatevarmobileContainersDataApplication<GUID>
- Check whether the XYZ App Shared folder is available under privatevarmobileContainersSharedAppGroup<GUID>
- Analyze:

- Mobile installation logs
- ApplicationState.db
- containers3.sqlite

3. If the app is still installed analyze:

1. Mobile Installation Logs (App install/uninstall time)
2. Data from the specific app parsed by different tools
3. Every and each file in the App Sandbox
4. SMS Messages related to app activation/usage (i.e. 2FA messages)
5. iOS Notifications (verify possibly deleted messages, including content)
6. Screenshots automatically/manually generated and related to the specific app (i.e. Telegram chat screenshot)
7. For chat/messaging app, analyze the main database and search for missing rows (verify possibly deleted messages, approximate timestamp)
8. Photos.sqlite (search for file saved by the app of interest)
9. InteractionC (verify possibly deleted messages, only metadata)
10. Power logs and backups (interaction with the App)
11. Biome (interactions with the App)
12. KnowledgeC (interactions with the App)
13. Screen Time
14. Net Usage / Data Usage
15. TCC (Permissions)
16. CrashLogs
17. storeUser.db (historical usage of the app by the iCloud user)
18. Raw keyword search for the app bundle and the app common name
19. Extract and analyze syslog archive from a Sysdiagnose

5. If the app is not installed:

1. Mobile Installation Logs (App install/uninstall time)
2. SMS Messages related to app activation/usage (i.e. 2FA messages)
3. iOS Notifications (verify possibly deleted messages, including content)
4. Screenshots automatically/manually generated and related to the specific app (i.e. Telegram chat screenshot)
5. Photos.sqlite (search for file saved by the app of interest)
6. InteractionC (verify possibly deleted messages, only metadata)
7. Power logs and backups (interaction with the App)
8. KnowledgeC (app install and uninstall)
9. Screen Time
10. Net Usage / Data Usage
11. TCC (Permissions)
12. CrashLogs
13. storeUser.db (historical usage of the app by the iCloud user)
14. Raw keyword search for the app bundle and the app common name
15. Extract and analyze syslog archive from a Sysdiagnose

Some personal and final thoughts on iOS, based on the tests conducted and previous experiences. **During testing, I found that the entries in Biome, KnowlegeC, TCC and ScreeTime are deleted when the app is uninstalled**. Entries in interactionC were deleted when Telegram was uninstalled, but not when Signal was uninstalled. Deleted rows could be recovered from all these databases, especially if the app was recently deleted.

  

**The chances of recovering content from the uninstalled app are limited to screenshots, photos saved by the app in the camera roll, files saved by the app anywhere on the device (e.g. the Files" app) and notifications**: these files are not deleted when the app is uninstalled. **Entries in the Power logs database are also not deleted when the app is uninstalled**, so this database and its backups may be relevant for identifying interactions with the app. The database that stores information the longest is the "storeUser.db" as it relates to the iCloud account and not specifically to the device you are analyzing.

  

If you have no way to get a full file system, you are limited when investigating deleted apps: you can still extract and analyze the camera roll, photos.sqlite, interactionc, data usage, tcc and SMS messages, but notifications, knowledgec, screen time, net usage, and power log are not available. In this case, it is mandatory to create and extract a sysdiagonose that gives you access to the mobile installation logs, power logs (current only, no backups) and syslog data.

**Suggested steps on Android**

1. Verify if the app is still installed or not

- Analyze the "/app" and "/data" folders in the "Userdata" partition and look for folders and APKs related to the package of interest.
- Analyze:

- packages.list
- packages.xml
- frosting.db
- localappstate.db
- gass.db

3. If the app is still installed analyze:

1. Data from the specific app parsed by different tools
2. Every and each file in the App Sandbox
3. SMS Messages related to app activation/usage (i.e. 2FA messages)
4. Screenshots automatically/manually generated and related to the specific app (i.e. Telegram chat screenshot)
5. Media stored in the emulated SD card
6. For chat/messaging app, analyze the main database and search for missing rows (verify possibly deleted messages, approximate timestamp)
7. Package permissions
8. Package run-time Permissions
9. Package tasks
10. Privacy Dashboard
11. AppOps
12. Digital Wellbeing
13. Firebase Cloud Messaging
14. PkgPredictions.db
15. notification\_log.db
16. Usage stats
17. Network stats
18. Battery stats
19. Graphic stats
20. Library.db
21. Raw keyword search for the app bundle and the app's common name
22. Extract and analyze a bugreport

5. If the app is not installed:

1. Analyze "localappstate.db" and "gass.db", and check which apps were installed via the Google Play Store and compare them to the list of actually installed apps
2. Search for SMS messages related to app usage (e.g. activation and 2FA messages)
3. Files stored on the emulated SD card
4. Screenshots created manually by the user that relate to the specific app
5. Digital Wellbeing
6. Firebase Cloud Messaging
7. PkgPredictions.db (Samsung devices only)
8. notification\_log.db (Samsung devices only, up to Android 10)
9. Usage stats
10. Battery stats
11. Graphic stats
12. Library.db
13. Raw keyword search for the app bundle and the app common name
14. Extract and analyze a bugreport

Some personal and final thoughts on Android, based on the tests conducted and previous experiences. During testing, I found that **package permissions, package runtime permissions, package tasks, privacy dashboard, appops, and network stats are deleted when an app is uninstalled**. 

  

**The chances of recovering content from the uninstalled app are limited to screenshots, files stored by the app on the emulated SD card, and Firebase cloud messaging**: these files are not deleted when the app is uninstalled. **Two relevant artifacts that need to be analyzed to investigate usage traces are usage stats and battery stats**: especially the latter still require a lot of research and are not supported by the tools. The database that stores information the longest is the "library.db" as it relates to the Gmail account and not specifically to the device you are analyzing.

  

If you have no way to get a full file system, you are very limited when investigating deleted apps: you can still extract and analyze the files in the emulated SD card and SMS messages, but localappstate.db, gass.db, digital wellbeing, firebase cloud messaging, PkgPredictions.db, notification\_log.db, Usage stats, Battery stats, Graphic stats, and Library.db are not available. In this case, it is mandatory to create and extract a bugreport that gives you at least access to usage stats and battery stats data.  

  

And that's all for the last post of 2023! See you next year!

  

Go to Source
