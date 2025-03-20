---
title: "Advancing account security as part of Cloudflare’s commitment to CISA’s Secure by Design pledge"
date: 2025-03-19
---

In May 2024, Cloudflare signed the Cybersecurity and Infrastructure Security Agency (CISA) Secure By Design pledge. Since then, Cloudflare has been working to enhance the security of our products, ensuring that users are better protected from evolving threats. 

Today we are excited to talk about the improvements we have made towards goal number one in the pledge, which calls for increased multi-factor authentication (MFA) adoption. MFA takes many forms across the industry, from app-based and hardware key authentication, to email or SMS. Since signing the CISA pledge we have continued to iterate on our MFA options for users, and most recently added support for social logins with Apple and Google, building on the strong foundation that both of these partners offer their users with required MFA for most accounts. Since introducing social logins last year, about 25% of our users use it weekly, and it makes up a considerable portion of our MFA secured users. There’s much more to do in this space, and we are continuing to invest in more options to help secure your accounts. 

### Mirror, mirror on the wall who is the most secure of them all?

According to the 2024 Verizon Data Breach Investigations Report, leaked credentials continue to be the top cause of application breaches. Even when users employ strong passwords, attackers often make use of techniques like credential stuffing, or password spraying, to gain unauthorized access to accounts. These approaches build on previous data breaches and are much quicker than brute force attacks of the past.  

Ultimately, the most effective defense against these threats is **multi-factor authentication (MFA)**. By requiring an additional verification step beyond just a password, MFA significantly strengthens account security. In fact, studies show that MFA can block **99.9% of automated attacks**, reducing the risk of unauthorized access even if your credentials are compromised. 

Every user on Cloudflare is protected by our built-in challenge system, which will prompt users for a multi-factor authentication code from their email whenever they log in from a new IP address. This provides an important layer of protection by default.

At Cloudflare, MFA is available to **all** Cloudflare customers, and we strongly encourage every user to enable at least one additional authentication factor to better protect their account.

### What’s new?

We made a number of improvements over the course of 2024 to protect you, with more ways to secure your account and adopt MFA. 

#### Social login with Google and Apple

Social login allows you to login to Cloudflare using the secure credentials you already use for your Google or Apple accounts. Most Apple and Google accounts have mandatory multi-factor authentication, so this approach provides a seamless and robust layer of security. By reducing the need to manage separate credentials, social login also makes it easier for customers to secure their accounts from the start. 

Social login has quickly become one of our top login methods, comprising about 25% of all logins weekly on Cloudflare. 

#### Leaked password notifications

Cloudflare automatically detects and notifies users who are using known, leaked passwords. These users are then asked to change their password when they log into Cloudflare. This ensures that users with leaked passwords can address this security lapse easily and keep themselves safe. 

### Improve your security posture

If you’re not already using MFA on your account, you have options. It’s never too late to reevaluate your security posture! 

#### Replace default passwords with strong passwords  

As much as we’re focused on MFA, creation of a strong password is the first line of defense for secure MFA! To safeguard our users, and in alignment with CISA Goal #2 (Default Passwords), Cloudflare does not provide users with preconfigured passwords, or  “default passwords”, during initial password generation. This helps reduce the risk of automated attacks such as credential stuffing and brute force attempts which often target default logins. 

Instead, Cloudflare advocates for strong user-generated passwords. Ideally, users choose unique passwords they have not used before and meet the CISA recommendations for password creation. Use of a password manager can help users adopt strong passwords and reduce friction. By enforcing unique strong passwords, our company ensures a higher level of security making unauthorized access significantly more difficult. 

#### Enable MFA for your account

Cloudflare supports multiple MFA methods. The most secure option is to use a phishing-resistant security key like a YubiKey, or a hardware key that is built into your primary computer like Windows Hello or Apple’s TouchID. We also support Time-Based One-Time passwords (TOTP) using a mobile authenticator app like Google Authenticator or Microsoft Authenticator. Importantly, these apps support optional backup to the cloud, so if you ever lose your phone, you’ll still be able to get into your account. Don’t forget to download backup codes and store them somewhere safe like your password manager in case you lose your MFA device! Configure MFA for your account now in the Cloudflare dashboard. 

#### Require MFA for all users in your Cloudflare account

If you’re an administrator for a Cloudflare account and want to ensure your users are all using MFA, you can set this as a policy on the account in the Manage Members experience. Note, this setting is not available if you have not used MFA, or if your users are using social login. For social login we encourage users to set up MFA on their associated accounts. 

#### Enable SSO for your enterprise

For enterprise customers, **single sign-on (SSO)** is one of the most secure and convenient ways to manage authentication at scale. At Cloudflare, we offer SSO free of charge to all enterprise customers and actively encourage organizations to enable it for stronger security. 

Go to Source
