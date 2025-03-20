---
title: "AMOS and Lumma stealers actively spread to Reddit users"
date: 2025-03-19
---

We were alerted to Mac and Windows stealers currently distributed via Reddit posts targeting users engaging in cryptocurrency trading. One of the common lures is a cracked software version of the popular trading platform TradingView.

The crooks are posting links to both Windows and Mac installers which have been laced with Lumma Stealer and Atomic Stealer (AMOS) respectively.

These two malware families have wreaked havoc, pillaging victims’ personal data and enabling their distributors to make substantial gains, mostly by taking over cryptocurrency wallets.

## Reddit posts target crypto enthusiasts

Scammers are lurking on subreddits visited by cryptocurrency traders and posting about free access to TradingView, a web-based platform and social network that provides charting tools for analyzing financial markets, including stocks, forex, cryptocurrencies, and commodities.

The offer claims that the programs are totally free and have been cracked directly from their official version, unlocking premium features.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_19956d.png?w=962)

While the original post gives a heads-up that you are installing these files at your own risk, further down in the thread we can read comments from the OP such as “_a real virus on a Mac would be wild_“.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_8d2f28.png?w=1024)

## Downloads hosted on unrelated website

We checked both links and noticed that the website hosting the files belongs to a Dubai cleaning company. It’s not totally clear why the scammers didn’t choose a service like Mega or similar, unless they wanted the ability to upload and update their code directly via a server they control.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_9ff5a0.png?w=1024)

Upon checking that website, we can see that it leaks its PHP version (7.3.33). It already reached its end of life in December 2021 and no longer receives official security updates, making it prone to exploitation and compromise.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_e4e157.png?w=1024)

## Double zipped malware

Both Mac and Windows files are double zipped, with the final zip being password protected. For comparison, a legitimate executable would not need to be distributed in such fashion.

On Mac, the installer is a new variant of AMOS, a popular macOS stealer. In its latest iterations, the malicious code checks for the presence of virtual machines and exits with error code 42 if it detects any.

```
osascript -e "set memData to do shell script "system_profiler SPMemoryDataType"if memData contains "QEMU" or memData contains "VMware" then    do shell script "exit 42"else    do shell script "exit 0"end if"
```

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_e5dd66.png?w=1024)

Analysis of the full script shows the function that exfiltrates user data via a POST request to 45.140.13.244, a server hosted in the Seychelles:

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_38203e.png)

On Windows, the payload is loaded via an obfuscated bat file (_Costs.tiff.bat_) that runs a malicious Autoit script (_Sad .com_):

```
"C:Windowssystem32cmd.exe" /c expand Costs.tiff Costs.tiff.bat & Costs.tiff.batcmd /c copy /b 701617Sad.com + Io + Thin + Experiment + Detect + Subsection + Meter + Well + Walls + Substantially + Mcdonald 701617Sad.com
```

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_b50702.png?w=1024)

The malware command and control server here is _cousidporke\[.\]icu_, registered about a week ago by someone in Russia.

We have heard of victims whose crypto wallets had been emptied, and were subsequently impersonated by the criminals who sent phishing links to their contacts.

## Conclusion

Cracked software has been prone to containing malware for decades, but clearly the lure of a free lunch is still very appealing. What’s interesting with this particular scheme is how involved the original poster is, going through the thread and being ‘helpful’ to users asking questions or reporting an issue.

Here are some things to look out for and stay safe:

- instructions to disable security software so the program can run (do not disable the antivirus that’s trying to protect you!)

- files that are password-protected (this is a common practice to thwart security scanners)

- files hosted on dubious online platforms

However, it is still easy to fall for these scams, especially if the recommendation came from a friend. Malwarebytes protects from both Mac and Windows payloads.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_6551c2.png?w=1024)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_252212.png?w=1024)

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

Go to Source
