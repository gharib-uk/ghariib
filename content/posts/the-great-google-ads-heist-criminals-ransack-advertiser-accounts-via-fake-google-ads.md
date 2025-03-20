---
title: "The great Google Ads heist: criminals ransack advertiser accounts via fake Google ads"
date: 2025-01-17
---

## Table of contents

- Overview

- Criminals impersonate Google Ads

- Lures hosted on Google Sites

- Phishing for Google account credentials

- Victimology

- Who is behind these campaigns?

- Fuel for other malware and scam campaigns

- Indicators of Compromise

## Overview

Online criminals are targeting individuals and businesses that advertise via Google Ads by phishing them for their credentials — ironically — via fraudulent Google ads.

The scheme consists of stealing as many advertiser accounts as possible by impersonating Google Ads and redirecting victims to fake login pages. We believe their goal is to resell those accounts on blackhat forums, while also keeping some to themselves to perpetuate these campaigns.

This is the most egregious malvertising operation we have ever tracked, getting to the core of Google’s business and likely affecting thousands of their customers worldwide. We have been reporting new incidents around the clock and yet keep identifying new ones, even at the time of publication.

The following diagram illustrates at a high level the mechanism by which advertisers are getting fleeced:

<figure>

![Figure 1: Process flow for this Google Ads heist campaign](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_290a44.png)

<figcaption>

_Figure 1: Process flow for this Google Ads heist campaign_

</figcaption>

</figure>

_Back to top_

## Criminals impersonate Google Ads

Advertisers are constantly trying to outbid each other to reach potential customers by buying ad space on the world’s number one search engine. This earned Google a whopping $175 billion in search-based ad revenues in 2023. Suffice to say, the budgets spent in advertising can be considerable and of interest to crooks for a number of reasons.

We first started noticing suspicious activity related to Google accounts somewhat accidentally, and after a deeper look we were able to trace it back to malicious ads for… Google Ads itself! Very quickly we were overwhelmed by the onslaught of fraudulent “Sponsored” results, specifically designed to impersonate Google Ads, as can be seen in _Figure 2_:

<figure>

![Figure 2: A malicious ad masquerading as Google Ads](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_b6573b.png)

<figcaption>

_Figure 2: A malicious ad masquerading as Google Ads_

</figcaption>

</figure>

While it is hard to believe such a thing could actually happen, the proof is there when you click on the 3-dot menu that shows more information about the advertiser. We have partially masked the victim’s name, but clearly it is not Google; they are just one of the many accounts that have already been compromised and abused to trick more users:

<figure>

![Figure 3: The advertiser behind this ad is not affiliated with Google at all](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_faf82f.png?w=1024)

<figcaption>

_Figure 3: The advertiser behind this ad is not affiliated with Google at all_

</figcaption>

</figure>

People who will see those ads are individuals or businesses that want to advertise on Google Search or already do. Indeed, we saw numerous ads specifically for each scenario, sign up or sign in, as seen in _Figure 4_:

<figure>

![Figure 4: Two ads for signing up and sign in to Google Ads respectively](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_2e86de.png)

<figcaption>

_Figure 4: Two ads for signing up and sign in to Google Ads respectively_

</figcaption>

</figure>

The fake ads for Google Ads come from a variety of individuals and businesses, in various locations. Some of those hacked accounts already had hundreds of other legitimate ads running, and one of them was for a popular Taiwanese electronics company.

<figure>

![Figure 5: Victim accounts spending their own budgets on fake Google Ads](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_6a5b44.png?w=1024)

<figcaption>

_Figure 5: Victim accounts spending their own budgets on fake Google Ads_

</figcaption>

</figure>

To get an idea of the geographic scope of these campaigns, we performed the same Google search simultaneously from several different geolocations (using proxies). First, here’s the malicious ad from a U.S. IP address belonging to a business registered in Paraguay:

<figure>

![Figure 6: U.S.-based search showing fake Google ad](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_c9ada3.png)

<figcaption>

_Figure 6: U.S.-based search showing fake Google ad_

</figcaption>

</figure>

Now, here’s that same ad that appears on Google Search in several other countries:

<figure>

![Figure 7: The same ad found in different countries](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_6bd40b.png)

<figcaption>

_Figure 7: The same ad found in different countries_

</figcaption>

