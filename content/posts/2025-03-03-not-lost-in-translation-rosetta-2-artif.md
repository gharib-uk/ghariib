---
title: "Not Lost in Translation Rosetta 2 Artifacts in macOS Intrusions"
date: Mon, 03 Mar 2025 14:00:00 +0000
draft: false
type: posts
---
# Not Lost in Translation Rosetta 2 Artifacts in macOS Intrusions





Written by: Joshua Goddard Executive Summary Rosetta 2 is Apple's translation technology for running x86-64 binaries on Apple Silicon (ARM64) macOS systems. Rosetta 2 translation creates a cache of Ahead-Of-Time (AOT) files that can serve as valuable forensic artifacts. Mandiant has observed sophisticated threat actors leveraging x86-64 compiled macOS malware, likely due to broader

Written by: Joshua Goddard

* * *

Executive Summary
-----------------

-   Rosetta 2 is Apple's translation technology for running x86-64 binaries on Apple Silicon (ARM64) macOS systems.
    
-   Rosetta 2 translation creates a cache of Ahead-Of-Time (AOT) files that can serve as valuable forensic artifacts.
    
-   Mandiant has observed sophisticated threat actors leveraging x86-64 compiled macOS malware, likely due to broader compatibility and relaxed execution policies compared to ARM64 binaries.
    
-   Analysis of AOT files, combined with FSEvents and Unified Logs (with a custom profile), can assist in investigating macOS intrusions.
    

Introduction
------------

