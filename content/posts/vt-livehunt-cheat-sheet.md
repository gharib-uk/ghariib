---
title: "VT Livehunt Cheat Sheet"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

Today we are happy to announce the release of our “**Livehunt Cheat Sheet**”, a guide to help you quickly implement monitoring rules in Livehunt. You can find the **PDF** **version** here.

VirusTotal Livehunt is a service that continuously scans all incoming indicators and notifies you when any of them matches your rules. Livehunt not only monitors files, but also domains, URLs, and IP addresses. In this post we detail a few practical examples along with useful tips.

  

## VT Module

This YARA module was created for VT Hunting services to provide all available context data, which is structured in two main sections: metadata and behaviour (sandbox execution). You can find more information about the VT module here.

  

## Using metadata information in Livehunt rules

Analysts can create rules to hunt based on the metadata information that VirusTotal gathers and processes. We are referring to hunting files by characteristics (type, size, signatures), reputation (antivirus detections, submission patterns), and even contextual details (file names, tags, etc).

For example, this would allow analysts to detect files of a certain type that were submitted several times from a given country, and that more than 5 antiviruses have flagged as malicious. Here you have some detailed examples:

  

**Example 1: Malicious DOCX files that use macros:**

This example defines a rule focused on detecting potentially malicious DOCX files with macros.

First we check the file type with **vt.metadata.file\_type == vt.FileType.DOCX**.

The next condition (**vt.metadata.analysis\_stats.malicious > 5**) matches files flagged as malicious by more than 5 antivirus engines in VirusTotal. This filters out most of the benign files, and can be adjusted according to the investigation.

Finally, it loops all tags given by security tools in the analysis pipeline and searches for the tag “macros”: **for any tag in vt.metadata.tags:(tag == “macros”)**

  

import "vt"  
  
rule malicious\_docx\_macros {  
  meta:  
    description = "Detect malicious documents using macros."  
  condition:  
    vt.metadata.file\_type == vt.FileType.DOCX and  
    vt.metadata.analysis\_stats.malicious > 5 and  
    for any tag in vt.metadata.tags:(tag == “macros”)  
}

  

**Example 2: Possible LNK execution through CommandLineArguments Exif metadata field:**

The following rule is designed to identify **PowerShell** execution by manipulating metadata fields of **.lnk files**. This technique is frequently utilized by malware to avoid detection and initiate attacks. For example, this malicious .lnk file report shows the target command line which will execute PowerShell code to download the “powercat.ps1” script.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHHeEqT_A4aaf3iGQ0cu2hyphenhyphenNXuLcBPtXDaz9zP8hjUFCJtTkTUdEF7kGwTMbpo1T0eFv6H0XflDHV6aSykUwm4n-DXOt3QwgPhYdQn1lndB413X6sETWtaZRBFSTVTpVm3nlB-2VoDA_MjLTMw5DwFWtSpmDorVBhgMzgoPvIFL_wsjU5qPcwSMK7wutg/w640-h240/lnk_metadata.png)

  

In this case, the condition checks for the “powershell” string within two **EXIF metadata** fields usually used to store the powershell command line - “CommandLineArguments” and “RelativePath”:

**vt.metadata.exiftool\["CommandLineArguments"\] icontains "powershell"  
vt.metadata.exiftool\["RelativePath"\] icontains "powershell"**

  

import "vt"  
  
rule LNK\_metadata\_execution\_powershell {  
  meta:  
    description = "Detect possible LNK execution through CommandLineArguments Exif metadata field"  
  condition:  

    vt.metadata.exiftool\["CommandLineArguments"\] icontains "powershell" or  
    vt.metadata.exiftool\["RelativePath"\] icontains "powershell"  
}  

  

## Using behaviour information in Livehunt rules

Dynamic analysis can bring great value on top of static one. In VirusTotal, we run executable files through multiple sandboxes and its output is normalized into a common format, which can be leveraged through the “vt” module.

  

**Example 3: Malicious files that use persistence using VBScript:**

The following rule identifies persistence under the **"RunOnce"** registry key using **VBS files**. This key allows programs to automatically execute once when a user logs in, often exploited by malware to maintain presence on a system.