</figure>

_Back to top_

## Lures hosted on Google Sites

Once victims click on those fraudulent ads, they are redirected to a page that looks like Google Ads’ home page, but oddly enough, it us hosted on Google Sites. These pages act as a sort of gateway to external websites specifically designed to steal the usernames and passwords from the coveted advertisers’ Google accounts.

<figure>

![Figure 8: A malicious Google Sites page impersonating Google Ads](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_cb6099.png)

<figcaption>

_Figure 8: A malicious Google Sites page impersonating Google Ads_

</figcaption>

</figure>

There’s a good reason to use Google Sites, not only because it’s a free and a disposable commodity but also because it allows for complete impersonation. Indeed, you cannot show a URL in an ad unless your landing page (final URL) matches the same domain name. While that is a rule meant to protect abuse and impersonation, it is one that is very easy to get around.

<figure>

![Figure 9: The rule that stipulates display URLs and final URLs must have matching domains](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_a11de7.png)

<figcaption>

_Figure 9: The rule that stipulates display URLs and final URLs must have matching domains_

</figcaption>

</figure>

Looking back at the ad and the Google Sites page, we see that this malicious ad does not strictly violate the rule since _sites**.google.com**_ uses the same root domains ads _ads**.google.com**_. In other words, it is allowed to show this URL in the ad, therefore making it indistinguishable from the same ad put out by Google LLC..

<figure>

![Figure 10: The malicious ad does not violate Google's rule on the use of the display URL](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_aecc0e.png)

<figcaption>

_Figure 10: The malicious ad does not violate Google’s rule on the use of the display URL_

</figcaption>

</figure>

_Back to top_

## Phishing for Google account credentials

After the victims click on the “Start now” button found on the Google Sites page, they are redirected to a different site which contains a phishing kit. JavaScript code fingerprints users while they go through each step to ensure all important data is being surreptitiously collected.

<figure>

![Figure 11: The actual phishing page that follows](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_baa2d6.png)

<figcaption>

_Figure 12: The actual phishing page that follows_

</figcaption>

</figure>

Finally, all the data is combined with the username and password and sent to the remote server via a POST request. We see that criminals even receive the victim’s geolocation, down to the city and internet service provider.

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_ea8b00.png?w=634)

<figcaption>

_Figure 12: POST web request with victim’s details_

</figcaption>

</figure>

_Back to top_

## Victimology

There are multiple online reports of people who saw the fake Google Ads and shared their experiences:

- Help with removing a dangerous scam in Google Ads (_Google Ads Help forum_)

- Google Ads Phishing Scam (_Reddit_)

- It’s just me or Google just sponsored a link to a phising site for Google ads? (_Reddit_)

- Be aware of fake google page, clicked by accident (_Reddit_)

- Warning! First sponsorized google answer for “Google ads” is a phishing attempt ! (_BlueSky_)

We were able to get in touch with a couple of victims who not only saw the ads but were actually scammed and lost money. Thanks to their testimony and our own research, we have a better idea of the criminals’ modus operandi:

- Victim enters their Google account information into phishing page

- Phishing kit collects unique identifier, cookies, credentials

- Victim may receive an email indicating a login from an unusual location (Brazil)

- If the victim fails to stop this attempt, a new administrator is added to the Google Ads account via a different Gmail address

- Threat actor goes on a spending spree, locks out victim if they can

_Back to top_

## Who is behind these campaigns?

We identified two main groups of criminals running this scheme but the more prolific by far is one made of Portuguese speakers likely operating out of Brazil. Victims have also shared that they had received a notification from Google indicating suspicious logins from Brazil. Unfortunately, those notifications often came too late or where dismissed as legitimate, and the criminals already had time to do some damage.

We should also note a third campaign that is very different from the other two, and where the threat actors’ main goal is to distribute malware. The Google Ads phishing scheme may have been a temporary run which was not their main focus.

### Brazilian team

In the span of a few days, we reported over 50 fraudulent ads to the Google Ad team all coming from this Brazilian group. We quickly realized that no matter how many reported incidents and takedowns, the threat actors managed to keep at least one malicious ad 24/7.

_Figure 13_ shows the network traffic resulting from a click on the ad. You will see multiple hops before finally arriving to the phishing portal. The second URL shows the crooks are using a paid service to detect fake traffic.

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_ddb2cc.png)

