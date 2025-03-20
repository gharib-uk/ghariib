---
title: "StilachiRAT analysis: From system reconnaissance to cryptocurrency theft"
date: 2025-03-19
---

In November 2024, Microsoft Incident Response researchers uncovered a novel remote access trojan (RAT) we named StilachiRAT that demonstrates sophisticated techniques to evade detection, persist in the target environment, and exfiltrate sensitive data. Analysis of the StilachiRAT’s _WWStartupCtrl64.dll_ module that contains the RAT capabilities revealed the use of various methods to steal information from the target system, such as credentials stored in the browser, digital wallet information, data stored in the clipboard, as well as system information.

Microsoft has not yet attributed StilachiRAT to a specific threat actor or geolocation. Based on Microsoft’s current visibility, the malware does not exhibit widespread distribution at this time. However, due to its stealth capabilities and the rapid changes within the malware ecosystem, we are sharing these findings as part of our ongoing efforts to monitor, analyze, and report on the evolving threat landscape.

Microsoft security solutions can detect activities related to attacks that use StilachiRAT. To help defenders protect their network, we are also sharing mitigation guidance to help reduce the impact of this threat, detection details, and hunting queries. Microsoft continues to monitor information on the delivery vector used in these attacks. Malware like StilachiRAT can be installed through multiple vectors; therefore, it is critical to implement security hardening measures to prevent the initial compromise. 

This blog presents our detailed findings on all the key capabilities of StilachiRAT, which include:

- **System reconnaissance**: Collects comprehensive system information, including operating system (OS) details, hardware identifiers, camera presence, active Remote Desktop Protocol (RDP) sessions, and running graphical user interface (GUI) applications, allowing detailed profiling of the target system.

- **Digital wallet targeting**: Scans for configuration data of 20 different cryptocurrency wallet extensions for the Google Chrome browser.

- **Credential theft**: Extracts and decrypts saved credentials from Google Chrome, gaining access to usernames and passwords stored in the browser.

- **Command-and-control (C2) connectivity**: Establishes communication with remote C2 servers using TCP ports 53, 443, or 16000, enabling remote command execution and potentially SOCKS like proxying.

- **Command execution**: Supports a variety of commands from the C2 server, including system reboots, log clearing, registry manipulation, application execution, and system suspension.

- **Persistence mechanisms**: Achieves persistence through the Windows service control manager (SCM) and uses watchdog threads to ensure self-reinstatement if removed.

- **RDP monitoring**: Monitors RDP sessions, capturing active window information and impersonating users, allowing for potential lateral movement within networks.

- **Clipboard and data collection**: Continuously monitors clipboard content, actively searching for sensitive data like passwords and cryptocurrency keys, while tracking active windows and applications.

- **Anti-forensics and evasion**: Employs anti-forensic tactics by clearing event logs, detecting analysis tools, and implementing sandbox-evading behaviors to avoid detection.

## Technical analysis of key capabilities

### System reconnaissance

StilachiRAT gathers extensive system information, including OS details, device identifiers, BIOS serial numbers, and camera presence. Information is collected through the Component Object Model (COM) Web-based Enterprise Management (WBEM) interfaces using WMI Query Language (WQL). Below are some of the queries it executes:

**Serial number**

![](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-44.webp)

**Camera**

![A black and green text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-3.jpg)

**OS / System info (server, model, manufacturer)**

![A black text on a white background](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Query3-1024x95.webp)

Additionally, the malware creates a unique identification on the infected device that is derived from the system’s serial number and attackers’ public RSA key. The information is stored in the registry under a CLSID key.

<figure>

![A screenshot of a computer code](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-45.webp)

<figcaption>

Figure 1. Example of a unique ID stored in the registry

</figcaption>

</figure>

### Digital wallet targeting

StilachiRAT targets a list of specific cryptocurrency wallet extensions for the Google Chrome browser. It accesses the settings in the following registry key and validates if any of the extensions are installed:

