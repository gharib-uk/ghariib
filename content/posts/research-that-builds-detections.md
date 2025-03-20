---
title: "Research that builds detections"
date: 2025-01-10
categories: 
  - "cybersecurity"
  - "general"
  - "security"
  - "virus"
---

**Note:** You can view the full content of the blog here.

## Introduction

Detection engineering is becoming increasingly important in surfacing new malicious activity. Threat actors might take advantage of previously unknown malware families - but a successful detection of certain methodologies or artifacts can help expose the entire infection chain.

In previous blog posts, we announced the integration of Sigma rules for macOS and Linux into VirusTotal, as well as ways in which Sigma rules can be converted to YARA to take advantage of VirusTotal Livehunt capabilities. In this post, we will show different approaches to hunt for interesting samples and derive new Sigma detection opportunities based on their behavior.

## Tell me what role you have and I'll tell you how you use VirusTotal

VirusTotal is a really useful tool that can be used in many different ways. We have seen how people from SOCs and Incident Response teams use it (in fact, we have our VirusTotal Academy videos for SOCs and IRs teams), and we have also shown how those who hunt for threats or analyze those threats can use it too.

But there's another really cool way to use VirusTotal - for people who build detections and those who are doing research. We want to show everyone how we use VirusTotal in our work. Hopefully, this will be helpful and also give people ideas for new ways to use it themselves.

To explain our process, we used examples of Lummac and VenomRAT samples that we found in recent campaigns. These caught our attention due to some behaviors that had not been identified by public detection rules in the community. For that reason we have created two Sigma rules to share with the community, but if you want to get all the details about how we identified it and started our research, go to our Google Threat Intelligence community blog.

## Our approach

As detection engineers, it is important to look for techniques that can be in use by multiple threat actors - as this makes tracking malicious activity more efficient. Prior to creating those detections, it is best to check existing research and rule collections, such as the Sigma rules repository. This can save time and effort, as well as provide insight into previously observed samples that can be further researched.

A different approach would be to instead look for malicious files that are not detected by existing Sigma rules, since they can uncover novel methodologies and provide new opportunities for detection creation.

One approach is to hunt for files that are flagged by at least five different AV vendors, were recently uploaded within the last month, have sandbox execution (in order to view their behavior), and which have not triggered any Crowdsourced Sigma rules.

```
p:5+ have:behavior fs:30d+ not have:sigma

```

This initial query can be adapted to incorporate additional filters that the researcher may find relevant. These could include modifiers to identify for example, the presence of the PowerShell process in the list of executed processes (behavior\_created\_processes:powershell.exe), filtering results to only include documents (type:document), or identifying communication with services like Pastebin (behavior\_network:pastebin.com).

Another way to go is to look at files that have been flagged by at least five AV’s and were tested in either Zenbox or CAPE. These sandboxes often have great logs produced by Sysmon, which are really useful for figuring out how to spot these threats. Again, we'd want to focus on files uploaded in the last month that haven't triggered any Sigma rules. This gives us a good starting point for building new detection rules.

```
p:5+ (sandbox_name:"CAPE Sandbox" or sandbox_name:"Zenbox") fs:30d+ not have:sigma

```

Lastly, another idea is to look for files that have not triggered many high severity detections from the Sigma Crowdsourced rules, as these can be more evasive. Specifically, we will look for samples with zero critical, high or medium alerts - and no more than two low severity ones.

```
p:5+ have:behavior fs:30d+ sigma_critical:0 sigma_high:0 sigma_medium:0 sigma_low:2-

```

With these queries, we can start investigating some samples that may be interesting to create detection rules.

## Our detections for the community

Our approach helps us identify behaviors that seem interesting and worth focusing on. In our blog, where we explain this approach in detail, we highlighted two campaigns linked to Lummac and VenomRAT that exhibited interesting activity. Because of this, we decided to share the Sigma rules we developed for these campaigns. Both rules have been published in Sigma's official repository for the community.

#### Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer

Sigma rule on GitHub: https://github.com/SigmaHQ/sigma/blob/master/rules-emerging-threats/2024/Malware/Lummac-Stealer/proc\_creation\_win\_malware\_lummac\_more\_vbc.yml