<figcaption>

Figure 13: Network traffic from the ‘Brazilian campaign’

</figcaption>

</figure>

Within the JavaScript code part of the phishing kit, there are comments in Portuguese. _Figure 14_ shows a portion of the code that does browser fingerprinting, which is a way of identifying users. Browser language, system CPU, memory, screen-width, and time zone are some of the data points collected and then hashed.

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_a3017d.png)

<figcaption>

Figure 14: Identifying users via various settings

</figcaption>

</figure>

### Asian team

The second group is using advertiser accounts from Hong Kong and appears to be Asia-based, perhaps from China. Interestingly, they also use the same kind of delivery chain by leveraging Google sites. However, their phishing kit is entirely different from their Brazilian counterparts.

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_807f00.png)

<figcaption>

Figure 15: Web traffic for the ‘Chinese campaign’

</figcaption>

</figure>

_Figure 16_ below shows a code extract with comments in Chinese, as well as a function called _xianshi_, pinyin for 显示 (Xiǎnshì) which means display (_thanks to the person leaving a comment and clarifying_).

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_260bfa.png)

<figcaption>

Figure 16: Code with comments in Chinese

</figcaption>

</figure>

### Third campaign (possibly Eastern European)

We observed another campaign which has a very different modus operandi. Google Sites is not involved at all, and instead they rely on a fake CAPTCHA lure and heavy obfuscation of the phishing page.

Interestingly, the malicious ad we found was for Google Authenticator, despite the obvious ads-goo\[.\]click domain name. However, for about day or so, the redirect from that domain lead directly to a phishing portal hosted at _ads-overview\[.\]com_.

The reason why we suggest the threat actors may be Eastern Europeans here is because of the type of redirects and obfuscation. There is also a distant feel of ‘software download via Google ads’ we have reported on previously (see _Threat actor impersonates Google via fake ad for Authenticator_).

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_ebf79e.png)

<figcaption>

Figure 17: A malicious ad for Google Authenticator and fake CAPTCHA

</figcaption>

</figure>

A PHP script (_cloch.php_) then determines if the visitor is genuine or not (likely doing a server-side IP check). VPNs, bot and detection tools will get a “white” page showing some bogus instructions on how to run a Google Ads campaign. Victims are instead redirected to _ads-overview\[.\]com_ which is a phishing portal for Google accounts.

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_06fde4.png)

<figcaption>

Figure 18: Cloaking in action with a ‘white’ page or the phishing page

</figcaption>

</figure>

When we checked back on this campaign a few days later, we saw that the ad URL now redirected to a fake Google Authenticator site, likely to download malware. The redirection mechanism is shown in _Figure_ 20:

<figure>

![](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_7d0edc.png)

<figcaption>

Figure 19: Web traffic for fake Google Authenticator site

</figcaption>

</figure>

_Back to top_

## Fuel for other malware and scam campaigns

Stolen Google Ads accounts are a valuable commodity among thieves. As we have detailed it many times on this blog, there are constant malvertising campaigns leveraging compromised advertiser accounts to buy ads that push scams or deliver malware.

- Printer problems? Beware the bogus help

- Malicious ad distributes SocGholish malware to Kaiser Permanente employees

- Hello again, FakeBat: popular loader returns after months-long hiatus

- Large scale Google Ads campaign targets utility software

If you think about it for a second, crooks are using someone else’s budget to further continue spreading malfeasance. Whether those dollars are spent towards legitimate ads or malicious ones, Google still earns revenues from those ad campaigns. The losers are the hacked advertisers and innocent victims that are getting phished.

As result, taking action on compromised ad accounts plays a key part in driving down malvertising attacks. Google has yet to show that it takes definitive steps to freeze such accounts until their security is restored, despite their own policy on the subject (_Figure 20_). For example, we recently saw a case where the same advertiser that had already been reported 30 times, was still active.

<figure>