**SOFTWAREGoogleChromePreferenceMACsDefaultextensions.settings**

The malware targets the following cryptocurrency wallet extensions:

| **Cryptocurrency wallet extension name** | **Chrome extension identifier** |
| --- | --- |
| Bitget Wallet (Formerly BitKeep) | jiidiaalihmmhddjgbnbgdfflelocpak |
| Trust Wallet | egjidjbpglichdcondbcbdnbeeppgdph |
| TronLink | ibnejdfjmmkpcnlpebklmnkoeoihofec |
| MetaMask (ethereum) | nkbihfbeogaeaoehlefnkodbefgpgknn |
| TokenPocket | mfgccjchihfkkindfppnaooecgfneiii |
| BNB Chain Wallet | fhbohimaelbohpjbbldcngcnapndodjp |
| OKX Wallet | mcohilncbfahbmgdjkbpemcciiolgcge |
| Sui Wallet | opcgpfmipidbgpenhmajoajpbobppdil |
| Braavos – Starknet Wallet | jnlgamecbpmbajjfhmmmlhejkemejdma |
| Coinbase Wallet | hnfanknocfeofbddgcijnmhnfnkdnaad |
| Leap Cosmos Wallet | fcfcfllfndlomdhbehjjcoimbgofdncg |
| Manta Wallet | enabgbdfcbaehmbigakijjabdpdnimlg |
| Keplr | dmkamcknogkgcdfhhbddcghachkejeap |
| Phantom | bfnaelmomeimhlpmgjnjophhpkkoljpa |
| Compass Wallet for Sei | anokgmphncpekkhclmingpimjmcooifb |
| Math Wallet | afbcbjpbpfadlkmhmclhkeeodmamcflc |
| Fractal Wallet | agechnindjilpccclelhlbjphbgnobpf |
| Station Wallet | aiifbnbfobpmeekipheeijimdpnlpgpp |
| ConfluxPortal | bjiiiblnpkonoiegdlifcciokocjbhkd |
| Plug | cfbfdhimifdmdehjmkdobpcjfefblkjm |

### Credential theft

StilachiRAT extracts Google Chrome’s _encryption\_key_ from the local state file in a user’s directory. However, since the key is encrypted when Chrome is first installed, it uses Windows APIs that rely on current user’s context to decrypt the master key. This allows access to the stored credentials in the password vault. The stored credentials are extracted from the following locations:

- _%LOCALAPPDATA%GoogleChromeUser DataLocal State_ – stores Chrome’s configuration data, including the encrypted key.

- _%LOCALAPPDATA%GoogleChromeUser DataDefaultLogin Data_ – stores entered user credentials.

The “Login Data**”** stores information using an SQLite database and the malware retrieves credentials using the following query:

![A black text on a white background](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-4.jpg)

### Command-and-control (C2)

There are two configured addresses for the C2 server – one is stored in obfuscated form and the other is an IP address converted to its binary format (instead of a regular string):

- app.95560\[.\]cc

- 194.195.89\[.\]47

The communications channel is established using TCP ports 53, 443, or 16000, selected randomly. Additionally, the malware checks for presence of _tcpview.exe_ and will not proceed if one is present. It also delays initial connection by two hours, presumably to evade detection. Once connected, a list of active windows is sent to the server. Additional technical findings regarding C2 communications functionality are listed in the section below.

<figure>

![A screenshot of a computer program](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-46.webp)

<figcaption>

Figure 2. The malware delays connection to evade detection

</figcaption>

</figure>

### Persistence mechanisms

StilachiRAT can be launched both as a Windows service or a standalone component. In both cases, there is a mechanism in place to ensure the malware isn’t removed.

A watchdog thread monitors both the EXE and dynamic link library (DLL) files used by the malware by periodically polling for their presence. If found absent, the files can be recreated from an internal copy obtained during initialization. Lastly, the Windows service component can be recreated by modifying the relevant registry settings and restarting it through the SCM.

