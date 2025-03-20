---
title: "Microsoft advertisers phished via malicious Google ads"
date: 2025-02-01
---

Just days after we uncovered a campaign targeting Google Ads accounts, a similar attack has surfaced, this time aimed at Microsoft advertisers. These malicious ads, appearing on Google Search, are designed to steal the login information of users trying to access Microsoft’s advertising platform.

Microsoft does purchase ad space on its rival’s dominant search engine; however, we found Google sponsored results for “Microsoft Ads” (formerly known as Bing Ads) that contained malicious links created by impostors.

Through shared artifacts, we were able to identify additional phishing infrastructure targeting Microsoft accounts going back to a couple of years at least. We have reported these incidents to Google.

## Fake Microsoft Ads caught on Google Search

Microsoft made an estimated $12.2 billion in search and news advertising revenues (including Bing) in 2023, which pales in comparison to its rival, Google, holding a much larger share of the search engine market.

Since the advertising ecosystem allows for an open competition between brands, Microsoft is trying to get traffic and earn clicks from Google searches. During our investigation, we saw sponsored results for Microsoft Ads and Bing Ads that managed to slip through Google’s security checks:

<figure>

![Figure 1: A Google search for 'microsoft ads'](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_d1e94e.png)

<figcaption>

Figure 1: A Google search for ‘microsoft ads’

</figcaption>

</figure>

## Redirection, cloaking and Cloudflare

The threat actors are using different techniques to evade detection and drop traffic from bots, security scanners and crawlers. Unwanted IP addresses (_e.g._ VPNs) are immediately redirected to a bogus marketing website (_Figure 2_). This is also known as a “white page”, meaning it looks innocent and hides its maliciousness.

<figure>

![Figure 2: Cloaking page](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_5b8f26.png)

<figcaption>

Figure 2: Cloaking page

</figcaption>

</figure>

Users that appear to be genuine are presented with a Cloudflare challenge to verify they are human. This is a legitimate instance of Cloudflare, unlike the “ClickFix” type-of-attacks that have become very common place and trick people into pasting and executing malicious code.

<figure>

![Figure 3: Cloudflare verification](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_afa5af.png)

<figcaption>

Figure 3: Cloudflare verification

</figcaption>

</figure>

## Rickroll for the cheaters

After a successful Cloudflare check, users are redirected to the final phishing page via a special URL, that acts as some sort of entry point for the malicious domain _ads\[.\]mcrosoftt\[.\]com_. You can see the network requests related to this redirection chain in _Figure 4_ below.

<figure>

![Figure 4: Network traffic for full redirection](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_fadae1.png)

<figcaption>

Figure 4: Network traffic for full redirection

</figcaption>

</figure>

If you were to visit that domain directly instead of going through the proper ad click you’d be greeted with a rickroll, an internet meme designed to make fun of someone. The sandbox for the web urlscan.io has several examples of crawl requests for URLs on that server (_37.120.222\[.\]165_) that all went to the rickroll.

<figure>

![Figure 5: Rickroll redirect](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_15d4ce.png)

<figcaption>

Figure 5: Rickroll redirect

</figcaption>

</figure>

## Phishing page

After much subversion, real victims finally see the phishing page for the Microsoft Advertising platform. The full URL in the address bar is meant to imitate the legitimate one (_ads.microsoft.com_).

<figure>

![Figure 6: Microsoft Advertising phishing page](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_d6853f.png?w=1024)

<figcaption>

Figure 6: Microsoft Advertising phishing page

</figcaption>

</figure>

The phishing page gives user a fake error message enticing them to reset their password and seemingly tries to get past 2-Step verification as well. Handling 2FA has become a standard feature in most phishing kits, due to the rise in user adoption of this additional security layer.

<figure>