![Figure 20: Google's policy regarding violations](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_faae66.png)

<figcaption>

_Figure 20: Google’s policy regarding violations_

</figcaption>

</figure>

As the scourge of fraudulent ads continues, we urge users to pay particular attention to sponsored results. Ironically, it’s quite possible that individuals and businesses that run ad campaigns are not using an ad-blocker (to see their ads and those from their competitors), making them even more susceptible to fall for these phishing schemes.

**We don’t just report on threats—we block them**

_Cybersecurity risks should never spread beyond a headline. Keep threats off by downloading Malwarebytes Browser Guard today._

_Back to top_

## Indicators of Compromise

Fake Google Sites pages

```
sites[.]google[.]com/view/ads-goo-vgsgoldxsites[.]google[.]com/view/ads-word-cmdwsites[.]google[.]com/view/ads-word-maktsites[.]google[.]com/view/ads-word-whishwsites[.]google[.]com/view/ads-word-wweswsites[.]google[.]com/view/ads-word-xvgtsites[.]google[.]com/view/ads3dfod6hbadvhj678sites[.]google[.]com/view/adwoordsites[.]google[.]com/view/aluado01sites[.]google[.]com/view/ap-rei-pandassites[.]google[.]com/view/appsd-adsdsites[.]google[.]com/view/asd-app-goosites[.]google[.]com/view/connectsing/addsssites[.]google[.]com/view/connectsingyn/adssites[.]google[.]com/view/entteraccesssites[.]google[.]com/view/exercitododeusvivosites[.]google[.]com/view/fjadssites[.]google[.]com/view/goitkm/google-adssites[.]google[.]com/view/hdgsttsites[.]google[.]com/view/helpp2ksites[.]google[.]com/view/hereon/1sku4yfsites[.]google[.]com/view/hgvfvdsites[.]google[.]com/view/joaope-defeijaosites[.]google[.]com/view/jthsjdsites[.]google[.]com/view/logincosturms/adssites[.]google[.]com/view/logins-words-officailssites[.]google[.]com/view/logins-words-officsdpsites[.]google[.]com/view/maneirionhosites[.]google[.]com/view/marchatrasdemarchasites[.]google[.]com/view/newmanage/pagesites[.]google[.]com/view/one-vegassites[.]google[.]com/view/one-vegaswsites[.]google[.]com/view/onvg-ads-wordsites[.]google[.]com/view/oversmart/newsites[.]google[.]com/view/pandareidelsites[.]google[.]com/view/polajdasod6hbadsites[.]google[.]com/view/ppo-adssites[.]google[.]com/view/quadrilhadohomemtanacasakaraiosites[.]google[.]com/view/ricobemnovinhossites[.]google[.]com/view/s-ad-officasites[.]google[.]com/view/s-wppasites[.]google[.]com/view/sdawjjsites[.]google[.]com/view/semcaosites[.]google[.]com/view/sites-gbsites[.]google[.]com/view/soarnovosites[.]google[.]com/view/so-ad-reisdsites[.]google[.]com/view/spiupiupp-gosites[.]google[.]com/view/start-smartssites[.]google[.]com/view/start-smarts/homepage/sites[.]google[.]com/view/umcincosetequebratudosites[.]google[.]com/view/vewsconnectsites[.]google[.]com/view/vinteequatroporquarentasites[.]google[.]com/view/xvs-wods-acesites[.]google[.]com/view/zeroumnaoezerodoissites[.]google[.]com/view/zeroumonlinecomosmp
```

Phishing domains

```
account-costumers[.]siteaccount-worda-ads[.]benephica[.]comaccount-worda-ads[.]cacaobliss[.]ptaccount[.]universitas-studio[.]esaccounts-ads[.]siteaccounts[.]google[.]lt1l[.]comaccounts[.]goosggles[.]comaccounts[.]lichseagame[.]comaccousnt-ads[.]tmcampos[.]ptaccousnt[.]benephica[.]ptaccousnt[.]hyluxcase[.]meaccousnt[.]whenin[.]ptads-goo[.]clickads-goog[.]linkads-google[.]io-es[.]comads-overview[.]comads1.google.lt1l.comads1[.]google[.]veef8f[.]comadsettings[.]siteadsg00gle-v3[.]vercel[.]appadsgsetups[.]shopadvertsing-acess[.]siteadvertsing-v3[.]siteas[.]vn-login[.]shopbenephica[.]ptcacaobliss[.]ptcolegiopergaminho[.]ptdocs-pr[.]toptmcampos[.]ptvietnamworks[.]vn-login[.]shop
```

_Back to top_

Go to Source
