---
title: "Know your enemies: An approach for CTI teams"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "virus"
---

VirusTotal’s **Threat Landscape** can be a valuable source of operational and tactical threat intelligence for CTI teams, for instance helping us find the latest **malware trends** used by a given Threat Actor to adjust our **intelligence-led security posture** accordingly. In this post, we will play the role of a CTI analyst working for a Singaporean financial institution.

  

As a first step, we search for threat actors that traditionally both targeted the **financial industry** and **Singaporean companies**.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhOYfPOF1sfQMsCFRPMBClNxlkMUuRqDUJu7BHziJtU6HkPOIO30_gk1vvbq8xFAk3xzheI-Qg3nhq4xQx8jGXtxTDIpRbibFtr4UE6S5VTRL2HUno3HTNOiKOiM4ZkciKairpUmBrB8jJPniPIWQHEH-RL08KLcUjFGNrGPOglM3-0wjr1vSySxvxkPHM/s1379/kyep1.png)

  

“**TA505**” and “**APT41**” both match these requirements. For the moment let’s focus on TA505, which seems more active at the moment.

  

## Understanding (TA505):

The **Threat Actor** card provides details on the actor, which seems to target organizations in the financial, healthcare, retail, and hospitality sectors across Europe, Asia Pacific region, Canada, India and the United States.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjLPvh_Z021Xb4CKQgwRFRX-0TTY-tTHqhtTUhRm0wjmY2p5WLI7yHpRgu6y5mS7adKxf6L1M4NESJpH1q3NuBeUFzPKenCLYQamRxMKN9FpZ7fO2lhlCr6386YZCCgIohG_v6j9JrWQ_ghO7kgkpETQVbKyR1VLqfdww3nImmbf2NyBAxUS3qBKdjBHRQ/s16000/kyep2.png)

  

According to the description TA505 seems related to **Dridex banking trojan** and **Locky ransomware** activity.

  

In VirusTotal we can find **two categories for TTPs**:  
\- The First are TTPs directly ingested from **MISP** and **MITRE**.  
\- The second (called Toolkit TTPs) shows TTPs obtained from **sandbox analysis of the IOCs** related to a particular actor.

  

In this case, for TA505 we can find the following **Toolkit TTPs**:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsXor4ZlNpvzrA3SWkbEpawRbYIKZElisjfG5-pC4Ywfp_Kx9R8Uw3cZpaafbQ6L0R_bIiYKVA3IsJMudc9VEY3pEOtoJOakEgA-Xag_627pS6uyN1UfdfJmv3lD-0Ka20YbdflEPxkzs1JPQNYsgXdKs85Hogk6ET8NLeL5VOpJC1ZEfG4LaZyqXC8Qg/s16000/kyep3.png)

  

The **T1486** tactic **(‘Data Encrypted technique for Impact')** seems potentially related to the use of **ransomware**, such as **Locky**, by this actor. This seems like a good point for us to retrieve some fresh data and understand this actor’s recent activity. For instance, the following query provide **fresh samples** from the actor (samples submitted after January 1st, 2024) that use data encryption, and tagged as ransom by AVs:

  

attack\_technique:T1486 threat\_actor:TA505 ls:2024-01-01+ engines:ransom

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIQeMWoIypq_2ootb1FVClD_5_9S0nlIexg50yzpY3n3LRgO8Ftki0R_y9alhKe4BEApxBErdPmDyTZZzoEhjlydfHGvPOgLbqeLp8mvKVLmsl5C6AY-E2t3UK-D_Jue33gBLtGzg5bEhyo-eKip2G4nGpYLzNlZh43kcS4mHus8m6MuD6_QSiaDpUzMQ/s16000/kyep4.png)

  

Multiple of the returned samples belong to the “locky” Collection **tagged as ‘locky’**, which contains **510 files** at the moment.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMADkBylU3Ua5_WIyPUatBioi5GrWhyQbiGRGXiyKOoJvdIHERuvpquxnCVMjOGXuCPp-JZq-K6i3vxqie5Z6RlfvfYFL9fscnqPjlAzPSHLQmvqfUgWDgg3mGXHfWAqjcyJO-WC0t8WaEEGoFEFv0NpFbs7ycoWeOncv7aWTjNvgkFQDGWRQQcHeIMEU/s16000/kyep5.png)

  

