---
title: "2021-009-Jasmine_Jackson-TheFluffy007-analyzing_android_apps-FRida-Part2"
date: Sun, 07 Mar 2021 19:13:34 +0000
draft: false
type: posts
categories: 
- 
---
# 2021-009-Jasmine_Jackson-TheFluffy007-analyzing_android_apps-FRida-Part2

<br/>

<br/>
@thefluffy007

A Bay Area Native (Berkeley)

I always tell people my computer journey started at 14, but it really started at 5th grade (have a good story to tell about this)

Was a bad student in my ninth grade year - almost kicked out of high school due to cutting. Had a 1.7 GPA. After my summer internship turned it around to a 4.0.

Once I graduated from high school, I knew I wanted to continue on the path of computers. Majored in Computer Science

Graduated with Bachelors and Masters in Computer Science. Graduate Certificate in Information Security and Privacy. Minor in Math.

Interested in security from a Yahoo! Group on Cryptography. Liked how you can turn text into gibberish and back again.

Became interested in penetration testing after moving to Charlotte, and moonlighted as a QA while a full-stack developer.

Co-workers did not want me to test their code because I would always find bugs.

Moved into penetration testing space.

Always had an interest in mobile, but never did mobile development and decided it wasn’t for me

Became interested in bug bounties and noticed that mobile payouts were higher.

At this time also completed SANS 575 - Mobile Device Security and Ethical Hacking.  
Realized the barrier to entry was VERY (almost non-existent) low in Android as it’s open source.

Started to learn/expand mobile hacking on my own time

The threat exposure is VERY high with mobile hacking. As you have a web app component, network component, and phone component. I always reference a slide from Secure Works.

