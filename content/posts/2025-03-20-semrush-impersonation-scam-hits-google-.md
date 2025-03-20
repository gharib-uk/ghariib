---
title: "Semrush impersonation scam hits Google Ads"
date: Thu, 20 Mar 2025 18:15:37 +0000
draft: false
type: posts
categories: 
- Malware News
---
# Semrush impersonation scam hits Google Ads

<br/>

<br/>
_This blog post was co-authored with_ [_Elie Berreby, Senior SEO Strategist_](https://semking.com)

> Introduction to Malware Binary Triage (IMBT) Course
> ---------------------------------------------------
> 
> Looking to level up your skills? Get **10% off** using coupon code: **MWNEWS10** for any flavor.
> 
> [Enroll Now and Save 10%: Coupon Code MWNEWS10](https://training.invokere.com/link/QHLuD5/MWNEWS10?url=https%3A%2F%2Ftraining.invokere.com)
> 
> _Note: Affiliate link – your enrollment helps support this platform at no extra cost to you._

Criminals are highly interested in online marketing and advertising tools that they can leverage as part of their ongoing malware campaigns.

In particular, we have previously [detailed](https://www.malwarebytes.com/blog/news/2025/01/the-great-google-ads-heist-criminals-ransack-advertiser-accounts-via-fake-google-ads) how Google advertiser accounts can be hijacked to create new malicious ads and perpetuate a vicious cycle leading to more compromised accounts.

As part of our investigations, we uncovered a new operation going after [Semrush](https://www.semrush.com/), a visibility management SaaS platform that offers SEO, advertising, and market research, amongst other things.

With [40% of Fortune 500](https://www.semrush.com/news/372886-semrush-2024-a-year-in-review/) companies and [117,000 paying customers](https://investors.semrush.com/news/news-details/2025/Semrush-Announces-Fourth-Quarter-and-Full-Year-2024-Financial-Results/default.aspx) relying on Semrush, the platform presents a highly attractive target for online criminals.

In this blog post, we detail how fraudsters are taking an indirect approach to hacking Google advertisers and by the same token likely gaining access to Semrush accounts.

We have diligently reported the malicious ads to Google. We would like to stress that we are not referring to any vulnerability or data breach with Semrush or its platform in this post. They are simply being targeted because of their growing popularity.

**Google Ads crew pivots**
--------------------------

Back in January, we [documented](https://www.malwarebytes.com/blog/news/2025/01/the-great-google-ads-heist-criminals-ransack-advertiser-accounts-via-fake-google-ads) a large phishing campaign targeting Google accounts via Google Ads using a very specific technique that abused Google Sites.

We believe the criminals behind it likely regrouped and switched to a less direct approach, yet one that might deliver just as much.

We observed this transition with a malicious ad for “Google Ads” that oddly enough redirected to a fraudulent login page for Semrush. While the phishing page uses the Semrush brand, only the “Log in with Google” option is enabled, forcing victims to authenticate with their Google account username and password.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_d081b6.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_d081b6.png)

**Semrush phishing campaign**
-----------------------------

Barely a day later, the campaign was starting to take shape with Google ads now fully moving away from the “Google Ads” brand to fully impersonating Semrush.

The infrastructure for this new wave was deployed recently and the domain names registered for it are all variations on the Semrush name.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_2bbc58.png?w=800)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_2bbc58.png)

Each ad uses a unique domain name which does a redirect to more static domains dedicated to the fake Semrush and Google account login pages.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_8869f3.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_8869f3.png)

Once again, the landing page here shows two different types of login but only the Google method is enabled. We believe this is because the threat actors are primarily interested in harvesting Google accounts.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_5669ec.png?w=905)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_5669ec.png)

This is confirmed by the malicious sign in page for Google which sends those credentials to the criminals. We should note that victims that arrive at this page are most likely Semrush users, given the path they took to get here.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_7cf9f9.png?w=907)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_7cf9f9.png)

**Google Analytics and Search Console Data Theft**
--------------------------------------------------