<figure>

![A screenshot of a computer program](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-47.webp)

<figcaption>

Figure 3. Monitoring for the presence of EXE and DLL files

</figcaption>

</figure>

<figure>

![A computer screen shot of a program code](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-4-start-the-mlaware-via-scm.webp)

<figcaption>

Figure 4. Start the malware via SCM

</figcaption>

</figure>

### RDP monitoring

StilachiRAT monitors RDP sessions by capturing foreground window information and duplicating security tokens to impersonate users. This is particularly risky on RDP servers hosting administrative sessions as it could enable lateral movement within networks.

The malware obtains the current session and actively launches foreground windows as well as enumerates all other RDP sessions. For each identified session, it will access the Windows Explorer shell and duplicate its privileges or security token. The malware then gains capabilities to launch applications with these newly obtained privileges.

<figure>

![A screen shot of a computer program](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-5-enumerate-rdp-sessions.webp)

<figcaption>

Figure 5. Enumerate RDP sessions

</figcaption>

</figure>

<figure>

![A screen shot of a computer code](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-6-launch-process-another-user.webp)

<figcaption>

Figure 6. Launch process as another user

</figcaption>

</figure>

### Data collection

StilachiRAT collects a variety of user data, including software installation records and active applications. It monitors active GUI windows, their title bar text, and file location, and sends this information to the C2 server, potentially allowing attackers to track user behavior.

<figure>

![A screenshot of a computer](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/image-48.webp)

<figcaption>

Figure 7. Registry path for installed software

</figcaption>

</figure>

<figure>

![A computer code with colorful text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-8-read-title-app-window.webp)

<figcaption>

Figure 8. Read the title of an application window

</figcaption>

</figure>

### Clipboard monitoring

StilachiRAT has a functionality that is responsible for monitoring clipboard data. Specifically, the malware can periodically read the clipboard, extract text based on search expressions, and then exfiltrate this data. Clipboard monitoring is continuous, with targeted searches for sensitive information such as passwords, cryptocurrency keys, and potentially personal identifiers.

The list below includes the regular search expressions used to extract certain credentials. These are associated with the Tron Cryptocurrency blockchain that is popular in Asia, especially in China.

