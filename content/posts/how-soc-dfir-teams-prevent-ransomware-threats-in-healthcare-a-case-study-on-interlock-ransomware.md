---
title: "How SOC/DFIR Teams Prevent Ransomware Threats in Healthcare – A Case Study on Interlock Ransomware"
date: 2025-01-29
---

Ransomware attacks targeting the healthcare sector have become increasingly challenging to manage due to financial losses and the risks posed to patient safety and operational continuity.

Researchers at **ANR.RUN analyze** the impact of ransomware threats in healthcare, using the notorious Interlock ransomware group as a case study focus is on how ANY.RUN’s advanced tools, including its Interactive Sandbox and Threat Intelligence (TI) Lookup, enable organizations to detect, analyze, and mitigate such attacks.

## **The Healthcare Sector Under Siege**

Healthcare organizations handle sensitive patient data, which makes them particularly attractive targets for ransomware actors. Recent major incidents include:

- **UnitedHealth**: Breach of 190 million records, marking one of the largest healthcare attacks.

- **Ascension**: Black Basta ransomware impacted 5.6 million patients.

- **Kootenai Health**: Leaked 464,000 patient records.

- **ConnectOnCall**: SaaS system breach exposed health data of over 910,000 patients.

- **Anna Jaques Hospital**: Attack disrupted medical services and exposed data for 316,000 patients.

Healthcare has become a prime target for cyberattacks due to several factors. Patient records are highly valuable and fetch significant prices on the black market, making them an attractive target for cybercriminals.

Additionally, the critical nature of healthcare services means that downtime directly affects patient care, often pressuring organizations to pay ransoms to restore operations.

Limited budgets in the sector tend to prioritize medical services over robust IT defenses, leaving many healthcare organizations with weak cybersecurity postures. Furthermore, delayed detection of intrusions allows ransomware to spread more extensively, increasing its impact and the potential cost of recovery.

**Integrate proactive threat analysis with ANY.RUN to strengthen your company’s security – Get 14-day free trial**

## **Spotlight on the Interlock Ransomware Group**

Interlock employs double-extortion techniques, encrypting data and threatening public release if demands aren’t met. Key attacks in late 2024 included.

In October 2024, the Brockton Neighborhood Health Center experienced a breach that remained undetected until December, highlighting a significant delay in identification.

Similarly, Legacy Treatment Services identified an attack on October 26, 2024, while the Drug and Alcohol Treatment Service faced a compromise just days earlier, on October 24, 2024.

These incidents underscore the persistent threats targeting healthcare organizations and the challenges they face in timely detection and response.

## **How ANY.RUN’s Tools Strengthen Cybersecurity**

### **1\. Initial Compromise Detection (TA0001): Drive-By Compromise**

Interlock used phishing websites disguised as software update hubs to deploy malware. A domain such as `apple-online.shop` was flagged by ANY.RUN nearly a month before public reports.

- **Interactive Sandbox**: Analysts could interact with a virtual machine to observe how the malicious website lured users into downloading malware.

- **Threat Intelligence (TI) Lookup**: Identified malicious domains and files (e.g., fake updater programs like `MSTeamsSetup.exe`) well in advance, allowing healthcare organizations to block the offending sites.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjYVIyu_JVp5GU8kRQwVPbS2KRbNAL4mMLE3mp3vQxlQqZG6W67eU2rTNHsg4uadhzYVXC0V-LlREZAYz95IxxFytpFNNu4-8G3HlCk0sCBLMfNVBXvLom2wOX6pKEN3ZcLLe7cOqwgcDkXU-v1f4fxJddfv6eJX5_jP1d1Qopvkk8eP44rnLmd4wC3PaeQ/s16000/results_interlock-1.webp)

**ANY.RUN expanded on known attack patterns,** revealing additional files with random naming patterns like:

- `upd_1416836.exe`

- SHA256 Hashes: `8d911ef72bdb4...`

**`Collect threat intelligence with TI Lookup to improve your company’s security - Get 50 Free Request`**

ANY.RUN has shed light on the tactics used in recent Interlock ransomware attacks, Through its **Interactive Sandbox** and Threat Intelligence Lookup to enables users to analyze malicious websites and files in real time, uncovering critical Indicators of Compromise (IOCs) and providing actionable insights.