```
title: Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer
  id: 19b3806e-46f2-4b4c-9337-e3d8653245ea
  status: experimental
  description: Detects the execution of more.com and vbc.exe in the process tree. This behaviors was observed by a set of samples related to Lummac Stealer. The Lummac payload is injected into the vbc.exe process.
  references:
      - https://www.virustotal.com/gui/file/14d886517fff2cc8955844b252c985ab59f2f95b2849002778f03a8f07eb8aef
      - https://strontic.github.io/xcyclopedia/library/more.com-EDB3046610020EE614B5B81B0439895E.html
      - https://strontic.github.io/xcyclopedia/library/vbc.exe-A731372E6F6978CE25617AE01B143351.html
  author: Joseliyo Sanchez, @Joseliyo_Jstnk
  date: 2024-11-14
  tags:
      - attack.defense-evasion
      - attack.t1055
  logsource:
      category: process_creation
      product: windows
  detection:
      # VT Query: behaviour_processes:"C:\Windows\SysWOW64\more.com" behaviour_processes:"C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe"
      selection_parent:
          ParentImage|endswith: 'more.com'
      selection_child:
          - Image|endswith: 'vbc.exe'
          - OriginalFileName: 'vbc.exe'
      condition: all of selection_*
  falsepositives:
      - Unknown
  level: high
```

#### Sysmon event for: Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer

```
{
  "System": {
    "Provider": {
      "Guid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}",
      "Name": "Microsoft-Windows-Sysmon"
    },
    "EventID": 1,
    "Version": 5,
    "Level": 4,
    "Task": 1,
    "Opcode": 0,
    "Keywords": "0x8000000000000000",
    "TimeCreated": {
      "SystemTime": "2024-11-26T16:23:05.132539500Z"
    },
    "EventRecordID": 692861,
    "Correlation": {},
    "Execution": {
      "ProcessID": 2396,
      "ThreadID": 3116
    },
    "Channel": "Microsoft-Windows-Sysmon/Operational",
    "Computer": "DESKTOP-B0T93D6",
    "Security": {
      "UserID": "S-1-5-18"
    }
  },
  "EventData": {
    "RuleName": "-",
    "UtcTime": "2024-11-26 16:23:05.064",
    "ProcessGuid": "{C784477D-F5E9-6745-6006-000000003F00}",
    "ProcessId": 4184,
    "Image": "C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe",
    "FileVersion": "14.8.3761.0",
    "Description": "Visual Basic Command Line Compiler",
    "Product": "Microsoft® .NET Framework",
    "Company": "Microsoft Corporation",
    "OriginalFileName": "vbc.exe",
    "CommandLine": "C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe",
    "CurrentDirectory": "C:\Users\george\AppData\Roaming\comlocal\RUYCLAXYVMFJ\",
    "User": "DESKTOP-B0T93D6\george",
    "LogonGuid": "{C784477D-9D9B-66FF-6E87-050000000000}",
    "LogonId": "0x5876e",
    "TerminalSessionId": 1,
    "IntegrityLevel": "High",
    "Hashes": {
      "SHA1": "61F4D9A9EE38DBC72E840B3624520CF31A3A8653",
      "MD5": "FCCB961AE76D9E600A558D2D0225ED43",
      "SHA256": "466876F453563A272ADB5D568670ECA98D805E7ECAA5A2E18C92B6D3C947DF93",
      "IMPHASH": "1460E2E6D7F8ECA4240B7C78FA619D15"
    },
    "ParentProcessGuid": "{C784477D-F5D4-6745-5E06-000000003F00}",
    "ParentProcessId": 6572,
    "ParentImage": "C:\Windows\SysWOW64\more.com",
    "ParentCommandLine": "C:\Windows\SysWOW64\more.com",
    "ParentUser": "DESKTOP-B0T93D6\george"
  }
} 
```

#### File Creation Related To RAT Clients

Sigma rule on GitHub: https://github.com/SigmaHQ/sigma/blob/fad4742996c55d8d4663e611f84877a2b741dc46/rules-emerging-threats/2024/Malware/Generic/file\_event\_win\_malware\_generic\_creation\_configuration\_rats.yml