The **Telemetry tab** provides information about **submissions** and **lookups**, which helps us understand malware family’s distribution and timeframes of operations.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDWIpq2eU-Fmk0Twg67amSaq_GYrt4iQY8TLkmVcj2Yw8XARxapIqQBybRYmBllJXbkQj27zsiKPwVdXioxlWzajs9m7RxrCY-mmD6XqMp4TI1WCwRHmt6V43QLeXFBS2MfmeU_bSm7GUZbtXdOHsBArdFzSl4OpcvCl00ZR6nXC4UF7peVm9jsZv1sDM/s16000/kyep6.png)

  

## Tailoring defenses:

In addition, the Collection’s **Rules** panel provides details on crowdsourced **Yara**, **sigma** and **IDS rules** that match different indicators files in this collection.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2-cDcNE2XuSnFVnC5uC8aTD7SMr7leJvBpMBYG3iFopbx3QZqc15N8c1sSKqEjl_tIwnUSAvzhtuCJ7fJjtnrrGxuFITgUFffSMjqIBU3UF-C0wgJ5nHKGM3S-y9n9Lgsrnt9p0x8qaAlymMNmSANW2gkBrQ5ZRqui3EKobK6AUq6YR8QLY8wnTiFLhg/s16000/kyep7.png)

  

In this case, the “**win\_locky\_auto**” yara rule matches almost all the files in this collection (505/510). This could help to enhance detection capabilities for this threat.

  

Collection’s **commonalities** refer to characteristics, behaviors, or technical attributes shared by a set of indicators, which helps to identify patterns. Let’s use this to create a new “**Livehunt rule**” to track this activity in the future. We will use only recent samples, we can filter them in the IOCS tab (“fs:180d+”):

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjp6zsOoykB_WZIyIsTdIadYqnQkbECnAjwMN0o2blQv3q01Cbo5vMiKYeTtnR-_T1opy5lxAyvkARZBCadq_z_lJQIu__8STHtuCpYHTB11owibRlERVtxznts-A3WGpHevh36Ec1BLxHtG9oJVdxuwYB30kcmdOZ74HnuMeyb0lYr1C2rzIUDJ8o2SI/s16000/kyep8.png)

  

Based on commonalities results, some **useful information** to create the livehunt rule may include:

  

**Metadata**:

- **File type**: EXE and DLL formats.  
    (vt.metadata.file\_type == vt.FileType.PE\_DLL or vt.metadata.file\_type == vt.FileType.PE\_EXE)
- **File size**: Less than 1Mb.  
    (vt.metadata.file\_size < 1000000)
- **Main icon**: Custom and specific icon.  
    (vt.metadata.main\_icon.dhash == "52c244c9a7a3998b")
- **Imphash**: Hash value calculated from PE's import table, that could be matching some locky samples.  
    (vt.metadata.imphash == "31553623c43827d554ad9e1b7dfa6a5a")

  

**Behavior**:

- **Sandbox attack techniques**: Detect T1486 Encryption Data technique.  
    (for any tec in vt.behaviour.mitre\_attack\_techniques: (tec.id == "T1486"))
- **Command execution**: Identification of possible rescue note and background set.  
    (for any ce in vt.behaviour.command\_executions: (ce icontains "\\Desktop\\\*.txt" or ce icontains "\\Desktop\\\*.bmp"))
- **Memory patterns**: Specific patterns observed in locky samples that could be reused.  
    (for any mem in vt.behaviour.memory\_pattern\_urls: (mem icontains "checkupdate" or mem icontains "userinfo.php"))

  

Remember you can always **follow** Threat Actor and/or collections and receive **fresh new IOCs** through the IoC Stream.

  

## Wrapping up:

Threat Landscape empowers CTI teams with insights for prioritizing threats, understanding threat actors and tracking their operations pivoting between **Threat Actors** <=> **Collections** <=> **IOCs**. This provides actionable details based on the technical capabilities of the malware used in these campaigns, including a set of TTPs based on sandbox detonation that we can use both for hunting and monitoring. Collections also provide “**Commonalities**” on different indicators, including which crowdsourced rules better detect them. This helps us to quickly create effective monitoring and hunting strategies for malware families and threats actors, as well as effective protections adjusted to recent campaigns and malicious activity.

  

If you have any **suggestions** or want to **share feedback** please feel free to reach out here.

  