Using ANY.RUN’s Interactive Sandbox, organizations can view the behavior of malicious websites, including the deceptive content used to target victims.

For instance, **At this session**, Interlock leveraged fake websites designed to appear legitimate, such as updates for popular applications, to trick users into downloading malware. By analyzing these tactics, healthcare organizations can train employees to recognize and avoid such threats in the future.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhdkD3fnB_27HwWRp_hosCUb6EZ-G8csHhAbey3_FXhnbf4UN-4mU57O5RxLbSqmbjuUT3tq-KeFpn0OtBKes4mfcSif0bOWgFd-lnQ2Ig9BAWkETXFwW26U7ofZ2DsvfG7PvsoKE-tOAJydWx2l4EmOmTw9R4YjJleuC-dDs8KtogTuq_Lw7DJTVyeWeK7/s16000/imaged-1-2048x1167.webp)

<figcaption>

_The malicious website used by Interlock displayed in ANY.RUN’s sandbox_

</figcaption>

</figure>

**ANY.RUN’s analysis** revealed that while attackers initially disguised their malware as a Google Chrome updater, they also mimicked other applications like Microsoft Teams and Microsoft Edge.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh7NH7HHZraggJDaO-x8pnITk5D0SGPKrZDsE1EV3BFEMXmJQSYetk6GDLm1w-amozQjp8xbXIbzh5twBJCFaKl6lDG_EFmDHb4qq_zDLmy6jfLUjo5Fl1N5Tl9UsLj6MWSF42IR2zoAbV4SNThHRwr_oONo91_DrakQerWBoGGS3rCKHr0Wpwk42yfcGmT/s16000/imagee.png)

<figcaption>

_ANY.RUN reports with analysis of Interlock’s fake updater programs_

</figcaption>

</figure>

Fake update files, such as _MSTeamsSetup.exe_ and _MicrosoftEdgeSetup.exe_, were identified during the investigation, expanding the known tactics used by Interlock. This broader understanding of their methods equips organizations to anticipate and defend against a wider range of file disguises.

**Detailed IOC Analysis**

ANY.RUN’s database also identified specific files used in Interlock’s attacks.

While reports previously highlighted _upd\_2327991.exe_, additional files, such as _**upd\_8816295.exe**_ and _**upd\_1416836.exe**_, were discovered, all featuring unique alphanumeric naming patterns.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQ2C9sa1v-MEtEs4DxUK8C0-VnbyVZNdFA3pVQqP4zNBGQV6auwCZ_vtlkZQ_Xo4UtFWbQ51QwYnuppVs5Tea5PqLdyK4FsaXY2PrSxnn466u4W0RyMeVKg0rs0uJZf9x_tIcZMKdJWfvtI72TMEOgHTLE4hfTg0OW2j9oBVrlMqi3aLIjsd_LeQAUkNiy/s16000/image1b.png)

Each file had distinct SHA256 hash values, which serve as critical IOCs for identifying and blocking malicious activity.

- 8d911ef72bdb4ec5b99b7548c0c89ffc8639068834a5e2b684c9d78504550927 

- 97105ed172e5202bc219d99980ebbd01c3dfd7cd5f5ac29ca96c5a09caa8af67 

### **2\. Execution Phase (TA0002)**

Once the malicious files were downloaded, Interlock used Remote Access Tools (RATs) disguised as legitimate software (e.g., Chrome and MSTeams updates) to execute payloads.

- **RAT Analysis**: **Sandbox sessions uncovered** encrypted URLs used by attackers for communication (e.g., XOR-encrypted URLs found in malware configurations).

- **YARA Search**: Helped craft detection rules to identify over 40 additional malicious files used by the group.

YARA Rule Example:

```
rule Interlock_RAT {
    strings:
        $ = "/MSTeamsSetup.exe\" xor
    condition:
        any of them
}
```

**During Execution**, ANY.RUN helps users: 

- **Discover IOCs**: Find additional file and network IoCs, including those found in configurations. 

- **Find Unknown Threats**: Discover previously unknown threats. 

- **Analyze Threats**: Safely explore suspicious URLs and detonate payloads. 

### 3\. **Credential Access (TA0006)**

