---
title: "Malvertising campaign leads to info stealers hosted on GitHub"
date: 2025-03-19
---

In early December 2024, Microsoft Threat Intelligence detected a large-scale malvertising campaign that impacted nearly one million devices globally in an opportunistic attack to steal information. The attack originated from illegal streaming websites embedded with malvertising redirectors, leading to an intermediary website where the user was then redirected to GitHub and two other platforms. The campaign impacted a wide range of organizations and industries, including both consumer and enterprise devices, highlighting the indiscriminate nature of the attack.

Learn more about this malvertising campaign's multi-stage attack chain

Listen to the Microsoft Threat Intelligence podcast

GitHub was the primary platform used in the delivery of the initial access payloads and is referenced throughout this blog post; however, Microsoft Threat Intelligence also observed one payload hosted on Discord and another hosted on Dropbox.

The GitHub repositories, which were taken down, stored malware used to deploy additional malicious files and scripts. Once the initial malware from GitHub gained a foothold on the device, the additional files deployed had a modular and multi-stage approach to payload delivery, execution, and persistence. The files were used to collect system information and to set up further malware and scripts to exfiltrate documents and data from the compromised host. This activity is tracked under the umbrella name Storm-0408 that we use to track numerous threat actors associated with remote access or information-stealing malware and who use phishing, search engine optimization (SEO), or malvertising campaigns to distribute malicious payloads.

In this blog, we provide our analysis of this large-scale malvertising campaign, detailing our findings regarding the redirection chain and various payloads used across the multi-stage attack chain. We further provide recommendations for mitigating the impact of this threat, detection details, indicators of compromise (IOCs), and hunting guidance to locate related activity. By sharing this research, we aim to raise awareness about the tactics, techniques, and procedures (TTPs) used in this widespread activity so organizations can better prepare and implement effective mitigation strategies to protect their systems and data.

We would like to thank the GitHub security team for their prompt response and collaboration in taking down the malicious repositories.

## GitHub activity and redirection chain

Since at least early December 2024, multiple hosts downloaded first-stage payloads from malicious GitHub repositories. The users were redirected to GitHub through a series of other redirections. Analysis of the redirector chain determined the attack likely originated from illegal streaming websites where users can watch pirated videos. The streaming websites embedded malvertising redirectors within movie frames to generate pay-per-view or pay-per-click revenue from malvertising platforms. These redirectors subsequently routed traffic through one or two additional malicious redirectors, ultimately leading to another website, such as a malware or tech support scam website, which then redirected to GitHub.

Multiple stages of malware were deployed in this campaign, as listed below, and the several different stages of activity that occurred depended on the payload dropped during the second stage.

- The first-stage payload that was hosted on GitHub served as the dropper for the next stage of payloads.

- The second-stage files were used to conduct system discovery and to exfiltrate system information that was Base64-encoded into the URL and sent over HTTP to an IP address. The information collected included data on memory size, graphic details, screen resolution, operating system (OS), and user paths.

- Various third-stage payloads were deployed depending on the second-stage payload. In general, the third-stage payload conducted additional malicious activities such as command and control (C2) to download additional files and to exfiltrate data, as well as defense evasion techniques.

The full redirect chain was composed of four to five layers. Microsoft researchers determined malvertising redirectors were contained within an iframe on illegal streaming websites.

<figure>

![A screenshot of code from a streaming video website and iframe showing the malvertising redirector URL](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-2.webp)

<figcaption>

_Figure 1. Code from website of streaming video and iframe showing malvertising redirector URL_

</figcaption>

</figure>

There were several redirections that occurred before arriving at the malicious content stored on GitHub.

<figure>

![A diagram of the redirection chain first depicting the illegal streaming website with iframe followed by the malicious redirector and counter, which redirects to the malvertising distributor, which finally lands on the malicious content hosted on GitHub.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image.jpg)

<figcaption>

_Figure 2. Redirection chain from pirate streaming website to malware files on GitHub_

</figcaption>

</figure>

## Attack chain

Once the redirection to GitHub occurred, the malware hosted on GitHub established the initial foothold on the user’s device and functioned as a dropper for additional payload stages and running malicious code. The additional payloads included information stealers to collect system and browser information on the compromised device, of which most were either Lumma stealer or an updated version of Doenerium. Depending on the initial payload, the deployment of NetSupport, a remote monitoring and management (RMM) software, was also often deployed alongside the infostealer. Besides the information stealers, PowerShell, JavaScript, VBScript, and AutoIT scripts were run on the host. The threat actors incorporated use of living-off-the-land binaries and scripts (LOLBAS) like _PowerShell.exe_, _MSBuild.exe__,_ and _RegAsm.exe_ for C2 and data exfiltration of user data and browser credentials.

After the initial foothold was gained, the activity led to a modular and multi-stage approach to payload delivery, execution, and persistence. Each stage dropped another payload with a different function, as outlined below. Actions conducted across these stages include system discovery (memory, GPU, OS, signed-in users, and others), opening browser credential files, Data Protection API (DPAPI) crypt data calls, and other functions such as obfuscated script execution and named pipe creations to conduct data exfiltration. Persistence was achieved through modification of the registry run keys and the addition of a shortcut file to the Windows _Startup_ folder.

Several stages of malicious activity to conduct deployment of additional malware, collections, and exfiltration of data to a C2 were observed. While not every single initial payload followed these exact steps, this is an overall view of what occurred across most incidents analyzed:

<figure>

![A diagram generally displaying the four stages. The first stage involves the malvertising website redirecting users to GitHub pages, leading to a payload downloading from the repo. In the second stage, the payload performs system discovery and exfiltrates collected system information and stage-two payloads drop additional payloads. In the third stage, if the payload is a PowerShell script, it downloads NetSupport RAT from C2, sets persistence, and it may deliver a Lumma Stealer payload using MSBuild.exe for exfiltration. If the third stage payload is an .exe, it creates and runs a .cmd file and drops renamed AutoIT interpreter with a .com file extension, leading to the fourth stage. In the final stage, AutoIT launches binary and may drop an AutoIT interpreter with .scr file extensions, where a JavaScript file is dropped for running and persistence of those files. Finally, the AutoIT payload uses RegAsm.exe or PowerShell.exe to open files, enable browser remote debugging, and exfiltrate data. PowerShell may be deployed to set exclusion paths for Defender and/or drop NetSupport.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/General-depiction-of-the-four-stages-diagram.webp)

<figcaption>

_Figure 3. General depiction of the four stages_

</figcaption>

</figure>

### First-stage payload: Establishing a foothold on the host

During the first stage, a payload is dropped onto the user’s device from the binary hosted on GitHub, establishing a foothold on that device. As of mid-January 2025, the first-stage payloads discovered were digitally signed with a newly created certificate. A total of twelve different certificates were identified, all of which have been revoked.

Most of these initial payloads dropped the following legitimate files to leverage their functionality. These files were either leveraged by the first-stage payload or by later-stage payloads, depending on the actions being conducted.

| **File name** | **Function** |
| --- | --- |
| _app-64.7z_ | This is a compressed archive that stores the second-stage payload and additional dropped files. |
| _app.asar_ | This is an archive file specific to Electron applications, which are directly installed programs. |
| _d3dcompiler\_47.dll_ | This file is often included in DirectX redistributables, which are commonly bundled with Microsoft installers for games and graphics applications. |
| _elevate.exe_ | This file is used by various installers and scripts to run processes with elevated privileges, not specific to Microsoft. |
| _ffmpeg.dll_ | This file is associated with FFmpeg, a popular multimedia framework used to handle video, audio, and other multimedia files and streams. |
| _libEGL.dll_ | This file is part of the ANGLE project, which is often found in applications that use OpenGL Embedded Systems (ES), including some web browsers and games. |
| _libEGLESv2.dll_ | This file is part of the ANGLE project, which is often found in applications that use OpenGL ES, including some web browsers and games. |
| _LICENSES.chromium.html_ | This file could contain information about the system or browser. |
| _nsis7z.dll_ | This file is associated with the plugins for the Nullsoft Scriptable Install System (NSIS), which is used to create installers for various software. |
| _StdUtils.dll_ | This file is associated with the plugins for the NSIS. |
| _System.dll_ | This file is part of the .NET Framework assembly, typically included in Microsoft installers for applications that rely on the .NET Framework. |
| _vk\_swiftshader.dll_ | This file is associated with SwiftShader, which is used in applications that need a CPU-based implementation of the Vulkan API. |
| _vulkan-1.dll_ | This file is associated with applications that use the Vulkan Graphics API, such as games and graphics software. |

Depending on the first-stage payload that was initially established on the compromised device, Microsoft observed different second-stage payloads and several different methods for delivering these payloads to the device.

