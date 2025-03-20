---
title: "Password reuse is rampant: nearly half of observed user logins are compromised"
date: 2025-03-19
---

Accessing private content online, whether it's checking email or streaming your favorite show, almost always starts with a “login” step. Beneath this everyday task lies a widespread human mistake we still have not resolved: **password reuse.** Many users recycle passwords across multiple services, creating a ripple effect of risk when their credentials are leaked.

Based on Cloudflare's observed traffic between September - November 2024, **41% of successful logins across websites protected by Cloudflare involve compromised passwords.** In this post, we’ll explore the widespread impact of password reuse, focusing on how it affects popular Content Management Systems (CMS), the behavior of bots versus humans in login attempts, and how attackers exploit stolen credentials to take over accounts at scale.

### Scope of the analysis

As part of our Application Security offering, we offer a free feature that checks if a password has been leaked in a known data breach of another service or application on the Internet. When we perform these checks, Cloudflare does not access or store plaintext end user passwords. We have built a privacy-preserving credential checking service that helps protect our users from compromised credentials. Passwords are hashed – i.e., converted into a random string of characters using a cryptographic algorithm – for the purpose of comparing them against a database of leaked credentials. This not only warns site owners that their end users’ credentials may be compromised; it also allows site owners to issue a password reset or enable MFA. These protections help prevent attacks that use automated attempts to guess information, like usernames and passwords, to gain access to a system or network. Read more on how our leaked credentials detection scans work here.

Our data analysis focuses on traffic from Internet properties on Cloudflare’s free plan, which includes leaked credentials detection as a built-in feature. Leaked credentials refer to usernames and passwords exposed in known data breaches or credential dumps — for this analysis, our focus is specifically on leaked passwords. With 30 million Internet properties, comprising some 20% of the web, behind Cloudflare, this analysis provides significant insights. The data primarily reflects trends observed after the detection system was launched during Birthday Week in September 2024.

### Nearly 41% of logins are at risk

One of the biggest challenges in authentication is distinguishing between legitimate human users and malicious actors. To understand human behavior, we focus on successful login attempts (those returning a 200 OK status code), as this provides the clearest indication of user activity and real account risk. Our data reveals that approximately 41% of successful human authentication attempts involve leaked credentials.

Despite growing awareness about online security, a significant portion of users continue to reuse passwords across multiple accounts. And according to a recent study by Forbes, users will, on average, reuse their password across four different accounts. Even after major breaches, many individuals don’t change their compromised passwords, or still use variations of them across different services. For these users, it’s not a matter of “if” attackers will use their compromised passwords, it’s a matter of “when”.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/71DMUKH3ZzY1hr0bsavGj3/16eed591ce9b34fd4aead27746a85acf/image4.png)

When we expand to include bot-driven traffic in this analysis, the problem of leaked credentials becomes even more noticeable. Our data reveals that 52% of all detected authentication requests contain leaked passwords found in our database of over 15 billion records, including the Have I Been Pwned (HIBP) leaked password dataset.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4yKNn90DQQTS0j9MjpJ3AT/07513fa7c81f3a37a5f30db4798c9955/image1.png)

This percentage represents hundreds of millions of daily authentication requests, originating from both bots and humans. While not every attempt succeeds, the sheer volume of leaked credentials in real-world traffic illustrates how common password reuse is. Many of these leaked credentials still grant valid access, amplifying the risk of account takeovers.

### Attackers heavily use leaked password datasets

Bots are the driving force behind credential-stuffing attacks, the data indicates that 95% of login attempts involving leaked passwords are coming from bots, indicating that they are part of credential stuffing attacks.

Equipped with credentials stolen from breaches, bots systematically target websites at scale, testing thousands of login combinations in seconds.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7rjFwHf1vwRE43M2JuNWkI/2147f2dfaa04f06ae4e75d1ad05232d2/image5.png)

Data from the Cloudflare network exposes this trend, showing that bot-driven attacks remain alarmingly high over time. Popular platforms like WordPress, Joomla, and Drupal are frequent targets, due to their widespread use and exploitable vulnerabilities, as we will explore in the upcoming section.

Once bots successfully breach one account, attackers reuse the same credentials across other services to amplify their reach. They even sometimes try to evade detection by using sophisticated evasion tactics, such as spreading login attempts across different source IP addresses or mimicking human behavior, attempting to blend into legitimate traffic.

The result is a constant, automated threat vector that challenges traditional security measures and exploits the weakest link: password reuse.

### Brute force attacks against WordPress

Content Management Systems (CMS) are used to build websites, and often rely on simple authentication and login plugins. This is convenient, but also makes them frequent targets of credential stuffing attacks due to their widespread adoption. WordPress is a very popular content management system with a well known user login page format. Because of this, websites built on WordPress often become common targets for attackers.

Across our network, WordPress accounts for a significant portion of authentication requests. This is unsurprising given its market share. However, what stands out is the alarming number of successful logins using leaked passwords, especially by bots.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3fJWsdEZJ9bo5eYbH9A35w/ed6a805386ef406565be08bdff5260b9/image3.png)

**76% of leaked password login attempts for websites built on WordPress are successful.**

Of these, 48% of successful logins are bot-driven. This is a shocking figure that indicates nearly half of all successful logins are executed by unauthorized systems designed to exploit stolen credentials. Successful unauthorized access is often the first step in account takeover (ATO) attacks.

The remaining 52% of successful logins originate from legitimate, non-bot users. This figure, higher than the average of 41% across all platforms, highlights how pervasive password reuse is among real users, putting their accounts at significant risk.

**Only 5% of leaked password login attempts result in access being denied.**

This is a low number compared to the successful bot-driven login attempts, and could be tied to a lack of security measures like rate-limiting or multi-factor authentication (MFA). If such measures were in place, we would expect the share of denied attempts to be higher. Notably, 90% of these denied requests are bot-driven, reinforcing the idea that while some security measures are blocking automated logins, many still slip through.

The overwhelming presence of bot traffic in this category points to ongoing automated attempts to brute-force access.

The **remaining 19%** of login attempts fall under other outcomes, such as timeouts, incomplete logins, or users who changed their passwords, so they neither count as direct “successes” nor do they register as “denials”.

### Keeping user accounts safe with Cloudflare

If you're a user, start with changing reused or weak passwords and use unique, strong ones for each website or application. Enable multi-factor authentication (MFA) on all of your accounts that support it, and start exploring passkeys as a more secure, phishing-resistant alternative to traditional passwords.

For website owners, activate leaked credentials detection to monitor and address these threats in real time and issue password reset flows on leaked credential matches.

Additionally, enable features like Rate Limiting and Bot Management tools to minimize the impact of automated attacks. Audit password reuse patterns, identify leaked credentials within your systems, and enforce robust password hygiene policies to strengthen overall security.

By adopting these measures, both individuals and organizations can stay ahead of attackers and build stronger defenses.

Go to Source