Interlock used a custom credential-stealing tool to collect usernames, passwords, and other sensitive data. Stolen credentials were stored in files like `chrgetpdsi.txt`.

ANY.RUN’s TI Lookup detected the malicious stealer tool months before publicized incidents, providing early warnings.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj3RpJ7Ka5WlsgApk7kcybOpPcLMas-ZSI__uoozXW3-dw3q1g_6OxcCltGRE5CH_fzTlCAbsPxypvo0IusjJSJRNsphvsj77HNXG7c_7R896Fe9bsnY1n7uXTf-Xhlg9y5xreC7kFdjriotsnlmllpcJJtoDPdQDo4gF3Km88eKu-ur4K_Ds2WiB8DrTXG/s16000/image20-2048x1223.webp)

<figcaption>

_Results of a TI Lookup search for a txt file used in the attack_

</figcaption>

</figure>

### 4\. **Lateral Movement (TA0008)**

Interlock used tools like **Putty**, **Anydesk**, and **RDP** to expand their reach within networks. Sandbox analysis by ANY.RUN flagged these tools when misused.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjU1mARRr_KcnBT3qBw_3yBN9PMJO2FIXZ6UfB9yyb3keUV0cdGi3j7t1ysxM2HzKbDfmigWF34p0jXHrOnIeC-8iwBV_d17qPsjg1ZWzyd9PwVnUEMok5impRHd0TAq1_HNecYJ_RelXqijL0IJTogfQqnF7JhneJ3OM9BBZO-glbeRyD0xvWdi2vmnfJt/s16000/image22-2048x1230.webp)

**Key Features:**

- **Detection**: Any malicious use of legitimate admin tools was flagged.

- **Behavioral Insights**: Analysts could monitor lateral movement patterns to improve internal defenses.

### 5\. **Data Exfiltration (TA0010)**

The final phase involved exfiltrating data to attacker-controlled servers via Azure cloud storage.

- **Traffic Monitoring**: ANY.RUN’s sandbox captured outbound traffic to attacker servers (e.g., `217[.]148[.]142[.]19` via port 443).

- **Data Decryption**: Using tools like CyberChef, analysts decrypted logs to identify exact data sent.

## **Key Benefits of ANY.RUN for Healthcare Security**

**ANY.RUN** helps more than 500,000 cybersecurity professionals worldwide. Our interactive sandbox simplifies malware analysis of threats that target both Windows and Linux systems.

Ransomware attacks like those by the Interlock group highlight the urgency of robust threat detection and analysis tools in healthcare.

**ANY.RUN’s Interactive Sandbox** and **Threat Intelligence Lookup** bridge critical gaps in cybersecurity preparedness, enabling healthcare organizations to identify threats early and understand and mitigate them effectively.

| **Key Benefit** | **Description** |
| --- | --- |
| **Threat Understanding** | Pin malicious indicators to actual threats, providing a clearer understanding of the risks your organization is facing. |
| **In-Depth Reporting** | Receive detailed reports with Indicators of Compromise (IOCs), Tactics, Techniques, and Procedures (TTPs), and malware behavior summaries. |
| **Efficient Threat Analysis** | Simplify and accelerate threat analysis for SOC team members at all levels, saving time and boosting productivity. |
| **Faster Alert Triage** | Speed up the alert triage process and reduce workloads with fast operations, a user-friendly interface, and smart automation. |
| **Private and Secure Analysis** | Safely examine sensitive data in private mode, ensuring compliance with cybersecurity and data protection requirements. |
| **Comprehensive Malware Insights** | Gain detailed insights into malware behavior, enabling a better understanding of threats to streamline incident response. |
| **Team Collaboration** | Collaborate with team members, share analysis results, and coordinate efforts effectively during incident handling. |
| **Cost Optimization** | Reduce incident response costs by leveraging detailed data and ANY.RUN’s interactive analysis to develop advanced detection and protection methods. |

For healthcare organizations, leveraging services such as ANY.RUN provides a cost-efficient, actionable approach to bolster defenses against evolving cyber threats.

**Request a free trial** to explore how ANY.RUN can safeguard your critical infrastructure against ransomware threats.

The post How SOC/DFIR Teams Prevent Ransomware Threats in Healthcare – A Case Study on Interlock Ransomware appeared first on Cyber Security News.

Go to Source
