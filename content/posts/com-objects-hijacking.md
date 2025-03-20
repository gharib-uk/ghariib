---
title: "COM Objects Hijacking"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

The COM Hijacking technique is often utilized by threat actors and various malware families to achieve both persistence and privilege escalation in target systems. It relies on manipulating Component Object Model (COM), exploiting the core architecture of Windows that enables communication between software components, by adding a new value on a specific registry key related to the COM object itself.

We studied the usage of this technique by different malware samples to pinpoint the most exploited COM objects in 2023.

# Abused COM Objects

We identified the most abused COM objects by samples using MITRE’s T1546.015 technique during sandbox execution. In addition to the most abused ones, we will also highlight other abused COM objects that we found interesting.

The chart below shows the distribution of how many samples abused different COM objects for persistence:

![](https://lh7-us.googleusercontent.com/h9F_1wEQn6e2aSnJXjER-y0oCTFnHMIuIlAPGbaXS7K1yDPWttnbG1eDp5wz8jWx6PEihyuhwPcLMdNA_JbgF6jRUybVK2FQTDOZFIWs3rkUSN4q6GTdrq-1mJb450-CKWKooEi1nfXMw5e-Vnhso_fPeNoka9TkBw1x5gKFxPdTNmCBqMkBRFnSZQRnciY)

You can find the most used COM / CLSIDs listed in the Appendix.

## Berbew

One of the main malware families we have observed abusing COM for persistence is Padodor/Berbew. This Trojan primarily focuses on stealing credentials and exfiltrating them to remote hosts controlled by attackers. The main COM objects abused by this family are as follows:

- {79ECA078-17FF-726B-E811-213280E5C831}
    
- {79FEACFF-FFCE-815E-A900-316290B5B738}
    
- {79FAA099-1BAE-816E-D711-115290CEE717}
    

The corresponding registry entries point to the malicious DLL. However, multiple samples of this family use a second registry key for persistence, which points to this previous CLSID we described, as in the following example :

![](https://lh7-us.googleusercontent.com/HOCWTX4YofORhFsq73bFupGrAYiKazOlk6-PFnfosNb2GGx5lMbCYa5cM9QWyKSEjvXPAReIPoV8d09uNZ2KHihlrb0Rojd1hRtadkT-zDluMTMKVzsD0U0lzAtzEn8VhW0wUaG3ZfHBAbP53ES1mJAJWIJg93MwvfxVmSBGRQsQTPX8vZnKE1OUmX1s-5I)

In this case, the registry key …CLSID{79ECA078-17FF-726B-E811-213280E5C831}InProcServer32(Default) points to the malicious DLL C:WindowsSysWow64Iimgdcia.dll. A second registry entry …Wow6432NodeMicrosoftWindowsCurrentVersionShellServiceObjectDelayLoadWeb Event Logger points to the previous CLSID {79ECA078-17FF-726B-E811-213280E5C831} which loads the malicious DLL.

The ShellServiceObjectDelayLoad registry entry (part of ShellServiceObjectDelayLoad), combined with the Web Event Logger subkey used here by Berbew, has frequently been utilized to initiate the loading of the genuine webcheck.dll. This DLL was tasked with monitoring websites within the Internet Explorer application.

The previously utilized CLSID by WebCheck registry key was {E6FB5E20-DE35-11CF-9C87-00AA005127ED} However, in certain instances today the CLSID {08165EA0-E946-11CF-9C87-00AA005127ED} is used. Both are responsible for loading the webcheck.dll DLL and are abused by malware samples.

## RATs

The CLSID {89565275-A714-4a43-912E-978B935EDCCC} seems to be extensively used by various RATs . This CLSID has primarily been associated with families like RemcosRAT and AsyncRAT in our observations. However, we've also encountered instances where BitRAT samples have used it. Researchers at Cisco Talos found this CLSID activity associated with the SugarGh0st RAT malware.

In the majority of cases, the DLL used for persistence with this CLSID is dynwrapx.dll. This DLL was found in the wild in a GitHub repository, currently unavailable, however the DLL originates from a project named DynamicWrapperX (first seen in VirusTotal in 2010). It executes shellcode to inject the RAT into a process.

A similar case is CLSID {26037A0E-7CBD-4FFF-9C63-56F2D0770214}. The associated DLL for persistence is dbggame.dll. First uploaded to VirusTotal in 2012, this DLL is deployed by various types of malware, including ransomware such as XiaoBa.

### RATs w/ vulnerabilities

To finish with RATs that use this technique, from late December 2023 to February 2024, there were various incidents linked to the CVE-2024-21412 vulnerability uncovered by the Trend Micro Zero Day Initiative team (ZDI). During these events, active campaigns were distributing the Darkme RAT. Throughout the infection process, a primary goal was to evade Microsoft Defender SmartScreen and introduce victims to the DarkMe malware.

The TrendMicro analysis highlights that the Darkme RAT sample utilizes the CLSID {74A94F46-4FC5-4426-857B-FCE9D9286279} to carry out the final load of the RAT. Yet, we've noted the utilization of other CLSIDs for persistence, including {D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4} in this sample.

Furthermore, to guarantee the DLL's execution, they generate a registry key employing Autorun keys. This key's objective is to initiate the CLSID using rundll32.exe and /sta parameter, which is used to load a COM object, in this case, the previous malicious COM object created.

```
EventID:13 
EventType:SetValue
Details:%windir%SysWOW64rundll32.exe /sta {D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4} "USB_Module"
TargetObject:HKUS-1-5-21-575823232-3065301323-1442773979-1000SoftwareMicrosoftWindowsCurrentVersionRunRunDllModule
```

## Why use one when you can use many?

Some samples (like this Sality one) use multiple CLSIDs:

- {EBEB87A6-E151-4054-AB45-A6E094C5334B}
    
- {57477331-126E-4FC8-B430-1C6143484AA9}
    
- {241D7F03-9232-4024-8373-149860BE27C0}
    
- {C07DB6A3-34FC-4084-BE2E-76BB9203B049}
    

The sample drops two different DLLs during execution, three of the registry keys point to one of them, the remaining one to the other. The sample also turns off the Windows firewall and UAC to carry out additional actions while infecting the system.

![](https://lh7-us.googleusercontent.com/CgMyQFCyp8QdRzn7oDZEjeNzyRRKAGdauSqFbB8dbgPigeeiSmfM5fBd6fbKbw9yC7jduHNgsNZsWfuzQa6qfOJehvBFm2jtNPikgGzs4tfKdTMepE57mQmmawpP09rQpGoj_jE-v4pLeEOdN3e5JaGC_12FohlKpf2dFVauablqF4ThWHBqqBiJV8w389s)

The Allaple worm family deploys multiple COM objects pointing to the malicious DLL during execution, like in this example:

![](https://lh7-us.googleusercontent.com/srr4NcbbgMs00yPDx3sji6jBvO4cZYTFrmiUTpr-RE98-A4nV0qPIAzGfP4Iu6onRyiLJknQ21ryWH2y0YQbU5vICwzVsc7DPpWF8UFpnNzyfKZ2UYGfTgvarleHGOENsAuC1-cKR16vsrxG3M7IfVmfJ4NjrxInM8HTDGLkVDQWCODlUcgOVOAtDT2yatU)

## Adware

Citrio, an adware web browser designed by Catalina Group, uses in its more recent versions a COM object for persistence with CLSID {F4CBF20B-F634-4095-B64A-2EBCDD9E560E}. It drops several harmful DLLs, one masquerades as Google Update (goopdate.dll), also observed as psuser.dll, that possesses the capability to establish services on the system along using a COM object for persistence.

![](https://lh7-us.googleusercontent.com/vmzA_B8lAkuJCNYSz0IWihUOUXtD0tCy2jsGuebDJESCgQCsCe_MMWjCyMTBCAHKSEAiGweELdegS0IuSqPlBg8I6qT8cbLx-8feKUgosuYUMDqrxjJk-RKEPlkxY9KlklQW5BiT_RDfPPWZkcVJRnM-miOK3lEo5zPHinbCb7liR96DKOWiyC4w6PzFovk)

# Common folders used to store the payloads

Most malicious DLLs we saw so far are typically stored in the C:Users<user>AppDataRoaming directory. It's also common to create subfolders within this directory, the most frequently found include:

- qmacro
    
- mymacro
    
- MacroCommerce
    
- Plugin
    
- Microsoft
    

In addition to these, we also found the following folders being frequently used to hide malicious DLLs:

- The C:WindowsSysWow64 is a folder found in 64-bit versions of Windows, containing legitimate 32-bit system files and libraries, and is oftenly used to conceal malicious DLLs. Its prevalence makes it an attractive hiding place, complicating detection efforts. However, permissions are required to create files in it.
    
- The C:Program Files (x86) folder is another legitimate directory used to store malicious COM hijacking payloads. Similar to AppDataRoaming, in this case we have observed that the malicious DLLs are stored under specific subfolders, such as “Google”, “Mozilla Firefox”, “Microsoft”, “Common Files” or “Internet Download Manager”.
    
- C:Users<user>AppDataLocal is another folder used for storing these payloads, including the “Temp”, “Microsoft” and “Google” subfolders.
    

# Detection

In order to detect unusual modifications to registry COM objects, there are a couple of crowdsourced Sigma rules to identify this behavior.

- Potential Persistence Via COM Hijacking From Suspicious Locations (high risk level). Detects suspicious when creating a registry key using COM objects.
    

  

- Potential Persistence Via COM Search Order Hijacking (medium risk level). Similar to the previous rule, it also filters out paths associated with legitimate behaviors.
    

These rules will detect uncommon registry modifications related to COM objects. You can use the following queries to retrieve samples triggered by the previous rules, respectively: VTI query for sigma1 and VTI query for sigma2.

You can also identify this behavior using Livehunt rules that target the creation of registry keys utilized for this purpose, for instance with the vt.behaviour.registry\_keys\_set modifier.

```
import "vt"

rule CLSID_COM_Hijacking:  {
  meta:
    target_entity = "file"
    hash = "a19472bd5dd89a6bd725c94c89469f12cdbfee3b0f19035a07374a005b57b5e0"
    author = "@Joseliyo_Jstnk"
    mitre_technique = "T1546.015"
    mitre_tactic = "TA0003"

  condition:
    vt.metadata.new_file and vt.metadata.analysis_stats.malicious >= 5 and 
    for any vt_behaviour_registry_keys_set in vt.behaviour.registry_keys_set: (
      vt_behaviour_registry_keys_set.key matches /\CLSID\{[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}}\InProcServer32\(Default)/
    )  
}
```

The rule above might generate some noise, so we suggest considering polishing it by excluding certain common families like Berbew, which as mentioned, heavily relies on this technique:

```
and not 
    (
        for any engine, signature in vt.metadata.signatures : (  
        signature icontains "berbew"  
        )  
    )
```

You can also use the paths listed in Appendix to identify suspicious samples using them.

A final idea is including interesting existing Sigma rules into our Livehunt. Given that these rules already cover the targeted registry keys, we don’t need to use vt.behaviour.registry\_keys\_set in our condition.

```
import "vt"

rule CLSID_COM_Hijacking:  {
  meta:
    target_entity = "file"
    hash = "a19472bd5dd89a6bd725c94c89469f12cdbfee3b0f19035a07374a005b57b5e0"
    author = "@Joseliyo_Jstnk"
    sigma_authors = "Maxime Thiebaut (@0xThiebaut), oscd.community, Cédric Hien"
    mitre_technique = "T1546.015"
    mitre_tactic = "TA0003"

  condition:
    vt.metadata.new_file and vt.metadata.analysis_stats.malicious >= 5 and 
    for any vt_behaviour_sigma_analysis_results in vt.behaviour.sigma_analysis_results: (
      vt_behaviour_sigma_analysis_results.rule_id == "7f5d257abc981b5eddb52d4a9a02fb66201226935cf3d39177c8a81c3a3e8dd4"
    )
}
```

# Wrapping up

The T1546.015 - Event Triggered Execution: Component Object Model Hijacking is just one of several techniques employed for persistence. Leveraging COM objects for this task is frequently straightforward for threat actors. The analysis of how malware abuses this technique helps us get a better understanding in how to identify different families and develop protection methods. Although the technique is not the most popular for persistence (that would be T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder), it is widely abused by many malware families.

Identifying some of the most abused CLSIDs can help us generate detection rules that identify possible malware abuses in our infrastructure. It can also serve as a good guide for prevalence in order to detect any anomalies for new suspicious activity.

The use of VirusTotal sandbox reports provides a very powerful tool to translate TTPs into actionable queries and monitoring. In this example we used it to better understand how attackers use COM objects, but could be used for any techniques employed by different threat actors.

We hope you join our fan club of Sigma and VirusTotal, and as always we are happy to hear your feedback.

# APPENDIX

## Abused CLSIDs

Next, you'll find a list of the main CLSIDs described in the blog, along with a chart to show which ones were used the most.

![](https://lh7-us.googleusercontent.com/Dn_TKOQbzGbN6WR3mwhz34h0pZAPNXhCVrEfwKkmfkk5MeYZ2LoRp3a0IwUgoEBsOau9ewHpjxuXMAoL9BsrY82z3iXijDpi65ZM3tvn1Cvo1OIfIp2xeurNAjRH3ugmBaraveflDx2MB-3kCIXMonrU94mM5mEHZmD7rttkjubPlLV4KgWnTn65DSChYyI)

|   CLSID - COM Objects   |
| --- |
|   79FAA099-1BAE-816E-D711-115290CEE717   |
|   EBEB87A6-E151-4054-AB45-A6E094C5334B   |
|   241D7F03-9232-4024-8373-149860BE27C0   |
|   C07DB6A3-34FC-4084-BE2E-76BB9203B049   |
|   79ECA078-17FF-726B-E811-213280E5C831   |
|   22C6C651-F6EA-46BE-BC83-54E83314C67F   |
|   F4CBF20B-F634-4095-B64A-2EBCDD9E560E   |
|   57477331-126E-4FC8-B430-1C6143484AA9   |
|   C73F6F30-97A0-4AD1-A08F-540D4E9BC7B9   |
|   89565275-A714-4a43-912E-978B935EDCCC   |
|   26037A0E-7CBD-4FFF-9C63-56F2D0770214   |
|   16426152-126E-4FC8-B430-1C6143484AA9   |
|   33414471-126E-4FC8-B430-1C6143484AA9   |
|   23716116-126E-4FC8-B430-1C6143484AA9   |
|   D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4   |
|   79FEACFF-FFCE-815E-A900-316290B5B738   |
|   74A94F46-4FC5-4426-857B-FCE9D9286279   |

## Common paths

Below you will find a list with some of the most common paths used during the creation of the COM objects for persistence. The table contains the 'parent' paths as well, while the chart includes only the 'subpaths'.

![](https://lh7-us.googleusercontent.com/Apy2CDZzU_kZqC5EwbVrWhJSZ8VcmyvGVwrxTn5PXiOdFez2r6Tm1JOJHzSt-7J6nGWg8qYbR9qRF5yFN2Ua6M8Id1dLg9-weNnfmrNFafRxJayw0oy7SNZG9y1gxSvjYsg3CuMMRyUrf5ecDe1YSsC-GwavMoumQa2_ruWN6NI9dKC91VHmNAiz0_d-keY)

|   Common paths used during COM object persistence   |
| --- |
|   C:Users<user>AppDataRoaming   |
|   C:Users<user>AppDataRoamingqmacro   |
|   C:Users<user>AppDataRoamingmymacro   |
|   C:Users<user>AppDataRoamingMacroCommerce   |
|   C:Users<user>AppDataRoamingPlugin   |
|   C:Users<user>AppDataRoamingMicrosoft   |
|   C:WindowsSysWow64   |
|   C:Program Files (x86)   |
|   C:Program Files (x86)Google   |
|   C:Program Files (x86)Mozilla Firefox   |
|   C:Program Files (x86)Microsoft   |
|   C:Program Files (x86)Common Files   |
|   C:Program Files (x86)Internet Download Manager   |
|   C:Users<user>AppDataLocal   |
|   C:Users<user>AppDataLocalTemp   |
|   C:Users<user>AppDataLocalMicrosoft   |
|   C:Users<user>AppDataLocalGoogle   |
|   C:WindowsTemp   |

The COM Hijacking technique is often utilized by threat actors and various malware families to achieve both persistence and privilege escalation in target systems. It relies on manipulating Component Object Model (COM), exploiting the core architecture of Windows that enables communication between software components, by adding a new value on a specific registry key related to the COM object itself.

We studied the usage of this technique by different malware samples to pinpoint the most exploited COM objects in 2023.

# Abused COM Objects

We identified the most abused COM objects by samples using MITRE’s T1546.015 technique during sandbox execution. In addition to the most abused ones, we will also highlight other abused COM objects that we found interesting.

The chart below shows the distribution of how many samples abused different COM objects for persistence:

![](https://lh7-us.googleusercontent.com/h9F_1wEQn6e2aSnJXjER-y0oCTFnHMIuIlAPGbaXS7K1yDPWttnbG1eDp5wz8jWx6PEihyuhwPcLMdNA_JbgF6jRUybVK2FQTDOZFIWs3rkUSN4q6GTdrq-1mJb450-CKWKooEi1nfXMw5e-Vnhso_fPeNoka9TkBw1x5gKFxPdTNmCBqMkBRFnSZQRnciY)

You can find the most used COM / CLSIDs listed in the Appendix.

## Berbew

One of the main malware families we have observed abusing COM for persistence is Padodor/Berbew. This Trojan primarily focuses on stealing credentials and exfiltrating them to remote hosts controlled by attackers. The main COM objects abused by this family are as follows:

- {79ECA078-17FF-726B-E811-213280E5C831}
    
- {79FEACFF-FFCE-815E-A900-316290B5B738}
    
- {79FAA099-1BAE-816E-D711-115290CEE717}
    

The corresponding registry entries point to the malicious DLL. However, multiple samples of this family use a second registry key for persistence, which points to this previous CLSID we described, as in the following example :

![](https://lh7-us.googleusercontent.com/HOCWTX4YofORhFsq73bFupGrAYiKazOlk6-PFnfosNb2GGx5lMbCYa5cM9QWyKSEjvXPAReIPoV8d09uNZ2KHihlrb0Rojd1hRtadkT-zDluMTMKVzsD0U0lzAtzEn8VhW0wUaG3ZfHBAbP53ES1mJAJWIJg93MwvfxVmSBGRQsQTPX8vZnKE1OUmX1s-5I)

In this case, the registry key …CLSID{79ECA078-17FF-726B-E811-213280E5C831}InProcServer32(Default) points to the malicious DLL C:WindowsSysWow64Iimgdcia.dll. A second registry entry …Wow6432NodeMicrosoftWindowsCurrentVersionShellServiceObjectDelayLoadWeb Event Logger points to the previous CLSID {79ECA078-17FF-726B-E811-213280E5C831} which loads the malicious DLL.

The ShellServiceObjectDelayLoad registry entry (part of ShellServiceObjectDelayLoad), combined with the Web Event Logger subkey used here by Berbew, has frequently been utilized to initiate the loading of the genuine webcheck.dll. This DLL was tasked with monitoring websites within the Internet Explorer application.

The previously utilized CLSID by WebCheck registry key was {E6FB5E20-DE35-11CF-9C87-00AA005127ED} However, in certain instances today the CLSID {08165EA0-E946-11CF-9C87-00AA005127ED} is used. Both are responsible for loading the webcheck.dll DLL and are abused by malware samples.

## RATs

The CLSID {89565275-A714-4a43-912E-978B935EDCCC} seems to be extensively used by various RATs . This CLSID has primarily been associated with families like RemcosRAT and AsyncRAT in our observations. However, we've also encountered instances where BitRAT samples have used it. Researchers at Cisco Talos found this CLSID activity associated with the SugarGh0st RAT malware.

In the majority of cases, the DLL used for persistence with this CLSID is dynwrapx.dll. This DLL was found in the wild in a GitHub repository, currently unavailable, however the DLL originates from a project named DynamicWrapperX (first seen in VirusTotal in 2010). It executes shellcode to inject the RAT into a process.

A similar case is CLSID {26037A0E-7CBD-4FFF-9C63-56F2D0770214}. The associated DLL for persistence is dbggame.dll. First uploaded to VirusTotal in 2012, this DLL is deployed by various types of malware, including ransomware such as XiaoBa.

### RATs w/ vulnerabilities

To finish with RATs that use this technique, from late December 2023 to February 2024, there were various incidents linked to the CVE-2024-21412 vulnerability uncovered by the Trend Micro Zero Day Initiative team (ZDI). During these events, active campaigns were distributing the Darkme RAT. Throughout the infection process, a primary goal was to evade Microsoft Defender SmartScreen and introduce victims to the DarkMe malware.

The TrendMicro analysis highlights that the Darkme RAT sample utilizes the CLSID {74A94F46-4FC5-4426-857B-FCE9D9286279} to carry out the final load of the RAT. Yet, we've noted the utilization of other CLSIDs for persistence, including {D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4} in this sample.

Furthermore, to guarantee the DLL's execution, they generate a registry key employing Autorun keys. This key's objective is to initiate the CLSID using rundll32.exe and /sta parameter, which is used to load a COM object, in this case, the previous malicious COM object created.

```
EventID:13 
EventType:SetValue
Details:%windir%SysWOW64rundll32.exe /sta {D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4} "USB_Module"
TargetObject:HKUS-1-5-21-575823232-3065301323-1442773979-1000SoftwareMicrosoftWindowsCurrentVersionRunRunDllModule
```

## Why use one when you can use many?

Some samples (like this Sality one) use multiple CLSIDs:

- {EBEB87A6-E151-4054-AB45-A6E094C5334B}
    
- {57477331-126E-4FC8-B430-1C6143484AA9}
    
- {241D7F03-9232-4024-8373-149860BE27C0}
    
- {C07DB6A3-34FC-4084-BE2E-76BB9203B049}
    

The sample drops two different DLLs during execution, three of the registry keys point to one of them, the remaining one to the other. The sample also turns off the Windows firewall and UAC to carry out additional actions while infecting the system.

![](https://lh7-us.googleusercontent.com/CgMyQFCyp8QdRzn7oDZEjeNzyRRKAGdauSqFbB8dbgPigeeiSmfM5fBd6fbKbw9yC7jduHNgsNZsWfuzQa6qfOJehvBFm2jtNPikgGzs4tfKdTMepE57mQmmawpP09rQpGoj_jE-v4pLeEOdN3e5JaGC_12FohlKpf2dFVauablqF4ThWHBqqBiJV8w389s)

The Allaple worm family deploys multiple COM objects pointing to the malicious DLL during execution, like in this example:

![](https://lh7-us.googleusercontent.com/srr4NcbbgMs00yPDx3sji6jBvO4cZYTFrmiUTpr-RE98-A4nV0qPIAzGfP4Iu6onRyiLJknQ21ryWH2y0YQbU5vICwzVsc7DPpWF8UFpnNzyfKZ2UYGfTgvarleHGOENsAuC1-cKR16vsrxG3M7IfVmfJ4NjrxInM8HTDGLkVDQWCODlUcgOVOAtDT2yatU)

## Adware

Citrio, an adware web browser designed by Catalina Group, uses in its more recent versions a COM object for persistence with CLSID {F4CBF20B-F634-4095-B64A-2EBCDD9E560E}. It drops several harmful DLLs, one masquerades as Google Update (goopdate.dll), also observed as psuser.dll, that possesses the capability to establish services on the system along using a COM object for persistence.

![](https://lh7-us.googleusercontent.com/vmzA_B8lAkuJCNYSz0IWihUOUXtD0tCy2jsGuebDJESCgQCsCe_MMWjCyMTBCAHKSEAiGweELdegS0IuSqPlBg8I6qT8cbLx-8feKUgosuYUMDqrxjJk-RKEPlkxY9KlklQW5BiT_RDfPPWZkcVJRnM-miOK3lEo5zPHinbCb7liR96DKOWiyC4w6PzFovk)

# Common folders used to store the payloads

Most malicious DLLs we saw so far are typically stored in the C:Users<user>AppDataRoaming directory. It's also common to create subfolders within this directory, the most frequently found include:

- qmacro
    
- mymacro
    
- MacroCommerce
    
- Plugin
    
- Microsoft
    

In addition to these, we also found the following folders being frequently used to hide malicious DLLs:

- The C:WindowsSysWow64 is a folder found in 64-bit versions of Windows, containing legitimate 32-bit system files and libraries, and is oftenly used to conceal malicious DLLs. Its prevalence makes it an attractive hiding place, complicating detection efforts. However, permissions are required to create files in it.
    
- The C:Program Files (x86) folder is another legitimate directory used to store malicious COM hijacking payloads. Similar to AppDataRoaming, in this case we have observed that the malicious DLLs are stored under specific subfolders, such as “Google”, “Mozilla Firefox”, “Microsoft”, “Common Files” or “Internet Download Manager”.
    
- C:Users<user>AppDataLocal is another folder used for storing these payloads, including the “Temp”, “Microsoft” and “Google” subfolders.
    

# Detection

In order to detect unusual modifications to registry COM objects, there are a couple of crowdsourced Sigma rules to identify this behavior.

- Potential Persistence Via COM Hijacking From Suspicious Locations (high risk level). Detects suspicious when creating a registry key using COM objects.
    

  

- Potential Persistence Via COM Search Order Hijacking (medium risk level). Similar to the previous rule, it also filters out paths associated with legitimate behaviors.
    

These rules will detect uncommon registry modifications related to COM objects. You can use the following queries to retrieve samples triggered by the previous rules, respectively: VTI query for sigma1 and VTI query for sigma2.

You can also identify this behavior using Livehunt rules that target the creation of registry keys utilized for this purpose, for instance with the vt.behaviour.registry\_keys\_set modifier.

```
import "vt"

rule CLSID_COM_Hijacking:  {
  meta:
    target_entity = "file"
    hash = "a19472bd5dd89a6bd725c94c89469f12cdbfee3b0f19035a07374a005b57b5e0"
    author = "@Joseliyo_Jstnk"
    mitre_technique = "T1546.015"
    mitre_tactic = "TA0003"

  condition:
    vt.metadata.new_file and vt.metadata.analysis_stats.malicious >= 5 and 
    for any vt_behaviour_registry_keys_set in vt.behaviour.registry_keys_set: (
      vt_behaviour_registry_keys_set.key matches /\CLSID\{[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}}\InProcServer32\(Default)/
    )  
}
```

The rule above might generate some noise, so we suggest considering polishing it by excluding certain common families like Berbew, which as mentioned, heavily relies on this technique:

```
and not 
    (
        for any engine, signature in vt.metadata.signatures : (  
        signature icontains "berbew"  
        )  
    )
```

You can also use the paths listed in Appendix to identify suspicious samples using them.

A final idea is including interesting existing Sigma rules into our Livehunt. Given that these rules already cover the targeted registry keys, we don’t need to use vt.behaviour.registry\_keys\_set in our condition.

```
import "vt"

rule CLSID_COM_Hijacking:  {
  meta:
    target_entity = "file"
    hash = "a19472bd5dd89a6bd725c94c89469f12cdbfee3b0f19035a07374a005b57b5e0"
    author = "@Joseliyo_Jstnk"
    sigma_authors = "Maxime Thiebaut (@0xThiebaut), oscd.community, Cédric Hien"
    mitre_technique = "T1546.015"
    mitre_tactic = "TA0003"

  condition:
    vt.metadata.new_file and vt.metadata.analysis_stats.malicious >= 5 and 
    for any vt_behaviour_sigma_analysis_results in vt.behaviour.sigma_analysis_results: (
      vt_behaviour_sigma_analysis_results.rule_id == "7f5d257abc981b5eddb52d4a9a02fb66201226935cf3d39177c8a81c3a3e8dd4"
    )
}
```

# Wrapping up

The T1546.015 - Event Triggered Execution: Component Object Model Hijacking is just one of several techniques employed for persistence. Leveraging COM objects for this task is frequently straightforward for threat actors. The analysis of how malware abuses this technique helps us get a better understanding in how to identify different families and develop protection methods. Although the technique is not the most popular for persistence (that would be T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder), it is widely abused by many malware families.

Identifying some of the most abused CLSIDs can help us generate detection rules that identify possible malware abuses in our infrastructure. It can also serve as a good guide for prevalence in order to detect any anomalies for new suspicious activity.

The use of VirusTotal sandbox reports provides a very powerful tool to translate TTPs into actionable queries and monitoring. In this example we used it to better understand how attackers use COM objects, but could be used for any techniques employed by different threat actors.

We hope you join our fan club of Sigma and VirusTotal, and as always we are happy to hear your feedback.

# APPENDIX

## Abused CLSIDs

Next, you'll find a list of the main CLSIDs described in the blog, along with a chart to show which ones were used the most.

![](https://lh7-us.googleusercontent.com/Dn_TKOQbzGbN6WR3mwhz34h0pZAPNXhCVrEfwKkmfkk5MeYZ2LoRp3a0IwUgoEBsOau9ewHpjxuXMAoL9BsrY82z3iXijDpi65ZM3tvn1Cvo1OIfIp2xeurNAjRH3ugmBaraveflDx2MB-3kCIXMonrU94mM5mEHZmD7rttkjubPlLV4KgWnTn65DSChYyI)

|   CLSID - COM Objects   |
| --- |
|   79FAA099-1BAE-816E-D711-115290CEE717   |
|   EBEB87A6-E151-4054-AB45-A6E094C5334B   |
|   241D7F03-9232-4024-8373-149860BE27C0   |
|   C07DB6A3-34FC-4084-BE2E-76BB9203B049   |
|   79ECA078-17FF-726B-E811-213280E5C831   |
|   22C6C651-F6EA-46BE-BC83-54E83314C67F   |
|   F4CBF20B-F634-4095-B64A-2EBCDD9E560E   |
|   57477331-126E-4FC8-B430-1C6143484AA9   |
|   C73F6F30-97A0-4AD1-A08F-540D4E9BC7B9   |
|   89565275-A714-4a43-912E-978B935EDCCC   |
|   26037A0E-7CBD-4FFF-9C63-56F2D0770214   |
|   16426152-126E-4FC8-B430-1C6143484AA9   |
|   33414471-126E-4FC8-B430-1C6143484AA9   |
|   23716116-126E-4FC8-B430-1C6143484AA9   |
|   D4D4D7B7-1774-4FE5-ABA8-4CC0B99452B4   |
|   79FEACFF-FFCE-815E-A900-316290B5B738   |
|   74A94F46-4FC5-4426-857B-FCE9D9286279   |

## Common paths

Below you will find a list with some of the most common paths used during the creation of the COM objects for persistence. The table contains the 'parent' paths as well, while the chart includes only the 'subpaths'.

![](https://lh7-us.googleusercontent.com/Apy2CDZzU_kZqC5EwbVrWhJSZ8VcmyvGVwrxTn5PXiOdFez2r6Tm1JOJHzSt-7J6nGWg8qYbR9qRF5yFN2Ua6M8Id1dLg9-weNnfmrNFafRxJayw0oy7SNZG9y1gxSvjYsg3CuMMRyUrf5ecDe1YSsC-GwavMoumQa2_ruWN6NI9dKC91VHmNAiz0_d-keY)

|   Common paths used during COM object persistence   |
| --- |
|   C:Users<user>AppDataRoaming   |
|   C:Users<user>AppDataRoamingqmacro   |
|   C:Users<user>AppDataRoamingmymacro   |
|   C:Users<user>AppDataRoamingMacroCommerce   |
|   C:Users<user>AppDataRoamingPlugin   |
|   C:Users<user>AppDataRoamingMicrosoft   |
|   C:WindowsSysWow64   |
|   C:Program Files (x86)   |
|   C:Program Files (x86)Google   |
|   C:Program Files (x86)Mozilla Firefox   |
|   C:Program Files (x86)Microsoft   |
|   C:Program Files (x86)Common Files   |
|   C:Program Files (x86)Internet Download Manager   |
|   C:Users<user>AppDataLocal   |
|   C:Users<user>AppDataLocalTemp   |
|   C:Users<user>AppDataLocalMicrosoft   |
|   C:Users<user>AppDataLocalGoogle   |
|   C:WindowsTemp   |

Go to Source