### Second-stage payload: System discovery, collection, and exfiltration

The main purpose of the second-stage payload is to conduct system discovery and collect that data for exfiltration to the C2. The system information collected includes data such as memory size, graphic card details, screen resolution, operating system, user paths, and a reference to the second-stage payload’s file name.

This was accomplished by querying the registry key _HKEY\_LOCAL\_MACHINESOFTWAREMicrosoftWindows NTCurrentVersionProductName_ for the Windows OS version and running commands, such as the _echo_ command, to gather the device’s name (_%COMPUTERNAME%)_ and domain name (_%USERDOMAIN%)._

System data collected by the second-stage payload is Base64-encoded and exfiltrated as a query parameter to an IP address.

<figure>

![Screenshot of code depicting the typical format of the URL observed when exfiltrating information collected from the compromised device. ](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image.webp)

<figcaption>

_Figure 4. Typical format of the URL observed when exfiltrating information collected from the compromised device_

</figcaption>

</figure>

### Third-stage payload: PowerShell and _.exe_ binary

Depending on the second-stage payload, either one or multiple executables are dropped onto the compromised device, and sometimes an accompanying encoded PowerShell script. These files initiate a chain of events that conduct command execution, payload delivery, defensive evasion, persistence, C2 communications, and data exfiltration. The analysis of the dropped executables is first discussed below, followed by review of the PowerShell scripts observed.

#### Third-stage .exe analysis

The second-stage payloads run the dropped third-stage executables using the command prompt (for example, _cmd.exe  /d /s /c “”C:Users<user>AppDataLocalTempApproachAllan.exe””_). The _/c_ flag ensures that the command runs and exits quickly. When the third-stage _.exe_ runs, it drops a command file (._cmd_) and launches it using the command prompt (for example, “_cmd.exe” /c copy Beauty Beauty.cmd && Beauty.cmd_). The _.cmd_ file performs several actions, such as running _tasklist_, to initiate the discovery of running programs. This is followed by the _findstr_ to search for keywords associated with security software:

| **_findstr_ keyword** | **Associated software** |
| --- | --- |
| wrsa | Webroot SecureAnywhere |
| opssvc | Quick Heal |
| AvastUI | Avast Antivirus |
| AVGUI | AVG Antivirus |
| bdservicehost | Bitdefender Antivirus |
| nsWscSvc | Norton Security |
| ekrn | ESET |
| SophosHealth | Sophos |

The _.cmd_ file also concatenates multiple files into one with a single character file name: _“cmd /c copy /b ..Verzeichnis + ..Controlling + ..Constitute + ..Enjoyed + ..Confusion + ..Min +..Statutory J”_. This single character filename is used next.

Following this, the third-stage _.exe_ produces an AutoIT v3 interpreter file that is renamed from the typical file name of _AutoIt3.exe_ and uses a _.com_ file extension. The _.cmd_ file initiates the execution of the _.com_ file against the single character binary (such as _Briefly.com J_). Note, most of the second-stage payloads follow this progression chain, and as mentioned a second-stage payload can also drop multiple executables, all following the same process. For example:

**First stage**

- _X-essentiApp.exe_

**Second stage**

- _Ionixnignx.exe_

**Third stage**

- _EverybodyViewing.exe_

- _ReliefOrganizational.exe_

- _InflationWinston.exe_

**Third-stage command files**

- _Beauty.cmd_

- _Possess.cmd_

- _Villa.cmd_

**Fourth-stage AutoIT _.com_ files**

- _Alexandria.com_

- _Kills.com_

- _Briefly.com_

We observed multiple _.com_ files originating from different dropped executables, each performing distinct functions while occasionally overlapping in behavior. These files facilitate persistence, process injection, remote debugging, and data exfiltration through various mechanisms. One _.com_ file, such as _Alexandria.com_, drops a _.scr_ file (another renamed AutoIT interpreter), and a _.js_ (JavaScript) file with the same name as the _.scr_ file. The purpose of the JavaScript file is to ensure persistence by creating a _.url_ internet shortcut that points to the JavaScript file and is placed in the _Startup_ folder, ensuring that the _.scr_ file executes when the _.js_ file executes (through _Wscript.exe_) upon user sign-in. Alternatively, persistence can be achieved using scheduled task creation. The _.scr_ file can initiate C2 connections, enable remote debugging on Chrome or Edge within a hidden desktop session, or create TCP listening sockets on ports 9220-9229. This functionality allows threat actors to monitor browsing activity and interact with an active browser instance. These files can also open sensitive data files, indicating their role in facilitating post-exploitation activities.

Another _.__com_ file, such as _affiliated.com_, also focuses on remote debugging and browser monitoring. In addition to remote monitoring, _affiliated.com_ initiates network connections to Telegram, Let’s Encrypt, and threat actor domains, potentially for C2 or exfiltration. It also accesses DPAPI to decrypt sensitive stored credentials and retrieve browser data.

The final observed _.__com_ file, such as _Briefly.com_, exhibits behavior similar to _affiliated.com_ but extends its capabilities to include screenshot capture, data exfiltration, and PowerShell-based execution. This file accesses browser and user data for collection, establishes connections to Pastebin and additional C2 domains, and drops the fourth-stage PowerShell script.

The order in which these _.__com_ files run is not strictly defined, as one or multiple files can perform overlapping functions depending on the third-stage payload. In many cases, the _.__com_ files also leverage LOLBAS like _RegAsm.exe_ by dropping a legitimate file into the _%TEMP%_ directory or injecting malicious code into it using _NtAllocateVirtualMemory_ and _SetThreadContext_ API function calls. _RegAsm.exe_ is used to establish C2 connections over TCP ports 15647 or 9000, exfiltrating data, accessing DPAPI for decryption, monitoring keystrokes using the _WH\_KEYBOARD\_LL_ hook, and more. This flexibility in execution allows threat actors to tailor their approach based on environmental factors, such as security configurations and user activity.

Browser data files seen accessed:

- _AppDataRoamingMozillaFirefoxProfiles<user profile uid>.default-releasecookies.sqlite_

- _AppDataRoamingMozillaFirefoxProfiles<user profile uid>.default-releaseformhistory.sqlite_

- _AppDataRoamingMozillaFirefoxProfiles<user profile uid>.default-releasekey4.db_

- _AppDataRoamingMozillaFirefoxProfiles<user profile uid>.default-releaselogins.json_

- _AppDataLocalGoogleChromeUser DataDefaultWeb Data_

- _AppDataLocalGoogleChromeUser DataDefaultLogin Data_

- _AppDataLocalMicrosoftEdgeUser DataDefaultLogin Data_

User data file paths seen accessed:

- _C:\\Users<user>\\OneDrive_

- _C:\\Users<user>\\Documents_

- _C:\\Users<user>\\Downloads_

#### Third-stage PowerShell analysis

If a PowerShell script is also dropped by the second-stage payload, it includes Base64-obfuscated commands to conduct actions, such as use _curl_ to download additional files like NetSupport from the C2, create persistence for the NetSupport RAT, and exfiltrate system information to C2 servers. To ensure no errors or the progress meter is displayed on the compromised device, the _curl_ command is often used with the _–silent_ option when downloading files from the C2. PowerShell is often configured to run without restrictions with the _\-ExecutionPolicy Bypass_ parameter.

As an example, in some of the incidents, when the second-stage payload runs, a PowerShell script is dropped and executed. The script sends the compromised device’s name to the C2 and downloads NetSupport RAT from the same C2.

- Second-stage payload: _Squarel.exe_

- PowerShell script: SHA-256: d70ccae7914fc8c36c9e11b2a7f10bebd7f5696e78d8836554f4990b0f688dbb

- C2 domain: _keikochio\[.\]com_

- NetSupport RAT: SHA-256: 32a828e2060e92b799829a12e3e87730e9a88ecfa65a4fc4700bdcc57a52d995

In another case, a second-stage payload drops a PowerShell script, which connects to _hxxps://ipinfo\[.\]io_ to gather the compromised device’s external-facing IP address. This information is sent to a Telegram chat, then drops _presentationhost.exe_ (a renamed NetSupport binary) and _remcmdstub.exe_ (NetSupport Command Manager) into the _%TEMP%_ directory. Finally, the PowerShell script establishes persistence for _presentationhost.exe_ by adding it to the auto-start extensibility points (ASEP) registry keys. When it runs, the NetSupport RAT connects to the C2 and captures a screenshot of the compromised device’s desktop. It also delivers a Lumma executable that drops a VBScript file with the same name. The VBScript file runs encoded PowerShell to initiate C2 connections and launches _MSBuild.exe_ to enable Chrome remote debugging on a hidden desktop. Additionally, _presentationhost.exe_ initiates _remcmdstub.exe_, which leverages _iScrPaint.exe_ (iTop Screen Recorder) to run _MSBuild.exe_ and access browser credential files for exfiltration. The _iScrPaint.exe_ file also establishes persistence by placing a ._lnk_ shortcut in the Windows _Startup_ folder, ensuring it runs on system reboot.

