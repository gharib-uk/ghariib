---
title: "Stories from the SOC: Registry Clues to PDF Blues: A Tale of PUA Persistence"
date: 2025-01-06
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
---

### Executive Summary

Establishing persistence on a system allows a threat actor continued access or process execution across system restarts or other changes. For this reason, monitoring for and investigating persistence indicators are key components of any robust cybersecurity platform.

Two common persistence techniques are using AutoStart Execution of programs during system boot or logon (T1547) and abusing scheduled task functions (T1053). However, legitimate application activity also frequently involves AutoStart Execution and scheduled task functions, so defending against these techniques requires not only detection monitoring but also analysis by a cybersecurity professional. 

During a recent incident involving a LevelBlue MDR SOC customer, an alarm that triggered for a Windows Autorun registry key for persistence was traced back to a potentially unwanted application (PUA). The PUA purportedly was acting as a PDF conversion application. A review of the initial alarm and relevant events revealed that the application had established a double layer of persistence by using both Scheduled Task creation and Autorun registry keys to execute JavaScript under the guise of a Chrome browser extension. Additional open-source intelligence (OSINT) tools identified the application as either a PUA or a potentially malicious file. An investigation was created for the customer with remediation recommendations and ultimately it was confirmed that the application was neither expected nor authorized within the customer’s environment, and it was removed.

The same application was later detected in another customer’s environment, but in this case, the customer had added a related file hash to an exclusion list. Because the LevelBlue MDR SOC analyst had recently investigated the application and identified it as potentially malicious, they were able to recommend removing the hash from the exclusion list and instead adding it to a blocklist.

### Investigation

#### Initial Alarm Review

The investigation began with the LevelBlue analyst receiving an alarm that a Windows Autorun registry key named “ChromeBrowserAutoLaunch” had been added on an endpoint in the customer environment. While at first glance this appeared to be a key set to auto-launch Chrome with a browser extension loaded, analysis of the source process command line revealed several items that warranted further investigation.