| **Credential** |  **Regular expression to extract credential**                                |
| --- | --- |
|  TRX Address |  \`bT\[0-9a-zA-Z\]{33}b\`                                      |
|  TRX Key     |  \`b(0x)?\[0-9a-fA-F\]{64}b\`                                  |
|  TRX Pass    |  \`^s\*b(\[0-9\]\*\[.\]\*\[a-wy-z\]\[a-z\]{2,}\[ t\]\*b){12}s\*(n$)\` |
|  TRX Pass    |  \`^s\*b(\[0-9\]\*\[.\]\*?\[a-wy-z\]\[a-z\]{2,}s\*b){12}s\*(n$)\` |

<figure>

![A screen shot of a computer code](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-9-access-clipboard-data.webp)

<figcaption>

Figure 9. Access clipboard data

</figcaption>

</figure>

<figure>

![A computer screen shot of a black background with white text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-10-access-clipboard.webp)

<figcaption>

Figure 10. Modify clipboard data

</figcaption>

</figure>

The same search expressions are then used to iterate files in the following locations:

- %USERPROFILE%Desktop

- %USERPROFILE%Recent

<figure>

![A screen shot of a computer code](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-11-access-users-files.webp)

<figcaption>

Figure 11. Access user’s files

</figcaption>

</figure>

### Anti-forensic measures

StilachiRAT displays anti-forensic behavior by clearing event logs and checking certain system conditions to evade detection. This includes looping checks for analysis tools and sandbox timers that prevent its full activation in virtual environments commonly used for malware analysis.

Additionally, Windows API calls are obfuscated in multiple ways and a custom algorithm is used to encode many text strings and values. This significantly slows down analysis time since extrapolating higher level logic and code design becomes a more complex effort.

The malware employs API-level obfuscation techniques to impede manual analysis, specifically by concealing its use of Windows APIs (e.g., RegOpenKey()). Instead of referencing API names directly, it encodes them as checksums that are resolved dynamically at runtime. While this is a common technique in malware, the authors have introduced additional layers of obfuscation.

Precomputed API checksums are stored in multiple lookup tables, each masked with an XOR value. During launch, the malware selects the appropriate table based on the hashed API name, applies the correct XOR mask to decode the value, and dynamically resolves the corresponding Windows API function. The resolved function pointer is then cached, but with an additional XOR mask applied, preventing straightforward memory scans from identifying API references.

<figure>

![A screen shot of a computer](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-12-example-function-calls-resolve-sleep-allocconsole.webp)

<figcaption>

Figure 12. Example of two function calls that resolve \*\*Sleep()\*\* and \*\*AllocConsole()\*\* Windows APIs

</figcaption>

</figure>

<figure>

![A computer screen shot of text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-13-function-initiates-api-resolution.webp)

<figcaption>

Figure 13. Function that initiates API resolution by identifying the correct lookup table for the checksum

</figcaption>

</figure>

### Commands launched from the C2 server

StilachiRAT can launch various commands received from the C2 server. These commands include system reboot, log clearing, credential theft, executing applications, and manipulating system windows. Additionally, it can suspend the system, modify Windows registry values, and enumerate open windows, indicating a versatile command set for both espionage and system manipulation. The C2 server’s command structure assigns specific numbers to what commands it will initiate. The following section presents details on the said commands.

#### 07 – Dialog box

Uses the Windows API function _ShowHTMLDialogEx()_ to display a dialog box with rendered HTML contents from a supplied URL.

<figure>

![A screen shot of a computer program](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-14-display-message-box.webp)

<figcaption>

Figure 14. Display a message box

</figcaption>

</figure>

#### 08 – Log clearing

Given an event log type, the relevant Windows APIs are used to open and then clear the log entries.

<figure>

![A screen shot of a computer](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-15-clear-event-logs.webp)

<figcaption>

Figure 15. Clear event logs

</figcaption>

</figure>

#### 09 – System reboot

Adjusts its own executing privileges to enable system shutdown and uses an undocumented Windows API to perform the action.

<figure>

![A computer screen shot of text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-16-shutdown-pc.webp)

<figcaption>

Figure 16. Shutdown the PC

</figcaption>

</figure>

#### 13 – Network sockets

Appears to contain capability to receive a network address from C2 server and establish a new outbound connection.

#### 14 – TCP incoming

Accepts an incoming network connection on the supplied TCP port.

#### 15 – Terminate

If there’s an open network connection, then close it and disable the Windows service controlling this process. This appears to be the self-removal (uninstall) command.

#### 16 – Initiate application

The malware creates a console window and initiates a command to launch the program provided by the C2 operator using the _WinExec()_ API.

<figure>

![A black background with white text](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/03/Figure-17-initiate-program.webp)

<figcaption>

Figure 17. Initiate a program

</figcaption>

</figure>

#### 19 – Enumerate Windows

Iterates all windows of the current desktop to look for a requested title bar text. This might allow the operator to access specific GUI applications and their contents, both onscreen and clipboard.

#### 26 – Suspend

Uses the _SetSuspendState()_ API to put the system into either a suspended (sleep) state or hibernation.

#### 30 – Chrome credentials

Launches the earlier mentioned functionality to steal Google Chrome passwords.

## Mitigations

Malware like StilachiRAT can be installed through various vectors. The following mitigations can help prevent this type of malware from infiltrating the system and reduce the attack surface:

- In some cases, RATs can masquerade as legitimate software or software updates. Always download software from the official website of the software developer or from reputable sources.

- Encourage users to use Microsoft Edge and other web browsers that support SmartScreen, which identifies and blocks malicious websites, including phishing sites, scam sites, and sites that host malware.

- Turn on Safe Links and Safe Attachments for Office 365. In organizations with Microsoft Defender for Office 365, Safe Links scanning protects your organization from malicious links that are used in phishing and other attacks. Specifically, Safe Links provides URL scanning and rewriting of inbound email messages during mail flow, and time-of-click verification of URLs and links in email messages, Microsoft Teams, and supported Office 365 apps. Safe Attachments provides an additional layer of protection for email attachments that have already been scanned by anti-malware protection in Exchange Online Protection (EOP).

- Enable network protection in Microsoft Defender for Endpoint to prevent applications or users from accessing malicious domains and other malicious content on the internet. You can audit network protection in a test environment to view which apps would be blocked before enabling network protection.

General hardening guidelines:

- Ensure that tamper protection is enabled in Microsoft Dender for Endpoint.

- Run endpoint detection and response in block mode so that Microsoft Defender for Endpoint can block malicious artifacts, even when your non-Microsoft antivirus does not detect the threat or when Microsoft Defender Antivirus is running in passive mode.

- Configure investigation and remediation in full automated mode to let Microsoft Defender for Endpoint take immediate action on alerts to resolve breaches, significantly reducing alert volume.

- Turn on Potentially unwanted applications (PUA) protection in block mode in Microsoft Defender Antivirus. PUA are a category of software that can cause your machine to run slowly, display unexpected ads, or install other software that might be unexpected or unapproved.

- Turn on cloud-delivered protection in Microsoft Defender Antivirus or the equivalent for your antivirus product to cover rapidly evolving attacker tools and techniques.

- Turn on Microsoft Defender Antivirus real-time protection.

## Microsoft Defender XDR detections

Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, apps to provide integrated protection against attacks like the threat discussed in this blog.

Customers with provisioned access can also use Microsoft Security Copilot in Microsoft Defender to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.

### Microsoft Defender Antivirus

Microsoft Defender Antivirus detects this threat as the following malware:

- TrojanSpy:Win64/Stilachi.A

### Microsoft Defender for Endpoint

The following alerts might indicate threat activity related to this threat. Note, however, that these alerts can be also triggered by unrelated threat activity.

- A process was injected with potentially malicious code

- Process hollowing detected

- Suspicious service launched

- Possible theft of passwords and other sensitive web browser information

## Microsoft Security Copilot

Security Copilot customers can use the standalone experience to create their own prompts or run the following pre-built promptbooks to automate incident response or investigation tasks related to this threat:

- Incident investigation

- Microsoft User analysis

- Threat actor profile

- Threat Intelligence 360 report based on MDTI article

- Vulnerability impact assessment

Note that some promptbooks require access to plugins for Microsoft products such as Microsoft Defender XDR or Microsoft Sentinel.

## Hunting queries

### Microsoft Defender XDR

Microsoft Defender XDR customers can run the following query to find related activity in their networks:

**Look for suspicious outbound network connections**

Monitor network traffic for malicious activity caused by remote access trojans by focusing on identifying unusual outbound connections, irregular port activity, and suspicious data exfiltration patterns that may indicate RAT presence.

Outbound ports associated with common data transfer protocols such as HTTP/HTTPS (port 80/443), SMB (port 445), and DNS (port 53) or less common ports like 16000 used for specific applications and services for network communications might indicate such activity.

```
let domains = dynamic(['domain1', 'domain2', 'domain3']);
DeviceNetworkEvents
| where RemotePort in (53, 443, 16000)
| where Protocol == "Tcp"
| where RemoteUrl has_any (domains)
| project Timestamp, DeviceName, RemoteIP, RemotePort, InitiatingProcessCommandLine, ActionType, DeviceId, LocalIP, RemoteUrl, InitiatingProcessFileName

