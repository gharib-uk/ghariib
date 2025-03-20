---
title: "X Phishing | Campaign Targeting High Profile Accounts Returns, Promoting Crypto Scams"
date: 2025-02-01
---

SentinelLABS has observed an active phishing campaign targeting high-profile X accounts to hijack and exploit them for fraudulent activity.

## Executive Summary

- An active phishing campaign is targeting high-profile X accounts in an attempt to hijack and exploit them for fraudulent activity.
- This campaign has been observed targeting a variety of individual and organization accounts such as U.S. political figures, leading international journalists, an X employee, large technology organizations, cryptocurrency organizations, and owners of valuable, short usernames.
- SentinelLABS’ analysis links this activity to a similar operation from last year that successfully compromised multiple accounts to spread scam content with financial objectives. While the activity detailed here is centered around X/Twitter accounts, this actor is not limited to a single social platform, and can be observed directing attention to other popular services as well, while seemingly pursuing the same financial objectives.

If you’ve encountered similar suspicious activity, SentinelLABS would love to hear from you — please reach out to the team at ThreatTips@sentinelone.com.

## Account Compromise Process

Thanks to tips from targets and collaboration with industry partners, SentinelLABS has observed a variety of phishing lures tied to this campaign over the past few weeks. One example is the classic account login notice. The links in the email received by the target are not legitimate and lead to credential phishing sites. Other observed lures use copyright violation themes. However, SentinelLABS notes that directly phishing users may not be the only access method employed by this attacker.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/new-login-phishing-email.jpg)

<figcaption>

An X ‘new login’-themed phishing email

</figcaption>

</figure>

In recent cases, we observed the actor abusing Google’s “AMP Cache” domain `cdn.ampproject[.]org` to evade email detections and redirect the user to a phishing domain:

```
https://cdn.ampproject[.]org/c/s/x-recoverysupport.com/reset/?username=[X-USERNAME]
```

This ultimately leads the targets to an actor-made phishing website seeking X account credentials:

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/x-creds-phishing-page.jpg)

<figcaption>

X credential phishing page

</figcaption>

</figure>

In the copyright infringement lure scenario, the user will first visit an Action Needed page before being prompted to enter credentials:

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/x-fake-copyright-infringement.jpg)

<figcaption>

X fake copyright infringement page

</figcaption>

</figure>

Once an account is taken over, the attacker swiftly locks out the legitimate owner and begins posting fraudulent cryptocurrency opportunities or links to external sites designed to lure additional targets, often with a crypto theft-related theme. Ultimately, compromising high-profile accounts enables the attacker to reach a broader audience of potential secondary victims, maximizing their financial gains.

## Widespread Activity

In recent activity associated with this campaign, the domain `securelogins-x[.]com` has been used to deliver emails and `x-recoverysupport[.]com` to host phishing pages. Our observations indicate a level of informality and flexibility of infrastructure use – meaning any of these domains can be considered email delivery or phishing page hosting.

An overall collection of recent activity can be observed hosted on `84.38.130[.]20`, an IP associated with a Belize-based VPS service called Dataclub. The domains themselves have been predominantly registered through Turkish hosting provider Turkticaret.

Inspecting the DNS history of `84.38.130[.]20` leads to a variety of interestingly related domains. As shown below, the cluster of activity began in mid-2024 and continues today. While this is only one phishing page hosting IP, it provides a good perspective of the length of this activity and its ability to avoid much attention for over a year.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/validin-infrastructure-analysis.jpg)

<figcaption>

Validin Infrastructure Analysis Timeline

</figcaption>

</figure>

Our observations suggest that the attacker is highly adaptable, continuously exploring new techniques while maintaining a clear financial motive. The targeting appears constrained, yet opportunistic. Notably, past public reports have attributed related activity to Turkish-speaking actors based on language phishing page source comment language. At this time, we do not attribute this campaign to a specific country or any widely-tracked threat actor.

Some of the malicious sites and content hosted across `84.38.130[.]20` are built using the FASTPANEL DIRECT service.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/FASTPANEL-landing-page.jpg)

<figcaption>

FASTPANEL landing page on buy-tanai\[.\]com

</figcaption>

</figure>

FASTPANEL is a website hosting and building service that specializes in rapid building and management of websites. While FASTPANEL is not a malicious service, it is frequently abused by bad actors due to the ease of use, rapid scalability, and relatively low cost. FASTPANEL is routinely utilized by drainer gains and phishing campaigns, and is also included in associated guides and tutorials distributed throughout cybercrime communication channels.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/example-discussion-FASTPANEL.jpg)

<figcaption>

Example discussion of FASTPANEL (RU crime forum)

</figcaption>

</figure>

Of the sites hosted on `84.38.130[.]20`, the `buy-tanai[.]com` and `emotionai[.]live` sites still present the FASTPANEL landing pages as of this writing.

## Publicly Linkable Activity

### Emerging Account Intrusions