```
title: File Creation Related To RAT Clients
  id: 2f3039c8-e8fe-43a9-b5cf-dcd424a2522d
  status: experimental
  description: File .conf created related to VenomRAT, AsyncRAT and Lummac samples observed in the wild.
  references:
      - https://www.virustotal.com/gui/file/c9f9f193409217f73cc976ad078c6f8bf65d3aabcf5fad3e5a47536d47aa6761
      - https://www.virustotal.com/gui/file/e96a0c1bc5f720d7f0a53f72e5bb424163c943c24a437b1065957a79f5872675
  author: Joseliyo Sanchez, @Joseliyo_Jstnk
  date: 2024-11-15
  tags:
      - attack.execution
  logsource:
      category: file_event
      product: windows
  detection:
      # VT Query: behaviour_files:"\AppData\Roaming\DataLogs\DataLogs.conf"
      # VT Query: behaviour_files:"DataLogs.conf" or behaviour_files:"hvnc.conf" or behaviour_files:"dcrat.conf"
      selection_required:
          TargetFilename|contains: 'AppDataRoaming'
      selection_variants:
          TargetFilename|endswith:
              - 'datalogs.conf'
              - 'hvnc.conf'
              - 'dcrat.conf'
          TargetFilename|contains:
              - 'mydata'
              - 'datalogs'
              - 'hvnc'
              - 'dcrat'
      condition: all of selection_*
  falsepositives:
      - Legitimate software creating a file with the same name
  level: high

```

#### Sysmon event for: File Creation Related To RAT Clients

```
{
  "System": {
    "Provider": {
      "Guid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}",
      "Name": "Microsoft-Windows-Sysmon"
    },
    "EventID": 11,
    "Version": 2,
    "Level": 4,
    "Task": 11,
    "Opcode": 0,
    "Keywords": "0x8000000000000000",
    "TimeCreated": {
      "SystemTime": "2024-12-02T00:52:23.072811600Z"
    },
    "EventRecordID": 1555690,
    "Correlation": {},
    "Execution": {
      "ProcessID": 2624,
      "ThreadID": 3112
    },
    "Channel": "Microsoft-Windows-Sysmon/Operational",
    "Computer": "DESKTOP-B0T93D6",
    "Security": {
      "UserID": "S-1-5-18"
    }
  },
  "EventData": {
    "RuleName": "-",
    "UtcTime": "2024-12-02 00:52:23.059",
    "ProcessGuid": "{C784477D-04C6-674D-5C06-000000004B00}",
    "ProcessId": 7592,
    "Image": "C:\Users\george\Desktop\ezzz.exe",
    "TargetFilename": "C:\Users\george\AppData\Roaming\MyData\DataLogs.conf",
    "CreationUtcTime": "2024-12-02 00:52:23.059",
    "User": "DESKTOP-B0T93D6\george"
  }
```

# Wrapping up

Detection engineering teams can proactively create new detections by hunting for samples that are being distributed and uploaded to our platform. Applying our approach can benefit in the development of detection on the latest behaviors that do not currently have developed detection mechanisms. This could potentially help organizations be proactive in creating detections based on threat hunting missions.

The Sigma rules created to detect Lummac activity have been used during threat hunting missions to identify new samples of this family in VirusTotal. Another use is translating them into the language of the SIEM or EDR available in the infrastructure, as they could help identify potential behaviors related to Lummac samples observed in late 2024. After passing quality controls and being published on Sigma's public GitHub, they have been integrated for use in VirusTotal, delivering the expected results. You can use them in the following way:

Lummac Stealer Activity - Execution Of More.com And Vbc.exe

```
sigma_rule:a1021d4086a92fd3782417a54fa5c5141d1e75c8afc9e73dc6e71ef9e1ae2e9c

```

File Creation Related To RAT Clients

```
sigma_rule:8f179585d5c1249ab1ef8cec45a16d112a53f91d143aa2b0b6713602b1d19252

```

