---
title: "Android Sec"
date: 2024-08-06
draft: false
type: posts
categories:
  - Sec, Android, Security
tags:
  - sec, android, security
summary: "Android Sec Tools"
---

# Android Security
A collection of Android security-related resources.

## Tools

### Online Analyzers

- [AndroTotal](http://andrototal.org/)
- [Appknox](https://www.appknox.com/) - not free
- [Virustotal](https://www.virustotal.com/) - max 128MB
- [Fraunhofer App-ray](http://app-ray.co/) - not free
- [NowSecure Lab Automated](https://www.nowsecure.com/blog/2016/09/19/announcing-nowsecure-lab-automated/) - Enterprise tool for mobile app security testing both Android and iOS mobile apps. Lab Automated features dynamic and static analysis on real devices in the cloud to return results in minutes. Not free
- [App Detonator](https://appdetonator.run/) - Detonate APK binary to provide source code level details including app author, signature, build, and manifest information. 3 Analysis/day free quota.
- [Pithus](https://beta.pithus.org/) - Open-Source APK analyzer. Still in Beta for the moment and limited to static analysis for the moment. Possible to hunt malware with Yara rules. More [here](https://beta.pithus.org/about/).
- [Oversecured](https://oversecured.com/) - Enterprise vulnerability scanner for Android and iOS apps, it offers app owners and developers the ability to secure each new version of a mobile app by integrating Oversecured into the development process. Not free.
- [AppSweep by Guardsquare](https://appsweep.guardsquare.com/) - Free, fast Android application security testing for developers
- [Koodous](https://koodous.com) - Performs static/dynamic malware analysis over a vast repository of Android samples and checks them against public and private Yara rules.
- [Immuniweb](https://www.immuniweb.com/mobile/). Does a "OWASP Mobile Top 10 Test", "Mobile App Privacy Check" and an application permissions test. Free tier is 4 tests per day incl. report after registration
- [BitBaan](https://malab.bitbaan.com/)
- [AVC UnDroid](http://undroid.av-comparatives.info/)
- [AMAaaS](https://amaaas.com) - Free Android Malware Analysis Service. A bare-metal service features static and dynamic analysis for Android applications. A product of [MalwarePot](https://malwarepot.com/index.php/AMAaaS).
- [AppCritique](https://appcritique.boozallen.com) - Upload your Android APKs and receive comprehensive free security assessments
- [NVISO ApkScan](https://apkscan.nviso.be/) - sunsetting on Oct 31, 2019
- [Mobile Malware Sandbox](http://www.mobilemalware.com.br/analysis/index_en.php)
- [IBM Security AppScan Mobile Analyzer](https://appscan.bluemix.net/mobileAnalyzer) - not free
- [Visual Threat](https://www.visualthreat.com/) - no longer an Android app analyzer
- [Tracedroid](http://tracedroid.few.vu.nl/)
- [habo](https://habo.qq.com/) - 10/day
- [CopperDroid](http://copperdroid.isg.rhul.ac.uk/copperdroid/)
- [SandDroid](http://sanddroid.xjtu.edu.cn/)
- [Stowaway](http://www.android-permissions.org/)
- [Anubis](http://anubis.iseclab.org/)
- [Mobile app insight](http://www.mobile-app-insight.org)
- [Mobile-Sandbox](http://mobile-sandbox.com)
- [Ijiami](http://safe.ijiami.cn/)
- [Comdroid](http://www.comdroid.org/)
- [Android Sandbox](http://www.androidsandbox.net/)
- [Foresafe](http://www.foresafe.com/scan)
- [Dexter](https://dexter.dexlabs.org/)
- [MobiSec Eacus](http://www.mobiseclab.org/eacus.jsp)
- [Fireeye](https://fireeye.ijinshan.com/)- max 60MB 15/day
- [approver](https://approver.talos-sec.com/) - Approver  is a fully automated security analysis and risk assessment platform for Android and iOS apps. Not free.

### Static Analysis Tools

- [Androwarn](https://github.com/maaaaz/androwarn/) - detect and warn the user about potential malicious behaviors developed by an Android application.
- [ApkAnalyser](https://github.com/sonyxperiadev/ApkAnalyser)
- [APKInspector](https://github.com/honeynet/apkinspector/)
- [Droid Intent Data Flow Analysis for Information Leakage](https://www.cert.org/secure-coding/tools/didfail.cfm)
- [DroidLegacy](https://bitbucket.org/srl/droidlegacy)
- [FlowDroid](https://blogs.uni-paderborn.de/sse/tools/flowdroid/)
- [Android Decompiler](https://www.pnfsoftware.com/) – not free
- [PSCout](https://security.csl.toronto.edu/pscout/) - A tool that extracts the permission specification from the Android OS source code using static analysis
- [Amandroid](http://amandroid.sireum.org/)
- [SmaliSCA](https://github.com/dorneanu/smalisca) - Smali Static Code Analysis
- [CFGScanDroid](https://github.com/douggard/CFGScanDroid) - Scans and compares CFG against CFG of malicious applications
- [Madrolyzer](https://github.com/maldroid/maldrolyzer) - extracts actionable data like C&C, phone number etc.
- [SPARTA](https://www.cs.washington.edu/sparta) - verifies (proves) that an app satisfies an information-flow security policy; built on the [Checker Framework](https://types.cs.washington.edu/checker-framework/)
- [ConDroid](https://github.com/JulianSchuette/ConDroid) - Performs a combination of symbolic + concrete execution of the app
- [DroidRA](https://github.com/serval-snt-uni-lu/DroidRA)
- [RiskInDroid](https://github.com/ClaudiuGeorgiu/RiskInDroid) - A tool for calculating the risk of Android apps based on their permissions, with an online demo available.
- [SUPER](https://github.com/SUPERAndroidAnalyzer/super) - Secure, Unified, Powerful and Extensible Rust Android Analyzer
- [ClassyShark](https://github.com/google/android-classyshark) - Standalone binary inspection tool which can browse any Android executable and show important info.
- [StaCoAn](https://github.com/vincentcox/StaCoAn) - Cross-platform tool which aids developers, bug-bounty hunters, and ethical hackers in performing static code analysis on mobile applications. This tool was created with a big focus on usability and graphical guidance in the user interface.
- [JAADAS](https://github.com/flankerhqd/JAADAS) - Joint intraprocedural and interprocedural program analysis tool to find vulnerabilities in Android apps, built on Soot and Scala
- [Quark-Engine](https://github.com/quark-engine/quark-engine) - An Obfuscation-Neglect Android Malware Scoring System
- [One Step Decompiler](https://github.com/b-mueller/apkx) - Android APK Decompilation for the Lazy
- [APKLeaks](https://github.com/dwisiswant0/apkleaks) - Scanning APK file for URIs, endpoints & secrets.
- [Mobile Audit](https://github.com/mpast/mobileAudit) - Web application for performing Static Analysis and detecting malware in Android APKs.
- [Smali CFG generator](https://github.com/EugenioDelfa/Smali-CFGs)
- [Several tools from PSU](http://siis.cse.psu.edu/tools.html)

### App Vulnerability Scanners

- [QARK](https://github.com/linkedin/qark/) - QARK by LinkedIn is for app developers to scan apps for security issues
- [AndroBugs](https://github.com/AndroBugs/AndroBugs_Framework)
- [Nogotofail](https://github.com/google/nogotofail)
- [Devknox](https://devknox.io/) - IDE plugin to build secure Android apps. Not maintained anymore.

### Dynamic Analysis Tools

- [Android DBI frameowork](http://www.mulliner.org/blog/blosxom.cgi/security/androiddbiv02.html)
- [Androl4b](https://github.com/sh4hin/Androl4b)- A Virtual Machine For Assessing Android applications, Reverse Engineering and Malware Analysis
- [House](https://github.com/nccgroup/house)- House: A runtime mobile application analysis toolkit with a Web GUI, powered by Frida, written in Python.
- [Mobile-Security-Framework MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) - Mobile Security Framework is an intelligent, all-in-one open-source mobile application (Android/iOS) automated pen-testing framework capable of performing static, dynamic analysis and web API testing.
- [AppUse](https://appsec-labs.com/AppUse/) – custom build for penetration testing
- [Droidbox](https://github.com/pjlantz/droidbox)
- [Drozer](https://github.com/mwrlabs/drozer)
- [Xposed](https://forum.xda-developers.com/xposed/xposed-installer-versions-changelog-t2714053) - equivalent of doing Stub-based code injection but without any modifications to the binary
- [Inspeckage](https://github.com/ac-pm/Inspeckage) - Android Package Inspector - dynamic analysis with API hooks, start unexported activities, and more. (Xposed Module)
- [Android Hooker](https://github.com/AndroidHooker/hooker) - Dynamic Java code instrumentation (requires the Substrate Framework)
- [ProbeDroid](https://github.com/ZSShen/ProbeDroid) - Dynamic Java code instrumentation
- [DECAF](https://github.com/sycurelab/DECAF) - Dynamic Executable Code Analysis Framework based on QEMU (DroidScope is now an extension to DECAF)
- [CuckooDroid](https://github.com/idanr1986/cuckoo-droid) - Android extension for Cuckoo sandbox
- [Mem](https://github.com/MobileForensicsResearch/mem) - Memory analysis of Android (root required)
- [Crowdroid](http://www.ida.liu.se/labs/rtslab/publications/2011/spsm11-burguera.pdf) – unable to find the actual tool
- [AuditdAndroid](https://github.com/nwhusted/AuditdAndroid) – android port of auditd, not under active development anymore
- [Android Security Evaluation Framework](https://code.google.com/p/asef/) - not under active development anymore
- [Aurasium](https://github.com/xurubin/aurasium) – Practical security policy enforcement for Android apps via bytecode rewriting and in-place reference monitor.
- [Android Linux Kernel modules](https://github.com/strazzere/android-lkms)
- [Appie](https://manifestsecurity.com/appie/) - Appie is a software package that has been pre-configured to function as an Android Pentesting Environment. It is completely portable and can be carried on a USB stick or smartphone. This is a one-stop answer for all the tools needed in Android Application Security Assessment and an awesome alternative to existing virtual machines.
- [StaDynA](https://github.com/zyrikby/StaDynA) - a system supporting security app analysis in the presence of dynamic code update features (dynamic class loading and reflection). This tool combines static and dynamic analysis of Android applications in order to reveal the hidden/updated behavior and extend static analysis results with this information.
- [DroidAnalytics](https://github.com/zhengmin1989/DroidAnalytics) - incomplete
- [Vezir Project](https://github.com/oguzhantopgul/Vezir-Project) - Virtual Machine for Mobile Application Pentesting and Mobile Malware Analysis
- [MARA](https://github.com/xtiankisutsa/MARA_Framework) - Mobile Application Reverse Engineering and Analysis Framework
- [Taintdroid](http://appanalysis.org) - requires AOSP compilation
- [ARTist](https://artist.cispa.saarland) - a flexible open-source instrumentation and hybrid analysis framework for Android apps and Android's Java middleware. It is based on the Android Runtime's (ART) compiler and modifies code during on-device compilation.
- [Android Malware Sandbox](https://github.com/Areizen/Android-Malware-Sandbox)
- [AndroPyTool](https://github.com/alexMyG/AndroPyTool) - a tool for extracting static and dynamic features from Android APKs. It combines different well-known Android app analysis tools such as DroidBox, FlowDroid, Strace, AndroGuard, or VirusTotal analysis.
- [Runtime Mobile Security (RMS)](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security) - is a powerful web interface that helps you to manipulate Android and iOS Apps at Runtime
- [PAPIMonitor](https://github.com/Dado1513/PAPIMonitor) – PAPIMonitor (Python API Monitor for Android apps) is a Python tool based on Frida for monitoring user-select APIs during the app execution.
- [Android_application_analyzer](https://github.com/NotSoSecure/android_application_analyzer) - The tool is used to analyze the content of the Android application in local storage.
- [Decompiler.com](https://www.decompiler.com/) - Online APK and Java decompiler
- [Android Tamer](https://androidtamer.com/) - Virtual / Live Platform for Android Security Professionals
- [Android Malware Analysis Toolkit](http://www.mobilemalware.com.br/amat/download.html) - (Linux distro) Earlier it use to be an [online analyzer](http://dunkelheit.com.br/amat/analysis/index_en.php)
- [Android Reverse Engineering](https://redmine.honeynet.org/projects/are/wiki) – ARE (android reverse engineering) not under active development anymore
- [ViaLab Community Edition](https://www.nowsecure.com/blog/2014/09/09/introducing-vialab-community-edition/)
- [Mercury](https://labs.mwrinfosecurity.com/tools/2012/03/16/mercury/)
- [Cobradroid](https://thecobraden.com/projects/cobradroid/) – custom image for malware analysis

### Reverse Engineering

- [Smali/Baksmali](https://github.com/JesusFreke/smali) – apk decompilation
- [emacs syntax coloring for smali files](https://github.com/strazzere/Emacs-Smali)
- [vim syntax coloring for smali files](http://codetastrophe.com/smali.vim)
- [AndBug](https://github.com/swdunlop/AndBug)
- [Androguard](https://github.com/androguard/androguard) – powerful, integrates well with other tools
- [Apktool](https://ibotpeaches.github.io/Apktool/) – really useful for compilation/decompilation (uses smali)
- [Android Framework for Exploitation](https://github.com/appknox/AFE)
- [Bypass signature and permission checks for IPCs](https://github.com/iSECPartners/Android-KillPermAndSigChecks)
- [Android OpenDebug](https://github.com/iSECPartners/Android-OpenDebug) – make any application on the device debuggable (using cydia substrate).
- [Dex2Jar](https://github.com/pxb1988/dex2jar) - dex to jar converter
- [Enjarify](https://github.com/google/enjarify) - dex to jar converter from Google
- [Dedexer](https://sourceforge.net/projects/dedexer/)
- [Fino](https://github.com/sysdream/fino)
- [Frida](https://www.frida.re/) - inject javascript to explore applications and a [GUI tool](https://github.com/antojoseph/diff-gui) for it
- [Indroid](https://bitbucket.org/aseemjakhar/indroid) – thread injection kit
- [IntentSniffer](https://www.nccgroup.com/us/our-research/intent-sniffer/)
- [Introspy](https://github.com/iSECPartners/Introspy-Android)
- [Jad]( https://varaneckas.com/jad/) - Java decompiler
- [JD-GUI](https://github.com/java-decompiler/jd-gui) - Java decompiler
- [CFR](http://www.benf.org/other/cfr/) - Java decompiler
- [Krakatau](https://github.com/Storyyeller/Krakatau) - Java decompiler
- [FernFlower](https://github.com/fesh0r/fernflower) - Java decompiler
- [Redexer](https://github.com/plum-umd/redexer) – apk manipulation
- [Simplify Android deobfuscator](https://github.com/CalebFenton/simplify)
- [Bytecode viewer](https://github.com/Konloch/bytecode-viewer)
- [Radare2](https://github.com/radare/radare2)
- [Jadx](https://github.com/skylot/jadx)
- [Dwarf](https://github.com/iGio90/Dwarf) - GUI for reverse engineering
- [Andromeda](https://github.com/secrary/Andromeda) - Another basic command-line reverse engineering tool
- [apk-mitm](https://github.com/shroudedcode/apk-mitm) - A CLI application that prepares Android APK files for HTTPS inspection
- [Noia](https://github.com/0x742/noia) - Simple Android application sandbox file browser tool
- [Obfuscapk](https://github.com/ClaudiuGeorgiu/Obfuscapk) - Obfuscapk is a modular Python tool for obfuscating Android apps without needing their source code.
- [ARMANDroid](https://github.com/Mobile-IoT-Security-Lab/ARMANDroid) - ARMAND (Anti-Repackaging through Multi-patternAnti-tampering based on Native Detection) is a novel anti-tampering protection scheme that embeds logic bombs and AT detection nodes directly in the apk file without needing their source code.
- [MVT (Mobile Verification Toolkit)](https://github.com/mvt-project/mvt) - a collection of utilities to simplify and automate the process of gathering forensic traces helpful to identify a potential compromise of Android and iOS devices
- [Dexmod](https://github.com/google/dexmod) - tool to exemplify patching Dalvik bytecode in a DEX (Dalvik Executable) file, and assist in the static analysis of Android applications.
- [Procyon](https://bitbucket.org/mstrobel/procyon/wiki/Java%20Decompiler) - Java decompiler
- [Smali viewer](http://blog.avlyun.com/wp-content/uploads/2014/04/SmaliViewer.zip)
- [ZjDroid](https://github.com/BaiduSecurityLabs/ZjDroid), [fork/mirror](https://github.com/yangbean9/ZjDroid)
- [Dare](http://siis.cse.psu.edu/dare/index.html) – .dex to .class converter

### Fuzz Testing

- [Radamsa Fuzzer](https://github.com/anestisb/radamsa-android)
- [Honggfuzz](https://github.com/google/honggfuzz)
- [An Android port of the Melkor ELF fuzzer](https://github.com/anestisb/melkor-android)
- [Media Fuzzing Framework for Android](https://github.com/fuzzing/MFFA)
- [AndroFuzz](https://github.com/jonmetz/AndroFuzz)
- [QuarksLab's Android Fuzzing](https://github.com/quarkslab/android-fuzzing)
- [IntentFuzzer](https://www.nccgroup.trust/us/about-us/resources/intent-fuzzer/)

### App Repackaging Detectors

- [FSquaDRA](https://github.com/zyrikby/FSquaDRA) - a tool for the detection of repackaged Android applications based on app resources hash comparison.

### Market Crawlers

- [Google Play crawler (Java)](https://github.com/Akdeniz/google-play-crawler)
- [Google Play crawler (Python)](https://github.com/egirault/googleplay-api)
- [Google Play crawler (Node)](https://github.com/dweinstein/node-google-play) - get app details and download apps from the official Google Play Store.
- [Aptoide downloader (Node)](https://github.com/dweinstein/node-aptoide) - download apps from Aptoide third-party Android market
- [Appland downloader (Node)](https://github.com/dweinstein/node-appland) - download apps from Appland third-party Android market
- [PlaystoreDownloader](https://github.com/ClaudiuGeorgiu/PlaystoreDownloader) - PlaystoreDownloader is a tool for downloading Android applications directly from the Google Play Store. After an initial (one-time) configuration, applications can be downloaded by specifying their package name.
- [APK Downloader](https://apkcombo.com/apk-downloader/) Online Service to download APK from Playstore for specific Android Device Configuration
- [Apkpure](https://apkpure.com/) - Online apk downloader. Provides also its own app for downloading.

### Misc Tools

- [smalihook](http://androidcracking.blogspot.com/2011/03/original-smalihook-java-source.html)
- [AXMLPrinter2](http://code.google.com/p/android4me/downloads/detail?name=AXMLPrinter2.jar) - to convert binary XML files to human-readable XML files
- [adb autocomplete](https://github.com/mbrubeck/android-completion)
- [mitmproxy](https://github.com/mitmproxy/mitmproxy)
- [dockerfile/androguard](https://github.com/dweinstein/dockerfile-androguard)
- [Android Vulnerability Test Suite](https://github.com/AndroidVTS/android-vts) - android-vts scans a device for set of vulnerabilities
- [AppMon](https://github.com/dpnishant/appmon)- AppMon is an automated framework for monitoring and tampering with system API calls of native macOS, iOS, and Android apps. It is based on Frida.
- [Internal Blue](https://github.com/seemoo-lab/internalblue) - Bluetooth experimentation framework based on Reverse Engineering of Broadcom Bluetooth Controllers
- [Android Mobile Device Hardening](https://github.com/SecTheTech/AMDH) - AMDH scans and hardens the device's settings and lists harmful installed Apps based on permissions.
- [Android Device Security Database](https://www.android-device-security.org/client/datatable) - Database of security features of Android devices
- [Opcodes table for quick reference](http://ww38.xchg.info/corkami/opcodes_tables.pdf)
- [APK-Downloader](http://codekiem.com/2012/02/24/apk-downloader/) - seems dead now
- [Dalvik opcodes](http://pallergabor.uw.hu/androidblog/dalvik_opcodes.html)

### Vulnerable Applications for practice

- [Damn Insecure Vulnerable Application (DIVA)](https://github.com/payatu/diva-android)
- [Vuldroid](https://github.com/jaiswalakshansh/Vuldroid)
- [ExploitMe Android Labs](http://securitycompass.github.io/AndroidLabs/setup.html)
- [GoatDroid](https://github.com/jackMannino/OWASP-GoatDroid-Project)
- [Android InsecureBank](https://github.com/dineshshetty/Android-InsecureBankv2)
- [Insecureshop](https://github.com/optiv/insecureshop)
- [Oversecured Vulnerable Android App (OVAA)](https://github.com/oversecured/ovaa)

## Academic/Research/Publications/Books

### Research Papers

- [Exploit Database](https://www.exploit-db.com/papers/)
- [Android security-related presentations](https://github.com/jacobsoo/AndroidSlides)
- [A good collection of static analysis papers](https://tthtlc.wordpress.com/2011/09/01/static-analysis-of-android-applications/)

### Books

- [SEI CERT Android Secure Coding Standard](https://www.securecoding.cert.org/confluence/display/android/Android+Secure+Coding+Standard)

### Others

- [OWASP Mobile Security Testing Guide Manual](https://github.com/OWASP/owasp-mstg)
- [doridori/Android-Security-Reference](https://github.com/doridori/Android-Security-Reference)
- [android app security checklist](https://github.com/b-mueller/android_app_security_checklist)
- [Mobile App Pentest Cheat Sheet](https://github.com/tanprathan/MobileApp-Pentest-Cheatsheet)
- [Android Reverse Engineering 101 by Daniele Altomare (Web Archive link)](http://web.archive.org/web/20180721134044/http://www.fasteque.com:80/android-reverse-engineering-101-part-1/)
- [Mobile Security Reading Room](https://mobile-security.zeef.com) - A reading room that contains well-categorized technical reading material about mobile penetration testing, mobile malware, mobile forensics, and all kind of mobile security-related topics

## Exploits/Vulnerabilities/Bugs

### List

- [Android Security Bulletins](https://source.android.com/security/bulletin/)
- [Android's reported security vulnerabilities](https://www.cvedetails.com/vulnerability-list/vendor_id-1224/product_id-19997/Google-Android.html)
- [AOSP - Issue tracker](https://code.google.com/p/android/issues/list?can=2&q=priority=Critical&sort=-opened)
- [OWASP Mobile Top 10 2016](https://www.owasp.org/index.php/Mobile_Top_10_2016-Top_10)
- [Exploit Database](https://www.exploit-db.com/search/?action=search&q=android) - click search
- [Vulnerability Google Doc](https://docs.google.com/spreadsheet/pub?key=0Am5hHW4ATym7dGhFU1A4X2lqbUJtRm1QSWNRc3E0UlE&single=true&gid=0&output=html)
- [Google Android Security Team’s Classifications for Potentially Harmful Applications (Malware)](https://source.android.com/security/reports/Google_Android_Security_PHA_classifications.pdf)
- [Android Devices Security Patch Status](https://kb.androidtamer.com/Device_Security_Patch_tracker/)

### Malware

- [androguard - Database Android Malware wiki](https://code.google.com/p/androguard/wiki/DatabaseAndroidMalwares)
- [Android Malware Github repo](https://github.com/ashishb/android-malware)
- [Android Malware Genome Project](http://www.malgenomeproject.org/policy.html) - contains 1260 malware samples categorized into 49 different malware families, free for research purposes.
- [Contagio Mobile Malware Mini Dump](http://contagiominidump.blogspot.com)
- [Drebin](https://www.sec.tu-bs.de/~danarp/drebin/)
- [Hudson Rock](https://www.hudsonrock.com/threat-intelligence-cybercrime-tools) - Free cybercrime intelligence toolset that can indicate if a specific APK package was compromised in an Infostealer malware attack.
- [Kharon Malware Dataset](http://kharon.gforge.inria.fr/dataset/) - 7 malware which have been reverse-engineered and documented
- [Android Adware and General Malware Dataset](https://www.unb.ca/cic/datasets/android-adware.html)
- [AndroZoo](https://androzoo.uni.lu/) - AndroZoo is a growing collection of Android Applications collected from several sources, including the official Google Play app market.
- [Android PRAGuard Dataset](http://pralab.diee.unica.it/en/AndroidPRAGuardDataset) - The dataset contains 10479 samples, obtained by obfuscating the MalGenome and the Contagio Minidump datasets with seven different obfuscation techniques.
- [Admire](http://admire.necst.it/)

### Bounty Programs

- [Android Security Reward Program](https://www.google.com/about/appsecurity/android-rewards/)

### How to report Security issues

- [Android - reporting security issues](https://source.android.com/security/overview/updates-resources.html#report-issues)
- [Android Reports and Resources](https://github.com/B3nac/Android-Reports-and-Resources) - List of Android Hackerone disclosed reports and other resources