- Second-stage payload: _Application.exe_

- PowerShell script: SHA-256: 483796a64f004a684a7bc20c1ddd5c671b41a808bc77634112e1703052666a64

- C2: _hxxp:_//_5__.10.250\[.\]240/fakeurl.htm_

The last observed third-stage PowerShell script was dropped by three second-stage payloads. The script sends the compromised device’s name to the C2 server. It then changes the working directory to _$env:APPDATA_, before using _Start-BitsTransfer_ to download NetSupport from the C2. To evade detection, it modifies system security settings forcing TLS1.2 for encrypted C2 communication. These files are extracted into a newly created _WinLibraryClient_ directory under _AppData_ and then are launched. The script establishes persistence for the _client32.exe (_NetSupport RAT_)_ by modifying the ASEP registry. _Client32.exe_ initiates C2 connections to _hxxp://79.132.128\[.\]77/fakeurl.htm_.

- Second-stage payloads: _SalmonSamurai.exe_, _LakerBaker.exe_, and _DisplayPhotoViewer.exe_

- PowerShell script: SHA-256: 670218cfc5c16d06762b6bc74cda4902087d812e72c52d6b9077c4c4164856b6

- C2 domain: _stocktemplates\[.\]net_

Additionally, one observed execution included registry enumeration of _HKCU:SoftwareMicrosoftWindowsCurrentVersionUninstall_ to identify installed applications and security software. It also queries the system’s domain status using Windows Management Instrumentation (WMI) and scans for cryptocurrency wallets, including Ledger Live, Trezor Suite, KeepKey, BCVault, OneKey, and BitBox, indicating potential financial data theft.

### Fourth-stage PowerShell analysis

Depending on the _.com_ file that ran (like _Briefly.com_), the renamed AutoIT file may drop a PowerShell script (SHA-256: 2a29c9904d1860ea3177da7553c8b1bf1944566e5bc1e71340d9e0ff079f0bd3). The obfuscated PowerShell code uses the _Add-MpPreference_ cmdlet to modify Microsoft Defender to add in exclusion paths for Microsoft Defender, so the specified folders are not scanned.

<figure>

![Screenshot of code depicting the deobfuscated commands to add exclusion paths to Windows Defender.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-1.webp)

<figcaption>

_Figure 5. Deobfuscated commands to add exclusion paths to Windows Defender_

</figcaption>

</figure>

The script above is sometimes followed by an instance of Base64-encoded PowerShell commands. The PowerShell commands perform the following actions:

- Sends a web request to _hxxps://360\[.\]net_ and closes the response.

- Sends a web request to _hxxps://baidu\[.\]com_ and closes the response.

- Downloads data from _hxxps://klipcatepiu0\[.\]shop/int\_clp\_sha.txt_ using a web client.

- Writes the downloaded data to a memory stream and saves it as a _.zip_ file named _null.zip_ (SHA-256: f07b8e5622598c228bfc9bff50838a3c4fffd88c436a7ef77e6214a40b0a2bae) in the _C:Users<Username>AppDataLocalTemp_ directory.

## Recommendations

Microsoft recommends the following mitigations to reduce the impact of this threat.

### Strengthen Microsoft Defender for Endpoint configuration

- Ensure that tamper protection is enabled in Microsoft Defender for Endpoint. 

- Enable network protection in Microsoft Defender for Endpoint. 

- Turn on web protection.

- Run endpoint detection and response (EDR) in block mode so that Microsoft Defender for Endpoint can block malicious artifacts, even when your non-Microsoft antivirus does not detect the threat or when Microsoft Defender Antivirus is running in passive mode. EDR in block mode works behind the scenes to remediate malicious artifacts that are detected post-breach.     

- Configure investigation and remediation in full automated mode to let Microsoft Defender for Endpoint take immediate action on alerts to resolve breaches, significantly reducing alert volume.  

- Microsoft Defender XDR customers can turn on the following attack surface reduction rules to prevent common attack techniques used by threat actors. 
    
    - Block executable files from running unless they meet a prevalence, age, or trusted list criterion 
    
    - Block execution of potentially obfuscated scripts
    
    - Block JavaScript or VBScript from launching downloaded executable content
    
    - Block process creations originating from PSExec and WMI commands
    
    - Block credential stealing from the Windows local security authority subsystem 
    
    - Block use of copied or impersonated system tools

### Strengthen operating environment configuration

- Require multifactor authentication (MFA). While certain attacks such as adversary-in-the-middle (AiTM) phishing attempt to circumvent MFA, implementation of MFA remains an essential pillar in identity security and is highly effective at stopping a variety of threats.
    - Leverage phishing-resistant authentication methods such as FIDO Tokens, or Microsoft Authenticator with passkey. Avoid telephony-based MFA methods to avoid risks associated with SIM-jacking.

- Implement Entra ID Conditional Access authentication strength to require phishing-resistant authentication for employees and external users for critical apps.

- Encourage users to use Microsoft Edge and other web browsers that support Microsoft Defender SmartScreen, which identifies and blocks malicious websites, including phishing sites, scam sites, and sites that host malware.

- Enable Network Level Authentication for Remote Desktop Service connections.

- Enable Local Security Authority (LSA) protection to block credential stealing from the Windows local security authority subsystem. 

- AppLocker can restrict specific software tools prohibited within the organization, such as reconnaissance, fingerprinting, and RMM tools, or grant access to only specific users.

## Microsoft Defender XDR detections

Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, apps to provide integrated protection against attacks like the threat discussed in this blog.

Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.

### Microsoft Defender Antivirus

Microsoft Defender Antivirus detects threat components as the following malware:

- Trojan:Win64/LummaStealer

- Trojan:Win32/Malgent

- Behavior:Win32/Eldorado

- Behavior:Win32/LuammaStealer

- Trojan:PowerShell/Powdow

- Trojan:Win64/Shaolaod

- Behavior:Win64/Shaolaod

### Microsoft Defender for Endpoint

The following alerts might indicate threat activity associated with this threat. These alerts, however, can be triggered by unrelated threat activity.

- Possible theft of passwords and other sensitive web browser information

- Possible Lumma Stealer activity

- Renamed AutoIt tool

- Use of living-off-the-land binary to run malicious code

- Suspicious startup item creation

- Suspicious Scheduled Task Process Launched

- Suspicious DPAPI Activity

- Suspicious implant process from a known emerging threat

- Security software tampering

- Suspicious activity linked to a financially motivated threat actor detected

- Ransomware-linked threat actor detected

- A file or network connection related to a ransomware-linked emerging threat activity group detected

- Information stealing malware activity

- Possible NetSupport Manager activity

- Suspicious sequence of exploration activities

- Defender detection bypass

- Suspicious Location of Remote Management Software

- A process was injected with potentially malicious code

- Process hollowing detected

- Suspicious PowerShell download or encoded command execution

- Suspicious PowerShell command line

- Suspicious behavior by cmd.exe was observed

- Suspicious Security Software Discovery

- Suspicious discovery indicative of Virtualization/Sandbox Evasion

- A process was launched on a hidden desktop

- Monitored keystrokes

- Suspicious Process Discovery

- Suspicious Javascript process

- A suspicious file was observed

- Anomaly detected in ASEP registry

### Microsoft Defender for Cloud

The following alerts might indicate threat activity associated with this threat. These alerts, however, can be triggered by unrelated threat activity.

- Detected suspicious combination of HTA and PowerShell

- Suspicious PowerShell Activity Detected

- Traffic detected from IP addresses recommended for blocking

- Attempted communication with suspicious sinkholed domain

- Communication with suspicious domain identified by threat intelligence

- Detected obfuscated command line

- Detected suspicious named pipe communications

## Microsoft Security Copilot

Security Copilot customers can use the standalone experience to create their own prompts or run the following pre-built promptbooks to automate incident response or investigation tasks related to this threat:

- Incident investigation

- Microsoft User analysis

- Threat actor profile

- Threat Intelligence 360 report based on MDTI article

- Vulnerability impact assessment

Note that some promptbooks require access to plugins for Microsoft products such as Microsoft Defender XDR or Microsoft Sentinel.

## Threat intelligence reports

Microsoft customers can use the following reports in Microsoft products to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.

### Microsoft Defender Threat Intelligence

- Storm-0408

