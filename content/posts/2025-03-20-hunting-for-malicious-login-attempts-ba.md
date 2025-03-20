---
title: "Hunting for malicious login attempts based on basic authentication"
date: Thu, 20 Mar 2025 16:25:39 +0000
draft: false
type: posts
categories: 
- Malware News
---
# Hunting for malicious login attempts based on basic authentication

<br/>

<br/>
![](https://cdn-images-1.medium.com/max/1024/1*lFBb12coTTatjaljJvXmsQ.png)

Nowadays, basic authentication is becoming less common and we use more secure ways to log in with our identities. Disabling basic authentication on your email server or application should be considered to ensure that all clients use stronger methods. Before making this change, it is mandatory to confirm compatibility and compliance with legacy requirements to ensure that current devices or applications support modern authentication. However, there are still cases where the use of basic authentication cannot be changed due to product/software limitations and lack of upgrades or budget to replace them with more secure ones.

Hackers know this, so even when the most of the organizations are using more advanced authentication, why not try to capture some cases where basic authentication is still active?

### The Threat Hunting Process

In my free time, I’ve been hunting for malicious activities related to the mentioned case, using the AADSignInEventsBeta table, where you can filter by singleFactorAuthentication. As usual, I also filter by countries known for high cyberattack activity to uncover related patterns. These insights have been quite useful for detecting broader threats — appreciate the intel, “hackers”!

AADSignInEventsBeta  
| where Country has “RU”  
| where AuthenticationRequirement has “singleFactorAuthentication”  
| distinct  Application, EndpointCall, ErrorCode, AuthenticationRequirement, UserAgent, ClientAppUsed, IPAddress , Country

![](https://cdn-images-1.medium.com/max/1024/0*snc4htqaNAoIsYBv)

As you can see in the results, I identified two type of user agents (BAV2ROPC and BAVROPC) which are associated to ROPC (Resource Owner Password Credentials)

The ROPC flow is considered insecure because it requires applications to handle user credentials directly, increasing the risk of credential theft. Microsoft discourages the use of ROPC and Basic Authentication in favor of more secure, modern authentication methods such as OAuth 2.0 with MFA and token-based authentication.

-   BAVROPC:This user agent is frequently associated with legacy email applications and devices that depend on basic authentication to access email accounts which transmits credentials (username and password) in plain text, making them susceptible to interception. Attackers often exploit this weakness through brute-force attempts to compromise accounts. The repeated login failures originating from various IPs and countries indicate an automated attack attempting to take advantage of basic authentication vulnerabilities.
-   BAConsumerV2ROPC: is basically a user agent string commonly associated with applications and services that rely on Basic Authentication using the Resource Owner Password Credentials (ROPC) flow.

Therefore, after identifying these suspicious agents, the next step was to determine whether authentication attempts using these compromised agents were occurring globally or were limited to Russia:

AADSignInEventsBeta  
| where UserAgent  has “BAV2ROPC” or UserAgent has “AConsumerV2ROPC”  
| where AuthenticationRequirement has “singleFactorAuthentication”  
| distinct  Application, EndpointCall, ErrorCode, AuthenticationRequirement, UserAgent, ClientAppUsed, IPAddress , Country

![](https://cdn-images-1.medium.com/max/1024/0*3GlWj8d-sd268Y_v)

Indeed, these type of malicious activities are happening from distinct countries.

### Responding to malicious Basic Authentication sign-in attempts

As I mentioned at the beginning, if possible disable basic authentication, it is the most secure (but also the most drastic) method. However, we have some alternatives:

Conditional Access Policies: If you’re using Microsoft 365, leverage conditional access policies to block BAV2ROPC and BAConsumerV2ROPC using policies that restrict access based on user agents.

Firewall Rules: I have read about the option to control this threat over a firewall because you can create rules to block traffic from IP addresses or countries associated with the mentioned agents. However, I don’t see it as the best option because nowadays, with the use of proxy servers , VPNs, and others, malicious actors would easily bypass these controls (to not mention the work to maintain a blocklist updated).

Detection Rules: After verifying that you have no legitimate connection (you can filter by ErrorCode=0 and the mentioned agents) that you are allowing by some of your devices/products/apps, I would recommend you to generate an alert/Ticket if there are successful connection using this agent and even require a password reset for the affected identity (![:pencil2:](https://malware.news/images/emoji/twitter/pencil2.png?v=12 ":pencil2:")remember to add the ReportId and Timestamp fields in the KQL Quers in order to create them).

### Conclusion

Even if you don’t have basic authentication enabled, it’s a good way to see how bad actors are working against your tenant using this technique. Also, it might help you detect some typical forgotten application that is compromised and being targeted by this leak method.

On the other hand, if for some reason you are still using basic authentication, I hope this article will help you define some controls and identify if your tenant is being a target by hackers.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=dd752ea07f0d)

[Hunting for malicious login attempts based on basic authentication](https://detect.fyi/hunting-for-malicious-login-attempts-based-on-basic-authentication-dd752ea07f0d) was originally published in [Detect FYI](https://detect.fyi) on Medium, where people are continuing the conversation by highlighting and responding to this story.

> Introduction to Malware Binary Triage (IMBT) Course
> ---------------------------------------------------
> 
> Looking to level up your skills? Get **10% off** using coupon code: **MWNEWS10** for any flavor.
> 
> [Enroll Now and Save 10%: Coupon Code MWNEWS10](https://training.invokere.com/link/QHLuD5/MWNEWS10?url=https%3A%2F%2Ftraining.invokere.com)
> 
> _Note: Affiliate link – your enrollment helps support this platform at no extra cost to you._

Article Link: [Hunting for malicious login attempts based on basic authentication | by Sergio Albea | Mar, 2025 | Detect FYI](https://detect.fyi/hunting-for-malicious-login-attempts-based-on-basic-authentication-dd752ea07f0d?source=rss----d5fd8f494f6a---4)

1 post - 1 participant

[Read full topic](https://malware.news/t/hunting-for-malicious-login-attempts-based-on-basic-authentication/92322)

#### [Source](https://malware.news/t/hunting-for-malicious-login-attempts-based-on-basic-authentication/92322)

<br/>
---