We hope you found this blog interesting and useful, and as always we are happy to hear your feedback.

**Note:** You can view the full content of the blog here.

## Introduction

Detection engineering is becoming increasingly important in surfacing new malicious activity. Threat actors might take advantage of previously unknown malware families - but a successful detection of certain methodologies or artifacts can help expose the entire infection chain.

In previous blog posts, we announced the integration of Sigma rules for macOS and Linux into VirusTotal, as well as ways in which Sigma rules can be converted to YARA to take advantage of VirusTotal Livehunt capabilities. In this post, we will show different approaches to hunt for interesting samples and derive new Sigma detection opportunities based on their behavior.

## Tell me what role you have and I'll tell you how you use VirusTotal

VirusTotal is a really useful tool that can be used in many different ways. We have seen how people from SOCs and Incident Response teams use it (in fact, we have our VirusTotal Academy videos for SOCs and IRs teams), and we have also shown how those who hunt for threats or analyze those threats can use it too.

But there's another really cool way to use VirusTotal - for people who build detections and those who are doing research. We want to show everyone how we use VirusTotal in our work. Hopefully, this will be helpful and also give people ideas for new ways to use it themselves.

To explain our process, we used examples of Lummac and VenomRAT samples that we found in recent campaigns. These caught our attention due to some behaviors that had not been identified by public detection rules in the community. For that reason we have created two Sigma rules to share with the community, but if you want to get all the details about how we identified it and started our research, go to our Google Threat Intelligence community blog.

## Our approach

As detection engineers, it is important to look for techniques that can be in use by multiple threat actors - as this makes tracking malicious activity more efficient. Prior to creating those detections, it is best to check existing research and rule collections, such as the Sigma rules repository. This can save time and effort, as well as provide insight into previously observed samples that can be further researched.

A different approach would be to instead look for malicious files that are not detected by existing Sigma rules, since they can uncover novel methodologies and provide new opportunities for detection creation.

One approach is to hunt for files that are flagged by at least five different AV vendors, were recently uploaded within the last month, have sandbox execution (in order to view their behavior), and which have not triggered any Crowdsourced Sigma rules.

```
p:5+ have:behavior fs:30d+ not have:sigma

```

This initial query can be adapted to incorporate additional filters that the researcher may find relevant. These could include modifiers to identify for example, the presence of the PowerShell process in the list of executed processes (behavior\_created\_processes:powershell.exe), filtering results to only include documents (type:document), or identifying communication with services like Pastebin (behavior\_network:pastebin.com).

Another way to go is to look at files that have been flagged by at least five AV’s and were tested in either Zenbox or CAPE. These sandboxes often have great logs produced by Sysmon, which are really useful for figuring out how to spot these threats. Again, we'd want to focus on files uploaded in the last month that haven't triggered any Sigma rules. This gives us a good starting point for building new detection rules.

```
p:5+ (sandbox_name:"CAPE Sandbox" or sandbox_name:"Zenbox") fs:30d+ not have:sigma

```

Lastly, another idea is to look for files that have not triggered many high severity detections from the Sigma Crowdsourced rules, as these can be more evasive. Specifically, we will look for samples with zero critical, high or medium alerts - and no more than two low severity ones.

```
p:5+ have:behavior fs:30d+ sigma_critical:0 sigma_high:0 sigma_medium:0 sigma_low:2-

```

With these queries, we can start investigating some samples that may be interesting to create detection rules.

## Our detections for the community

Our approach helps us identify behaviors that seem interesting and worth focusing on. In our blog, where we explain this approach in detail, we highlighted two campaigns linked to Lummac and VenomRAT that exhibited interesting activity. Because of this, we decided to share the Sigma rules we developed for these campaigns. Both rules have been published in Sigma's official repository for the community.

#### Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer

Sigma rule on GitHub: https://github.com/SigmaHQ/sigma/blob/master/rules-emerging-threats/2024/Malware/Lummac-Stealer/proc\_creation\_win\_malware\_lummac\_more\_vbc.yml

