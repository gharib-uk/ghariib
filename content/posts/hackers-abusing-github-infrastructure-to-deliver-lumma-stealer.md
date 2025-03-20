---
title: "Hackers Abusing GitHub Infrastructure to Deliver Lumma Stealer"
date: 2025-02-01
---

Cybersecurity researchers have uncovered a sophisticated campaign leveraging GitHub’s trusted release infrastructure to distribute the Lumma Stealer malware.

This information-stealing malware, part of a growing trend of cybercriminals abusing legitimate platforms, poses significant risks by exfiltrating sensitive data and deploying additional malicious payloads.

The attackers utilized GitHub repositories to host malicious files disguised as legitimate software.

In one example, users were tricked into downloading files such as `Pictore.exe` and `App_aelGCY3g.exe` from GitHub-hosted URLs.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhXLSBMNjJQ4XSpOe3vQ-KwCwo4vskavAgXBlPRPvePEYGCYNTlYiebTtFlBT4rix-okBu3785xQyqweUy1faMs-ZiRguCiYacCvHU69UATaxOSWuVX5cn_Cw-G4ZRSW0tFZJrzynjyoUfH2inZF3kSeynyepBKGhx1dTk-FxnUMDLUFR_qv2UBJYEDuus/s16000/Downloading%20malicious%20file%20'Pictore.exe'%20from%20GitHub%20repository%20(Source%20-%20TrendMicro).webp)

<figcaption>

Downloading malicious file ‘Pictore.exe’ from GitHub repository (Source – TrendMicro)

</figcaption>

</figure>

These files were signed with revoked certificates from ConsolHQ LTD and Verandah Green Limited, adding an initial layer of credibility before being flagged as malicious.

The URLs used for distribution included pre-signed links with parameters like `X-Amz-Expires=300`, ensuring the download links remained active only for a short duration.

Analsysts at Trend Micro identified that this tactic limited detection opportunities while maintaining a sense of urgency for victims.

## **Malware Capabilities**

Once executed, Lumma Stealer initiates several malicious activities:-

1. **Data Exfiltration**: The malware targets credentials, cryptocurrency wallets, browser data (e.g., cookies, autofill information), and local system configurations. It communicates with command-and-control (C2) servers at IPs such as `192[.]142[.]10[.]246` and `192[.]178[.]54[.]36` using HTTP POST requests.

4. **Persistence Mechanisms**: Lumma Stealer employs PowerShell scripts and shell commands to establish persistence. For instance:

```
   powershell.exe -NoProfile -NoLogo -InputFormat Text -NoExit ExecutionPolicy Unrestricted -Command
```

This command allows unrestricted script execution while avoiding detection.

3. **Payload Deployment**: The malware drops additional tools like SectopRAT and Vidar within temporary directories. These tools further compromise the system by stealing browser data or injecting processes.

6. **File Extraction**: Using embedded utilities like `nsis7z.dll`, Lumma Stealer extracts files from archives such as `app-64.7z`.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgpf2oKmbOEsuxgTF7ICrx7Rl3bKOGPctFobZVePpNa_dK4xWz_8eCbXxlpX7Qj7f16fPqpI2eAMK4cA-x8JS_xNv9abcCIk3bO4MKNUhF8-qvl5kvF8_5IQyUKHjgHIXPaFFs-eNxnipiZAh6UHG02ZnRABjz_Lw5VQIsxSiP2GSdL7YgZ5fZ8Ubc7TsQ/s16000/Extracted%20files%20from%20archive%20'app-64.7z'%20(Source%20-%20TrendMicro).webp)

<figcaption>

Extracted files from archive ‘app-64.7z’ (Source – TrendMicro)

</figcaption>

</figure>

The extracted files include components like `chrome_100_percent.pak` and `snapshot_blob.bin`, indicating potential use of Electron-based apps for malicious purposes.

<figure>

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_lXWaI-XIKzeNEC_4W4QUNvA7JJCu9P9aMPIYUbDqcT4W0iaNZgb0HrLbV2Wp_iSSS9txmbK9_6_INKSwXQ0phFGjcaEo82VVBZoeMhOcXkMV36B2-6rC_1f2BXqxv7fWiw2SQJ87NP5fHkY53v-0xxNfgbARC9G4Br_pNT_8J-fTWerdAaNxvXMIwX0/s16000/Extracted%20files%20(Source%20-%20TrendMicro).webp)

<figcaption>

Extracted files (Source – TrendMicro)

</figcaption>

</figure>

The campaign aligns with tactics used by the Stargazer Goblin group, known for exploiting trusted platforms like GitHub and compromised websites for payload distribution.

The attackers demonstrated adaptability by combining multiple malware families—Lumma Stealer, SectopRAT, Vidar—into a modular attack framework.

To defend against threats like Lumma Stealer, always validate URLs and digital certificates before downloading files, and use endpoint security solutions to detect unauthorized shell commands.

Blocking communication with known malicious IPs enhances protection, while employee training helps identify phishing attempts.

Additionally, regularly patching systems and enabling MFA further strengthens security.

## **Indicators of Compromise**

- File names: `Pictore.exe`, `App_aelGCY3g.exe`

- C2 IPs: `192[.]142[.]10[.]246`, `84[.]200[.]24[.]26`

- Shell commands:

```
   cmd /c copy /b ..Sen + ..Silver + ..Reprints t
```

This connects the files for staging data.

**`Collect Threat Intelligence with TI Lookup to Improve Your Company’s Security - Get 50 Free Request`**

The post Hackers Abusing GitHub Infrastructure to Deliver Lumma Stealer appeared first on Cyber Security News.

Go to Source
