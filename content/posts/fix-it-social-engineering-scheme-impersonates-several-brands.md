---
title: "<div>‘Fix It’ social-engineering scheme impersonates several brands</div>"
date: 2025-01-03
categories: 
  - "security"
---

More and more, threat actors are leveraging the browser to deliver malware in ways that can evade detection from antivirus programs. Social engineering is a core part of these schemes and the tricks we see are sometimes very clever.

Case in point, there has been an increase in attacks that involve copying a malicious command into the clipboard, only to be later pasted and executed by the victims themselves. Who would have though that copy/paste could be so dangerous?

The new campaign we observed uses a a combination of malicious ads and decoy pages for software brands, followed by a fake Cloudflare notification that instructs users to manually run a few key combinations. Unbeknownst to them, they are actually executing PowerShell code that retrieves and installs malware.

## The discovery

Our investigation into this campaign started from a suspicious ad for ‘notepad’ while performing a Google search. Such search queries have been a hot spot for criminals who want to lure victims that are looking to download programs onto their computer.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_d32618.png)

Based on previous evidence, criminals are tricking victims into visiting a lookalike site with the goal of downloading malware. In this case, the first part was true, but what unfolded next was new to us.

When we clicked the Download button, we were redirected to a new page that appeared to be Cloudflare asking us to “_verify you are human by completing the action below_“. This type of message is more and more common, as site owners try to prevent bots and other unwanted traffic.

But rather than having to solve a CAPTCHA, we saw another unexpected message: _“Your browser does not support correct offline display of this document. Please follow the instructions below using the “Fix it” button_“.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/fixit2.gif)

## Powerful technique

This technique is actually not new in itself, and similar variants have been seen both via email spam and compromised websites before. It is sometimes referred to as ClearFake or ClickFix, and requires users to perform a manual action to execute a malicious PowerShell command.

Clicking on the ‘Fix It’ button copies that command into memory (the machine’s clipboard). Of course the user has no idea what it is, and may follow the instructions that ask to press the Windows and ‘R’ key to open the Run command dialog. CTRL+V pastes that command and Enter executes it.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_4a0264.png)

Once the code runs, it will download a file from a remote domain (_topsportracing\[.\]com_) within the script. We tested that payload in a sandbox and observed immediate fingerprinting:

```
C:UsersAdminAppDataLocalTemp10.exe    C:WindowsSYSTEM32systeminfo.exe        C:Windowssystem32cmd.exe            C:Windowssystem32cmd.exe /c "wmic computersystem get manufacturer"
```

The information is then sent back to a command and control server (_peter-secrets-diana-yukon\[.\]trycloudflare\[.\]com_) abusing Cloudflare tunnels:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_465ef6.png)

The use of Cloudflare tunnels by criminals was previously reported by Proofpoint to deliver RATs. We weren’t able to observe a final payload but it is likely of a similar kind, perhaps an infostealer.

## Campaign targets several brands

This was not an isolated campaign for Notepad, as we soon found additional sites with a similar lure. There was a Microsoft Teams landing page which used exactly the same trick, followed by others such as FileZilla, UltraViewer, CutePDF and Advanced IP Scanner.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_f617bb.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_0f8fa6.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_bde12d.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_470eb7.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_efa8e7.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_3359bf.png?w=1024)

Oddly, we saw a lure for a cruise booking site. We have no idea how that came to be, unless the criminals agree that everyone needs a vacation sometimes.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_7bfacc.png?w=1024)

## Overlap with other campaigns

As mentioned previously, this type of social engineering attack is getting more and more popular. Researchers are tracking several different families under different names such as the original SocGholish.

Interestingly, the same domain (_topsportracing\[.\]com_) we saw in the malicious PowerShell command for Notepad++ was also used recently in another campaign known as #KongTuke:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_a487ad.png)

As these schemes are being increasingly used by criminals, it is important to be aware of the processes involved. The Windows key and the letter ‘R’ pressed together open the Run dialog box. This is not something that most users will ever need to do, so always think carefully whenever you are instructed to perform this.

Malwarebytes customers are protected against this attack via our web protection engine for both the malicious sites and PowerShell command.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2024/12/image_ab6f07.png)

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

## Indicators of Compromise

Malicious domains

```
notepad-plus-plus.bonuscos[.]commicrosoft.team-chaats[.]comcute-pdf[.]comultra-viewer[.]comglobalnetprotect[.]comsunsetsailcruises[.]comjam-softwere[.]comadvanceipscaner[.]comfilezila-project[.]comvape-wholesale-usa[.]com
```

Servers used before Cloudflare proxy

```
185.106.94[.]19089.31.143[.]9094.156.177[.]6141.8.192[.]93
```

Malware download URLs

```
hxxp[://]topsportracing[.]com/wpnot21hxxp[://]topsportracing[.]com/wp-s2hxxp[://]topsportracing[.]com/wp-s3hxxp[://]topsportracing[.]com/wp-25hxxp[://]chessive[.]com/10[.]exehxxp[://]212[.]34[.]130[.]110/1[.]e
```

Malware C2

```
peter-secrets-diana-yukon[.]trycloudflare[.]com
```

Go to Source