![Figure 7: Phishing steps](https://www.malwarebytes.com/wp-content/uploads/sites/2/2025/01/image_66f74b.png?w=1024)

<figcaption>

Figure 7: Phishing steps

</figcaption>

</figure>

## Larger campaign

Going back to _urlscanio_, we fed it the special entry URL and it was able to navigate to the phishing page. From there, we can look at the various web requests and find something to pivot on in order to identify additional infrastructure.

The _favicon.ico_ file is one starting point and we can query for any scans that match its hash, excluding the official Microsoft domain. The results show that in the past week, there were several other domains that appear to be related to the theft of Microsoft Ads accounts.

But this campaign appears to go back further at least a couple of years and maybe more, although it becomes somewhat tricky to know if the malicious infrastructure is tied to the same threat actors. It’s worth noting that several of the domains are either hosted in Brazil or have the ‘_.com.br_‘ Brazilian top-level domain.

What we discovered may only be the tip of the iceberg; by starting to investigate compromised advertiser accounts we may very well have opened Pandora’s box. This isn’t only Google or Microsoft ad accounts we are talking about, but potentially for Facebook, and many others. Of course, our scope so far has been Google Search, but we know that other platforms are rife with such phishing attacks.

These recent malvertising campaigns highlight the ongoing threat of phishing through online advertising. While tech companies like Google work to combat these issues, users must remain vigilant. Here are some key steps you can take to protect yourself:

- **Verify URLs:** Always carefully examine the URL in your browser’s address bar before entering any credentials. Scrutinize URLs for inconsistencies or misspellings.

- **Use 2-Step verification wisely:** it adds an extra layer of security to your accounts, but you still need to pay attention to requests before granting them access.

- **Regularly monitor your accounts:** Check your advertising accounts for any suspicious activity such as changes in administrator accounts.

- **Report Ads:** If you encounter a suspicious ad, report it to for the benefit of other users.

* * *

**We don’t just report on threats—we block them**

_Malwarebytes Browser Guard offers traditional ad-blocking augmented with advanced heuristic detection. Download it today._

## Indicators of Compromise

The following IOCs are comprised of domains that shared attributes with our initial phishing page, including the favicon and images. Some of them go back further but are provided for threat hunters who may wish to further investigate these campaigns.

```
30yp[.]comaboutadvertselive[.]comaboutblngmicro[.]cloudaccount-microsoft[.]onlineaccount-microsoft[.]siteaccount-mircrosoft-ads[.]comaccount[.]colndcx-app[.]comaccounts-ads[.]siteaccounts-mircrosoft-ads[.]onlineacount-exchang[.]storeadmicrosoft[.]comadmicrsdft[.]comads-adversitingb[.]comads-dsas[.]siteads-microsoft[.]clickads-microsoft[.]coachb-learning[.]comads-microsoft[.]liveads-microsoft[.]lubrine[.]com[.]brads-microsoft[.]onlineads-microsoft[.]shopads-microsoftz[.]onlineads-miicrosoft[.]comads-mlcrosft[.]comads-mlcrosoft-com[.]blokchaln[.]comads[.]microsoft[.]com[.]euroinvest[.]geads[.]mlcr0soft[.]comads[.]mlcrosoft[.]com[.]ciree[.]com[.]brads[.]mlcrosoft[.]com[.]poezija[.]com[.]hrads[.]rnlcrosoft[.]com[.]euroinvest[.]geadslbing[.]comadsmicro[.]exchangefastex[.]cloudadsmicrosoft[.]shopadsverstoni[.]comadvertiseliveonline[.]comadvertising-bing[.]siteadvertising-mlcrosoft[.]orgadverts2023[.]onlineadvertsingsinginbing[.]comagency-wasabi[.]comapp[.]beefylswap[.]topbîlkub[.]combing-ads[.]combing[.]login-acount[.]mebitmax-us[.]comblngad[.]onlineblseaccount[.]cloudbltrue[.]colnhouse-fr[.]uscôinlíst[.]onlinecolneex-plalform[.]cloudconnec-exchan[.]sitedigitechmedia[.]agencyforteautomobile[.]comglobal-verifications[.]comglobal-verify[.]comhomee-acount[.]comitlinks[.]com[.]cnkrakeri-login[.]comlogin-adsmicrosoft[.]helpexellent[.]comlogin[.]adsadvertising[.]onlinelogin[.]microsofttclicks[.]livemicrasofit[.]xyzmicroosft[.]accounts-ads[.]sitemicrosoft-ads[.]websitemicrosoftadss[.]commicrosoftadversiting[.]cloudmicrosoftbingads[.]commicrosofyt[.]adversing-publicidade[.]promictrest[.]mnws[.]rumlcrosoft-bing-acces[.]clickmlcrosoftadvertlsing[.]onlinemudinhox[.]sitendnet[.]shopphlyd[.]comportfoliokrakenus[.]comportfoliolkraken[.]comportfoliopro-us[.]comportfolioskranen[.]comportofolioprospots[.]compotfoliokeiolenen[.]compotfoliokelaken[.]compotfoliokelaneken[.]compotfoliokenaiken[.]compotfoliokenkren[.]compotfolioketonelen[.]compotfolioskaneken[.]compotfolioskenaken[.]compotfolioskraineken[.]compotfolioskranaken[.]compotfolioskraneken[.]compro-digitalus[.]comprokrakenportfolio[.]comrnlcrosoft[.]smartlabor[.]itsig-in-mlcrosoft-advertisings[.]siteuiiadvertise[.]onlinewvvw-microsoft[.]xyzwww-bingads[.]comwww-microsoftsads[.]comwww-v[.]userads[.]digitalwww34[.]con-webs[.]comwww55[.]con-webs[.]comads-microsoft[.]bewears[.]comads[.]msicrosoft[.]com
```

Go to Source