Happy Hunting!

VirusTotal’s **Threat Landscape** can be a valuable source of operational and tactical threat intelligence for CTI teams, for instance helping us find the latest **malware trends** used by a given Threat Actor to adjust our **intelligence-led security posture** accordingly. In this post, we will play the role of a CTI analyst working for a Singaporean financial institution.

  

As a first step, we search for threat actors that traditionally both targeted the **financial industry** and **Singaporean companies**.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhOYfPOF1sfQMsCFRPMBClNxlkMUuRqDUJu7BHziJtU6HkPOIO30_gk1vvbq8xFAk3xzheI-Qg3nhq4xQx8jGXtxTDIpRbibFtr4UE6S5VTRL2HUno3HTNOiKOiM4ZkciKairpUmBrB8jJPniPIWQHEH-RL08KLcUjFGNrGPOglM3-0wjr1vSySxvxkPHM/s1379/kyep1.png)

  

“**TA505**” and “**APT41**” both match these requirements. For the moment let’s focus on TA505, which seems more active at the moment.

  

## Understanding (TA505):

The **Threat Actor** card provides details on the actor, which seems to target organizations in the financial, healthcare, retail, and hospitality sectors across Europe, Asia Pacific region, Canada, India and the United States.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjLPvh_Z021Xb4CKQgwRFRX-0TTY-tTHqhtTUhRm0wjmY2p5WLI7yHpRgu6y5mS7adKxf6L1M4NESJpH1q3NuBeUFzPKenCLYQamRxMKN9FpZ7fO2lhlCr6386YZCCgIohG_v6j9JrWQ_ghO7kgkpETQVbKyR1VLqfdww3nImmbf2NyBAxUS3qBKdjBHRQ/s16000/kyep2.png)

  

According to the description TA505 seems related to **Dridex banking trojan** and **Locky ransomware** activity.

  

In VirusTotal we can find **two categories for TTPs**:  
\- The First are TTPs directly ingested from **MISP** and **MITRE**.  
\- The second (called Toolkit TTPs) shows TTPs obtained from **sandbox analysis of the IOCs** related to a particular actor.

  

In this case, for TA505 we can find the following **Toolkit TTPs**:

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsXor4ZlNpvzrA3SWkbEpawRbYIKZElisjfG5-pC4Ywfp_Kx9R8Uw3cZpaafbQ6L0R_bIiYKVA3IsJMudc9VEY3pEOtoJOakEgA-Xag_627pS6uyN1UfdfJmv3lD-0Ka20YbdflEPxkzs1JPQNYsgXdKs85Hogk6ET8NLeL5VOpJC1ZEfG4LaZyqXC8Qg/s16000/kyep3.png)

  

The **T1486** tactic **(‘Data Encrypted technique for Impact')** seems potentially related to the use of **ransomware**, such as **Locky**, by this actor. This seems like a good point for us to retrieve some fresh data and understand this actor’s recent activity. For instance, the following query provide **fresh samples** from the actor (samples submitted after January 1st, 2024) that use data encryption, and tagged as ransom by AVs:

  

attack\_technique:T1486 threat\_actor:TA505 ls:2024-01-01+ engines:ransom

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjIQeMWoIypq_2ootb1FVClD_5_9S0nlIexg50yzpY3n3LRgO8Ftki0R_y9alhKe4BEApxBErdPmDyTZZzoEhjlydfHGvPOgLbqeLp8mvKVLmsl5C6AY-E2t3UK-D_Jue33gBLtGzg5bEhyo-eKip2G4nGpYLzNlZh43kcS4mHus8m6MuD6_QSiaDpUzMQ/s16000/kyep4.png)

  

Multiple of the returned samples belong to the “locky” Collection **tagged as ‘locky’**, which contains **510 files** at the moment.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMADkBylU3Ua5_WIyPUatBioi5GrWhyQbiGRGXiyKOoJvdIHERuvpquxnCVMjOGXuCPp-JZq-K6i3vxqie5Z6RlfvfYFL9fscnqPjlAzPSHLQmvqfUgWDgg3mGXHfWAqjcyJO-WC0t8WaEEGoFEFv0NpFbs7ycoWeOncv7aWTjNvgkFQDGWRQQcHeIMEU/s16000/kyep5.png)

  