While we have not yet established a high-confidence link, a recent compromise of a Tor Project account closely mirrors our observations. On January 30, 2025, the official X account for the Tor Project was breached. While it is possible that the same threat actor is responsible, we lack sufficient evidence to confirm the connection as of this writing.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/x-tor-project-potential-comp.jpg)

<figcaption>

X post from The Tor Project account on January 30, 2025 advising users of a potential compromise

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/tor-project-account-comp-notice.jpg)

<figcaption>

Tor Project account compromise notice

</figcaption>

</figure>

The Decentralized Autonomous Wireless Network (DAWN) was another victim of this type of attack. The threat actor leveraged the compromised DAWN-related social media accounts to lure victims into entering credentials into phishing pages targeting X and Telegram credentials.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/DAWN-x-post.jpg)

<figcaption>

DAWN X Posts

</figcaption>

</figure>

The compromise of DAWN’s X accounts goes back to mid-January 2025.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/DAWN-rewards-comp-post.jpg)

<figcaption>

January 14, 2025 – DAWN rewards compromise post

</figcaption>

</figure>

### Crypto-Themed Project Placeholders

In some cases, we’ve observed cryptocurrency themed projects seemingly acting as placeholders for future use, or direct pump-and-dump schemes. In one example, `buy-tanai[.]com` was pitched as such: “_$TANA AI. Dawn’s AI project, Tana is the first AI-powered LP and trading agent, now live on the Solana blockchain.”_

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/tana-ai-pump-fun.jpg)

<figcaption>

Tana AI (TANA) on Pump\[.\]fun

</figcaption>

</figure>

The domain`buy-tanai[.]com` currently displays default FASTPANEL landing pages, suggesting it — along with other similar domains — is being staged for future attacks. Since FASTPANEL-managed sites can be rapidly updated, these domains serve as adaptable templates for phishing campaigns.

Notably, TANA AI (TANA) was launched by Dawn in mid-January to promote AI-driven trading and liquidity provision in the cryptocurrency market. Despite losing most of its initial value within days, the currency remains actively traded across multiple decentralized exchanges.

Given the crypto-related nature of these domains, it is likely that threat actors are using them as flexible phishing infrastructure. By keeping them as blank templates, they can quickly modify hosted content to align with ongoing campaigns as needed.

### Crimeware Relations

Several other domains share overlaps in both use and unique infrastructure details, yet they represent a fork from the previously described high-profile social media profile attacks, including:

- dataoptimix\[.\]com
- gamecodestudios\[.\]com
- shortwayscooter\[.\]com

The domain `shortwayscooter[.]com` hosts fake captchas that deliver the DanaBot banking trojan. DataOptimix is branded as a generative AI solution, though there are few details about what the service does.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/dataoptimix.jpg)

<figcaption>

DataOptimix

</figcaption>

</figure>

### Historical Connections

In mid-2024, a campaign used related infrastructure in similar phishing messages, including those which compromised the Linus Tech Tips Twitter account along with several other high profile users. At the time, @LinusTech had roughly 1.8million followers, which may represent the highest profile account successfully hijacked and linked to this actor.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/linus-tech-tips-compromise.jpg)

<figcaption>

Linus Tech Tips Twitter compromise

</figcaption>

</figure>

## Conclusion

The cryptocurrency landscape offers financially-motivated threat actors multiple opportunities for profit and fraud. While marketing for coins and tokens has long been irreverent and meme-driven, recent developments have further blurred the line between legitimate projects and scams.

A striking example occurred in January 2025, when the X account of the late crypto-enthusiast and antivirus founder John McAfee was reactivated to promote a new coin, $AIntivirus. The marketing style and brand voice of this purportedly legitimate token closely resemble tactics used in known scam campaigns, highlighting how easily crypto enthusiasts can be misled in an already murky ecosystem.

To safeguard your X account, we strongly recommend using a unique password, enabling two-factor authentication (2FA), and avoiding credential sharing with third-party services. Be especially cautious of messages containing links to account alerts or security notices. Always verify URLs before clicking, and if a password reset is needed, initiate it directly through the official website or app rather than relying on unsolicited links.

If you’ve encountered similar suspicious activity, we’d love to hear from you. Contact SentinelLABS at ThreatTips@sentinelone.com.

## Indicators of Compromise

**Domains**  
buy-tanai\[.\]com  
dataoptimix\[.\]com  
gamecodestudios\[.\]com  
infringe-x\[.\]com  
protection-x\[.\]com  
rewards-dawn\[.\]com  
securelogins-x\[.\]xyz  
shortwayscooter\[.\]com  
violationappeal-x\[.\]com  
violationcenter-x\[.\]com  
x-accountcenter\[.\]com  
x-changealerts\[.\]com  
x-logincheck\[.\]com  
x-loginhelp\[.\]com  
x-passwordrecovery\[.\]com  
x-recoveraccount\[.\]com  
x-suspiciouslogin\[.\]com

**SHA-1**  
e2221e5c58a1a976e59fe1062c6db36d4951b81e – PHP file containing URL associated with X credential phishing activity

Go to Source