```

**Look for signs of persistence**

The malware can be run both as a Windows Service or a standalone component. To identify persistence and suspicious services, monitor for the following event IDs:

- **Event ID 7045** – a new service was installed on the system. Monitor for suspicious services.

- **Event ID 7040** – start type of a service is changed (boot, on-request). Boot may be a vector for the RAT to persist during a system reboot. On request indicates that the process must request the SCM to start the service.

- Correlated with **Event ID 4697** – a service was installed on the system (Security log)

```
DeviceEvents
|where ActionType == “ServiceInstalled”
| project Timestamp, DeviceId,ActionType, FileName, FolderPath, InitiatingProcessCommandLine

```

**Look for anti-forensic behavior**

To identify potential event log clearing, monitor for the following event IDs:

- **Event ID 1102** (Security log)

- **Event ID 104** (System log)

### Microsoft Sentinel

Microsoft Sentinel customers can use the TI Mapping analytics (a series of analytics all prefixed with ‘TI map’) to automatically match the malicious domain/IP/Hash indicators mentioned in this blog post with data in their workspace. If the TI Map analytics are not currently deployed, customers can install the Threat Intelligence solution from the Microsoft Sentinel Content Hub to have the analytics rule deployed in their Sentinel workspace.

Additionally, Sentinel users can use the following query to detect when the security event log has been cleared, a potential indicator of an attempt to erase system evidence.

```
SecurityEvent
  | where EventID == 1102 and EventSourceName == "Microsoft-Windows-Eventlog"
  | summarize StartTimeUtc = min(TimeGenerated), EndTimeUtc = max(TimeGenerated), EventCount = count() by Computer, Account, EventID, Activity
  | extend HostName = tostring(split(Computer, ".")[0]), DomainIndex = toint(indexof(Computer, '.'))
  | extend HostNameDomain = iff(DomainIndex != -1, substring(Computer, DomainIndex + 1), Computer)
  | extend AccountName = tostring(split(Account, @'')[1]), AccountNTDomain = tostring(split(Account, @'')[0])

