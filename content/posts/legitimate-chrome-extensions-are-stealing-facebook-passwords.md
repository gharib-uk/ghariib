---
title: "Legitimate Chrome extensions are stealing Facebook passwords"
date: 2025-01-17
tags: 
  - "cybersecurity"
  - "data_breaches"
  - "infostealers"
  - "linux"
  - "malware"
  - "privacy"
  - "security"
  - "siem"
---

Supply-chain attacks use trojanized legitimate Google Chrome extensions for data theft.

Right after Christmas, news broke of a multi-stage attack targeting developers of popular Chrome extensions. Ironically, the biggest-name target was a cybersecurity extension created by Cyberhaven — compromised just before the holidays (we’d previously warned about such risks). As the incident investigation unfolded, the list grew to include no fewer than 35 popular extensions, with a combined total of 2.5 million installations. The attackers’ goal was to steal data from the browsers of users who installed trojanized updates of these extensions. The focus of the campaign was on stealing credentials for Meta services to compromise business accounts and display ads at victims’ expense. However, that’s not the only data that malicious extensions can steal from browsers. We explain how the attack works, and what measures you can take to protect yourself against it at different stages.

## Attacking developers: OAuth abuse

To inject trojan functionality into popular Chrome extensions, cybercriminals have developed an original phishing scheme. They send developers emails disguised as standard Google alerts claiming that their extension violates Chrome Web Store policies and needs a new description. The text and layout of the message mimic typical Google emails, so the victim is often convinced. Moreover, the email is often sent from a domain set up to attack a specific extension and containing the name of the extension in the actual domain name.

Clicking the link in the email takes the user to a legitimate Google authentication page. After that, the developer sees another standard Google screen prompting to sign in via OAuth to an app called “Privacy Policy Extension”, and to grant certain permissions to it as part of the authentication process. This standard procedure takes place on legitimate Google pages, except that the “Privacy Policy Extension” app requests permission to publish other extensions to the Chrome Web Store. If this permission is granted, the creators of “Privacy Policy Extension” are able to publish updates to the Chrome Web Store on behalf of the victim.

In this case, there’s no need for the attackers to steal the developer’s password or other credentials, or to bypass multi-factor authentication (MFA). They simply abuse Google’s system for granting permissions to trick developers into authorizing the publication of updates to their extensions. Judging by the long list of domains registered by the attackers, they attempted to attack far more than 35 extensions. In cases where the attack was successful, they released an updated version of the extension, adding two files for stealing Facebook cookies and other data (worker.js and content.js).

## Attacking users

Chrome extensions typically receive updates automatically, so users who switched on their machines between December 25 and December 31, and opened Chrome, may have received an infected update of a previously installed extension.

In this event, a malicious script runs in the victim’s browser and sends data needed for compromising Facebook business accounts to the attackers’ server. In addition to Facebook identifiers and cookies, the malware steals information required to log in to the target’s advertising account, such as the user-agent data to identify the user’s browser. On facebook.com, even mouse-click data is intercepted to help the threat actors bypass CAPTCHA and two-factor authentication (2FA). If the victim manages ads for their company or private business on Meta, the cybercriminals get to spend their advertising budget on their own ads — typically promoting scams and malicious sites (malvertising). On top of the direct financial losses, the targeted organization faces legal and reputational risks, as the fake ads are published under its name.

The malware can conceivably steal data from other sites too, so it’s worth checking your browser even if you don’t manage Facebook ads for a company.

## What to do if you installed an infected extension update

To stop the theft of information from your browser, the first thing you need to do is to uninstall the compromised extension or update it to a patched version. See here for a list of all known infected extensions with their current remediation status. Unfortunately, simply uninstalling or updating the infected extension is **not enough**. You should also reset any passwords and API keys that were stored in the browser or used during the incident period.

Then, check the available logs for signs of communication with the attackers’ servers. IoCs are available here and here. If communication with malicious servers was made, look for traces of unauthorized access in all services that were opened in the infected browser.

After that, if Meta or any other advertising accounts were accessed from the infected browser, manually check all running ads, and stop any unauthorized advertising activity you find. Lastly, deactivate any compromised Facebook account sessions on all devices (Log out all other devices), clear the browser cache and cookies, log in to Facebook again, and change the account password.

## Incident takeaways

This incident is another example of supply-chain attacks. In the case of Chrome, it’s made worse by the fact that updates are installed automatically without notifying the user. While updates are usually a good thing, here the auto-update mechanism allowed malicious extensions to spread quickly. To mitigate the risks of this scenario, companies are advised to do the following:

- Use group policies or the Google Admin console to restrict the installation of browser extensions to a trusted list;
- Create a list of trusted extensions based on business needs and information security practices used by the developers of said extensions;
- Apply version pinning to disable automatic extension updates. At the same time, it’ll be necessary to put in place a procedure for update monitoring and centralized updating of approved extensions by administrators;
- Install an EDR solution on all devices in your organization to protect against malware and monitor suspicious events.

Companies that publish software, including web extensions, need to ensure that permission to publish is granted to the minimum number of employees necessary — ideally from a privileged workstation with additional layers of protection, including MFA and tightly configured application launch control and website access. Employees authorized to publish need to undergo regular information security training, and be familiar with the latest attacker tactics, including spear phishing.

Go to Source