_Disclaimer: The following is not taken from a real compromise but rather is meant to illustrate the importance and extent of owning the credentials for a valuable Google account._

Google Analytics (GA) and Google Search Console (GSC) contain critical and confidential information for businesses, revealing detailed perspectives on website performance, user behavioral patterns, and strategic business focuses.

If a Google account is compromised, the malicious actors can access the raw data directly without having to log into Semrush.

E-commerce tracking in GA shows revenue, transaction volumes, average order values, and conversion rates by channel (organic search, paid ads).

Here’s a local shop selling products to a niche audience in a major U.S. city.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_1776e7.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_1776e7.png)

When malicious actors access the Google Analytics account, they can see a wealth of confidential information belonging to the publisher. For companies, this is a direct peek into financial performance.

The GSC account below is connected to Semrush. In GSC, the bad actors could see historical data for the past 16 months, including but not limited to search queries, pages, countries, devices, search appearance and dates.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_84f708.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_84f708.png)

**Semrush Fraud and spear-phishing**

_Disclaimer: Similarly, the following screenshots were not taken from an actual compromise, but highlight the interconnectivity between Google and Semrush accounts._

As mentioned earlier, Google Analytics and Google Search Console data is often integrated with tools like Semrush for enhanced analysis.

For new projects, the SaaS platform requests validation from a Google account to allow Semrush to see and download GA and GSC data.

Once this is done, we can export behavioral data and KPIs coming directly from Google Search Console (GSC) without direct access to the Google account.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_525d2c.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_525d2c.png)

There is additional information stored in a Semrush account (name, phone, business name, address, email and the last 4 digits of a Visa card) that a threat actor could leverage to impersonate an individual or business.

Posing as the business, a threat actor could deceive vendors or partners into sending payments to fraudulent accounts, exploiting the trust tied to the business’s identity.

[![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_007b52.png?w=1024)](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/03/image_007b52.png)

The combination of billing information and card details could be used to mount a more comprehensive attack. Someone posing as Semrush support, referencing an upcoming payment or the billing update process, could trick the victim into providing full credit card details.

**Conclusion**
--------------

Brand impersonation continues to be a popular attack vector used by online criminals to get access to valuable account credentials.

As Google Search is a central part of the SEO and ad ecosystems, individuals and businesses who inadvertently click on a malicious ad are at a major risk of losing extremely sensitive data and feel the impact of fraud on many levels. 

This should be a wakeup call to take steps to prevent such exposure by enforcing guard rails to anyone who manages an account for themselves or a company.

If you are a Malwarebytes customer, you are already protected against the malicious ads and sites used in this campaign. All these incidents have also been reported directly to Google.

_We would like to thank the folks at Silent Push for giving us access to their [platform](https://www.silentpush.com/community-edition/), enabling us to uncover additional infrastructure._

**Malicious** Semrush domains
-----------------------------

adsense-word\[.\]com  
auth\[.\]semrush\[.\]help  
sem-russhh\[.\]com  
sem-rushhh\[.\]com  
sem-rushh\[.\]com  
semrush\[.\]click  
semrussh\[.\]sbs  
semrush\[.\]tech  
seemruush\[.\]com  
semrush-auth\[.\]com  
auth.seem-rush\[.\]com  
ads-semrush\[.\]com  
semrush-pro\[.\]co  
semrush-pro\[.\]click  
auth.sem-ruush\[.\]com  
semrush\[.\]works

**We don’t just report on threats – we help safeguard your entire digital identity**

Cybersecurity risks should never spread beyond a headline. Protect your—and your family’s—personal information by using [identity protection](https://www.malwarebytes.com/identity-theft-protection).

Article Link: [Semrush impersonation scam hits Google Ads | Malwarebytes](https://www.malwarebytes.com/blog/cybercrime/2025/03/semrush-impersonation-scam-hits-google-ads)

1 post - 1 participant

[Read full topic](https://malware.news/t/semrush-impersonation-scam-hits-google-ads/92338)

#### [Source](https://malware.news/t/semrush-impersonation-scam-hits-google-ads/92338)

<br/>
---