![levelblue soc alarm](https://cyber.levelblue.com/m/673bdbdcd860cf9/original/Stories-from-the-SOC1.png)

Figure 1: The initial alarm for the autorun registry key creation

- The “–no-startup-window” option: although this is commonly used for legitimate purposes, it can also indicate an attempt to hide activity from the end user. The pathway of the extension being loaded showed it was not an extension that the user had installed from the Chrome webstore. The expected pathway for extensions from the webstore would be “C:Users<username>AppDataLocalGoogleChromeUser DataDefaultExtensions”. While a sideloaded extension could still be legitimate, this gave additional cause to identify the origin of the registry key and extension.
- No verifiable browser extension with the name “Extension Optimizer” was found in OSINT queries. 
- Abuse of browser extensions (T1176) is a known technique and malicious extensions have a history of being used for infostealing, adware, and browser hijack or redirect behaviors. 

### Expanded Investigation

#### Events Search

The analyst conducted an event search to identify the origin of the browser extension “ExtensionOptimizer”. This search returned process creation events that revealed the registry key was being added by a node.exe JavaScript process executing from an AppData folder named “PDFFlex” in the pathway “C:Users<username>AppDataLocalPDFFlexnode.exe”.  An additional event was logged at the same time showing that node.exe was also being used to load the extension manually.

![Initial alarm](https://cyber.levelblue.com/m/9e49be067a7fe2b/original/Stories-from-the-SOC2.png)

![levelblue soc 3 events](https://cyber.levelblue.com/m/6fc05d8c22c67db/original/Stories-from-the-SOC3.png)

Figure 2: Events showing the registry keys origin and manual loading of the extension

The analyst searched for “PDFFlex” to understand if the application was common in the customer’s environment and to obtain additional artifacts regarding its origin or nature. The search revealed the application’s presence was anomalous and also uncovered events that could be used for further research.

The analyst obtained the filename of the application’s MSI installer, the version and publisher of the application, and an event that showed the creation of a daily scheduled task. This task was configured to execute “node.exe update.js –check-update” from the same “PDFFlex” folder pathway seen in the registry creation events. Further analysis showed that this task was responsible for executing the process that was creating the Autorun registry key in an apparent double layer of persistence established on the endpoint. 

![Levelblue soc](https://cyber.levelblue.com/m/4affaa7d787c67a6/original/Stories-from-the-SOC4.png)

Figure 3: Scheduled task created to persistently add the registry key each day

![Levelblue soc](https://cyber.levelblue.com/m/3935c594b8a4d5ae/original/Stories-from-the-SOC5.png)

Figure 4: Event showing the name of the application’s MSI installer file found in the user’s downloads folder

![Levelblue soc](https://cyber.levelblue.com/m/1b5672c3156968f1/original/Stories-from-the-SOC6.png)

Figure 5: Installation event showing the version and publisher of the application “PDFFlex”

#### Event Deep-Dive

The analyst then performed several OSINT searches using the information obtained in event searches to verify the use case and potential legitimacy of the application. 

- No verifiable information was found for the MSI file “FreePDF\_49402039.msi” or the publisher PDFFlex.io. 
- The analyst conducted a Whois search of the domain “pdfflex.io” and found that it was not registered.  
- A web search for “PDFFlex 3.202.1208.0” returned a verdict of “malicious activity” from the sandbox tool ANY.RUN, which provided a SHA256 file hash of 9c5d756045fd479a742b81241ccf439d02fc668581a3002913811a341278de43. 
- A search of the hash on VirusTotal revealed that it had been flagged as potentially malicious by multiple security vendors, including Sophos and Fortinet. 
- The analyst leveraged SentinelOne Deep Visibility to confirm that the hash for the MSI file on the customer’s endpoint matched the hash in the ANY.RUN report. At the time of the alarm, incidents were not being triggered on the hash. The SentinelOne tool also showed that the MSI file was signed by “Eclipse Media Inc,” which proved key in a later incident for another LevelBlue customer. 

![levelblue soc](https://cyber.levelblue.com/m/4f66672ac8dd66a7/original/Stories-from-the-SOC7.png)

Figure 6:Deep Visibility search in SentinelOne showing the file hash for the MSI file found on the endpoint

### Response

#### Building the Investigation

The analyst’s investigation and OSINT research returned several points to indicate that the “PDFFlex” application was likely not a desired application in the environment:

- The presence of the application on the endpoint was anomalous for the environment as events for it were not observed for other endpoints.
- The application had established what appeared to be a double layer of persistence by using a scheduled task and autorun registry key to create and launch an unverified browser extension “ExtensionOptimizer.”
- OSINT reports for the MSI file indicated potentially malicious behavior.

Together, these data points indicated that the application was neither desired nor expected in the customer environment and could be classified as a PUA/PUP, if not as outright malicious, and thus should be removed from the endpoint.

#### Customer Interaction

The analyst created an investigation that detailed the findings regarding the application “PDFFlex,” the browser extension “ExtensionOptimizer,” the observed persistence behaviors, and the findings of the OSINT research. They recommended that the customer reimage the endpoint or remove the associated AppData folders for “PDFFlex” and “ExtensionOptimizer” the scheduled tasks, and the associated registry keys. Shortly after the initial investigation, the LevelBlue MDR SOC identified another endpoint in the customer’s environment that was exhibiting the same persistence indicators under the application name “PDFTool.” The customer confirmed that the applications were not authorized and ultimately elected to remove the endpoints from service and replace them.

While the MSI file initially did not trigger an alarm, several days after the investigation, its hash was added to the SentinelOne Cloud global blocklist and began to trigger alarms. During review of one of these for another customer, a LevelBlue analyst found that the customer had added a hash-based exclusion for a similarly named pdf-related MSI file with a different file hash but also signed by “Eclipse Media Inc.” 

This customer had previously observed the threat but added the hash to the exclusion list in SentinelOne due to no negative reports observed while researching the file using OSINT tools. The LevelBlue team’s knowledge of the signer “Eclipse Media Inc” along with their recent analysis of the application allowed them to inform the customer about the risks of the application. Based on the analyst’s recommendation, the exclusion was removed and a blocklist action for the alternate hash was added instead.

### Conclusion

This incident highlights not only the need for monitoring and alerting on scheduled task and Autorun registry key creation but also the value of having expert analysis of these events. In this investigation, the analyst’s use of OSINT and sandboxing tools such as ANY.RUN provided the critical context needed to protect the customer’s environment from threats. In addition, the analyst’s research and prior knowledge of the file signer “Eclipse Media Inc” later proved key in protecting another LevelBlue customer that had created an exclusion for what was likely the same PUA under a different file hash.

### Executive Summary

Establishing persistence on a system allows a threat actor continued access or process execution across system restarts or other changes. For this reason, monitoring for and investigating persistence indicators are key components of any robust cybersecurity platform.

Two common persistence techniques are using AutoStart Execution of programs during system boot or logon (T1547) and abusing scheduled task functions (T1053). However, legitimate application activity also frequently involves AutoStart Execution and scheduled task functions, so defending against these techniques requires not only detection monitoring but also analysis by a cybersecurity professional. 

During a recent incident involving a LevelBlue MDR SOC customer, an alarm that triggered for a Windows Autorun registry key for persistence was traced back to a potentially unwanted application (PUA). The PUA purportedly was acting as a PDF conversion application. A review of the initial alarm and relevant events revealed that the application had established a double layer of persistence by using both Scheduled Task creation and Autorun registry keys to execute JavaScript under the guise of a Chrome browser extension. Additional open-source intelligence (OSINT) tools identified the application as either a PUA or a potentially malicious file. An investigation was created for the customer with remediation recommendations and ultimately it was confirmed that the application was neither expected nor authorized within the customer’s environment, and it was removed.

The same application was later detected in another customer’s environment, but in this case, the customer had added a related file hash to an exclusion list. Because the LevelBlue MDR SOC analyst had recently investigated the application and identified it as potentially malicious, they were able to recommend removing the hash from the exclusion list and instead adding it to a blocklist.

### Investigation

#### Initial Alarm Review

The investigation began with the LevelBlue analyst receiving an alarm that a Windows Autorun registry key named “ChromeBrowserAutoLaunch” had been added on an endpoint in the customer environment. While at first glance this appeared to be a key set to auto-launch Chrome with a browser extension loaded, analysis of the source process command line revealed several items that warranted further investigation.

![levelblue soc alarm](https://cyber.levelblue.com/m/673bdbdcd860cf9/original/Stories-from-the-SOC1.png)

Figure 1: The initial alarm for the autorun registry key creation

- The “–no-startup-window” option: although this is commonly used for legitimate purposes, it can also indicate an attempt to hide activity from the end user. The pathway of the extension being loaded showed it was not an extension that the user had installed from the Chrome webstore. The expected pathway for extensions from the webstore would be “C:Users<username>AppDataLocalGoogleChromeUser DataDefaultExtensions”. While a sideloaded extension could still be legitimate, this gave additional cause to identify the origin of the registry key and extension.
- No verifiable browser extension with the name “Extension Optimizer” was found in OSINT queries. 
- Abuse of browser extensions (T1176) is a known technique and malicious extensions have a history of being used for infostealing, adware, and browser hijack or redirect behaviors. 

### Expanded Investigation

#### Events Search

The analyst conducted an event search to identify the origin of the browser extension “ExtensionOptimizer”. This search returned process creation events that revealed the registry key was being added by a node.exe JavaScript process executing from an AppData folder named “PDFFlex” in the pathway “C:Users<username>AppDataLocalPDFFlexnode.exe”.  An additional event was logged at the same time showing that node.exe was also being used to load the extension manually.

![Initial alarm](https://cyber.levelblue.com/m/9e49be067a7fe2b/original/Stories-from-the-SOC2.png)

![levelblue soc 3 events](https://cyber.levelblue.com/m/6fc05d8c22c67db/original/Stories-from-the-SOC3.png)

Figure 2: Events showing the registry keys origin and manual loading of the extension

The analyst searched for “PDFFlex” to understand if the application was common in the customer’s environment and to obtain additional artifacts regarding its origin or nature. The search revealed the application’s presence was anomalous and also uncovered events that could be used for further research.

The analyst obtained the filename of the application’s MSI installer, the version and publisher of the application, and an event that showed the creation of a daily scheduled task. This task was configured to execute “node.exe update.js –check-update” from the same “PDFFlex” folder pathway seen in the registry creation events. Further analysis showed that this task was responsible for executing the process that was creating the Autorun registry key in an apparent double layer of persistence established on the endpoint. 

![Levelblue soc](https://cyber.levelblue.com/m/4affaa7d787c67a6/original/Stories-from-the-SOC4.png)

Figure 3: Scheduled task created to persistently add the registry key each day

![Levelblue soc](https://cyber.levelblue.com/m/3935c594b8a4d5ae/original/Stories-from-the-SOC5.png)

Figure 4: Event showing the name of the application’s MSI installer file found in the user’s downloads folder

![Levelblue soc](https://cyber.levelblue.com/m/1b5672c3156968f1/original/Stories-from-the-SOC6.png)

Figure 5: Installation event showing the version and publisher of the application “PDFFlex”

#### Event Deep-Dive

The analyst then performed several OSINT searches using the information obtained in event searches to verify the use case and potential legitimacy of the application. 

- No verifiable information was found for the MSI file “FreePDF\_49402039.msi” or the publisher PDFFlex.io. 
- The analyst conducted a Whois search of the domain “pdfflex.io” and found that it was not registered.  
- A web search for “PDFFlex 3.202.1208.0” returned a verdict of “malicious activity” from the sandbox tool ANY.RUN, which provided a SHA256 file hash of 9c5d756045fd479a742b81241ccf439d02fc668581a3002913811a341278de43. 
- A search of the hash on VirusTotal revealed that it had been flagged as potentially malicious by multiple security vendors, including Sophos and Fortinet. 
- The analyst leveraged SentinelOne Deep Visibility to confirm that the hash for the MSI file on the customer’s endpoint matched the hash in the ANY.RUN report. At the time of the alarm, incidents were not being triggered on the hash. The SentinelOne tool also showed that the MSI file was signed by “Eclipse Media Inc,” which proved key in a later incident for another LevelBlue customer. 

![levelblue soc](https://cyber.levelblue.com/m/4f66672ac8dd66a7/original/Stories-from-the-SOC7.png)

Figure 6:Deep Visibility search in SentinelOne showing the file hash for the MSI file found on the endpoint

### Response

#### Building the Investigation

The analyst’s investigation and OSINT research returned several points to indicate that the “PDFFlex” application was likely not a desired application in the environment:

- The presence of the application on the endpoint was anomalous for the environment as events for it were not observed for other endpoints.
- The application had established what appeared to be a double layer of persistence by using a scheduled task and autorun registry key to create and launch an unverified browser extension “ExtensionOptimizer.”
- OSINT reports for the MSI file indicated potentially malicious behavior.

Together, these data points indicated that the application was neither desired nor expected in the customer environment and could be classified as a PUA/PUP, if not as outright malicious, and thus should be removed from the endpoint.

#### Customer Interaction

The analyst created an investigation that detailed the findings regarding the application “PDFFlex,” the browser extension “ExtensionOptimizer,” the observed persistence behaviors, and the findings of the OSINT research. They recommended that the customer reimage the endpoint or remove the associated AppData folders for “PDFFlex” and “ExtensionOptimizer” the scheduled tasks, and the associated registry keys. Shortly after the initial investigation, the LevelBlue MDR SOC identified another endpoint in the customer’s environment that was exhibiting the same persistence indicators under the application name “PDFTool.” The customer confirmed that the applications were not authorized and ultimately elected to remove the endpoints from service and replace them.

While the MSI file initially did not trigger an alarm, several days after the investigation, its hash was added to the SentinelOne Cloud global blocklist and began to trigger alarms. During review of one of these for another customer, a LevelBlue analyst found that the customer had added a hash-based exclusion for a similarly named pdf-related MSI file with a different file hash but also signed by “Eclipse Media Inc.” 

This customer had previously observed the threat but added the hash to the exclusion list in SentinelOne due to no negative reports observed while researching the file using OSINT tools. The LevelBlue team’s knowledge of the signer “Eclipse Media Inc” along with their recent analysis of the application allowed them to inform the customer about the risks of the application. Based on the analyst’s recommendation, the exclusion was removed and a blocklist action for the alternate hash was added instead.

### Conclusion

This incident highlights not only the need for monitoring and alerting on scheduled task and Autorun registry key creation but also the value of having expert analysis of these events. In this investigation, the analyst’s use of OSINT and sandboxing tools such as ANY.RUN provided the critical context needed to protect the customer’s environment from threats. In addition, the analyst’s research and prior knowledge of the file signer “Eclipse Media Inc” later proved key in protecting another LevelBlue customer that had created an exclusion for what was likely the same PUA under a different file hash.

Go to Source