- Agent Tesla credential theft malware

- Information stealers

- Lumma stealer

- Abuse of remote monitoring and management tools

- Malicious use of PowerShell

Microsoft Security Copilot customers can also use the Microsoft Security Copilot integration in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the embedded experience in the Microsoft Defender portal to get more information about this threat actor.

## Hunting queries

### Microsoft Defender XDR

Microsoft Defender XDR customers can run the following query to find related activity in their networks:

**Github-hosted first-stage payload certificate serial numbers**

```
let specificSerialNumbers = dynamic(["70093af339876742820d7941", "15042512e67e8275f3f7f36b", "5608cab7e2ce34d53abcbb73",
 "0fa27d2553f24da79d1cc6bd8773ee9a", "7a7bf2ae0cbc0f5500db2946", "30d6c83a715bddb32e7956fe52d6b352",
  "301385aa36fae635e74bb88e", "30013cbbb16a7fd3c57f82707fb99c32", "5d00264a6b804ae6b28d9b16",
   "3a9c76f8304f77bd271921d9982f1ab6", "01f2c6c363767056abd80e9c", "0b09c88c0c8d15bed51a9eb4440f4bb0"]); 
union
(
    DeviceFileCertificateInfo
    | where CertificateSerialNumber in (specificSerialNumbers)
    | project DeviceName, CertificateSerialNumber, Signer, SHA1, IsSigned, Issuer, Timestamp
),
(
    DeviceTvmCertificateInfo
    | where SerialNumber in (specificSerialNumbers)
    | project DeviceId, SerialNumber, SignatureAlgorithm, Thumbprint, Path, IssueDate, ExpirationDate
)

```

**Dropbox-hosted first-stage payload certificate serial number**

Surface devices that may contain first-stage payloads hosted on Dropbox related to this activity. This query will search for the unique serial number of the known certificate related to this activity.

```
let specificSerialNumbers = dynamic(["7a7bf2ae0cbc0f5500db2946"]); 
union
(
    DeviceFileCertificateInfo
    | where CertificateSerialNumber in (specificSerialNumbers)
    | project DeviceName, CertificateSerialNumber, Signer, SHA1, IsSigned, Issuer, Timestamp
),
(
    DeviceTvmCertificateInfo
    | where SerialNumber in (specificSerialNumbers)
    | project DeviceId, SerialNumber, SignatureAlgorithm, Thumbprint, Path, IssueDate, ExpirationDate
)

```

**Second-stage C2 IP addresses**

Surface devices that may have communicated with second stage C2 IP addresses related to this activity.

```
let ipAddressToSearch = dynamic(["159.100.18.192", "192.142.10.246", "79.133.46.35", "84.200.24.191", "84.200.24.26", "89.187.28.253", "185.92.181.1"]);
union isfuzzy=true
(
    AzureDiagnostics
    | where identity_claim_ipaddr_s == ipAddressToSearch or conditions_sourceIP_s == ipAddressToSearch or CallerIPAddress == ipAddressToSearch or clientIP_s == ipAddressToSearch or clientIp_s == ipAddressToSearch or primaryIPv4Address_s == ipAddressToSearch or conditions_destinationIP_s == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AzureDiagnostics", IPAddress = coalesce(identity_claim_ipaddr_s, conditions_sourceIP_s, CallerIPAddress, clientIP_s, clientIp_s, primaryIPv4Address_s, conditions_destinationIP_s), AdditionalInfo = tostring(AdditionalFields)
),
(
    IdentityQueryEvents
    | where IPAddress == ipAddressToSearch or DestinationIPAddress == ipAddressToSearch
    | project Timestamp, Table = "IdentityQueryEvents", IPAddress = coalesce(IPAddress, DestinationIPAddress), AdditionalInfo = Query
),
(
    AADSignInEventsBeta
    | where IPAddress == ipAddressToSearch
    | project Timestamp, Table = "AADSignInEventsBeta", IPAddress, AdditionalInfo = UserAgent
),
(
    Heartbeat
    | where ComputerIP == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "Heartbeat", IPAddress = ComputerIP, AdditionalInfo = OSName
),
(
    CloudAppEvents
    | where IPAddress == ipAddressToSearch
    | project Timestamp, Table = "CloudAppEvents", IPAddress, AdditionalInfo = UserAgent
),
(
    DeviceNetworkEvents
    | where LocalIP == ipAddressToSearch or RemoteIP == ipAddressToSearch
    | project Timestamp, Table = "DeviceNetworkEvents", IPAddress = coalesce(LocalIP, RemoteIP), AdditionalInfo = InitiatingProcessCommandLine
),
(
    AADUserRiskEvents
    | where IpAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AADUserRiskEvents", IPAddress = IpAddress, AdditionalInfo = RiskEventType
),
(
    AADNonInteractiveUserSignInLogs
    | where IPAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AADNonInteractiveUserSignInLogs", IPAddress, AdditionalInfo = UserAgent
),
(
    MicrosoftAzureBastionAuditLogs
    | where TargetVMIPAddress == ipAddressToSearch or ClientIpAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "MicrosoftAzureBastionAuditLogs", IPAddress = coalesce(TargetVMIPAddress, ClientIpAddress), AdditionalInfo = UserAgent
)
| sort by Timestamp desc

```

**Fourth-stage C2 IP addresses**

Surface devices that may have communicated with fourth stage C2 IP addresses related to this activity.

```
let ipAddressToSearch = dynamic(["45.141.84.60", "91.202.233.18", "154.216.20.131", "5.10.250.240", "79.132.128.77"]);
union isfuzzy=true
(
    AzureDiagnostics
    | where identity_claim_ipaddr_s == ipAddressToSearch or conditions_sourceIP_s == ipAddressToSearch or CallerIPAddress == ipAddressToSearch or clientIP_s == ipAddressToSearch or clientIp_s == ipAddressToSearch or primaryIPv4Address_s == ipAddressToSearch or conditions_destinationIP_s == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AzureDiagnostics", IPAddress = coalesce(identity_claim_ipaddr_s, conditions_sourceIP_s, CallerIPAddress, clientIP_s, clientIp_s, primaryIPv4Address_s, o),
(
    IdentityQueryEvents
    | where IPAddress == ipAddressToSearch or DestinationIPAddress == ipAddressToSearch
    | project Timestamp, Table = "IdentityQueryEvents", IPAddress = coalesce(IPAddress, DestinationIPAddress), AdditionalInfo = Query
),
(
    AADSignInEventsBeta
    | where IPAddress == ipAddressToSearch
    | project Timestamp, Table = "AADSignInEventsBeta", IPAddress, AdditionalInfo = UserAgent
),
(
    Heartbeat
    | where ComputerIP == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "Heartbeat", IPAddress = ComputerIP, AdditionalInfo = OSName
),
(
    CloudAppEvents
    | where IPAddress == ipAddressToSearch
    | project Timestamp, Table = "CloudAppEvents", IPAddress, AdditionalInfo = UserAgent
),
(
    DeviceNetworkEvents
    | where LocalIP == ipAddressToSearch or RemoteIP == ipAddressToSearch
    | project Timestamp, Table = "DeviceNetworkEvents", IPAddress = coalesce(LocalIP, RemoteIP), AdditionalInfo = InitiatingProcessCommandLine
),
(
    AADUserRiskEvents
    | where IpAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AADUserRiskEvents", IPAddress = IpAddress, AdditionalInfo = RiskEventType
),
(
    AADNonInteractiveUserSignInLogs
    | where IPAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "AADNonInteractiveUserSignInLogs", IPAddress, AdditionalInfo = UserAgent
),
(
    MicrosoftAzureBastionAuditLogs
    | where TargetVMIPAddress == ipAddressToSearch or ClientIpAddress == ipAddressToSearch
    | project Timestamp = TimeGenerated, Table = "MicrosoftAzureBastionAuditLogs", IPAddress = coalesce(TargetVMIPAddress, ClientIpAddress), AdditionalInfo = UserAgent
)
| sort by Timestamp desc

```

**Browser remote debugging** 

Identify AutoIT scripts launching chromium-based browsers (such as _chrome.exe_, _msedge.exe_, _brave.exe_) in remote debugging mode.

```
DeviceProcessEvents 
| where InitiatingProcessVersionInfoInternalFileName == "AutoIt3.exe" // Check for "AutoIt" scripts, even if it's renamed.  
| where ProcessCommandLine has "--remote-debugging-port" // Identify Chromium based browsers (chrome.exe, msedge.exe, brave.exe etc) being launched in remote debugging mode. 
| project DeviceId, Timestamp, InitiatingProcessParentFileName, InitiatingProcessFileName, InitiatingProcessFolderPath, InitiatingProcessVersionInfoInternalFileName, InitiatingProcessCommandLine, FileName, ProcessCommandLine

```