The **Telemetry tab** provides information about **submissions** and **lookups**, which helps us understand malware family’s distribution and timeframes of operations.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiDWIpq2eU-Fmk0Twg67amSaq_GYrt4iQY8TLkmVcj2Yw8XARxapIqQBybRYmBllJXbkQj27zsiKPwVdXioxlWzajs9m7RxrCY-mmD6XqMp4TI1WCwRHmt6V43QLeXFBS2MfmeU_bSm7GUZbtXdOHsBArdFzSl4OpcvCl00ZR6nXC4UF7peVm9jsZv1sDM/s16000/kyep6.png)

  

## Tailoring defenses:

In addition, the Collection’s **Rules** panel provides details on crowdsourced **Yara**, **sigma** and **IDS rules** that match different indicators files in this collection.

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2-cDcNE2XuSnFVnC5uC8aTD7SMr7leJvBpMBYG3iFopbx3QZqc15N8c1sSKqEjl_tIwnUSAvzhtuCJ7fJjtnrrGxuFITgUFffSMjqIBU3UF-C0wgJ5nHKGM3S-y9n9Lgsrnt9p0x8qaAlymMNmSANW2gkBrQ5ZRqui3EKobK6AUq6YR8QLY8wnTiFLhg/s16000/kyep7.png)

  

In this case, the “**win\_locky\_auto**” yara rule matches almost all the files in this collection (505/510). This could help to enhance detection capabilities for this threat.

  

Collection’s **commonalities** refer to characteristics, behaviors, or technical attributes shared by a set of indicators, which helps to identify patterns. Let’s use this to create a new “**Livehunt rule**” to track this activity in the future. We will use only recent samples, we can filter them in the IOCS tab (“fs:180d+”):

  

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjp6zsOoykB_WZIyIsTdIadYqnQkbECnAjwMN0o2blQv3q01Cbo5vMiKYeTtnR-_T1opy5lxAyvkARZBCadq_z_lJQIu__8STHtuCpYHTB11owibRlERVtxznts-A3WGpHevh36Ec1BLxHtG9oJVdxuwYB30kcmdOZ74HnuMeyb0lYr1C2rzIUDJ8o2SI/s16000/kyep8.png)

  

Based on commonalities results, some **useful information** to create the livehunt rule may include:

  

**Metadata**:

- **File type**: EXE and DLL formats.  
    (vt.metadata.file\_type == vt.FileType.PE\_DLL or vt.metadata.file\_type == vt.FileType.PE\_EXE)
- **File size**: Less than 1Mb.  
    (vt.metadata.file\_size < 1000000)
- **Main icon**: Custom and specific icon.  
    (vt.metadata.main\_icon.dhash == "52c244c9a7a3998b")
- **Imphash**: Hash value calculated from PE's import table, that could be matching some locky samples.  
    (vt.metadata.imphash == "31553623c43827d554ad9e1b7dfa6a5a")

  

**Behavior**:

- **Sandbox attack techniques**: Detect T1486 Encryption Data technique.  
    (for any tec in vt.behaviour.mitre\_attack\_techniques: (tec.id == "T1486"))
- **Command execution**: Identification of possible rescue note and background set.  
    (for any ce in vt.behaviour.command\_executions: (ce icontains "\\Desktop\\\*.txt" or ce icontains "\\Desktop\\\*.bmp"))
- **Memory patterns**: Specific patterns observed in locky samples that could be reused.  
    (for any mem in vt.behaviour.memory\_pattern\_urls: (mem icontains "checkupdate" or mem icontains "userinfo.php"))

  

Remember you can always **follow** Threat Actor and/or collections and receive **fresh new IOCs** through the IoC Stream.

  

## Wrapping up:

Threat Landscape empowers CTI teams with insights for prioritizing threats, understanding threat actors and tracking their operations pivoting between **Threat Actors** <=> **Collections** <=> **IOCs**. This provides actionable details based on the technical capabilities of the malware used in these campaigns, including a set of TTPs based on sandbox detonation that we can use both for hunting and monitoring. Collections also provide “**Commonalities**” on different indicators, including which crowdsourced rules better detect them. This helps us to quickly create effective monitoring and hunting strategies for malware families and threats actors, as well as effective protections adjusted to recent campaigns and malicious activity.

  

If you have any **suggestions** or want to **share feedback** please feel free to reach out here.

  

Happy Hunting!

Go to Source