For this rule, we iterate over **vt.behaviour.registry\_keys\_set** looking for **"\\CurrentVersion\\RunOnce\\"** with a value that ends with **".vbs"**.

  

import "vt"  
  
rule persistence\_runonce\_vbs {  
  meta:  
    description = "Detect persistence by establishing a VBS file in the runonce key"  
  condition:  

    for any registry\_key in   
     vt.behaviour.registry\_keys\_set: (registry\_key.key icontains  
     "\\CurrentVersion\\RunOnce\\") and (registry\_key.value endswith ".vbs")  
}  

  

**Example 4: Suspicious shell scripts in "profile.d" folder:**

This rule detects activity involving the creation or modification of **shell scripts** (.sh files) within the **"/etc/profile.d/"** directory on Linux systems. This directory is often used to host scripts that automatically execute during user login, making it a common target for malware seeking persistence or automatic execution.

First condition iterates through files dropped (**vt.behaviour.files\_dropped**) during execution as observed in VirusTotal's behavioral analysis and checks if the dropped file's path contains "/etc/profile.d/" and ends with ".sh" in order to match shell scripts.

The second condition is very similar but checks the file path for files written (**vt.behaviour.files\_written**) during detonation.

  

import "vt"  
  
rule profile\_folder\_shell\_script {  
  meta:  
    description = "Detects shell script creation in "profile.d" path."  
  condition:  
    for any dropped in vt.behaviour.files\_dropped :(  
     dropped.path contains"/etc/profile.d/"  
     and dropped.path endswith ".sh"  
    )  
    or  
    for any file\_path in vt.behaviour.files\_written :(  
     file\_path contains"/etc/profile.d/"  
     and file\_path endswith ".sh"  
    )  
}  

  

## Wrapping up

The VirusTotal (vt) YARA module brings you unprecedented flexibility in crafting Livehunt rules combining traditional file content analysis with rich metadata information and behavioral patterns from dynamic analysis.

Our “VT Intelligence Cheat Sheet” provides a quick guide to implement some of the most used techniques. If you have any suggestions or want to share feedback please feel free to reach out here.

  

Happy Hunting!

Today we are happy to announce the release of our “**Livehunt Cheat Sheet**”, a guide to help you quickly implement monitoring rules in Livehunt. You can find the **PDF** **version** here.

VirusTotal Livehunt is a service that continuously scans all incoming indicators and notifies you when any of them matches your rules. Livehunt not only monitors files, but also domains, URLs, and IP addresses. In this post we detail a few practical examples along with useful tips.

  

## VT Module

This YARA module was created for VT Hunting services to provide all available context data, which is structured in two main sections: metadata and behaviour (sandbox execution). You can find more information about the VT module here.

  

## Using metadata information in Livehunt rules

Analysts can create rules to hunt based on the metadata information that VirusTotal gathers and processes. We are referring to hunting files by characteristics (type, size, signatures), reputation (antivirus detections, submission patterns), and even contextual details (file names, tags, etc).

For example, this would allow analysts to detect files of a certain type that were submitted several times from a given country, and that more than 5 antiviruses have flagged as malicious. Here you have some detailed examples:

  

**Example 1: Malicious DOCX files that use macros:**

This example defines a rule focused on detecting potentially malicious DOCX files with macros.

First we check the file type with **vt.metadata.file\_type == vt.FileType.DOCX**.

The next condition (**vt.metadata.analysis\_stats.malicious > 5**) matches files flagged as malicious by more than 5 antivirus engines in VirusTotal. This filters out most of the benign files, and can be adjusted according to the investigation.

Finally, it loops all tags given by security tools in the analysis pipeline and searches for the tag “macros”: **for any tag in vt.metadata.tags:(tag == “macros”)**

  

import "vt"  
  
rule malicious\_docx\_macros {  
  meta:  
    description = "Detect malicious documents using macros."  
  condition:  
    vt.metadata.file\_type == vt.FileType.DOCX and  
    vt.metadata.analysis\_stats.malicious > 5 and  
    for any tag in vt.metadata.tags:(tag == “macros”)  
}

  