**DPAPI decryption via AutoIT**

Identify DPAPI decryption activity originating from AutoIT scripts.

```
DeviceEvents
| where ActionType == "DpapiAccessed"
| where InitiatingProcessVersionInfoInternalFileName == "AutoIt3.exe"
| where (AdditionalFields has_any("Google Chrome", "Microsoft Edge") and AdditionalFields has_any("SPCryptUnprotect"))
| extend json = parse_json(AdditionalFields)
| extend dataDesp = tostring(json.DataDescription.PropertyValue)
| extend opType = tostring(json.OperationType.PropertyValue)
| where (dataDesp in~ ("Google Chrome", "Microsoft Edge") and opType =~ "SPCryptUnprotect")
| project Timestamp, ReportId, DeviceId, ActionType, InitiatingProcessParentFileName, InitiatingProcessFileName, InitiatingProcessVersionInfoInternalFileName, InitiatingProcessCommandLine, AdditionalFields, dataDesp, opType

```

**DPAPI decryption via LOLBAS binaries**

Identify DPAPI decryption activity originating from LOLBAS binaries (_RegAsm.exe_ and _MSBuild.exe_).

```
DeviceEvents
| where ActionType == "DpapiAccessed"
| where InitiatingProcessFileName has_any ("RegAsm.exe", "MSBuild.exe")
| where (AdditionalFields has_any("Google Chrome", "Microsoft Edge") and  AdditionalFields has_any("SPCryptUnprotect"))
| extend json = parse_json(AdditionalFields)
| extend dataDesp = tostring(json.DataDescription.PropertyValue)
| extend opType = tostring(json.OperationType.PropertyValue)
| where (dataDesp in~ ("Google Chrome", "Microsoft Edge") and opType =~ "SPCryptUnprotect")
| project Timestamp, ReportId, DeviceId, ActionType, InitiatingProcessParentFileName, InitiatingProcessFileName, InitiatingProcessVersionInfoInternalFileName, InitiatingProcessCommandLine, AdditionalFields, dataDesp, opType

```

**Sensitive browser file access via AutoIT**

Identify AutoIT scripts (renamed or otherwise) accessing sensitive browser files.

```
let browserDirs = pack_array(@"GoogleChromeUser Data", @"MicrosoftEdgeUser Data", @"MozillaFirefoxProfiles"); 
let browserSensitiveFiles = pack_array("Web Data", "Login Data", "key4.db", "formhistory.sqlite", "cookies.sqlite", "logins.json", "places.sqlite", "cert9.db");
DeviceEvents
| where AdditionalFields has_any ("FileOpenSource") // Filter for "File Open" events.
| where InitiatingProcessVersionInfoInternalFileName == "AutoIt3.exe"
| where (AdditionalFields has_any(browserDirs) or  AdditionalFields has_any(browserSensitiveFiles)) 
| extend json = parse_json(AdditionalFields)
| extend File_Name = tostring(json.FileName.PropertyValue)
| where (File_Name has_any (browserDirs) and File_Name has_any (browserSensitiveFiles))
| project Timestamp, ReportId, DeviceId, InitiatingProcessParentFileName, InitiatingProcessFileName, InitiatingProcessVersionInfoInternalFileName, InitiatingProcessCommandLine, File_Name

```

**Sensitive browser file access via LOLBAS binaries**

Identify LOLBAS binaries (_RegAsm.exe_ and _MSBuild.exe_) accessing sensitive browser files.

```
let browserDirs = pack_array(@"GoogleChromeUser Data", @"MicrosoftEdgeUser Data", @"MozillaFirefoxProfiles"); 
let browserSensitiveFiles = pack_array("Web Data", "Login Data", "key4.db", "formhistory.sqlite", "cookies.sqlite", "logins.json", "places.sqlite", "cert9.db");
DeviceEvents
| where AdditionalFields has_any ("FileOpenSource") // Filter for "File Open" events.
| where InitiatingProcessFileName has_any ("RegAsm.exe", "MSBuild.exe")
 | where (AdditionalFields has_any(browserDirs) or  AdditionalFields has_any(browserSensitiveFiles)) 
| extend json = parse_json(AdditionalFields)
| extend File_Name = tostring(json.FileName.PropertyValue)
| where (File_Name has_any (browserDirs) and File_Name has_any (browserSensitiveFiles))
| project Timestamp, ReportId, DeviceId, InitiatingProcessParentFileName, InitiatingProcessFileName, InitiatingProcessVersionInfoInternalFileName, InitiatingProcessCommandLine, File_Name

```

### Microsoft Sentinel

Microsoft Sentinel customers can use the TI Mapping analytics (a series of analytics all prefixed with ‘TI map’) to automatically match the malicious domain indicators mentioned in this blog post with data in their workspace. If the TI Map analytics are not currently deployed, customers can install the Threat Intelligence solution from the Microsoft Sentinel Content Hub to have the analytics rule deployed in their Sentinel workspace.

## Indicators of compromise

**Streaming website domains with malicious iframe**

| **Indicator**  | **Type**  |
| --- | --- |
|  _movies7\[.\]net_ |  Domain |
|  _0123movie\[.\]art_ |  Domain |

**Malicious iframe redirector domains**

| **Indicator**  | **Type**  |
| --- | --- |
|  _fle-rvd0i9o8-moo\[.\]com_ |  Domain |
|  _0cbcq8mu\[.\]com_ |  Domain |

**Malvertisement distributor**

| **Indicator**  | **Type**  |
| --- | --- |
|  _widiaoexhe\[.\]top_ |  Domain |

**Malvertising website domains**

| **Indicator**  | **Type**  |
| --- | --- |
| _widiaoexhe\[.\]top_ |  Domain |
| _predictivdisplay\[.\]com_ |  Domain |
| _buzzonclick\[.\]com_ |  Domain |
| _pulseadnetwork\[.\]com_ |  Domain |
| _onclickalgo\[.\]com_ | Domain |
| _liveadexchanger\[.\]com_ | Domain |
| _greatdexchange\[.\]com_ | Domain |
| _dexpredict\[.\]com_ | Domain |
| _onclickperformance\[.\]com_ | Domain |

**GitHub referral URLs**

| **Indicator**  | **Type**  |
| --- | --- |
| _hxxps://pmpdm\[.\]com/webcheck35/_ | URL |
| _hxxps://startherehosting\[.\]net/todaypage/_ | URL |
| _hxxps://kassalias\[.\]com/pageagain/_ | URL |
| _hxxps://sacpools\[.\]com/pratespage/_ | URL |
| _hxxps://dreamstorycards\[.\]com/amzpage/_ | URL |
| _hxxps://primetimeessentials\[.\]com/newpagyes/_ | URL |
| _hxxps://razorskigrips\[.\]com/perfect/_ | URL |
| _hxxps://lakeplacidluxuryhomes\[.\]com/webpage37_ | URL |
| _hxxps://ageless-skincare\[.\]com/gn/_ | URL |
| _hxxps://clarebrownmusic\[.\]com/goodday/_ | URL |
| _hxxps://razorskigrips\[.\]com/gn/_ | URL |
| _hxxps://compass-point-yachts\[.\]com/nicepage77/pro77.php_ | URL |
| _hxxps://razorskigrips\[.\]com/goodk/_ | URL |
| _hxxps://lilharts\[.\]com/propage6/_ | URL |
| _hxxps://enricoborino\[.\]com/propage66/_ | URL |
| _hxxps://afterpm\[.\]com/pricedpage/_ | URL |
| _hxxps://eaholloway\[.\]com/updatepage333/_ | URL |
| _hxxps://physicaltherapytustin\[.\]com/webhtml/_ | URL |
| _hxxps://physicaltherapytustin\[.\]com/web-X/_ | URL |
| _hxxps://razorskigrips\[.\]com/newnewpage/_ | URL |
| _hxxps://statsace\[.\]com/web\_us/_ | URL |
| _hxxps://nationpains\[.\]com/safeweb3/_ | URL |
| _hxxps://vjav\[.\]com/_ | URL |
| _hxxps://thegay\[.\]com/_ | URL |
| _hxxps://olopruy\[.\]com/_ | URL |
| _hxxps://desi-porn\[.\]tube/_ | URL |
| _hxxps://cumpaicizewoa\[.\]net/partitial/_ | URL |
| _hxxps://ak.ptailadsol\[.\]net/partitial/_ | URL |
| _hxxps://egrowz\[.\]com/webview/_ | URL |
| _hxxps://or-ipo\[.\]com/nice/_ | URL |

**GitHub URLs**

