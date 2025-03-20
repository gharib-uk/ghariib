---
title: "BYOSI - Evade EDRs The Simple Way By Not Touching Any Of The APIs They Hook"
date: Tue, 17 Sep 2024 08:30:00 -0300
draft: false
type: posts
categories: 
- BYOSI,Edr Bypass,Edr Evasion,PowerShell,Redteam,Restrictions,Scan,Scripting,Windows
---
# BYOSI - Evade EDRs The Simple Way By Not Touching Any Of The APIs They Hook

<br/>

<br/>
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhx2uZznHQk3TnWEfG0XdGxewaDBr9JWJ9-Dveb9j_Jj1fzOQmVmg_jGlllGyBW6Wz5YbI3Heq9VMq2YuDE7WZPXacZtDKGwwijTppPXSdgbB-Vgl7YjTtylLGlbplXWbgW1pfinopyq0sf8oOz63kgWc1PlZkawUIZUxj-AozCjJAelPTV0bC5mgSJ9MeW/w640-h364/Screenshot%20from%202024-09-16%2000-10-44.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhx2uZznHQk3TnWEfG0XdGxewaDBr9JWJ9-Dveb9j_Jj1fzOQmVmg_jGlllGyBW6Wz5YbI3Heq9VMq2YuDE7WZPXacZtDKGwwijTppPXSdgbB-Vgl7YjTtylLGlbplXWbgW1pfinopyq0sf8oOz63kgWc1PlZkawUIZUxj-AozCjJAelPTV0bC5mgSJ9MeW/s1452/Screenshot%20from%202024-09-16%2000-10-44.png)

  

Evade EDR's the simple way, by not touching any of the API's they hook.

Theory
------

I've noticed that most EDRs fail to scan [scripting](https://www.kitploit.com/search/label/Scripting "scripting") files, treating them merely as text files. While this might be unfortunate for them, it's an opportunity for us to profit.

Flashy methods like residing in memory or thread [injection](https://www.kitploit.com/search/label/Injection "injection") are heavily monitored. Without a binary signed by a valid Certificate Authority, execution is nearly impossible.

Enter BYOSI (Bring Your Own Scripting Interpreter). Every scripting interpreter is signed by its creator, with each certificate being valid. Testing in a live environment revealed surprising results: a highly signatured PHP script from this repository not only ran on systems monitored by CrowdStrike and Trellix but also established an external connection without triggering any EDR detections. EDRs typically overlook script files, focusing instead on binaries for implant delivery. They're configured to detect high [entropy](https://www.kitploit.com/search/label/Entropy "entropy") or suspicious sections in binaries, not simple scripts.

This attack method capitalizes on that oversight for significant profit. The [PowerShell](https://www.kitploit.com/search/label/PowerShell "PowerShell") script's steps mirror what a developer might do when first entering an environment. Remarkably, just four lines of [PowerShell](https://www.kitploit.com/search/label/PowerShell "PowerShell") code completely evade EDR detection, with Defender/AMSI also blind to it. Adding to the effectiveness, GitHub serves as a trusted deployer.

  

What this script does
---------------------

The PowerShell script achieves EDR/AV [evasion](https://www.kitploit.com/search/label/Evasion "evasion") through four simple steps (technically 3):

```
1.) It fetches the PHP archive for Windows and extracts it into a new directory named 'php' within 'C:\Temp'.2.) The script then proceeds to acquire the implant PHP script or shell, saving it in the same 'C:\Temp\php' directory.3.) Following this, it executes the implant or shell, utilizing the whitelisted PHP binary (which exempts the binary from most restrictions in place that would prevent the binary from running to begin with.)
```

With these actions completed, congratulations: you now have an active shell on a Crowdstrike-monitored system. What's particularly amusing is that, if my memory serves me correctly, Sentinel One is unable to scan PHP file types. So, feel free to let your imagination run wild.

Disclaimer.
-----------

I am in no way responsible for the misuse of this. This issue is a major blind spot in EDR protection, i am only bringing it to everyones attention.

Thanks Section
--------------

A big thanks to @im4x5yn74x for affectionately giving it the name BYOSI, and helping with the env to test in bringing this attack method to life.

Edit
----

It appears as though MS Defender is now flagging the PHP script as malicious, but still fully allowing the Powershell script full execution. so, modify the PHP script.

Edit
----

hello sentinel one :) might want to make sure that you are making links not embed.

  
  

**[Download BYOSI](https://github.com/oldkingcone/BYOSI "Download BYOSI")**

[Source](http://www.kitploit.com/2024/09/byosi-evade-edrs-simple-way-by-not.html)
<br/>
---