Link to YouTube Channel → [thefluffy007 - YouTube](https://www.youtube.com/channel/UCTiJrj32RNZ87lbv0mEF9uQ)

[thefluffy007 – A security researchers thoughts on all things security – web, mobile, and cloud](https://thefluffy007.com/)

[The Mobile App Security Company | NowSecure](https://www.nowsecure.com/)

[owasp-mstg/Crackmes at master · OWASP/owasp-mstg · GitHub](https://github.com/OWASP/owasp-mstg/tree/master/Crackmes)

[Rana Android Malware (reversinglabs.com)](https://blog.reversinglabs.com/blog/rana-android-malware)

[These 21 Android Apps Contain Malware | PCMag](https://www.pcmag.com/news/these-21-android-apps-contain-adware)

Android Tamer  -[Android Tamer](https://androidtamer.com/)

[The Diary of an (Inexperienced) Bug Hunter - Intro to Android Hacking | Bugcrowd](https://www.bugcrowd.com/resources/webinars/the-diary-of-an-inexperienced-bug-hunter-intro-to-android-hacking/)

[**Android Debug Bridge (adb)  |  Android Developers**](https://developer.android.com/studio/command-line/adb)

Goal: discussing best practices and methods to reverse engineer Android applications

[Introduction to Java (w3schools.com)](https://www.w3schools.com/java/java_intro.asp)

[JavaScript Introduction (w3schools.com)](https://www.w3schools.com/js/js_intro.asp)

[Introduction to Python (w3schools.com)](https://www.w3schools.com/python/python_intro.asp)

[Frida • A world-class dynamic instrumentation framework | Inject JavaScript to explore native apps on Windows, macOS, GNU/Linux, iOS, Android, and QNX](https://frida.re/) (Frida can be used with JavaScript, and Python, along with other languages)

[GitHub - dweinstein/awesome-frida: Awesome Frida - A curated list of Frida resources http://www.frida.re/ (](https://github.com/dweinstein/awesome-frida)[https://github.com/frida/frida](https://github.com/frida/frida)[)](https://github.com/dweinstein/awesome-frida)

Android APK crackme: [owasp-mstg/0x05c-Reverse-Engineering-and-Tampering.md at master · OWASP/owasp-mstg · GitHub](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05c-Reverse-Engineering-and-Tampering.md)

[Reverse-Engineering - YobiWiki](http://wiki.yobi.be/wiki/Reverse-Engineering)

[Apktool - A tool for reverse engineering 3rd party, closed, binary Android apps. (ibotpeaches.github.io)](https://ibotpeaches.github.io/Apktool/)

[GitHub - MobSF/Mobile-Security-Framework-MobSF: Mobile Security Framework (MobSF) is an automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis and security assessment framework capable of performing static and dynamic analysis.](https://github.com/MobSF/Mobile-Security-Framework-MobSF)

[IntroAndroidSecurity download | SourceForge.net](https://sourceforge.net/projects/introandroidsecurity/) ←- link to my virtual machine and Androidx86 emulator

Background:

\*\*consider this a primer for any class you might teach, a teaser, if you will\*\*

**Why do we want to be able to reverse engineer APKs and IPKs?** 

Android APKS (Android Packages) holds the source code to the application. If you can reverse this you will essentially have the keys to the kingdom. Developers and companies (if they’re proprietary) will add obfuscation - a technique to make the code unreadable to thwart reverse engineers from finding out their code.

**What are some of the structures and files contained in APKs that are useful for ppl analyzing binaries?**

Android applications have to have a MainActivity (written in Java). This activity is the entry point to the application.

Android applications also have an AndroidManifest.xml file which is the skeleton of the application. This describes the main activity, intents, service providers, permissions, and what Android operating system can run the application.

**When testing apps for security, how easy is it to emulate security and physical controls if you’re not on a handset?** 

Pretty easy. You can use an emulator. I must forewarn though - you will need A LOT of memory for it to work effectively.

Are there ever any times you HAVE to use a handset? An app that tests something like Android’s Safetynet and won’t run without it? Do they ever want perf testing on their apps?

Was thinking about how you check events in logs, battery drain, using apps on older Android/iOS versions? 

When organizations or developers ask you to test an app, is there anything in particular in scope? Out of scope?  
  

How do progressive web apps differ than a more traditional app?

Lab setup

IntroToAndroidSecurity VM

Android Emulator

Tools to use

Why use them? (free, full-featured)

Setup and installation

OS-specific tools?

Tools used - Frida, Jadx-GUI (or command line), text editor. All of these items are free.

No setup required if using my virtual machine :-)

These apps are OS specific if you choose Linux or Windows.

Callbacks

Methodology

Decompile the application - can use a tool titled - Apktool (free)

Look “under the hood” of the application - Jadx-GUI (Graphical User Interface) or Jadx-CLI (command line)

Connect your emulator/device using Android Debug Bridge (adb)

Get version of Frida on device

Look online to find correct version of Frida \*\*this is important\*\*

Start to play around with the tool and see if you receive error messages/prompts. Can then go back to code that was reverse engineered and see where it’s located.

Best practices

Leave no stones unturned! Meaning you might see something that seems too rudimentary to work - and yet it does.

Cert pinning - 

Typical issues seen

Hard-coded passwords, data that is not being encrypted in rest or transit. 

Check out our Store on Teepub! **https://brakesec.com/store**

**Join us on our #Slack Channel! Send a request to @brakesec on Twitter or email [bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

**#AmazonMusic:** [https://brakesec.com/amazonmusic](https://brakesec.com/amazonmusic) 

**#Spotify**: [https://brakesec.com/spotifyBDS](https://brakesec.com/spotifyBDS)  
  
**#Pandora**: [https://brakesec.com/pandora](https://brakesec.com/pandora) 

**#RSS**: [https://brakesec.com/BrakesecRSS](https://brakesec.com/BrakesecRSS)

**#Youtube Channel**:  [http://www.youtube.com/c/BDSPodcast](http://www.youtube.com/c/BDSPodcast)

**#iTunes** Store Link: [https://brakesec.com/BDSiTunes](https://brakesec.com/BDSiTunes)

**#Google Play** Store: [https://brakesec.com/BDS-GooglePlay](https://brakesec.com/BDS-GooglePlay)

Our main site:  [https://brakesec.com/bdswebsite](https://brakesec.com/bdswebsite)

**#iHeartRadio** App:  [https://brakesec.com/iHeartBrakesec](https://brakesec.com/iHeartBrakesec)

**#SoundCloud**: [https://brakesec.com/SoundcloudBrakesec](https://brakesec.com/SoundcloudBrakesec)

Comments, Questions, Feedback: **[bds.podcast@gmail.com](mailto:bds.podcast@gmail.com)**

Support Brakeing Down Security Podcast by using our **#Paypal**: [https://brakesec.com/PaypalBDS](https://brakesec.com/PaypalBDS) OR our **#Patreon**

[https://brakesec.com/BDSPatreon](https://brakesec.com/BDSPatreon)

**#Twitter**: [@brakesec](https://twitter.com/brakesec) [@boettcherpwned](https://twitter.com/boettcherpwned) [@bryanbrake](https://twitter.com/bryanbrake) [@infosystir](https://twitter.com/infosystir)

**#Player.FM** : [https://brakesec.com/BDS-PlayerFM](https://brakesec.com/BDS-PlayerFM)

**#Stitcher** Network: [https://brakesec.com/BrakeSecStitcher](https://brakesec.com/BrakeSecStitcher)

**#TuneIn** Radio App: [https://brakesec.com/TuneInBrakesec](https://brakesec.com/TuneInBrakesec)

#### [Source](http://brakeingsecurity.com/2021-009-jasmine_jackson-thefluffy007-analyzing_android_apps-frida-part2)

<br/>
---