Rosetta 2 (internally known on macOS as OAH) was introduced in macOS 11 (Big Sur) in 2020 to enable binaries compiled for x86-64 architectures to run on Apple Silicon (ARM64) architectures. Rosetta 2 translates signed and unsigned x86-64 binaries just-in-time or ahead-of-time at the point of execution. Mandiant has identified several new highly sophisticated macOS malware variants over the past year, notably compiled for x86-64 architecture. Mandiant assessed that this choice of architecture was most likely due to increased chances of compatibility on victim systems and more relaxed execution policies. Notably, macOS enforces [stricter code signing requirements](https://support.apple.com/en-gb/guide/security/secebb113be1/web) for ARM64 binaries compared to x86-64 binaries running under Rosetta 2, making unsigned ARM64 binaries more difficult to execute. Despite this, in the newly identified APT malware families observed by Mandiant over the past year, all were self-signed, likely to avoid other compensating security controls in place on macOS.

The Rosetta 2 Cache
-------------------

When a x86-64 binary is executed on a system with Rosetta 2 installed, the Rosetta 2 Daemon process (`oahd`) checks if an ahead-of-time (AOT) file already exists for the binary within the Rosetta 2 cache directory on the Data volume at `/var/db/oah/<UUID>/`. The UUID value in this file path appears to be randomly generated on install or update. If an AOT file does not exist, one will be created by writing translation code to a `.in_progress` file and then renaming it to a `.aot` file of the same name as the original binary. The Rosetta 2 Daemon process then runs the translated binary.

The `/var/db/oah` directory and its children are protected and owned by the OAH Daemon user account `_oahd`. Interaction with these files by other user accounts is only possible if [System Integrity Protection](https://support.apple.com/en-us/102149) (SIP) is disabled, which requires booting into recovery mode.

The directories under `/var/db/oah/<UUID>/` are binary UUID values that correspond to translated binaries. Specifically, these binary UUID values are SHA-256 hashes generated from a [combination](https://github.com/FFRI/AotPoisoning) of the binary file path, the Mach-O header, timestamps (created, modified, and changed), size, and ownership information. If the same binary is executed with any of these attributes changed, a new Rosetta AOT cache directory and file is created. While the content of the binaries is not part of this hashing function, changing the content of a file on an APFS file system will update the changed timestamp, which effectively means content changes can cause the creation of a new binary UUID and AOT file. Ultimately, the mechanism is designed to be extremely sensitive to any changes to x86-64 binaries at the byte and file system levels to reduce the risk of AOT poisoning.

![Sample Rosetta 2 cache directory structure and contents](https://storage.googleapis.com/gweb-cloudblog-publish/images/rosetta2-fig1.max-1000x1000.png)

Figure 1: Sample Rosetta 2 cache directory structure and contents

The Rosetta 2 cache binary UUID directories and the AOT files they contain appear to persist until macOS system updates. System updates have been found to cause the deletion of the cache directory (the Random UUID directory). After the upgrade, a directory with a different UUID value is created, and new Binary UUID directories and AOT files are created upon first launch of x86-64 binaries thereafter.

Translation and Universal Binaries
----------------------------------

When [universal binaries](https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary) (containing both x86-64 and ARM64 code) are executed by a x86-64 process running through Rosetta 2 translation, the x86-64 version of these binaries is executed, resulting in the creation of AOT files.

![Overview of execution of universal binaries with X864-64 processes translated through Rosetta 2 versus ARM64 processes](https://storage.googleapis.com/gweb-cloudblog-publish/images/rosetta2-fig2.max-1000x1000.png)

Figure 2: Overview of execution of universal binaries with X864-64 processes translated through Rosetta 2 versus ARM64 processes

In a Democratic People's Republic of Korea (DPRK) crypto heist investigation, Mandiant observed a x86-64 variant of the [POOLRAT](https://cloud.google.com/blog/topics/threat-intelligence/3cx-software-supply-chain-compromise) macOS backdoor being deployed and the attacker proceeding to execute universal system binaries including `ping`, `chmod`, `sudo`, `id`, and `cat` through the backdoor. This resulted in AOT files being created and provided evidence of attacker interaction on the system through the malware (Figure 5).

In some cases, the initial infection vector in macOS intrusions has involved legitimate x86-64 code that executes malware distributed as universal binaries. Because the initial x86-64 code runs under Rosetta 2, the x86-64 versions of malicious universal binaries are executed, leaving behind Rosetta 2 artifacts, including AOT files. In one case, a malicious Python 2 script led to the downloading and execution of a malicious universal binary. The Python 2 interpreter ran under Rosetta 2 since no ARM64 version was available, so the system executed the x86-64 version of the malicious universal binary, resulting in the creation of AOT files. Despite the attacker deleting the malicious binary later, we were able to analyze the AOT file to understand its functionality.

Unified Logs
------------

The Rosetta 2 Daemon emits logs to the macOS Unified Log; however, the binary name values are marked as private. These values can be configured to be shown in the logs with a [custom profile installed](https://cloud.google.com/blog/topics/threat-intelligence/reviewing-macos-unified-logs/). Informational logs are recorded for AOT file lookups, when cached AOT files are available and utilized, and when translation occurs and completes. For binaries that are not configured to log to the Unified Log and are not launched interactively, in some cases this was found to be the only evidence of execution within the Unified Logs. Execution may be correlated with other supporting artifacts; however, this is not always possible.

```
0x21b1afc Info 0x0 1596 0 oahd: <private>(1880): 
Aot lookup request for <private>

0x21b1afc Info 0x0 1596 0 oahd: <private>(1880): 
Translating image <private> -> <private>

0x21b1afc Info 0x0 1596 0 oahd: <private>(1880): 
Translation finished for <private>

0x21b1afc Info 0x0 1596 0 oahd: <private>(1880): 
Aot lookup request for <private>

0x21b1afc Info 0x0 1596 0 oahd: <private>(1880): 
Using cached aot <private> -> <private>
```

Figure 3: macOS Unified Logs showing Rosetta lookups, using cached files, and translating with private data disabled (default)

```
0x2ec304 Info 0x0 668 0 oahd: my_binary (Re(34180): 
Aot lookup request for /Users/Mandiant/my_binary

0x2ec304 Info 0x0 668 0 oahd: my_binary (Re(34180): 
Translating image /Users/Mandiant/my_binary -> 
/var/db/oah/237823680d6bdb1e9663d60cca5851b63e79f6c
8e884ebacc5f285253c3826b8/1c65adbef01f45a7a07379621
b5800fc337fc9db90d8eb08baf84e5c533191d9/my_binary.in_progress

0x2ec304 Info 0x0 668 0 oahd: my_binary (Re(34180): 
Translation finished for /Users/Mandiant/my_binary

0x2ec304 Info 0x0 668 0 oahd: my_binary(34180): 
Aot lookup request for /Users/Mandiant/my_binary

0x2ec304 Info 0x0 668 0 oahd: my_binary(34180): 
Using cached aot /Users/Mandiant/my_binary -> 
/var/db/oah/237823680d6bdb1e9663d60cca5851b63e
79f6c8e884ebacc5f285253c3826b8/1c65adbef01f45a7
a07379621b5800fc337fc9db90d8eb08baf84e5c533191d9/my_binary.aot
```

Figure 4: macOS Unified Logs showing Rosetta lookups, using cached files, and translating with private data enabled (with custom profile installed)

FSEvents
--------

FSEvents can be used to identify historical execution of x86-64 binaries even if Unified Logs or files in the Rosetta 2 Cache are not available or have been cleared. These records will show the creation of directories within the Rosetta 2 cache directory, the creation of `.in_progress` files, and then the renaming of the file to the AOT file, which will be named after the original binary.

```
private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/5660060629e3493074db75fba3
5cff449a366ea59b26af7d54c59779cdfac161 
FolderEvent; FolderCreated; 35102230

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/5660060629e3493074db75fba35
cff449a366ea59b26af7d54c59779cdfac161/
com.apple.systemsettings.cache.aot.in_progress 
FileEvent; Created;Renamed;Modified; 35102231

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/5660060629e3493074db75fba35
cff449a366ea59b26af7d54c59779cdfac161/com.apple.systemsettings.cache.aot 
FileEvent; Renamed; 35102231

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/31a32b61c112dae22363556599f
73f25fab17744eb0c51b8d0071c53bb878471 
FolderEvent; FolderCreated; 35102223

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/31a32b61c112dae22363556599
f73f25fab17744eb0c51b8d0071c53bb878471/sudo.aot.in_progress 
FileEvent; Created;Renamed;Modified; 35102224

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/31a32b61c112dae22363556599
f73f25fab17744eb0c51b8d0071c53bb878471/sudo.aot
FileEvent; Renamed; 35102224

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/9d8b46ee31e24e4988f923ac2e5
23171b7868f20b671cbaeb39d8db6199f4629 
FolderEvent; FolderCreated; 35102259

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/9d8b46ee31e24e4988f923ac2e5
23171b7868f20b671cbaeb39d8db6199f4629/id.aot.in_progress 
FileEvent; Created;Renamed;Modified; 35102260

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/9d8b46ee31e24e4988f923ac2e5
23171b7868f20b671cbaeb39d8db6199f4629/id.aot 
FileEvent; Renamed; 35102260
private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/fc9130581b8efed813de3826cfd
4bb34586b0872a0977efaa1d51f0861f564c9 
FolderEvent; FolderCreated; 35102355

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/fc9130581b8efed813de3826cfd
4bb34586b0872a0977efaa1d51f0861f564c9/chmod.aot.in_progress 
FileEvent; Created;Renamed;Modified; 35102356

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/fc9130581b8efed813de3826cfd
4bb34586b0872a0977efaa1d51f0861f564c9/chmod.aot 
FileEvent; Renamed; 35102356

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/c38ceae510d3b2a96bfa6f040fdf
7587121eb388f508c5d50f53efc40cf35dde 
FolderEvent; FolderCreated; 35102671

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/c38ceae510d3b2a96bfa6f040fdf
7587121eb388f508c5d50f53efc40cf35dde/cat.aot.in_progress 
FileEvent; Created;Renamed;Modified; 35102672

private/var/db/oah/433955637247d22a05957b32fa7f08e0b2f022
ed40311775d461444ce17beadb/c38ceae510d3b2a96bfa6f040fdf
7587121eb388f508c5d50f53efc40cf35dde/cat.aot 
FileEvent; Renamed; 35102672
```

Figure 5: Decoded FSEvents records showing the translation of a x86-64 POOLRAT variant on macOS, and subsequent universal system binaries executed by the malware as x86-64

AOT File Analysis
-----------------

The AOT files within the Rosetta 2 cache can provide valuable insight into historical evidence of execution of x86-64 binaries. In multiple cases over the past year, Mandiant identified macOS systems being the initial entry vector by APT groups targeting cryptocurrency organizations. In the majority of these cases, Mandiant identified evidence of the attackers deleting the malware on these systems within a few minutes of a cryptocurrency heist being perpetrated. However, the AOT files were left in place, likely due to the protection by SIP and the relative obscurity of this forensic artifact.

From a forensic perspective, the creation and modification timestamps on these AOT files provide evidence of the first time a specified binary was executed on the system with a unique combination of the attributes used to generate the SHA-256 hash. These timestamps can be corroborated with other artifacts related to binary execution where available (for example, Unified Logs or ExecPolicy, XProtect, and TCC Databases), and file system activity through FSEvents records, to build a more complete picture of infection and possible attacker activity if child processes were executed.

Where multiple AOT files exist for the same origin binary under different Binary UUID directories in the Rosetta 2 cache, and the content (file hashes) of those AOT files is the same, this is typically indicative of a change in file data sections, or more commonly, file system metadata only.

Mandiant has [previously shown](https://cloud.google.com/blog/topics/threat-intelligence/north-korea-supply-chain) that AOT files can be analyzed and used for malware identification through correlation of symbols. AOT files are Mach-O binaries that contain x86-64 instructions that have been translated from the original ARM64 code. They contain jump-backs into the original binary and contain no API calls to reference. Certain functionality can be determined through reverse engineering of AOT files; however, no static data, including network-based indicators or configuration data, are typically recoverable. In one macOS downloader observed in a notable DPRK cryptocurrency heist, Mandiant observed developer file path strings as part of the basic Mach-O information contained within the AOT file. The original binary was not recovered due to the attacker deleting it after the heist, so this provided useful data points to support threat actor attribution and malware family assessment.

```
/Users/crown/Library/Developer/Xcode/DerivedData/
DownAndMemload-becawjfobisdcocirecqedzcixcf/Build/Intermediates.noindex/
DownAndMemload.build/Release/DownAndMemload.build/
Objects-normal/x86_64/main.o
/Users/crown/Library/Developer/Xcode/DerivedData/
DownAndMemload-becawjfobisdcocirecqedzcixcf/Build/Intermediates.noindex/
DownAndMemload.build/Release/DownAndMemload.build/Objects-normal/
x86_64/queue.o
/Volumes/Data/Development/DownAndMemload/DownAndMemload/
_g_szServerUrl
_szgetpwuid
_szsleep
```

Figure 6: Interesting strings from an AOT file related to a malicious DPRK downloader that was unrecoverable

In any case, determining malware functionality is more effective using the original complete binary instead of the AOT file, because the AOT file lacks much of the contextual information present in the original binary. This includes static data and complete Mach-O headers.

Poisoning AOT Files
-------------------

Much has been written within the industry about the potential for the poisoning of the Rosetta 2 cache through modification or introduction of AOT files. Where SIP is disabled, this is a valid attack vector. Mandiant has not yet seen this technique in the wild; however, during hunting or investigation activities, it is advisable to be on the lookout for evidence of AOT poisoning. The best way to do this is by comparing the contents of the ARM64 AOT files with what would be expected based on the original x86-64 executable. This can be achieved by taking the original x86-64 executable and using it to generate a known-good AOT file, then comparing this to the AOT file in the cache. Discrepancies, particularly the presence of injected shellcode, could indicate AOT poisoning.

Conclusion
----------

There are several forensic artifacts on macOS that may record historical evidence of binary execution. However, in cases of advanced intrusions with forensically aware attackers, original binaries being deleted, and no further security monitoring solutions, combining FSEvents, Unified Logs, and, crucially, residual AOT files on disk has provided the residual evidence of intrusion on a macOS system.

Whilst signed macOS ARM64 binaries may be the future, for now AOT files and the artifacts surrounding them should be reviewed in analysis of any suspected macOS intrusion and leveraged for hunting opportunities wherever possible.

The behavior identified in the cases presented here was identified on various versions of macOS between 13.5 and 14.7.2. Future or previous versions of macOS and Rosetta 2 may behave differently.

Acknowledgements
----------------

Special thanks to Matt Holley, Mohamed El-Banna, Robert Wallace, and Adrian Hernandez.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/rosetta2-artifacts-macos-intrusions/)

