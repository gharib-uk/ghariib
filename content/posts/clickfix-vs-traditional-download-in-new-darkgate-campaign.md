---
title: "ClickFix vs. traditional download in new DarkGate campaign"
date: 2025-02-01
---

During the past several months there have been numerous malware campaigns that use a technique something referred to as “ClickFix”. It often consists of a fake CAPTCHA or similar traffic validation page where visitors are instructed to paste and execute code in order to proceed.

We have started to see ClickFix attacks more and more via malicious Google ads as well. This is in contrast to typical phishing pages where victims download a so-called installer that contains malware.

In a recent malvertising campaign targeting the Notion brand, we observed these two techniques used at play. It’s quite possible the threat actors were collecting metrics to determine which one of the two gave them the most conversions via malware installs.

This blog post details this campaign that ultimately delivered the DarkGate malware loader.

## Overview

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_ac5d87.png)

## Web traffic view

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_4a6cf7.png)

## Delivery #1: PowerShell code via “ClickFix”

### Malicious ad and social engineering

Threat actors created a Google ad for the popular utility application Notion. The first time we clicked on the ad, we were redirected to a site showing a “Verify you are human” page, also known as Cloudflare Turnstile. Except this was not the real Cloudflare, and merely a social engineering trick.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_832429.png?w=1024)

The HTML source code was obfuscated to only show gibberish interlaced with Russian comments, which we later determined was Rot13, a letter substitution cipher. This was likely used to hide the offending code from prying eyes and network rules:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_92069d.png)

After checking the box to verify we are human, we see new instructions, “Verification steps”, that involve pressing a number of key combinations. _Windows + R_ launches the run dialog while _Ctrl + V_ will paste whatever is in the clipboard. Supposedly this code is part of the verification process, but instead when pressing Enter, the victim will run a malicious command:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_523d04.png)

### PowerShell and payload

The code copied into the clipboard is actually a command line that runs PowerShell:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_7ca93a.png)

The Base64 encoded string retrieves the following code from _hxxps\[:\]//s2notion\[.\]com/in.php?action=1_:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_08b601.png)

This downloads a binary from _hxxps\[:\]//s2notion\[.\]com/in.php?action=2_. and runs its. That file contains an AutoIt script that launches from:

```
"c:temptestAutoit3.exe" c:temptestscript.a3x
```

The following DarkGate configuration was extracted from it:

```
{'DarkGate': {'C2': [['155.138.149.77']], 'unknown_8': ['No'], 'name': ['DarkGate'], 'unknown_12': ['R0ijS0qCVITtS0e6xeZ'], 'unknown_13': ['6'], 'unknown_14': ['Yes'], 'port': ['80'], 'startup_persistence': ['Yes'], 'unknown_32': ['No'], 'check_display': ['Yes'], 'check_disk': ['No'], 'min_disk_size': ['100'], 'check_ram': ['No'], 'min_ram_size': ['4096'], 'check_xeon': ['No'], 'unknown_21': ['No'], 'unknown_23': ['Yes'], 'unknown_31': ['No'], 'unknown_24': ['N-traff'], 'campaign_id': ['user1'], 'unknown_26': ['No'], 'xor_key': ['sDcGdADE'], 'unknown_28': ['No'], 'unknown_29': ['2'], 'unknown_35': ['No'], 'tabla': ['a2THNyA]7u6Kiv$8k.F*ZrO"do1wL9P0 3}eCGDY{XVzctg,&EhJfsx=n)mpQUqljIW5SRMb4B([']}}
```

## Delivery #2: signed executable

### Malicious ad and decoy site

We saw this scenario after revisiting the malicious ad for a second time. Notice how the URL path is now including “/download/”.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_ef9508.png?w=1024)

This is the more traditional approach to malvertising for software downloads that we’ve seen for a while now. Victims download an executable after being tricked with a lookalike site. The file was found hosted on Github under the user profile _herawtisabela1992_:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_e3dbfc.png)

This fake Notion installer was digitally signed (now revoked) by KDL CENTRAL LIMITED. Similar to the other binary mentioned in the first delivery technique, this one also extracts an AutoIt payload, with the same DarkGate configuration.

It’s interesting to note that the same GitHub user account previously distributed a backdoor called Warmcookie (aka Badspace) from:

```
raw[.]]githubusercontent[.]com/herawtisabela1992/check/refs/heads/main/920836164_x64.exe
```

## Conclusion

We were not surprised to see the ClickFix social engineering attack here, but what made this campaign interesting what that it alternated between ClickFix and the typical file download.

It’s quite likely someone is tracking stats and comparing numbers to see which of the two delivery methods yields the most successful installs. If we had to put our money on it, we would bet that ClickFix is ahead. The file download technique remains effective, especially if the payload is digitally signed, but it could be relegated to second place in the near future.

Malwarebytes detects both payloads as Trojan.Dropper and Backdoor.DarkGate.

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

## Indicators of Compromise

Malvertising infrastructure

```
notionbox[.]orgs2notion[.]com
```

Payloads

```
6b6676267c70fbeb3257f0bb9bce1587f0bdec621238eb32dd9f84b2bcd7e3ea4fe8bbc88d7a8cc0eec24bd74951f1f00b5127e3899ae53de8dabd6ff417e6db
```

Go to Source