| **Indicator**  | **Type**  |
| --- | --- |
| _hxxps://github\[.\]com/down4up/_ |  URL |
| _hxxps://github\[.\]com/g1lsetup/iln77_ | URL |
| _hxxps://github\[.\]com/g1lsetup/v2025_ | URL |
| _hxxps://github\[.\]com/git2312now/DownNew152/_ | URL |
| _hxxps://github\[.\]com/muhammadshahblis/_ | URL |
| _hxxps://github\[.\]com/Jimelecar_ | URL |
| _hxxps://github\[.\]com/kloserw_ | URL |
| _hxxps://github\[.\]com/kopersparan/_ | URL |
| _hxxps://github\[.\]com/zotokilowa_ | URL |
| _hxxps://github\[.\]com/colvfile/bmx84542_ | URL |
| _hxxps://github\[.\]com/colvfile/yesyes333_ | URL |
| _hxxps://github\[.\]com/mp3andmovies/_ | URL |
| _hxxps://github\[.\]com/anatfile/newl_ | URL |
| _hxxps://github\[.\]com/downloadprov/www_ | URL |
| _hxxps://github\[.\]com/abdfilesup/readyyes_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/898537481_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/898072392/_  | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/902107140_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/902405338_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/901430321/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/903047306/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/899121225_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/899472962/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/900979287/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/901553970_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/901617842/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/897657726_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/903499100/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/903509708/_ | URL |
| _hxxps://objects.githubusercontent\[.\]com/github-production-release-asset-2e65be/915668132/_ | URL |

**DropBox URL**

| **Indicator**  | **Type** |
| --- | --- |
|  _hxxps://uc8ce1a0cf2efa109cd4540c0c22.dl.dropboxusercontent\[.\]com/cd/0/get/CgHUWBzFWtX1ZE6CwwKXVb1EvW4tnDYYhbX8Iqj70VZ5e2uwYlkAq6V-xQcjX0NMjbOJrN3\_FjuanOjW66WdjPHNw2ptSNdXZi4Sey6511OjeNGuzMwxtagHQe5qFOFpY2xyt1sWeMfLwwHkvGGFzcKY/file?dl=1#_  | URL |

**Discord URL**

| **Indicator**  | **Type** |
| --- | --- |
| _hxxps://cdn.discordapp\[.\]com/attachments/1316109420995809283/1316112071376769165/NativeApp\_G4QLIQRa.exe_ |  URL |

**First stage GitHub-hosted payloads**

| **Filename** | **SHA-256** |
| --- | --- |
| _NanoPhanoTool.exe_ | cd207b81505f13d46d94b08fb5130ddae52bd1748856e6b474688e590933a718 |
| _Squarel\_JhZjXa.exe_ | b87ff3da811a598c284997222e0b5a9b60b7f79206f8d795781db7b2abd41439 |
| _PriceApp\_1jth1MMk.exe_ | ef2d8f433a896575442c13614157261b32dd4b2a1210aca3be601d301feb1fef |
| _Paranoide.exe_ | 5550ea265b105b843f6b094979bfa0d04e1ee2d1607b2e0d210cd0dea8aab942 |
| _AliasApp.exe_ | 0c2d5b2a88a703df4392e060a7fb8f06085ca3e88b0552f7a6a9d9ef8afdda03 |
| _X-essentiApp.exe_ | d8ae7fbb8db3b027a832be6f1acc44c7f5aebfdcb306cd297f7c30f1594d9c45 |
| _QilawatProtone.exe_ | 823d37f852a655088bb4a81d2f3a8bfd18ea4f31e7117e5713aeb9e0443ccd99 |
| _ElectronApp.exe_ | 588071382ac2bbff6608c5e7f380c8f85cdd9e6df172c5edbdfdb42eb74367dc |
| _NativeApp\_dRRgoZqi.exe_ | dd8ce4a2fdf4af4d3fc4df88ac867efb49276acdcacaecb0c91e99110477dbf2 |
| _NativeApp\_G5L1NHZZ.exe_ | 380920dfcdec5d7704ad1af1ce35feba7c3af1b68ffa4588b734647f28eeabb7 |
| _NativeApp\_86hwwNjq.exe_ | 96cc7c9fc7ffbda89c920b2920327a62a09f8cb4fcf400bbfb02de82cdd8dba1 |
| _NativeApp\_01C02RhQ.exe_ | 800c5cd5ec75d552f00d0aca42bdade317f12aa797103b9357d44962e8bcd37a |
| _App\_aeIGCY3g.exe_ | afdc1a1e1e934f18be28465315704a12b2cd43c186fbee94f7464392849a5ad0 |
| _Pictore.exe_ | de6fcdf58b22a51d26eacb0e2c992d9a894c1894b3c8d70f4db80044dacb7430 |
| _ScenarioIT.exe_ | f677be06af71f81c93b173bdcb0488db637d91f0d614df644ebed94bf48e6541 |
| _CiscoProton.exe_ | 7b88f805ed46f4bfc3aa58ef94d980ff57f6c09b86c14afa750fc41d32b7ada8 |
| _Alarmer.exe_ | dc8e5cae55181833fa9f3dd0f9af37a2112620fd47b22e2fd9b4a1b05c68620f |
| _AevellaAi.2.exe_ | 3e8ef8ab691f2d5b820aa7ac805044e5c945d8adcfc51ee79d875e169f925455 |
| _avs.exe_ | d2e9362ae88a795e6652d65b9ae89d8ff5bdebbfec8692b8358aa182bc8ce7a4 |
| _mrg.exe_ | 113290aaa5c0b0793d50de6819f2b2eead5e321e9300d91b9a36d62ba8e5bbc1 |
| _mrg.exe_ | 732b4874ac1a1d4326fc1d71d16910fce2835ceb87e76ad4ef2e40b1e948a6cc |
| _Application.exe_ | aea0892bf9a533d75256212b4f6eaede2c4c9e47f0725fc3c61730ccfba25ec8 |
| _Application.exe_ | ea2e21d0c09662a0f9b42d95ce706b5ed26634f20b9b5027ec681635a4072453 |
| _SalmonSamurai.exe_ | 83679dfd6331a0a0d829c0f3aed5112b69a7024ff1ceebf7179ba5c2b4d21fc5 |
| _Arendada.exe_ | 47ef2b7e8f35167fab1ecdd5ddb73d41e40e6a126f4da7540c1c0394195cb3df |
| _Arduino.exe_ | 92d457b286fb63d2f5ec9413fd234643448c5f8d2c0763e43ed5cf27ab47eb02 |
| _SecondS.exe_ | 9d5c551f076449af0dbd7e05e1c2e439d6f6335b3dd07a8fa1b819c250327f39 |
| _ultraedit.msi_ | 0e20bea91c3b70259a7b6eef3bff614ce9b6df25e078bc470bfef9489c9c76e6 |

**First-stage Dropbox-hosted payload**

| **Filename** | **SHA-256** |
| --- | --- |
| _App\_File-x38.3.exe_ | c0bc1227bdc56fa601c1c5c0527a100d7c251966e40b2a5fa89b39a2197dda67 |

**First-stage Discord-hosted payload**

| **Filename** | **SHA-256** |
| --- | --- |
| _NativeApp\_G4QLIQRa.exe_ | 87200e8b43a6707cd66fc240d2c9e9da7f3ed03c8507adf7c1cfe56ba1a9c57d |

**Certificate signatures of GitHub-hosted payloads**

| **Indicator**  |
| --- |
| c855f7541e50c98a5ae09f840fa06badb97ab46c |
| 94c21e6384f2ffb72bd856c1c40b788f314b5298 |
| 74df2582af3780d81a8071e260c2b04259efc35a |
| 07728484b1bb8702a87c6e5a154e0d690af2ff38 |
| 901f3fe4e599cd155132ce2b6bf3c5f6d1e0387c |
| be7156bd07dd7f72521fae4a3d6f46c48dd2ce9e |
| 686b7ebba606303b5085633fcaa0685272b4d9b9 |
| 74a8215a54f52f792d351d66bd56a0ac626474fb |
| 561620a3f0bf4fb96898a99252b85b00c468e5af |
| 8137f599ac036b0eaae9486158e40e90ebdbce94 |
| E9007755cfe5643d18618786de1995914098307f |

**Certificate signature of Dropbox-hosted payload**

| **Indicator**  |
| --- |
|  fa6146f1fdad58b8db08411c459cb70acf82846d |

**Second-stage payloads**

