---
title: "University site cloned to evade ad detection distributes fake Cisco installer"
date: 2025-02-06
---

There is a constant “cat and mouse” game between defenders and attackers, the latter trying to outsmart and get a head start on the former. In the context of online advertising, this involves creating fake identities or using stolen ones to push out malicious ads.

An attacker not only needs to evade detection but also create a lure that will be convincing to most people. In this blog post, we focus on what malvertisers use in almost all of their campaigns, namely decoys also known as “white pages” in order to fool the advertising entity.

The particular case is a malicious Google ad for Cisco AnyConnect, a tool often used by employees to remotely connect to company networks, but also by universities. In fact, we found that threat actors were using the name of a German university to create a fake website designed not to fool actual victims, but rather to bypass detection from security systems.

To be sure, victims were part of the overall scheme, but they were instead redirected to a lookalike Cisco site linking to a malicious installer containing the NetSupport RAT remote access Trojan.

## The perfect disguise

The malicious ad comes up from a Google search for the keywords “_cisco annyconnect_“. The ad displays a URL that looks somewhat convincing for the domain _anyconnect-secure-client\[.\]com_. We should note that this domain was registered less than a day before the ad appeared.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_97aa4e.png)

Upon clicking on the ad, server-side checks will determine whether this is a potential victim or not. Typically, a real victim has a residential IP address and other network settings that differentiate it from crawlers, bots, VPNs or proxies.

In recent times, we have seen criminals rely on AI to generate fake pages that look innocuous. These are also referred to as “white pages” and they do serve an important purpose. If it’s obviously so fake and bad, it will raise suspicion. We thought that in this case the perpetrator had a rather clever idea by stealing content from a university that actually does use Cisco AnyConnect.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_6e1fb8.png)

Technische Universität Dresden (TU Dresden), is a public research university in Germany whose site can be found here. Funnily enough, the threat actors left a trail while doing their copy/paste. We can see that they added the cookie opt-in notification which is required for websites in Europe, which here leaked their browser language (Russian).

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_af681d.png)

## Real victims get infected with malware

As good as this template looks, real victims will never see it. Instead, upon connecting to the malicious server they will be immediately redirected to a phishing site for Cisco AnyConnect.

The payload is downloaded in a similar way to a campaign we had already observed before, using a PHP script that provides the direct download URL. We can see from the network traffic capture below that the file is hosted on a likely compromised WordPress site.

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_e3ce13.png)

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_f1ceb1.png)

There is not much to be said about the fake installer other than it being digitally signed with a valid certificate. Upon execution it extracts _client32.exe_, a name notorious for being associated with NetSupport RAT.

```
cisco-secure-client-win-5.0.05040-core-vpn-predeploy-k9.exe  -> client32.exe  -> "icacls" "C:ProgramDataCiscoMedia" /grant *S-1-1-0:(F) /grant Users:(F) /grant Everyone:(F) /T /C
```

The remote access Trojan connects to the following two IP addresses: 91.222.173\[.\]67 and 199.188.200\[.\]195, further granting a remote attacker access to the victim’s machine.

## Conclusion

Brand impersonation is a common theme with search ads. As Google enforces various policies and uses algorithms to detect malicious activity, threat actors need to constantly come up with new ideas.

Reusing a university page was a clever idea, but there were a couple of things that made this attack shy of being perfect. The domain name, while very strong for impersonation, was newly registered. Since it was part of the ad’s display URL, it could have potentially been detected by Google. We also noted that the perpetrators left a trail when they copy/pasted the code from the university website, which identified their likely country of origin.

Having said that, the malware payload was digitally signed and had few detections when first seen, so this attack may have had a decent success rate.

As always, we recommend that users take precautions whenever looking up programs to download, and to be especially wary of sponsored results.

* * *

**We don’t just report on threats—we remove them**

Cybersecurity risks should never spread beyond a headline. Keep threats off your devices by downloading Malwarebytes today.

## Indicators of Compromise

Malvertising infrastructure

```
anyconnect-secure-client[.]comcisco-secure-client[.]com[.]vissnatech[.]com
```

NetSupport RAT download

```
berrynaturecare[.]com/wp-admin/images/cisco-secure-client-win-5[.]0[.]05040-core-vpn-predeploy-k9[.]exe78e1e350aa5525669f85e6972150b679d489a3787b6522f278ab40ea978dd65d
```

NetSupport RAT C2s

```
monagpt[.]commtsalesfunnel[.]com91.222.173[.]67/fakeurl.htm199.188.200[.]195/fakeurl.htm
```

Go to Source