**Example 2: Possible LNK execution through CommandLineArguments Exif metadata field:**

The following rule is designed to identify **PowerShell** execution by manipulating metadata fields of **.lnk files**. This technique is frequently utilized by malware to avoid detection and initiate attacks. For example, this malicious .lnk file report shows the target command line which will execute PowerShell code to download the “powercat.ps1” script.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHHeEqT_A4aaf3iGQ0cu2hyphenhyphenNXuLcBPtXDaz9zP8hjUFCJtTkTUdEF7kGwTMbpo1T0eFv6H0XflDHV6aSykUwm4n-DXOt3QwgPhYdQn1lndB413X6sETWtaZRBFSTVTpVm3nlB-2VoDA_MjLTMw5DwFWtSpmDorVBhgMzgoPvIFL_wsjU5qPcwSMK7wutg/w640-h240/lnk_metadata.png)

  

In this case, the condition checks for the “powershell” string within two **EXIF metadata** fields usually used to store the powershell command line - “CommandLineArguments” and “RelativePath”:

**vt.metadata.exiftool\["CommandLineArguments"\] icontains "powershell"  
vt.metadata.exiftool\["RelativePath"\] icontains "powershell"**

  

import "vt"  
  
rule LNK\_metadata\_execution\_powershell {  
  meta:  
    description = "Detect possible LNK execution through CommandLineArguments Exif metadata field"  
  condition:  

    vt.metadata.exiftool\["CommandLineArguments"\] icontains "powershell" or  
    vt.metadata.exiftool\["RelativePath"\] icontains "powershell"  
}  

  

## Using behaviour information in Livehunt rules

Dynamic analysis can bring great value on top of static one. In VirusTotal, we run executable files through multiple sandboxes and its output is normalized into a common format, which can be leveraged through the “vt” module.

  

**Example 3: Malicious files that use persistence using VBScript:**

The following rule identifies persistence under the **"RunOnce"** registry key using **VBS files**. This key allows programs to automatically execute once when a user logs in, often exploited by malware to maintain presence on a system.

For this rule, we iterate over **vt.behaviour.registry\_keys\_set** looking for **"\\CurrentVersion\\RunOnce\\"** with a value that ends with **".vbs"**.

  

import "vt"  
  
rule persistence\_runonce\_vbs {  
  meta:  
    description = "Detect persistence by establishing a VBS file in the runonce key"  
  condition:  

    for any registry\_key in   
     vt.behaviour.registry\_keys\_set: (registry\_key.key icontains  
     "\\CurrentVersion\\RunOnce\\") and (registry\_key.value endswith ".vbs")  
}  

  

**Example 4: Suspicious shell scripts in "profile.d" folder:**

This rule detects activity involving the creation or modification of **shell scripts** (.sh files) within the **"/etc/profile.d/"** directory on Linux systems. This directory is often used to host scripts that automatically execute during user login, making it a common target for malware seeking persistence or automatic execution.

First condition iterates through files dropped (**vt.behaviour.files\_dropped**) during execution as observed in VirusTotal's behavioral analysis and checks if the dropped file's path contains "/etc/profile.d/" and ends with ".sh" in order to match shell scripts.

The second condition is very similar but checks the file path for files written (**vt.behaviour.files\_written**) during detonation.

  

import "vt"  
  
rule profile\_folder\_shell\_script {  
  meta:  
    description = "Detects shell script creation in "profile.d" path."  
  condition:  
    for any dropped in vt.behaviour.files\_dropped :(  
     dropped.path contains"/etc/profile.d/"  
     and dropped.path endswith ".sh"  
    )  
    or  
    for any file\_path in vt.behaviour.files\_written :(  
     file\_path contains"/etc/profile.d/"  
     and file\_path endswith ".sh"  
    )  
}  

  

## Wrapping up

The VirusTotal (vt) YARA module brings you unprecedented flexibility in crafting Livehunt rules combining traditional file content analysis with rich metadata information and behavioral patterns from dynamic analysis.

Our “VT Intelligence Cheat Sheet” provides a quick guide to implement some of the most used techniques. If you have any suggestions or want to share feedback please feel free to reach out here.

  

Happy Hunting!

Go to Source