| **File name** | **SHA-256** |
| --- | --- |
| _NanoTool.exe_ | 9f958b85dc42ac6301fe1abfd4b11316b637c0b8c0bf627c9b141699dc18e885 |
| _Squarel.exe_ | 29539039c19995d788f24329ebb960eaf5d86b1f8df76272284d08a63a034d42 |
| _ParanoidResolver.exe_ | 1f73a00b5a7ac31ffc89abbedef17ee2281cf065423a3644787f6c622295ff29 |
| _AliasInstall.exe_ | 997671c13bb78a9acc658e2c3a1abf06aedc4f1f4f1e5fd8d469a912fc93993b |
| _IoNixNginx.exe_ | 1d8ab53874b2edfb058dd64da8a61d92c8a8e302cc737155e0d718dbe169ba36 |
| _QilawatProton.exe_  | 885f8a704f1b3aaa2c4ddf7eab779d87ecb1290853697a1e6fb6341c4f825968 |
| _ProtonEditor.exe_ | 48f422bf2b878d142f376713a543d113e9f964f6761d15d4149a4d71441739e5 |
| _AlEditor.exe_  | 9daa63046978d7097ea20bfbb543d82374cf44ba37f966b87488f63daf20999e |
| _Scielfic.exe_ | 6ec86b4e200144084e07407200a5294985054bdaddb3d6c56358fc0657e48157 |
| _Pictore.exe_ | 18959833da3df8d5d8d19c3fce496c55aa70140824d3a942fe43d547b9a8c065 |
| _AlarmWalker Solid.exe_ | 552f23590bdf301f481e62a9ce3c279bab887d64f4ba3ea3d81a348e3eff6c45 |
| _Aevella.exe_  | 2a738f41b42f47b64be7dc2d16a4068472b860318537b5076814891a7d00b3bb |
| _Application.exe_ | 5b50d0d67db361da72af2af20763b0dde9e5e86b792676acb9750f32221e955c |
| _ArchiverApp.exe_ | cfeac95017edbfe9a0ad8f24e7539f54482012d11dc79b7b6f41ff4ff742d9c6 |
| _LakerBaker.exe_ | af7454ca632dead16a36da583fb89f640f70df702163f5a22ba663e985f80d88 |
| _NanoTool.exe_ | efdcd37ee0845e0145084c2a10432e61b1b4bf6b44ecd41d61a54b10e3563650 |
| _DisplayPhotoViewer.exe_ | 86ae0078776c0411504cf97f4369512013306fcf568cc1dc7a07e180dde08eda |
| _CheryLady Application.exe_ | 773d3cb5edef063fb5084efcd8d9d7ac7624b271f94706d4598df058a89f77fd |
| _SalmonSamurai.exe_ | 40abba1e7da7b3eaad08a6e3be381a9fc2ab01b59638912029bc9a4aa1e0c7a7 |
| _Heaveen Application.exe_ | 39dbf19d5c642d48632bfaf2f83518cfbd2b197018642ea1f2eb3d81897cf17d |
| _Cisco Application.exe_ | 234971ecd1bf152c903841fac81bdaa288954a2757a73193174cde02fa6f937b |
| _Simplify.exe_ | 221615de3d66e528494901fb5bd1725ecda336af33fe758426295f659141b931 |
| _SecondS.tmp_ | 5185f953be3d0842416d679582b233fdc886301441e920cb9d11642b3779d153 |

**Second-stage C2s**

| **Indicator**  | **Type**  |
| --- | --- |
| 159.100.18\[.\]192 |  C2 |
| 192.142.10\[.\]246 |  C2 |
| 79.133.46\[.\]35 | C2 |
| 84.200.24\[.\]191 | C2 |
| 84.200.24\[.\]26 | C2 |
| 89.187.28\[.\]253 |  C2 |
| 185.92.181\[.\]1 | C2 |
| 188.245.94\[.\]250 |  C2 |

**Third-stage payloads: _.exe_ and PowerShell files**

| **File name** | **SHA-256** |
| --- | --- |
| _ApproachAllan.exe_ | 4e5fafffb633319060190a098b9ea156ec0243eb1279d78d27551e507d937947 |
| _DiscoConvicted.exe_ | 008aed5e3528e2c09605af26b3cda88419efb29b85ed122cab59913c18f7dc75 |
| _AwesomeTrader.exe_ | 21d4252a6492270f24282f8de9e985c9b8c61412f42d169ff4b128fd689d4753 |
| _CiteLips.exe_ | c9713c06526673bf18dbdaf46ea61ca9dd8fefe8ceec3be06c63db17e01e3741 |
| _RepublicChoir.exe_ | f649f66116a3351b60aa914e0b1944c2181485b1cf251fc9c1f6dab8a9db426b |
| _6Zh7MvxYtHTBFX90Mn.exe_ | b96360d48c2755ded301dd017b37dfdce921bdea7731c4b31958d945c8a0b8f5 |
| _ExclusivePottery.exe_ | 54c8a4f58b548c0cf6dbea2522e258723263ccde11d23e48985bdd1fd3535ce2 |
| _squarel.ps1_ | d70ccae7914fc8c36c9e11b2a7f10bebd7f5696e78d8836554f4990b0f688dbb |
| _MadCountries.exe_ | 9fe2c00641ece18898267b3c6e4ee0cb82ffefbc270c0767c441c3f38b63a12a |
| _HockeyTract.exe_ | f136fa82ff73271708afe744f4e6a19cd5039e08ecd3ddad8e4d238f338f4d58 |
| _BruneiPlugins.exe_ | 453de65c9cc2dc62a67c502cd8bc26968acad9a671c1e095312c1fa6db4a7c74 |
| _CnnCylinder.exe_ | a76548a500d81dbb6f50419784a9b0323f5e42245ac7067af2adee0558167116 |
| _specreal.ps1_ | d70ccae7914fc8c36c9e11b2a7f10bebd7f5696e78d8836554f4990b0f688dbb |
| _InflationWinston.exe_ | dfbba64219fc63815db538ae8b51e07ec7132f4b39ba4a556c64bd3a5f024c2d |
| _netsup.ps1_  | d70ccae7914fc8c36c9e11b2a7f10bebd7f5696e78d8836554f4990b0f688dbb |
| _CfUltra.exe_ | 7880714c47260dba1fd4a4e4598e365b2a5ed0ad17718d8d192d28cf75660584 |
| _CalvinShoppercom.exe_ | 345a898d5eab800b7b7cbd455135c5474c5f0a9c366df3beb110f225ba734519 |
| _EscortUnavailable.exe_ | 258efd913cccdb70273c9410070f093337d5574b74c683c1cdff33baff9ffd7c |
| _DisagreeProceed.exe_ | 9c82a2190930ec778688779a5ad52537d8b0856c8142c71631b308f1f8f0e772 |
| _BarbieBiblical.exe_ | 34f43bfc0a6f0d0f70b6eee0fa29c6dc62596ab2b867bbabd27c68153ea47f24 |
| _MysqlManaging.exe_ | ef1f9d507a137a4112ac92c576fc44796403eb53d71fe2ddb00376419c8a604e |
| _PillsHarvest.exe_ | 4af3898ba3cf8b420ea1e6c5ce7cdca7775a4c9b78f67b493a9c73465432f1d3 |
| _BelfastProt.exe_ | ad470bffbd120fc3a6c2c2e52af3c12f9f0153e76fee5e2b489a3d1870bdff03 |
| _HowardLikelihood.exe_ | cc08892ace9ac746623b9d0178cd4d149f6a9ab10467fb9059d16f2c0038dcf9 |
| _SorryRequiring.exe_ | 4a2346d453b2ac894b67625640347c15e74e3091a9aa15629c3a808caaff1b2b |
| _SearchMed.exe_ | b0aab51b5e4a9cdd5b3d2785e4dea1ec06b20bc00e4015ccd79e0ba395a20fbd |
| _RepublicChoir.exe_ | f649f66116a3351b60aa914e0b1944c2181485b1cf251fc9c1f6dab8a9db426b |
| _DesignersCrawford.exe_ | e8452a65a452abdb4b2e629f767a038e0792e6e2393fb91bf17b27a0ce28c936 |
| _HumanitarianProvinces.exe_ | 25cfd6e6a9544990093566d5ea9d7205a60599bfda8c0f4d59fca31e58a7640b |
| _ResetEngaging.exe_ | 51fbc196175f4fb9f38d843ee53710cde943e5caf1b0552624c7b65e6c231f7e |
| _EducationalDerby.exe_ | 4a9a8c46ff96e4f066f51ff7e64b1c459967e0cdeb74b6de02cf1033e31c1c7b |
| _StringsGrill.exe_ | f2a8840778484a56f1215f0fa8f6e8b0fb805fce99e62c01ff0a1f541f1d6808 |
| _CongressionalMechanics.exe_ | 2060509a63180c2f5075faf88ce7079c48903070c1c6b09fa3f9d6db05b8d9da |
| _SexuallyWheat.exe_ | d39075915708d012f12b7410cd63e19434d630b2b7dbe60bd72ce003cd2efeaf |
| _PerceptionCircuits.exe_ | 0e7dd3aa100d9e22d367cb995879ac4916cb4feb1c6085e06139e02cc7270bba |
| _WWv63SKrHflebBd4VW.ps1_ | 483796a64f004a684a7bc20c1ddd5c671b41a808bc77634112e1703052666a64 |
| _WritingsShanghai.exe_ | fa131ea3ce9a9456e1d37065c7f7385ce98ffa329936b5fdd0fd0e78ade88ecb |
| _IUService.exe_ | d5a6714ab95caa92ef1a712465a44c1827122b971bdb28ffa33221e07651d6f7 |
| _RttHlp.exe_ | 8aed681ad8d660257c10d2f0e85ae673184055a341901643f27afc38e5ef8473 |
| _ASmartService.exe_ | 75712824b916c1dc8978f65c060340dc69b1efa0145dddbf54299689b9f4a118 |
| _ClaireSpecifically.exe_ | 746abef4bde48da9f9bff3c23dd6edf8f1bea4b568df2a7d369cb30536ec9ce0 |
| _report.exe_ | 6daccc09f5f843b1fa4adde64ad282511f591a641cb474e123fed922167df6ae |
| _xh6yIa7PXFCsasc0H5.exe_ | 5f17501193f5f823f419329bc20534461a7195aa4c456e27af6b0df5b0788041 |
| _yL6Iwcawoz3KDjg60m.exe_ | 5ecb4240fae36893973fb306c52c7e548308ebcfba6d101aad4e083407968a96 |
| _CustomsCampbell.exe_ | 5b80c7d65bb655ccb6e3264f4459a968edcda28084e0ddde16698f642b2d7d83 |
| _HoldemRover.exe_ | 4c60cdd1ee4045eb0b3bfda8326802d17565f3d1ff6829ac05775ebc6d9ca2dc |
| _QUCvpZLobnhvno5v1t.exe_ | 4bac608722756c80c29fee6f73949c011ea78243e5267e86b7b20b3beeb79f9e |
| _EmilyHaiti.exe_ | 3221f1356a91d4f06d1deee988be04597cc11bc1cab199ba9c43b9d80dfa88bd |
| _PIPIPOO.exe_ | 15bf7a141a5a5e7e5c19ffbfbb5b781ae8db52d9ba5ffeb1364964580ed55b13 |
| _ReliefOrganizational.exe_ | 02533f92d522d47b9d630375633803dd8d6b4723e87d914cd29460d404134a66 |
| _HelloWorld.ps1_ | 670218cfc5c16d06762b6bc74cda4902087d812e72c52d6b9077c4c416485 |
| _251.zip_ | 0997201124780f11a16662a0d718b1a3ef3202c5153191f93511d7ecd0de4d8d |
| _251.exe_ | 4b50e7fba5e33bac30b98494361d5ab725022c38271b3eb89b9c4aab457dca78 |