```
title: Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer
  id: 19b3806e-46f2-4b4c-9337-e3d8653245ea
  status: experimental
  description: Detects the execution of more.com and vbc.exe in the process tree. This behaviors was observed by a set of samples related to Lummac Stealer. The Lummac payload is injected into the vbc.exe process.
  references:
      - https://www.virustotal.com/gui/file/14d886517fff2cc8955844b252c985ab59f2f95b2849002778f03a8f07eb8aef
      - https://strontic.github.io/xcyclopedia/library/more.com-EDB3046610020EE614B5B81B0439895E.html
      - https://strontic.github.io/xcyclopedia/library/vbc.exe-A731372E6F6978CE25617AE01B143351.html
  author: Joseliyo Sanchez, @Joseliyo_Jstnk
  date: 2024-11-14
  tags:
      - attack.defense-evasion
      - attack.t1055
  logsource:
      category: process_creation
      product: windows
  detection:
      # VT Query: behaviour_processes:"C:\Windows\SysWOW64\more.com" behaviour_processes:"C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe"
      selection_parent:
          ParentImage|endswith: 'more.com'
      selection_child:
          - Image|endswith: 'vbc.exe'
          - OriginalFileName: 'vbc.exe'
      condition: all of selection_*
  falsepositives:
      - Unknown
  level: high
```

#### Sysmon event for: Detect The Execution Of More.com And Vbc.exe Related to Lummac Stealer

```
{
  "System": {
    "Provider": {
      "Guid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}",
      "Name": "Microsoft-Windows-Sysmon"
    },
    "EventID": 1,
    "Version": 5,
    "Level": 4,
    "Task": 1,
    "Opcode": 0,
    "Keywords": "0x8000000000000000",
    "TimeCreated": {
      "SystemTime": "2024-11-26T16:23:05.132539500Z"
    },
    "EventRecordID": 692861,
    "Correlation": {},
    "Execution": {
      "ProcessID": 2396,
      "ThreadID": 3116
    },
    "Channel": "Microsoft-Windows-Sysmon/Operational",
    "Computer": "DESKTOP-B0T93D6",
    "Security": {
      "UserID": "S-1-5-18"
    }
  },
  "EventData": {
    "RuleName": "-",
    "UtcTime": "2024-11-26 16:23:05.064",
    "ProcessGuid": "{C784477D-F5E9-6745-6006-000000003F00}",
    "ProcessId": 4184,
    "Image": "C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe",
    "FileVersion": "14.8.3761.0",
    "Description": "Visual Basic Command Line Compiler",
    "Product": "Microsoft® .NET Framework",
    "Company": "Microsoft Corporation",
    "OriginalFileName": "vbc.exe",
    "CommandLine": "C:\Windows\Microsoft.NET\Framework\v4.0.30319\vbc.exe",
    "CurrentDirectory": "C:\Users\george\AppData\Roaming\comlocal\RUYCLAXYVMFJ\",
    "User": "DESKTOP-B0T93D6\george",
    "LogonGuid": "{C784477D-9D9B-66FF-6E87-050000000000}",
    "LogonId": "0x5876e",
    "TerminalSessionId": 1,
    "IntegrityLevel": "High",
    "Hashes": {
      "SHA1": "61F4D9A9EE38DBC72E840B3624520CF31A3A8653",
      "MD5": "FCCB961AE76D9E600A558D2D0225ED43",
      "SHA256": "466876F453563A272ADB5D568670ECA98D805E7ECAA5A2E18C92B6D3C947DF93",
      "IMPHASH": "1460E2E6D7F8ECA4240B7C78FA619D15"
    },
    "ParentProcessGuid": "{C784477D-F5D4-6745-5E06-000000003F00}",
    "ParentProcessId": 6572,
    "ParentImage": "C:\Windows\SysWOW64\more.com",
    "ParentCommandLine": "C:\Windows\SysWOW64\more.com",
    "ParentUser": "DESKTOP-B0T93D6\george"
  }
} 
```

#### File Creation Related To RAT Clients

Sigma rule on GitHub: https://github.com/SigmaHQ/sigma/blob/fad4742996c55d8d4663e611f84877a2b741dc46/rules-emerging-threats/2024/Malware/Generic/file\_event\_win\_malware\_generic\_creation\_configuration\_rats.yml

