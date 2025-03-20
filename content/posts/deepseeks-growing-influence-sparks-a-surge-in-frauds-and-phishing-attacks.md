---
title: "DeepSeek’s Growing Influence Sparks a Surge in Frauds and Phishing Attacks"
date: 2025-02-01
---

The rapid rise of DeepSeek, a Chinese artificial intelligence (AI) company, has not only disrupted the AI industry but also attracted the attention of cybercriminals.

As its AI Assistant app became the most downloaded free app on the iOS App Store in January 2025, surpassing OpenAI’s ChatGPT, malicious actors have exploited its popularity to launch phishing campaigns, investment scams, and malware attacks.

Cybercriminals have created fraudulent websites mimicking DeepSeek’s platform to target cryptocurrency users.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAxPEcEqW13SriF0Ai9nQKwekjABJRAPkEy-beTB1qVkcjeepmrHtmssxk37SPiToh_TfY6LK7lGpCX4bwpd41MbmzAV5regAm_nHCPuS2mWzBf9Qz-Vj050fEGouwW-5kissMC2CYw4u-4-tD_hHXhvFajHM3ZUcXDYWReeogBHoDdD-qlkeqcTDn0fE/s16000/Crypto%20phishing%20website%20impersonating%20DeepSeek%20(Source%20-%20Cyble).webp)

<figcaption>

Crypto phishing website impersonating DeepSeek (Source – Cyble)

</figcaption>

</figure>

These sites, such as `abs-register[.]com` and `deep-whitelist[.]com`, lure victims into connecting their cryptocurrency wallets.

Upon scanning a QR code presented on these fake platforms, users unknowingly compromise their wallets, leading to potential loss of funds.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjDeBcG5QvlowWvqLxBj3GI80s1cyr_OiLij3UaZUh5ptjgq7fJTJ76IOFXaRyLxLW4XHwX-CIY92hNBfnlgox_9Rtcafgfu5Y1jIXlzJnhKkPY0nZhM76XZ01GtMR05iVx9kUVtlIhRRyNDI5KJbjuUP28PoKnnjF0hk6ulGNRDqyYXqI5xChRBw-gPWw/s16000/Phishing%20site%20displaying%20QR%20code%20(Source%20-%20Cyble).webp)

<figcaption>

Phishing site displaying QR code (Source – Cyble)

</figcaption>

</figure>

Besides this, analysts at Cyble discovered that the phishing sites often impersonate legitimate wallet services like MetaMask and WalletConnect, making them highly convincing.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiB8Kx4_OZqKTldwqs4Uqr_HOnfHnRz-h1qaNiOOuGjqs6SKk6uZlE6ifobMlVmLF548kXbKH05I48zbsMcQuDJuSaFOGPkQj7Ketzbd9KBslHRqYlZSTO88HxmWjZgwbdpKqPngN7GJyooCCW9l2l1tYuqddB09IEjpoVBlAtiff0_IUstopvtO2-bO7g/s16000/Phishing%20websites%20presenting%20a%20list%20of%20different%20crypto%20wallets%20(Source%20-%20Cyble).webp)

<figcaption>

Phishing websites presenting a list of different crypto wallets (Source – Cyble)

</figcaption>

</figure>

Here below we have mentioned the technical details:-

- **Targeted Wallets:** MetaMask, WalletConnect

- **Attack Mechanism:** QR code phishing

- **Example URLs:**

- `hxxp://abs-register[.]com`

- `hxxps://deep-whitelist[.]com`

## **Surge in Frauds & Phishing Attacks**

Another prevalent scam involves fake cryptocurrency tokens marketed as “DeepSeekAI Agent.”

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjQen3B7sc8MsZ12WQayYzMBcxwsbcbz-7tylVNmjvuRu5gOZrnKUnDmN2k-4o7wpbLd-XgTHivbLW5BxsgxGgDW_eDsyvCaEc4Sm4fsk-94HUID5l-IhO424-qGcb2sWu7JEbQHyc-UhtG4Rrhi_akpKaKqe246EYOyId_mNOdIb81Jps9AOp4fwTM27o/s16000/Fraud%20website%20promoting%20DeepSeekAI%20Agent%20token%20(Source%20-%20Cyble).webp)

<figcaption>

Fraud website promoting DeepSeekAI Agent token (Source – Cyble)

</figcaption>

</figure>

Fraudulent sites provide a wallet address for purchasing these tokens but prevent victims from withdrawing or trading them. The token address `0x27238b76965387f5628496d1e4d2722b663d2698` has been blacklisted as a honeypot.

Domains like `deepseek-shares[.]com` falsely advertise pre-IPO shares of DeepSeek to deceive investors.

DeepSeek remains privately held with no IPO announcements, and these scams aim to harvest sensitive personal information for further exploitation.

Threat actors have also used DeepSeek’s name to distribute malware disguised as legitimate downloads for its app.

Malicious files such as AMOS Stealer have been detected in the wild. These malware samples are capable of data exfiltration, credential theft, and remote command execution.

Malware indicators:-

- **File Names:** Variants starting with “DeepSeek”

- **SHA256 Hashes:**

- `e596da76aaf7122176eb6dac73057de4417b7c24378e00b10c468d7875a6e69e`

- `a3d06ffcb336cba72ae32e4d0ac5656400decfaf40dc28862de9289254a47698`

DeepSeek’s open-source language models (LLMs) have proven vulnerable to jailbreaking techniques like “Crescendo” and “Deceptive Delight.”

These methods bypass safety protocols, enabling the generation of harmful outputs such as phishing templates, keylogger scripts, and even chemical weapon instructions.

Example code output:-

```
import win32com.client

def execute_command(command):
    shell = win32com.client.Dispatch("WScript.Shell")
    shell.Run(command)
```

This script demonstrates how attackers could use DeepSeek-generated code for malicious purposes.

In addition to targeted scams, DeepSeek recently exposed over one million sensitive records due to an unsecured database.

The breach included API keys, chat logs, and backend operational data. While the issue was quickly resolved, it underscores the platform’s security vulnerabilities.

Users should verify official sources before engaging with any DeepSeek-related content and avoid scanning unverified QR codes or downloading apps from unofficial websites.

Using strong antivirus software helps detect potential threats, while staying informed about cybersecurity best practices enhances overall protection against risks.

**`Collect Threat Intelligence with TI Lookup to Improve Your Company’s Security - Get 50 Free Request`**

The post DeepSeek’s Growing Influence Sparks a Surge in Frauds and Phishing Attacks appeared first on Cyber Security News.

Go to Source