**Fourth-stage AutoIT, NetSupport RAT, PowerShell, and Lumma**

| **File name(s)** | **SHA-256** |
| --- | --- |
| _Korea.com_   _Fabric.com_   _Affiliated.com_   _Weeks.com_   _Briefly.com_   _Denmark.com_   _Tanzania.com_   _Cookies.com_   _Spice.com_   _SophieHub.scr_   _SpaceWarp.scr_   _SkillSync.scr_   _Quantify.scr_   _HealthPulse_   _CogniFlow.scr_   _ArgonautGuard.scr_ | 865347471135bb5459ad0e647e75a14ad91424b6f13a5c05d9ecd9183a8a1cf4 |
| _Warrant.com_   _Ford.com_   _AutoIt3.exe_   _Seq.com_   _Underwear.com_ | 1300262a9d6bb6fcbefc0d299cce194435790e70b9c7b4a651e202e90a32fd49 |
| _Presentationhost.exe_ | 18df68d1581c11130c139fa52abb74dfd098a9af698a250645d6a4a65efcbf2d |
| _erLX7UsT.ps1_ | 2a29c9904d1860ea3177da7553c8b1bf1944566e5bc1e71340d9e0ff079f0bd3 |
| _675aff18abddc.exe_ | adf5a9c2db09a782b3080fc011d45eb6eb597d8b475c3c27755992b1d7796e91 |
| _675aff18abddc.vbs_ | 5f2b66cf3370323f5be9d7ed8a0597bffea8cc1f76cd96ebb5a8a9da3a1bdc71 |
| _251.exe_ | 707a23dcd031c4b4969a021bc259186ca6fd4046d6b7b1aaffc90ba40b2a603b |

**Third-stage C2s**

| **Indicator**  | **Type** |
| --- | --- |
| _hxxp://keikochio\[.\]com/staz/gribs.zip_ |  C2 |
| _hxxp://keikochio\[.\]com/incall.php?=compName=<computer name>_ |  C2 |
| _hxxps://stocktemplates\[.\]net/input.php?compName=<computer name>_ |  C2 |
| _hxxp://89.23.96\[.\]126/?v=3&event=ready&url=hxxp://188.245.94\[.\]250:443/auto/28cd7492facfd54e11d48e52398aefa7/251.exe_ |  C2 |

**Fourth-stage C2s**

| **Indicator**  | **Type**  |
| --- | --- |
| 45.141.84\[.\]60 |  IP address |
| 91.202.233\[.\]18 |  IP address |
| 154.216.20\[.\]131 |  IP address |
| 5.10.250\[.\]240 |  IP address |
| 79.132.128\[.\]77 |  IP address |
| _hxxps://shortlearn\[.\]click_ | URL |
| _hxxps://wrathful-jammy\[.\]cyou_ | URL |
| _hxxps://mycomp\[.\]cyou_ | URL |
| _hxxps://kefuguy\[.\]shop_ | URL |
| _hxxps://lumdukekiy\[.\]shop_ | URL |
| _hxxps://lumquvonee\[.\]shop_ | URL |
| _hxxps://klipcatepiu0\[.\]shop_ | URL |
| _hxxps://gostrm\[.\]shop_ | URL |
| _hxxps://ukuhost\[.\]net_ | URL |
| _hxxps://silversky\[.\]club_ | URL |
| _hxxps://pub.culture-quest\[.\]shop_ | URL |
| _hxxps://se-blurry\[.\]biz_ | URL |
| _hxxps://zinc-sneark\[.\]biz_ | URL |
| _hxxps://dwell-exclaim\[.\]biz_ | URL |
| _hxxps://formy-spill\[.\]biz_ | URL |
| _hxxps://covery-mover\[.\]biz_ | URL |
| _hxxps://dare-curbys\[.\]biz_ | URL |
| _hxxps://impend-differ\[.\]biz_ | URL |
| _hxxps://dreasd\[.\]xyz_ | URL |
| _hxxps://ikores\[.\]sbs_ | URL |
| _hxxps://violettru\[.\]click_ | URL |
| _hxxps://marshal-zhukov\[.\]com_ | URL |
| _hxxps://tailyoveriw\[.\]my_ | URL |

**Fourth-stage testing connectivity sites**

| **Indicator**  | **Type**  |
| --- | --- |
| _hxxps://baidu.com_ | URL |
| _hxxps://360.net_ | URL |
| _hxxps://praxlonfire73.live_ | URL |

## References

- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe

- https://github.com/antivirusevasion69/doenerium

- https://curl.se/docs/manpage.html#-s

- https://www.virustotal.com/gui/file/2a29c9904d1860ea3177da7553c8b1bf1944566e5bc1e71340d9e0ff079f0bd3

## Learn more

For the latest security research from the Microsoft Threat Intelligence community, check out the Microsoft Threat Intelligence Blog: https://aka.ms/threatintelblog.

To get notified about new publications and to join discussions on social media, follow us on LinkedIn at https://www.linkedin.com/showcase/microsoft-threat-intelligence, and on X (formerly Twitter) at https://x.com/MsftSecIntel.

Hear more about this discovery and how threat actors in this campaign leverage trusted platforms and advanced techniques to achieve their malicious goals in this episode of the Microsoft Threat Intelligence podcast, hosted by Sherrod DeGrippo: https://thecyberwire.com/podcasts/microsoft-threat-intelligence/39/notes. To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the Microsoft Threat Intelligence podcast: https://thecyberwire.com/podcasts/microsoft-threat-intelligence.

The post Malvertising campaign leads to info stealers hosted on GitHub appeared first on Microsoft Security Blog.

Go to Source