```
title: File Creation Related To RAT Clients
  id: 2f3039c8-e8fe-43a9-b5cf-dcd424a2522d
  status: experimental
  description: File .conf created related to VenomRAT, AsyncRAT and Lummac samples observed in the wild.
  references:
      - https://www.virustotal.com/gui/file/c9f9f193409217f73cc976ad078c6f8bf65d3aabcf5fad3e5a47536d47aa6761
      - https://www.virustotal.com/gui/file/e96a0c1bc5f720d7f0a53f72e5bb424163c943c24a437b1065957a79f5872675
  author: Joseliyo Sanchez, @Joseliyo_Jstnk
  date: 2024-11-15
  tags:
      - attack.execution
  logsource:
      category: file_event
      product: windows
  detection:
      # VT Query: behaviour_files:"\AppData\Roaming\DataLogs\DataLogs.conf"
      # VT Query: behaviour_files:"DataLogs.conf" or behaviour_files:"hvnc.conf" or behaviour_files:"dcrat.conf"
      selection_required:
          TargetFilename|contains: 'AppDataRoaming'
      selection_variants:
          TargetFilename|endswith:
              - 'datalogs.conf'
              - 'hvnc.conf'
              - 'dcrat.conf'
          TargetFilename|contains:
              - 'mydata'
              - 'datalogs'
              - 'hvnc'
              - 'dcrat'
      condition: all of selection_*
  falsepositives:
      - Legitimate software creating a file with the same name
  level: high

```

#### Sysmon event for: File Creation Related To RAT Clients

```
{
  "System": {
    "Provider": {
      "Guid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}",
      "Name": "Microsoft-Windows-Sysmon"
    },
    "EventID": 11,
    "Version": 2,
    "Level": 4,
    "Task": 11,
    "Opcode": 0,
    "Keywords": "0x8000000000000000",
    "TimeCreated": {
      "SystemTime": "2024-12-02T00:52:23.072811600Z"
    },
    "EventRecordID": 1555690,
    "Correlation": {},
    "Execution": {
      "ProcessID": 2624,
      "ThreadID": 3112
    },
    "Channel": "Microsoft-Windows-Sysmon/Operational",
    "Computer": "DESKTOP-B0T93D6",
    "Security": {
      "UserID": "S-1-5-18"
    }
  },
  "EventData": {
    "RuleName": "-",
    "UtcTime": "2024-12-02 00:52:23.059",
    "ProcessGuid": "{C784477D-04C6-674D-5C06-000000004B00}",
    "ProcessId": 7592,
    "Image": "C:\Users\george\Desktop\ezzz.exe",
    "TargetFilename": "C:\Users\george\AppData\Roaming\MyData\DataLogs.conf",
    "CreationUtcTime": "2024-12-02 00:52:23.059",
    "User": "DESKTOP-B0T93D6\george"
  }
```

# Wrapping up

Detection engineering teams can proactively create new detections by hunting for samples that are being distributed and uploaded to our platform. Applying our approach can benefit in the development of detection on the latest behaviors that do not currently have developed detection mechanisms. This could potentially help organizations be proactive in creating detections based on threat hunting missions.

The Sigma rules created to detect Lummac activity have been used during threat hunting missions to identify new samples of this family in VirusTotal. Another use is translating them into the language of the SIEM or EDR available in the infrastructure, as they could help identify potential behaviors related to Lummac samples observed in late 2024. After passing quality controls and being published on Sigma's public GitHub, they have been integrated for use in VirusTotal, delivering the expected results. You can use them in the following way:

Lummac Stealer Activity - Execution Of More.com And Vbc.exe

```
sigma_rule:a1021d4086a92fd3782417a54fa5c5141d1e75c8afc9e73dc6e71ef9e1ae2e9c

```

File Creation Related To RAT Clients

```
sigma_rule:8f179585d5c1249ab1ef8cec45a16d112a53f91d143aa2b0b6713602b1d19252

```

We hope you found this blog interesting and useful, and as always we are happy to hear your feedback.

Go to Source
