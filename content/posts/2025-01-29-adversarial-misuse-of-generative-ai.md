---
title: "Adversarial Misuse of Generative AI"
date: Wed, 29 Jan 2025 14:00:00 +0000
draft: false
type: posts
---
# Adversarial Misuse of Generative AI

<br/>

<br/>
Rapid advancements in artificial intelligence (AI) are unlocking new possibilities for the way we work and accelerating innovation in science, technology, and beyond. In cybersecurity, AI is poised to transform digital defense, empowering defenders and enhancing our collective security. Large language models (LLMs) open new possibilities for defenders, from sifting
<br/>
Rapid advancements in artificial intelligence (AI) are unlocking new possibilities for the way we work and accelerating innovation in [science](https://deepmind.google/technologies/alphafold/), [technology](https://deepmind.google/discover/blog/how-alphachip-transformed-computer-chip-design/), and beyond. In cybersecurity, AI is poised to transform digital defense, [empowering defenders and enhancing our collective security](https://blog.google/technology/safety-security/google-ai-cyber-defense-initiative/). Large language models (LLMs) open new possibilities for defenders, from sifting through complex telemetry to [secure coding](https://cloud.google.com/products/gemini/code-assist), [vulnerability](https://googleprojectzero.blogspot.com/2024/10/from-naptime-to-big-sleep.html) [discovery](https://security.googleblog.com/2024/11/leveling-up-fuzzing-finding-more.html), and streamlining operations. However, some of these same AI capabilities are also available to attackers, leading to understandable anxieties about the potential for AI to be misused for malicious purposes. 

Much of the current discourse around cyber threat actors' misuse of AI is confined to theoretical research. While these studies demonstrate the potential for malicious exploitation of AI, they don't necessarily reflect the reality of how AI is currently being used by threat actors in the wild. To bridge this gap, we are sharing a comprehensive analysis of how threat actors interacted with Google's AI-powered assistant, Gemini. Our analysis was grounded by the expertise of Google's Threat Intelligence Group (GTIG), which combines decades of experience tracking threat actors on the front lines and protecting Google, our users, and our customers from government-backed attackers, targeted 0-day exploits, coordinated information operations (IO), and serious cyber crime networks.

We believe the private sector, governments, educational institutions, and other stakeholders [must work together](https://blog.google/technology/ai/a-policy-agenda-for-responsible-ai-progress-opportunity-responsibility-security/) to maximize AI's benefits while also [reducing the risks](https://blog.google/technology/safety-security/google-ai-saif-risk-assessment/) of abuse. At Google, we are committed to developing responsible AI guided by [our principles](https://ai.google/responsibility/principles/#our-ai-principles-in-action), and we share [resources](https://blog.google/technology/safety-security/introducing-googles-secure-ai-framework/) and [best practices](https://ai.google.dev/responsible/docs) to enable responsible AI development across the industry. We continuously improve [our AI models](https://blog.google/technology/ai/) to make them [less susceptible to misuse](https://gemini.google/policy-guidelines/), and we apply our intelligence to improve Google's defenses and protect users from cyber threat activity. We also proactively disrupt malicious activity to protect our users and help make the internet safer. We share our findings with the security community to raise awareness and enable stronger protections for all. 

aside\_block

<ListValue: \[StructValue(\[('title', 'Adversarial Misuse of Generative AI'), ('body', <wagtail.rich\_text.RichText object at 0x3e9738578e50>), ('btn\_text', 'Download now'), ('href', 'https://services.google.com/fh/files/misc/adversarial-misuse-generative-ai.pdf'), ('image', <GAEImage: adversarial gen AI cover>)\])\]>

Executive Summary
-----------------

Google Threat Intelligence Group (GTIG) is committed to tracking and protecting against cyber threat activity. We relentlessly defend Google, our users, and our customers by building the most complete threat picture to disrupt adversaries. As part of that effort, we investigate activity associated with threat actors to protect against malicious activity, including the misuse of generative AI or LLMs. 

This report shares our findings on government-backed threat actor use of the Gemini web application. The report encompasses new findings across advanced persistent threat (APT) and coordinated information operations (IO) actors tracked by GTIG. By using a mix of analyst review and LLM-assisted analysis, we investigated prompts by APT and IO threat actors who attempted to misuse Gemini.

_**Advanced Persistent Threat (APT)** refers to government-backed hacking activity, including cyber espionage and destructive computer network attacks._  

_**Information Operations (IO)** attempt to influence online audiences in a deceptive, coordinated manner. Examples include sockpuppet accounts and comment brigading._ 

GTIG takes a holistic, intelligence-driven approach to detecting and disrupting threat activity, and our understanding of government-backed threat actors and their campaigns provides the needed context to identify threat enabling activity. We use a wide variety of technical signals to track government-backed threat actors and their infrastructure, and we are able to correlate those signals with activity on our platforms to protect Google and our users. By tracking this activity, we’re able to leverage our insights to counter threats across Google platforms, including disrupting the activity of threat actors who have misused Gemini. We also actively share our insights with the public to raise awareness and enable stronger protections across the wider ecosystem.

Our analysis of government-backed threat actor use of Gemini focused on understanding how threat actors are using AI in their operations and if any of this activity represents novel or unique AI-enabled attack or abuse techniques. Our findings, which are consistent with those of our industry peers, reveal that while AI can be a useful tool for threat actors, it is not yet the game-changer it is sometimes portrayed to be. While we do see threat actors using generative AI to perform common tasks like troubleshooting, research, and content generation, we do not see indications of them developing novel capabilities. 

Our key findings include:

-   **We did not observe any original or persistent attempts by threat actors to use prompt attacks or other machine learning (ML)-focused threats as outlined in the** [**Secure AI Framework (SAIF) risk taxonomy**](https://saif.google/secure-ai-framework/risks)**.** Rather than engineering tailored prompts, threat actors used more basic measures or publicly available jailbreak prompts in unsuccessful attempts to bypass Gemini's safety controls. 
-   **Threat actors are experimenting with Gemini to enable their operations, finding productivity gains but not yet developing novel capabilities**. At present, they primarily use AI for research, troubleshooting code, and creating and localizing content. 
-   **APT actors used Gemini to support several phases of the attack lifecycle**, including researching potential infrastructure and free hosting providers, reconnaissance on target organizations, research into vulnerabilities, payload development, and assistance with malicious scripting and evasion techniques. Iranian APT actors were the heaviest users of Gemini, using it for a wide range of purposes. Of note, we observed limited use of Gemini by Russian APT actors during the period of analysis.
-   **IO actors used Gemini for research; content generation including developing personas and messaging; translation and localization; and to find ways to increase their reach**. Again, Iranian IO actors were the heaviest users of Gemini, accounting for three quarters of all use by IO actors. We also observed Chinese and Russian IO actors using Gemini primarily for general research and content creation. 
-   **Gemini's safety and security measures restricted content that would enhance adversary capabilities** as observed in this dataset. Gemini provided assistance with common tasks like creating content, summarizing, explaining complex concepts, and even simple coding tasks. Assisting with more elaborate or explicitly malicious tasks generated safety responses from Gemini. 
-   **Threat actors attempted unsuccessfully to use Gemini to enable abuse of Google products,** including researching techniques for Gmail phishing, stealing data, coding a Chrome infostealer, and bypassing Google's account verification methods. 

Rather than enabling disruptive change, generative AI allows threat actors to move faster and at higher volume. For skilled actors, generative AI tools provide a helpful framework, similar to the use of Metasploit or Cobalt Strike in cyber threat activity. For less skilled actors, they also provide a learning and productivity tool, enabling them to more quickly develop tools and incorporate existing techniques. However, **current LLMs on their own are unlikely to enable breakthrough capabilities for threat actors**. ****We note that the AI landscape is in constant flux, with new AI models and agentic systems emerging daily. As this evolution unfolds, GTIG anticipates the threat landscape to evolve in stride as threat actors adopt new AI technologies in their operations****.

AI-Focused Threats
------------------

Attackers can use LLMs in two ways. One way is attempting to leverage LLMs to accelerate their campaigns (e.g., by generating code for malware or content for phishing emails). The overwhelming majority of activity we observed falls into this category. The second way attackers can use LLMs is to instruct a model or AI agent to take a malicious action (e.g., finding sensitive user data and exfiltrating it). These risks are outlined in Google's [Secure AI Framework (SAIF) risk taxonomy](https://saif.google/secure-ai-framework/risks). 

**We did not observe any original or persistent attempts by threat actors to use prompt attacks or other AI-specific threats.** Rather than engineering tailored prompts, threat actors used more basic measures, such as rephrasing a prompt or sending the same prompt multiple times. These attempts were unsuccessful.

### Jailbreak Attempts: Basic and Based on Publicly Available Prompts

We observed a handful of cases of low-effort experimentation using publicly available jailbreak prompts in unsuccessful attempts to bypass Gemini's safety controls. Threat actors copied and pasted publicly available prompts and appended small variations in the final instruction (e.g., basic instructions to create ransomware or malware). Gemini responded with safety fallback responses and declined to follow the threat actor's instructions. 

In one example of a failed jailbreak attempt, an APT actor copied publicly available prompts into Gemini and appended basic instructions to perform coding tasks. These tasks included encoding text from a file and writing it to an executable and writing Python code for a distributed denial-of-service (DDoS) tool. In the former case, Gemini provided Python code to convert Base64 to hex, but provided a safety filtered response when the user entered a follow-up prompt that requested the same code as a VBScript.

The same group used a different publicly available jailbreak prompt to request Python code for DDoS. Gemini provided a safety filtered response stating that it could not assist, and the threat actor abandoned the session and did not attempt further interaction.

_**What is an AI jailbreak?**_ 

_Jailbreaks are one type of [Prompt Injection attack](https://saif.google/secure-ai-framework/risks), causing an AI model to behave in ways that they've been trained to avoid (e.g., outputting unsafe content or leaking sensitive information). Prompt Injections generally cause the LLM to execute malicious "injected" instructions as part of data that were not meant to be executed by the LLM._ 

_Controls against prompt injection include input/output validation and sanitization as well as adversarial training and testing. Training, tuning, and evaluation processes also help fortify models against prompt injection._

![Example of a jailbreak prompt publicly available on Github](https://storage.googleapis.com/gweb-cloudblog-publish/images/adversarial-gen-ai.max-1000x1000.png)

Example of a jailbreak prompt publicly available [on GitHub](https://github.com/elder-plinius/L1B3RT4S/blob/main/GOOGLE.mkd)

Some malicious actors unsuccessfully attempted to prompt Gemini for guidance on abusing Google products, such as advanced phishing techniques for Gmail, assistance coding a Chrome infostealer, and methods to bypass Google's account creation verification methods. These attempts were unsuccessful. Gemini did not produce malware or other content that could plausibly be used in a successful malicious campaign. Instead, the responses consisted of safety-guided content and generally helpful, neutral advice about coding and cybersecurity. In our continuous work to protect Google and our users, we have not seen threat actors either expand their capabilities or better succeed in their efforts to bypass Google's defenses.

Findings: Government-Backed Threat Actors Misusing Gemini
---------------------------------------------------------

**At a Glance: Government-Backed Attackers**

Government-backed attackers attempted to use Gemini for coding and scripting tasks, gathering information about potential targets, researching publicly known vulnerabilities, and enabling post-compromise activities, such as defense evasion in a target environment. 

-   **Iran**: Iranian APT actors were the heaviest users of Gemini, using it for a wide range of purposes, including research on defense organizations, vulnerability research, and creating content for campaigns. APT42 focused on crafting phishing campaigns, conducting reconnaissance on defense experts and organizations, and generating content with cybersecurity themes. 
-   **China**: Chinese APT actors used Gemini to conduct reconnaissance, for scripting and development, to troubleshoot code, and to research how to obtain deeper access to target networks. They focused on topics such as lateral movement, privilege escalation, data exfiltration, and detection evasion. 
-   **North Korea**: North Korean APT actors used Gemini to support several phases of the attack lifecycle, including researching potential infrastructure and free hosting providers, reconnaissance on target organizations, payload development, and assistance with malicious scripting and evasion techniques. They also used Gemini to research topics of strategic interest to the North Korean government, such as the South Korean military and cryptocurrency. Of note, North Korean actors also used Gemini to draft cover letters and research jobs—activities that would likely support North Korea's efforts to [place clandestine IT workers](https://cloud.google.com/blog/topics/threat-intelligence/mitigating-dprk-it-worker-threat) at Western companies. 
-   **Russia**: With Russian APT actors, we observed limited use of Gemini during the period of analysis. Their Gemini use focused on coding tasks, including converting publicly available malware into another coding language and adding encryption functions to existing code.

Google analyzed Gemini activity associated with known APT actors and identified APT groups from more than 20 countries that used Gemini. The highest volume of usage was from Iran and China. APT actors used Gemini to support several phases of the attack lifecycle, including researching potential infrastructure and free hosting providers, reconnaissance on target organizations, research into vulnerabilities, payload development, and assistance with malicious scripting and evasion techniques. The top use cases by APT actors focused on: 

-   Assistance with coding tasks, including troubleshooting, tool and script development and converting or rewriting existing code 
    
-   Vulnerability research focused on publicly reported vulnerabilities and specific CVEs 
    
-   General research on various technologies, translations and technical explanations 
    
-   Reconnaissance about likely targets, including details about specific organizations 
    
-   Enabling post-compromise activity, such seeking advice on techniques to evade detection, escalate privileges or conduct internal reconnaissance in a target environment
    

We observed APT actors use Gemini attempting to support all phases of the attack lifecycle.

**Attack Lifecycle**

**Topics of Gemini Usage**

**Reconnaissance**

Attacker gathers information about the target

**Iran**

-   Recon on experts, international defense organizations, government organizations
    
-   Topics related to Iran-Israel proxy conflict
    

**North Korea**

-   Research on companies across multiple sectors and geos
    
-   Recon on US military and operations in South Korea 
    
-   Research free hosting providers
    

**China**

-   Research on US military, US-based IT service providers 
    
-   Understand public database of US intelligence personnel  
    
-   Research on target network ranges; determine domain names of targets 
    

**Weaponization**

Attacker develops or acquires tools to exploit target

-   Develop webcam recording code in C++
    
-   Convert Chrome infostealer function from Python to Node.js
    
-   Rewrite publicly available malware into another language
    
-   Add AES encryption functionality to provided code
    

**Delivery**

Attacker delivers weaponized exploit or payload to the target system

-   Better understanding advanced phishing techniques
    
-   Generating content for targeting a US defense organization
    
-   Generating content with cybersecurity and AI themes
    

**Exploitation**

Attacker exploits vulnerability to gain access

-   Reverse engineer endpoint detection and response (EDR) server components for health check and authentication
    
-   Access Microsoft Exchange using password hash
    
-   Research vulnerabilities in WinRM protocol
    
-   Understand publicly reported vulnerabilities, including Internet of Things (IoT) bugs
    

**Installation**

Attacker installs tools or malware to maintain access

-   Sign an Outlook Visual Studio Tools for Office (VSTO) plug-in and deploy it silently to all computers
    
-   Add a self-signed certificate to Active Directory
    
-   Research Mimikatz for Windows 11
    
-   Research Chrome extensions that provide parental controls and monitoring
    

**Command and control (C2)**

Attacker establishes communication channel with the compromised system

-   Generate code to remotely access Windows Event Log
    
-   Active Directory management commands
    
-   JSON Web Token (JWT) security and routing rules in Ruby on Rails
    
-   Character encoding issues in smbclient
    
-   Command to check IPs of admins on the domain controller
    

**Actions on objectives**

Attacker achieves their intended goal such as data theft or disruption

-   Automate workflows with Selenium (e.g. logging into compromised account)
    
-   Generate a PHP script to extract emails from Gmail into electronic mail (EML) files
    
-   Upload large files to OneDrive
    
-   Solution to TLS 1.3 visibility challenges
    

![Iran AI misuse](https://storage.googleapis.com/gweb-cloudblog-publish/images/iran-misuse-ai.max-1000x1000.png)

### Iranian Government-Backed Actors 

Iranian government-backed actors accounted for the largest Gemini use linked to APT actors. Across Iranian government-backed actors, we observed a broad scope of research and use cases, including to enable reconnaissance on targets, for research into publicly reported vulnerabilities, to request translation and technical explanations, and to create content for possible use in future campaigns. Their use reflected strategic Iranian interests including research focused on defense organizations and experts, defense systems, foreign governments, individual dissidents, the Israel-Hamas conflict, and social issues in Iran.

**At a Glance: Iranian APT Actors Using Gemini**

-   **Over 10 Iran-backed groups** observed using Gemini
-   **Google abuse-focused use cases:** 
    -   -   Researching methods for extracting data from Android devices, including SMS messages, accounts, contacts, and social media accounts
-   **Example use cases:** 
    -   -   Coding and scripting 
            -   -   PowerShell and Linux commands
                -   Python code for website scraping
                -   Debugging and improving a Ghidra script 
                -   Developing PHP scripts to collect and store user IP addresses and browser information in a MySQL database
                -   Assistance with C# programming
                -   Modifying assembly code 
                -   Help understanding error messages
        -   Vulnerability research
            -   -   Research on specific CVEs and technologies, such as WinRM and IoT devices
                -   Exploitation techniques and proof-of-concept code 
                -   Research on server-side request forgery (SSRF) exploitation techniques
                -   Research on the open-source router exploitation tool RomBuster 
        -   Research about organizations 
            -   -   International defense organizations
                -   Military and government organizations 
                -   Cybersecurity companies
                -   International organizations that monitor development of advanced weapons
        -   Research about warfare defenses
            -   -   Information on the Iran-Israel proxy conflict
                -   Unmanned aerial vehicles (UAV)
                -   Anti-drone systems
                -   Satellite technology
                -   Remote sensing technology
                -   Israel defense systems
        -   Generating content
            -   -   Generating content with cybersecurity and AI themes 
                -   Tailoring content to target a defense organization
                -   Translating various texts into Farsi, Hebrew, and English

#### Crafting Phishing Campaigns 

Over 30% of Iranian APT actors' Gemini use was linked to APT42. APT42's Gemini activity reflected the group's focus on crafting successful phishing campaigns. We observed the group using Gemini to conduct reconnaissance into individual policy and defense experts, as well as organizations of interest for the group. 

In addition to reconnaissance, APT42 used the text generation and editing capabilities of Gemini to craft material for phishing campaigns, including generating content with cybersecurity themes and tailoring the output to a US defense organization. APT42 also utilized Gemini for translation including localization, or tailoring content for a local audience. This includes content tailored to local culture and local language, such as asking for translations to be in fluent English.

#### Vulnerability Research

The majority of APT42's efforts focused on research into publicly known vulnerabilities, such as a request to generate a list of critical vulnerabilities from 2023. They also focused on vulnerabilities in specific products such as Mikrotik, Apereo, and Atlassian.

Of note, APT42 appeared to be researching how to use generative AI tools for offensive purposes, asking Gemini for help preparing training content for a red team focused on how offensive teams can use AI tools in their operations.  

#### Research Into Military and Weapons Systems 

APT42 also appears to have used Gemini's translation and explanation functions to better understand publicly available information on defense systems. Their efforts included general research into the Israel-Hamas conflict, as well as strategic trends in China's defense industry. The threat actor also used Gemini for technical explanations about US-made aerospace systems.

Another Iranian APT group also focused on understanding warfare defenses including specific research into satellite signal jamming and anti-drone systems. Other Iranian APT actors researched specific defense systems, including researching information about specific unmanned aerial vehicle (UAV) models, jamming F-35 fighter jets, anti-drone systems, and Israel's missile defense systems. 

![China AI misuse](https://storage.googleapis.com/gweb-cloudblog-publish/images/china-misuse-ai.max-1000x1000.png)

### People’s Republic of China (PRC) Government-Backed Actors

Government-backed actors linked to the People's Republic of China (PRC) attempted to use Gemini to enable reconnaissance on targets, for scripting and development, to request translation and explanation of technical concepts, and attempting to enable deeper access to a network following initial compromise. PRC threat actors' usage resembled an IT admin seeking to streamline, troubleshoot, or automate their tasks. In a malicious context, however, this activity could be used to enable lateral movement, privilege escalation, data exfiltration, and detection evasion.

**At a Glance: People's Republic of China APT Actors Using Gemini**

-   **Over 20 China-backed groups** observed using Gemini
-   **Notable use cases:** 
    -   -   Reconnaissance
            -   -   Research US military and US-based IT organizations
                -   Gather US government network ranges
                -   Understand publicly available information about US intelligence community personnel 
                -   Determine domain names of targets spanning eight countries, mostly government agencies
                -   Access Microsoft Exchange using password hash
        -   Vulnerability research
            -   -   Reverse engineer Carbon Black EDR's server components for health check and authentication
        -   Scripting and development
            -   -   Generate code to remotely access Windows Event Log
                -   Active Directory management commands
        -   Translation and explanation
            -   -   Understand graph databases (Nebula Graph)
                -   Solutions to TLS 1.3 visibility challenges
                -   Understand a malicious PHP script
                -   Web JWT security and routing rules in Ruby on Rails
        -   Deeper system access and post-compromise actions 
            -   -   Sign an Outlook VSTO plug-in and deploy it silently to all computers
                -   Add a self-signed certificate to Active Directory
                -   Upload large files to OneDrive
                -   Character encoding issues in smbclient
                -   Command to check IPs of admins on the Domain Controller
                -   Record passwords on the VMware vCenter
                -   Impacket troubleshooting

#### Enabling Deeper Access in a Target Network 

PRC-backed APT actors also used Gemini to work through scripting and development tasks, many of which appeared intended to enable deeper access in a target network after threat actors obtained initial access. For example, one PRC-backed group asked Gemini for assistance figuring out how to sign a plugin for Microsoft Outlook and silently deploy it to all computers. The same actor also asked Gemini to generate code to remotely access Windows Event Log; sought instructions on how to add a self-signed certificate to Active Directory; and asked Gemini for a command to identify the IP addresses of administrators on the domain controller. Other actors used Gemini for help troubleshooting Chinese character encoding issues in smbclient and how to record passwords on the VMware vCenter. 

In another example, PRC-backed APT actors asked Gemini for assistance with Active Directory management commands and requested help troubleshooting impacket, a Python-based tool for working with network protocols. While impacket is commonly used for benign purposes, the context of the threat actor made it clear that the actor was using the tool for malicious purposes. 

#### Explaining Tools, Concepts, and Code 

PRC actors utilized Gemini to learn about specific tools and technologies and develop solutions to technical challenges. For example, a PRC APT actor used Gemini to break down how to use the graph database Nebula Graph. In another instance, the same actor used Gemini to offer possible solutions to TLS 1.3 visibility challenges. Another PRC-backed APT group sought to understand a malicious PHP script. 

#### Vulnerability Research and Reverse Engineering

In one case, a PRC-backed APT actor attempted unsuccessfully to get Gemini's help reverse engineering the endpoint detection and response (EDR) tool Carbon Black. The same threat actor copied disassembled Python bytecode into Gemini to convert the bytecode into Python code. It's not clear what their objective was. 

#### Unsuccessful Attempts to Elicit Internal System Information From Gemini

In one case, the PRC-backed APT actor APT41 attempted unsuccessfully to use Gemini to learn about Gemini's underlying infrastructure and systems. The actor asked Gemini to share details such as its IP address, kernel version, and network configuration. Gemini responded but did not disclose sensitive information. In a helpful tone, the responses provided publicly available details that would be widely known about the topic, while also indicating that the requested information is kept secret to prevent unauthorized access. 

![north Korea AI misuse](https://storage.googleapis.com/gweb-cloudblog-publish/images/nk-misuse-ai.max-1000x1000.png)

### North Korean Government-Backed Actors

North Korean APT actors used Gemini to support several phases of the attack lifecycle, including researching potential infrastructure and free hosting providers, reconnaissance on target organizations, payload development, and assistance with malicious scripting and evasion techniques. They also used Gemini to research topics of strategic interest to the North Korean government, such as South Korean nuclear technology and cryptocurrency. We also observed that North Korean actors were using LLMs in likely attempts to enable North Korea's efforts to [place clandestine IT workers](https://cloud.google.com/blog/topics/threat-intelligence/mitigating-dprk-it-worker-threat) at Western companies.

**At a Glance: North Korean APT Actors Using Gemini**

-   **Nine North Korea-backed groups** observed using Gemini
-   **Google-focused use cases:** 
    -   -   Research advanced techniques for phishing Gmail
        -   Scripting to steal data from compromised Gmail accounts
        -   Understanding a Chrome extension that provides parental controls (capable of taking screenshots, keylogging)
        -   Convert Chrome infostealer function from Python to Node.js
        -   Bypassing restrictions on Google Voice 
        -   Generate code snippets for a Chrome extension 
-   **Notable use cases:** 
    -   -   Enabling clandestine IT worker scheme 
            -   -   Best Discord servers for freelancers
                -   Exchange with overseas employees 
                -   Jobs on LinkedIn 
                -   Average salary
                -   Drafting work proposals
                -   Generate cover letters from job postings
        -   Research on topics 
            -   -   Free hosting providers 
                -   Cryptocurrency
                -   Operational technology (OT) and industrial networks 
                -   Nuclear technology and power plants in South Korea
                -   Historic cyber events (e.g., major worms and DDoS attacks; Russia-Ukraine conflict) and cyber forces of foreign militaries
        -   Research about organizations 
            -   -   Companies across 11 sectors and 13 countries
                -   South Korean military 
                -   US military 
                -   German defense organizations
        -   Malware development 
        -   Evasion techniques
        -   Automating workflows for logging into compromised accounts 
        -   Understanding Mimikatz for Windows 11 
        -   Scripting and troubleshooting

#### Clandestine IT Worker Threat

North Korean APT actors used Gemini to draft cover letters and research jobs—activities that would likely support [efforts by North Korean nationals](https://cloud.google.com/blog/topics/threat-intelligence/mitigating-dprk-it-worker-threat) to use fake identities and obtain freelance and full-time jobs at foreign companies while concealing their true identities and locations. One North Korea-backed group utilized Gemini to draft cover letters and proposals for job descriptions, researched average salaries for specific jobs, and asked about jobs on LinkedIn. The group also used Gemini for information about overseas employee exchanges. Many of the topics would be common for anyone researching and applying for jobs. 

While normally employment-related research would be typical for any job seeker, we assess the usage is likely related to North Korea's [ongoing efforts](https://www.ic3.gov/Media/Y2024/PSA240516) to place clandestine workers in freelance gigs or full-time jobs at Western firms. The scheme, which [involves](https://www.justice.gov/usao-dc/pr/charges-and-seizures-brought-fraud-scheme-aimed-denying-revenue-workers-associated-north) thousands of North Korean workers and [has affected](https://www.justice.gov/usao-dc/pr/charges-and-seizures-brought-fraud-scheme-aimed-denying-revenue-workers-associated-north) hundreds of US-based companies, uses IT workers with false identities to complete freelance work and send wages back to the North Korean regime.

**North Korea’s AI toolkit** 

Outside of their use of Gemini, North Korean cyber threat actors have shown a long-standing interest in AI tools. They likely use AI applications to augment malicious operations and improve efficiency and capabilities, and for producing content to support their campaigns, such as phishing lures and profile photos for fake personas. We assess with high confidence that North Korean cyber threat actors will continue to demonstrate an interest in these emerging technologies for the foreseeable future. 

DPRK IT Workers

We have observed [DPRK IT Workers](https://cloud.google.com/blog/topics/threat-intelligence/mitigating-dprk-it-worker-threat) leverage accounts on assistive writing tools, Monica (monica.im) and Ahrefs (ahrefs.com), which could potentially aid the group's work despite a lack of language fluency. Additionally, the group has maintained accounts on Data Annotation Tech, a company hiring individuals to train AI models. Notably, a profile photo used by a suspected IT worker bore a noticeable resemblance to multiple different images on the internet, suggesting that a manipulation tool was used to generate the threat actor's profile photo. 

APT43

Google Threat Intelligence Group (GTIG) has detected evidence of [APT43](https://cloud.google.com/blog/topics/threat-intelligence/apt43-north-korea-cybercrime-espionage) actors accessing multiple publicly available LLM tools; however, the intended purpose is not clear. Based on the capabilities of these platforms and historical APT43 activities, it is possible these applications could be used in the creation of rapport-building emails, lure content, and malicious PowerShell and scripting efforts. 

-   GTIG has detected APT43 actors reference publicly available AI chatbot tools alongside the topic "북핵 해결" (translation: "North Korean nuclear issue solution"), indicating the group is using AI applications to conduct technical research as well as open-source analysis on South Korean foreign and military affairs and nuclear issues. 
    
-   GTIG has identified APT43 actors accessing multiple publicly available AI image generation tools, including tools used for image manipulation and creating realistic-looking human portraits. 
    

#### Target Research and Reconnaissance

North Korean actors also engaged with Gemini with several questions that appeared focused on conducting initial research and reconnaissance into prospective targets. They also researched organizations and industries that are typical targets for North Korean actors, including the US and South Korean militaries and defense contractors. One North Korean APT group asked Gemini for information about companies and organizations across a variety of industry sectors and regions. Some of this Gemini usage related directly to organizations that the same group had attempted to target in phishing and malware campaigns that Google previously detected and disrupted.

In addition to research into companies, North Korean APT actors researched nuclear technology and power plants in South Korea, such as site locations, recent news articles, and the security status of the plants. Gemini responded with widely available, public information and facts that would be easily discoverable in an online search. 

#### Help with Scripting, Payload Development, Defense Evasion 

North Korean actors also tried to use Gemini to assist with development and scripting tasks. One North Korea-backed group attempted to use Gemini to help develop webcam recording code in C++. Gemini provided multiple versions of code, and repeated efforts by the actor potentially suggested their frustration by Gemini's answers. The same group also asked Gemini to generate a robots.txt file to block crawlers and an .htaccess file to redirect all URLs except CSS extensions. 

One North Korean APT actor used Gemini for assistance developing code for sandbox evasion. For example, the threat actor utilized Gemini to write code in C++ to detect VM environments and Hyper-V virtual machines. Gemini provided responses with short code snippets to perform simple sandbox checks. The same group also sought help troubleshooting Java errors when implementing AES encryption, and separately asked Gemini if it is possible to acquire a system password on Windows 11 using Mimikatz. 

![Russia AI misuse](https://storage.googleapis.com/gweb-cloudblog-publish/images/russia-misuse-ai.max-1000x1000.png)

### Russian Government-Backed Actors 

During the period of analysis, we observed limited use of Gemini by Russia-backed APT actors. Of this limited use, the majority of usage appeared benign, rather than threat-enabling. The reasons for this low engagement are unclear. It is possible Russian actors avoided Gemini out of operational security considerations, staying off Western-controlled platforms to avoid monitoring of their activities. They may be using AI tools produced by Russian firms or locally hosting LLMs, which would ensure full control of their infrastructure. Alternatively, they may have favored other Western LLMs.

One Russian government-backed group used Gemini to request help with a handful of tasks, including help rewriting publicly available malware into another language, adding encryption functionality to code, and explanations for how a specific block of publicly available malicious code functions.

**At a Glance: Russian APT Actors Using Gemini**

-   **Three Russia-backed groups** observed using Gemini
-   **Notable use cases:** 
    -   -   Scripting
            -   -   Help rewriting public malware into another language
        -   Payload crafting
            -   -   Add AES encryption functionality to provided code
        -   Translation and explanation 
            -   -   Understand how some public malicious code works

### Financially Motivated Actors Using LLMs

Threat actors in underground marketplaces are advertising ways to bypass security guardrails to help LLMs with malware development, phishing, and other malicious tasks. The offerings include jailbroken LLMs that are ready-made for malicious use. 

Throughout 2023 and 2024, Google Threat Intelligence Group (GTIG) observed underground forum posts related to LLMs, indicating there is a burgeoning market for nefarious versions of LLMs. Some advertisements boast customized and jailbroken LLMs that don't have restrictions for malware development purposes, or they tout a lack of security measures typically found on legitimate services, allowing the user to prompt the LLM about any topic or task without incurring security guardrails or limits on their queries. [Examples](https://www.darkreading.com/threat-intelligence/fraudgpt-malicious-chatbot-for-sale-dark-web) [include](https://krebsonsecurity.com/2023/08/meet-the-brains-behind-the-malware-friendly-ai-chat-service-wormgpt/) FraudGPT, which has been advertised on Telegram as having no limitations, and WormGPT, a privacy focused, "uncensored" LLM capable of developing malware.   

Financially motivated actors are using LLMs to help augment business email compromise (BEC) operations. GTIG has noted evidence of financially motivated actors using manipulated video and voice content in business email compromise (BEC) scams. Media reports indicate that financially motivated actors have reportedly used WormGPT to create more persuasive BEC messages.

Findings: Information Operations (IO) Actors Misusing Gemini
------------------------------------------------------------

**At a Glance: Information Operations Actors** 

IO actors attempted to use Gemini for research, content generation, translation and localization, and to find ways to increase their reach.

-   **Iran:** Iranian IO actors used Gemini for a wide range of tasks, accounting for three quarters of all IO prompts. They used Gemini for content creation and manipulation, including generating articles, rewriting text with a specific tone, and optimizing it for better reach. Their activity also focused on translation and localization, adapting content for different audiences, and for general research into news, current events, and political issues. 
-   **China**: Pro-China IO actors used Gemini primarily for general research on various topics, including a variety of topics of strategic interest to the Chinese government. The most prolific IO actor we track, DRAGONBRIDGE, was responsible for the majority of this activity. They also used Gemini to research current events and politics, and in a few cases, they used Gemini to generate articles or content on specific topics.
-   **Russia**: Russian IO actors used Gemini primarily for general research, content creation, and translation. For example, their use involved assistance drafting content, rewriting article titles, and planning social media campaigns. Some activity demonstrated an interest in developing AI capabilities, asking for information on tools for creating online AI chatbots, developer tools for interacting with LLMs, and options for textual content analysis.

IO actors used Gemini for research, content generation including developing personas and messaging, translation and localization, and to find ways to increase their reach. Common use cases include general research into news and current events as well as specific research into individuals and organizations. In addition to creating content for campaigns, including personas and content, the actors researched increasing the efficacy of campaigns, including automating distribution, using search engine optimization (SEO) to optimize the reach of campaigns, and increasing operational security. As with government-backed groups, IO actors also used Gemini for translation and localization and for understanding the meanings or context of content. 

![Iran misuse IO](https://storage.googleapis.com/gweb-cloudblog-publish/images/iran-misuse-io.max-1000x1000.png)

### Iran-Linked Information Operations Actors

Iran-based information operations (IO) groups used Gemini for a wide range of tasks, including general research, translation and localization, content creation and manipulation, and generating content with a specific bias or tone. We also observed Iran-based IO actors engage with Gemini about news events and ask Gemini to provide details on economic and political issues in Iran, the US, the Middle East, and Europe.

In line with their practice of mixing original and borrowed content, Iranian IO actors translated existing material, including news-like articles. They then used Gemini to explain the context and meaning of particular phrases within the given text.

Iran-based IO actors also used Gemini to localize the content, seeking human-like translation and asking Gemini for help with tasks like making the text sound like a native English speaker. They used Gemini to manipulate text (e.g., asking for help rewriting existing text on immigration and crime in a specific style or tone). 

Iran's activity also included research about improving the reach of their campaigns. For example, they attempted to generate SEO-optimized content, likely in an effort to reach a larger audience. Some actors also used Gemini to suggest strategies for increasing engagement on social media.

**At a Glance: Iran-Linked IO Actors Using Gemini**

-   **Eight Iran-linked IO groups** observed using Gemini 
-   **Example use cases:** 
    -   -   Content creation - text 
            -   -   Generate article titles
                -   Generate SEO-optimized content and titles
                -   Draft a report critical of Bahrain 
                -   Draft titles and hashtags in English and Farsi for videos that are catchy or create urgency to watch the content
                -   Draft titles and descriptions promoting Islam
        -   Translation - content in / out of native language
            -   -   Translate into Farsi-provided texts about a variety of topics, including the Iranian election, human rights, international law, Islam, and other topics
                -   Translate Farsi-language idioms and proverbs to other languages
                -   Translate news about the US economy, US government, and politics into Farsi, using a specified tone
                -   Draft a French-language headline to get viewers to engage with specific content
        -   Content manipulation - copy editing to refine content
            -   -   Reformulate specific text about Sharia law
                -   Paraphrase content describing specific improvements to Iran's export economy
                -   Rewrite a provided text about diplomacy and economic challenges with countries like China and Germany
                -   Provide synonyms for specific words or phrases
                -   Rewrite provided text about Islam and Iraq in different styles or tones
                -   Proofread provided content 
        -   Content creation - biased text
            -   -   Generate or reformulate text to criticize a government minister and other individuals for failures or other actions
                -   Describe how a popular American TV show perpetuates harmful stereotypes
                -   Generate Islam-themed titles for thumbnail previews on social media
        -   General research - news and events
            -   -   Provide an overview of current events in specific regions
                -   Research about the Iran-Iraq war
                -   Define specific terms
                -   Suggest social media channels for information about Islam and the Quran
                -   Provide information on countries' policies toward the Middle East
        -   Create persona - photo generation
            -   -   Create a logo

![China misuse IO](https://storage.googleapis.com/gweb-cloudblog-publish/images/china-misuse-io.max-1000x1000.png)

### PRC-Linked Information Operations Actors

IO actors linked to the People's Republic of China (PRC) used Gemini primarily for general research on a wide variety of topics. The most prolific IO actor we track, the pro-China group [DRAGONBRIDGE](https://blog.google/threat-analysis-group/google-disrupted-dragonbridge-activity-q1-2024/), was responsible for approximately three quarters of this activity. Of their activity, the majority use was general research about a wide variety of topics, ranging from details about the features of various social media platforms to questions about various topics of strategic interest to the PRC government. Actors researched information on current events and politics in other regions, with a focus on the US and Taiwan. They also showed interest in assessing the impact and risk of certain events. In a handful of cases, DRAGONBRIDGE used Gemini to generate articles or content on specific topics.

**At a Glance: PRC-Linked IO Actors Using Gemini**

-   **Three PRC-linked IO groups** observed using Gemini 
-   **Example use cases:** 
    -   -   General research - political and social topics
            -   -   Research about specific countries, organizations, and individuals 
                -   Research relations between specific countries and China
                -   Research on topics sensitive to the the Chinese government (e.g., five poisons) 
                -   Research on Taiwanese politicians and their actions toward China
                -   Research on US politics and political figures and their attitudes on China
                -   Research foreign press coverage about China
                -   Summarize key takeaways from a video
        -   General research - technology 
            -   -   Compare functionality and features of different social media platforms 
                -   Explain technical concepts and suggestions for useful tools
        -   Translation - content in / out of native language 
            -   -   Translate and summarize text between Chinese and other languages 
        -   Content creation - text
            -   -   Draft articles on topics such as the use of AI and social movements in specific regions
                -   Generate a summary of a movie trailer about a Chinese dissident
        -   Create persona - text generation
            -   -   Generate a company profile for a media company

DRAGONBRIDGE has experimented with other generative AI tools to create synthetic content in support of their IO campaigns. As early as 2022, the group [used](https://public-assets.graphika.com/reports/graphika-report-deepfake-it-till-you-make-it.pdf) a commercial AI service in videos on YouTube to depict AI-generated news presenters. Their use of AI-generated video [continued](https://blog.google/threat-analysis-group/google-disrupted-dragonbridge-activity-q1-2024/) through 2024 but has not resulted in significantly higher engagement from real viewers. Google detected and [terminated](https://blog.google/threat-analysis-group/tag-bulletin-q4-2024/) the channels distributing this content immediately upon discovery. DRAGONBRIDGE's use of AI-generated videos or images [has not resulted](https://blog.google/threat-analysis-group/google-disrupted-dragonbridge-activity-q1-2024/) in significantly higher engagement from real viewers.

![Russia misuse IO](https://storage.googleapis.com/gweb-cloudblog-publish/images/russia-misuse-io.max-1000x1000.png)

### Russia-Linked Information Operations Actors

Russian IO actors used Gemini for general research, content creation, and translation. Half of this activity was associated with the Russian IO actor we track as [KRYMSKYBRIDGE](https://services.google.com/fh/files/blogs/google_fog_of_war_research_report.pdf), which is linked to a Russian consulting firm that works with the Russian government. Approximately 40% of activity was linked to actors associated with Russian state sponsored entities [formerly controlled](https://blog.google/threat-analysis-group/prigozhin-interests-and-russian-information-operations/) by the late Russian oligarch [Yevgeny Prigozhin](https://cloud.google.com/blog/topics/threat-intelligence/io-campaigns-russian-prigozhin-persist?e=48754805). We also observed usage by actors tracked publicly as Doppelganger. 

The majority of Russian IO actor usage was related to general research tasks, ranging from the Russia-Ukraine war to details about various tools and online services. Russian IO actors also used Gemini for content creation, rewriting article titles and planning social media campaigns. Translation to and from Russian was also a common task. 

Russian IO actors focused on the generative AI landscape, which may indicate an interest in developing native capabilities in AI on infrastructure they control. They researched tools that can be used to create an online AI chatbot and developer tools for interacting with LLMs. One Russian IO actor used Gemini to suggest options for textual content analysis. 

Pro-Russia IO actors have used AI in their influence campaigns in the past. In 2024, the actor known as CopyCop likely used LLMs to generate content, and some stories on their sites [included metadata](https://go.recordedfuture.com/hubfs/reports/cta-2024-0509.pdf) indicating an LLM was prompted to rewrite articles from genuine news sources with a particular political perspective or tone. CopyCop's inauthentic news sites pose as US- and Europe-based news outlets and post Kremlin-aligned views on Western policy, the war in Ukraine, and domestic politics in the US and Europe.

**At a Glance: Russia-Linked IO Actors Using Gemini**

-   **Four Russia-linked IO groups** observed using Gemini
-   **Example use cases:** 
    -   -   General research
            -   -   Research into the Russia-Ukraine war 
                -   Explain subscription plans and API details for online services
                -   Research on different generative AI platforms, software, and systems for interacting with LLMs 
                -   Research on tools and methods for creating an online chatbot 
                -   Research tools for content analysis  
        -   Translation - content in / out of native language
            -   -   Translate technical and business terminology into Russian 
                -   Translate text to/from Russian
        -   Content creation - text 
            -   -   Draft a proposal for a social media agency 
                -   Rewrite article titles to garner more attention 
        -   Plan and strategize campaigns 
            -   -   Develop content strategy for different social media platforms and regions  
                -   Brainstorm ideas for a PR campaign and accompanying visual designs

Building AI Safely and Responsibly 
-----------------------------------

We believe our approach to AI must be both bold and responsible. To us, that means developing AI in a way that maximizes the positive benefits to society while addressing the challenges. Guided by our [AI Principles](https://ai.google/responsibility/responsible-ai-practices/), Google designs AI systems with robust security measures and strong safety guardrails, and we continuously test the security and safety of our models to improve them. Our [policy guidelines](https://gemini.google/policy-guidelines/?hl=en) and prohibited use [policies](https://policies.google.com/terms/generative-ai/use-policy) prioritize safety and responsible use of Google's generative AI tools. Google's [policy development process](https://transparency.google/our-approach/our-policy-process/) includes identifying emerging trends, thinking end-to-end, and designing for safety. We continuously enhance safeguards in our products to offer scaled protections to users across the globe.  

At Google, we leverage threat intelligence to disrupt adversary operations. We investigate abuse of our products, services, users and platforms, including malicious cyber activities by government-backed threat actors, and work with law enforcement when appropriate. Moreover, our learnings from countering malicious activities are fed back into our product development to improve safety and security for our AI models. Google DeepMind also develops threat models for generative AI to identify potential vulnerabilities, and creates new evaluation and training techniques to address misuse caused by them. In conjunction with this research, DeepMind has shared how they're actively deploying defenses within AI systems along with measurement and monitoring tools, one of which is a [robust evaluation framework](https://security.googleblog.com/2025/01/how-we-estimate-risk-from-prompt.html?m=1) used to automatically red team an AI system's vulnerability to indirect prompt injection attacks. Our AI development and Trust & Safety teams also work closely with our threat intelligence, security, and modelling teams to stem misuse.

The potential of AI, especially generative AI, is immense. As innovation moves forward, the industry needs security standards for building and deploying AI responsibly. That's why we introduced the [Secure AI Framework (SAIF)](https://blog.google/technology/safety-security/introducing-googles-secure-ai-framework/), a conceptual framework to secure AI systems. We've shared a comprehensive [toolkit for developers](https://ai.google.dev/) with [resources and guidance](https://ai.google.dev/responsible) for designing, building, and evaluating AI models responsibly. We've also shared best practices for [implementing safeguards](https://ai.google.dev/responsible/docs/safeguards), [evaluating model safety](https://ai.google.dev/responsible/docs/evaluation#red-teaming), and [red teaming](https://blog.google/technology/safety-security/googles-ai-red-team-the-ethical-hackers-making-ai-safer/) to test and secure AI systems. 

About the Authors
-----------------

Google Threat Intelligence Group brings together the Mandiant Intelligence and Threat Analysis Group (TAG) teams, and focuses on identifying, analyzing, mitigating, and eliminating entire classes of cyber threats against Alphabet, our users, and our customers. Our work includes countering threats from government-backed attackers, targeted 0-day exploits, coordinated information operations (IO), and serious cyber crime networks. We apply our intelligence to improve Google's defenses and protect our users and customers.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/adversarial-misuse-generative-ai/)

<br/>
---