```

Sentinel users can also use the following query to detect service installations or modifications in service settings, which may indicate potential persistence mechanisms used by attackers.

```
Event 
  // 7045: A service was installed in the system
 //  7040: A service setting has been changed
  | where Source == "Service Control Manager" 
  | where EventID in ( '7045', '7040')
  | parse EventData with * 'ServiceName">' ServiceName "<" * 'ImagePath">' ImagePath "<" *
  | parse EventData with * 'AccountName">' AccountName "<" *
  | summarize StartTime = min(TimeGenerated), EndTime = max(TimeGenerated) by EventID, Computer, ServiceName, ImagePath, AccountName

```

## Indicators of compromise

| **Indicator** | **Type** | **Description** |
| --- | --- | --- |
| 394743dd67eb018b02e069e915f64417bc1cd8b33e139b92240a8cf45ce10fcb | SHA-256 | WWStartupCtrl64.dll |
| 194.195.89\[.\]47   | IP address | C2 |
| app.95560\[.\]cc   | Domain name | C2 |

## Learn more

For the latest security research from the Microsoft Threat Intelligence community, check out the Microsoft Threat Intelligence Blog: https://aka.ms/threatintelblog.

To get notified about new publications and to join discussions on social media, follow us on LinkedIn at https://www.linkedin.com/showcase/microsoft-threat-intelligence, and on X (formerly Twitter) at https://x.com/MsftSecIntel.

To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the Microsoft Threat Intelligence podcast: https://thecyberwire.com/podcasts/microsoft-threat-intelligence.

Microsoft is committed to delivering comprehensive customer experience through various Microsoft Offerings. Our approach goes beyond traditional support by focusing on detection, prevention, and in-depth mitigation to help customers quickly respond to security incidents and build resiliency. **Want to know how to Build a More Secure Tomorrow**? Check our Unified and Security eBook and visit https://aka.ms/Unified

_**Dmitriy Pletnev** and **Daria Pop**  
Microsoft Incident Response_

The post StilachiRAT analysis: From system reconnaissance to cryptocurrency theft appeared first on Microsoft Security Blog.

Go to Source
