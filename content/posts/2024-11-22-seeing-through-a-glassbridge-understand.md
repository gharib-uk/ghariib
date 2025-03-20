---
title: "Seeing Through a GLASSBRIDGE Understanding the Digital Marketing Ecosystem Spreading Pro-PRC Influence Operations"
date: Fri, 22 Nov 2024 10:00:00 +0000
draft: false
type: posts
categories: 
- Threat Intelligence
---
# Seeing Through a GLASSBRIDGE Understanding the Digital Marketing Ecosystem Spreading Pro-PRC Influence Operations

<br/>

<br/>
Written by: Vanessa Molter

Special thanks to Mandiant's Ryan Serabian for his contributions to this analysis.

* * *

_UPDATE (December 4): This blog post was updated to include example domains associated with GLASSBRIDGE._

This blog post details GLASSBRIDGE—an umbrella group of four different companies that operate networks of inauthentic news sites and newswire services tracked by the Google Threat Intelligence Group (consisting of Google’s Threat Analysis Group (TAG) and Mandiant). Collectively these firms bulk-create and operate hundreds of domains that pose as independent news websites from dozens of countries, but are in fact publishing thematically similar, inauthentic content that emphasizes narratives aligned to the political interests of the People’s Republic of China (PRC). Since 2022, Google has blocked more than a thousand GLASSBRIDGE-operated websites from eligibility to appear in Google News features and Google Discover because these sites violated our [policies](https://support.google.com/news/publisher-center/answer/6204050?hl=en) that prohibit deceptive behavior and require editorial transparency. 

We cannot attribute who hired these services to create the sites and publish content, but assess the firms may be taking directions from a shared customer who has outsourced the distribution of pro-PRC content via imitation news websites.

These campaigns are [another example](https://blog.google/threat-analysis-group/prigozhin-interests-and-russian-information-operations/) of private public relations (PR) firms conducting coordinated influence campaigns—in this case, spreading content aligned with the PRC’s views and political agenda to audiences dispersed across the globe. By using private PR firms, the actors behind the information operations (IO) gain plausible deniability, obscuring their role in the dissemination of coordinated inauthentic content.

The Basics
----------

These inauthentic news sites are operated by a small number of stand-alone digital PR firms that offer newswire, syndication and marketing services. They pose as independent outlets that republish articles from PRC state media, press releases, and other content likely commissioned by other PR agency clients. In some cases, they publish localized news content copied from legitimate news outlets. We have also observed content from [DRAGONBRIDGE](https://blog.google/threat-analysis-group/google-disrupted-dragonbridge-activity-q1-2024/), the most prolific IO actor TAG tracks, disseminated in these campaigns. 

Although the four PR firms discussed in this post are separate from one another, they operate in a similar fashion, bulk-creating dozens of domains at a time and sharing thematically similar inauthentic content. Based on the set of inauthentic news domain names, the firms target audiences outside the PRC, including Australia, Austria, Czechia, Egypt, France, Germany, Hungary, Kenya, India, Indonesia, Japan, Luxemburg, Macao, Malaysia, New Zealand, Nigeria, Poland, Portugal, Qatar, Russia, Saudi Arabia, Singapore, South Korea, Spain, Switzerland, Taiwan, Thailand, Turkey, the United States, Vietnam, and the Chinese-speaking diaspora.

The use of newswire services is a shared tactic across all campaigns, and two of the PR firms directly control and operate the newswire services.

![GLASSBRIDGE is an ecosystem of companies and newswire services that publish inauthentic news content](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/glassbridge-fig1a.gif)

Figure 1: GLASSBRIDGE is an ecosystem of companies and newswire services that publish inauthentic news content

The Most Prolific: Shanghai Haixun Technology
---------------------------------------------

Of the PR and marketing firms we have observed supporting pro-China IO campaigns, the most prolific is Shanghai Haixun Technology Co., Ltd or “Haixun”. Since TAG first began [tracking](https://blog.google/threat-analysis-group/tag-bulletin-q3-2022/) Haixun, Google has removed more than 600 policy-violating domains linked to the firm from the ability to appear in Google News features. The sites target English- and Chinese-speaking audiences, as well as audiences in a number of countries such as Brazil, India, Japan, Kenya, Korea, Malaysia, Saudi Arabia, Singapore, Spain, Russia, Thailand, Qatar, and Vietnam. Google has also terminated a limited number of policy-violating YouTube channels tied to the group. 

In July 2023, [Mandiant identified Haixun using both Times Newswire and World Newswire](https://cloud.google.com/blog/topics/threat-intelligence/pro-prc-haienergy-us-news/) to place pro-Beijing content on the subdomains of legitimate news outlets. Mandiant also identified Haixun’s use of freelance services such as Fiverr to recruit for-hire social media accounts to promote pro-Beijing content.

Haixun’s inauthentic news sites are generally low quality, and much of the content on the domains is spammy and repetitive. Mixed in with “filler” articles on topics such as the metaverse, the sites publish news content that is politically aligned to the views of the PRC government. This includes articles from the Global Times, a PRC state-controlled media outlet, and narratives aligned to common PRC talking points on Beijing’s territorial claims in the South China Sea, Taiwan, ASEAN, Falun Gong, Xinjiang, and the COVID-19 pandemic.

![Haixun inauthentic news featuring a mix of content, including PRC government talking points, Global Times articles, and content on the metaverse](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig2.max-1000x1000.png)

Figure 2: Haixun inauthentic news featuring a mix of content, including PRC government talking points, Global Times articles, and content on the metaverse

Times Newswire and Shenzhen Haimai Yunxiang Media 
--------------------------------------------------

In February 2024, we removed policy-violating domains from appearing on Google News surfaces associated with a pro-PRC coordinated influence campaign reported by Citizen Lab as [PAPERWALL](https://citizenlab.ca/2024/02/paperwall-chinese-websites-posing-as-local-news-outlets-with-pro-beijing-content/) that operated a network of over 100 websites in more than 30 countries masquerading as local news outlets. The imitation news sites published localized news content copied from legitimate local news outlets alongside articles republished from PRC state-controlled media, as well as press releases, conspiracy theories, and ad hominem attacks targeting specific individuals. 

Based on technical indicators, TAG determined the inauthentic news websites were operated and controlled directly by Times Newswire, one of the news wire services that has distributed content on behalf of Haixun. TAG believes Times Newswire is, in turn, operated by another Chinese media company, Shenzhen Haimai Yunxiang Media Co., Ltd., or “Haimai”, which bills itself as a service provider specialized in global media communication and overseas network promotion. 

The views expressed in the conspiracy and smear content were similar to past pro-PRC IO campaigns—for example, character attacks against the Chinese virologist Yan Limeng and claims that the US is conducting biological experiments on humans. Much of the smear content targeting specific individuals was ephemeral—it was posted on imitation news sites for a short period of time and then removed. 

DURINBRIDGE
-----------

Another example of a commercial firm distributing content linked to pro-China IO campaigns is DURINBRIDGE, an alias we use to track a technology and marketing company that has multiple subsidiaries that provide news and PR services. DURINBRIDGE operates a network of over 200 websites designed to look like independent media outlets that publish news content on various topics. These domains violated our policies and have been blocked from appearing on Google News surfaces and Discover.

Importantly, DURINBRIDGE itself is not an IO actor and likely published the IO content on behalf of a customer or partner. Most of the content on the sites is news and press releases from various sources and has no apparent links to coordinated influence campaigns. However, a small portion of the content includes pro-PRC narratives and content directly linked to IO campaigns from Haixun and DRAGONBRIDGE. DURINBRIDGE sites also used articles and images from Times Newswire, which is operated by the aforementioned Chinese PR firm Haimai. 

We identified multiple DRAGONBRIDGE articles published to DURINBRIDGE’s news sites. The content included narratives focused on exiled businessman Guo Wengui, a perennial topic for DRAGONBRIDGE, and multiple narratives amplified by DRAGONBRIDGE in the lead up to the Taiwanese presidential election.

![DRAGONBRIDGE content published to inauthentic news sites operated by DURINBRIDGE](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig3.max-1000x1000.png)

Figure 3: DRAGONBRIDGE content published to inauthentic news sites operated by DURINBRIDGE

![“Secret History of Tsai Ing-Wen,” on DURINBRIDGE-operated inauthentic news site](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig4.max-1000x1000.png)

Figure 4: “Secret History of Tsai Ing-Wen,” on DURINBRIDGE-operated inauthentic news site

![Narratives about then-candidate Lai Ching-te promoted by DRAGONBRIDGE prior to the Taiwanese presidential election](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig5.max-1000x1000.png)

Figure 5: Narratives about then-candidate Lai Ching-te promoted by DRAGONBRIDGE prior to the Taiwanese presidential election

Shenzhen Bowen Media
--------------------

In early 2024, TAG and Mandiant identified a fourth marketing firm that operates a network of over 100 domains that pose as independent news sites focused on countries and cities across Europe, the Americas, Asia, and Australia. These domains violated our policies and have been blocked from appearing on Google News surfaces and Discover. The operator of the sites, Shenzhen Bowen Media Information Technology Co., Ltd., is a PRC-based marketing firm that also operates World Newswire, the same press release service used by Haixun to place content on the subdomains of legitimate news outlets.

![Sites linked to Shenzhen Bowen with localized content for Brazil and Germany](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig6.max-1000x1000.png)

Figure 6: Sites linked to Shenzhen Bowen with localized content for Brazil and Germany

Shenzhen Bowen’s sites present themselves as local outlets focused on a particular country or city, with articles in the local language about business, sports, and politics. The content is in multiple languages, aligned to each target audience, including Chinese, English, French, German, Japanese, and Thai. The sites do not disclose their connection to the marketing firm. 

Side-by-side with local content, the sites include narratives promoting the Chinese government’s interests, much of it sourced from World Newswire. In more than one case, TAG and Mandiant have identified content linked to DRAGONBRIDGE published on Shenzhen Bowen-operated sites.

![DRAGONBRIDGE content on “Boston Journal” website linked to Shenzhen Bowen Media](https://storage.googleapis.com/gweb-cloudblog-publish/images/glassbridge-fig7.max-1000x1000.png)

Figure 7: DRAGONBRIDGE content on “Boston Journal” website linked to Shenzhen Bowen Media

Conclusion
----------

The inauthentic news sites operated by GLASSBRIDGE illustrate how information operations actors have embraced methods beyond social media in an attempt to spread their narratives. We have observed similar behavior from [Russian](https://blog.google/threat-analysis-group/prigozhin-interests-and-russian-information-operations/) and [Iranian](https://blog.google/threat-analysis-group/tag-bulletin-q3-2024/) IO actors. By posing as independent, and often local news outlets, IO actors are able to tailor their content to specific regional audiences and present their narratives as seemingly legitimate news and editorial content. In fact, the content has been crafted or amplified by PR and newswire firms who conceal their role, or actively misrepresent their content as local and independent news coverage. In the case of GLASSBRIDGE, the consistency in content, behavioral similarities, connections across firms, and pro-PRC messaging suggests the private firms take direction from a shared customer who outsourced the creation of influence campaigns. Google is committed to information transparency, and we will continue tracking GLASSBRIDGE and blocking their inauthentic content on Google’s platforms. We regularly disclose our latest enforcement actions in the [TAG Bulletin](https://blog.google/threat-analysis-group/). 

Indicators of Compromise
------------------------

**Group**

**Domain**

Shenzhen Haimai Yunxiang Media

updatenews\[.\]info

Shenzhen Haimai Yunxiang Media

volgogradpost\[.\]com

Shenzhen Haimai Yunxiang Media

ulstergrowth\[.\]com

Shenzhen Haimai Yunxiang Media

tulunet\[.\]com

Shenzhen Haimai Yunxiang Media

torinohuman\[.\]com

Shenzhen Haimai Yunxiang Media

teotihuacaneco\[.\]com

Shenzhen Haimai Yunxiang Media

taurustimes\[.\]com

Shenzhen Haimai Yunxiang Media

tarragonapost\[.\]com

Shenzhen Haimai Yunxiang Media

stptb\[.\]org

Shenzhen Haimai Yunxiang Media

sendaishimbun\[.\]com

Shenzhen Bowen Media

pollandobserver\[.\]com

Shenzhen Bowen Media

japandigest\[.\]net

Shenzhen Bowen Media

melbournenews\[.\]org

Shenzhen Bowen Media

japanheadline\[.\]com

Shenzhen Bowen Media

voiceofthailand\[.\]com

Shenzhen Bowen Media

dailybangkokpost\[.\]com

Shenzhen Bowen Media

kolnerzeitschrift\[.\]de

Shenzhen Bowen Media

italiannotizie\[.\]com

Shenzhen Bowen Media

macaoposts\[.\]com

Shenzhen Bowen Media

dailysydney\[.\]net

Shenzhen Bowen Media

bitcoinnewstokyo\[.\]com

Shanghai Haixun Technology

eutimes\[.\]fr

Shanghai Haixun Technology

hurriyetbusiness\[.\]com

Shanghai Haixun Technology

saudiweekly\[.\]com

Shanghai Haixun Technology

newzealandgazette\[.\]com

Shanghai Haixun Technology

macaomorning\[.\]com

Shanghai Haixun Technology

technologykr\[.\]com

Shanghai Haixun Technology

thaicitynews\[.\]com

Shanghai Haixun Technology

taiwanweekly\[.\]com

Shanghai Haixun Technology

malaysiaunion\[.\]com

Shanghai Haixun Technology

bitcherrynet\[.\]cn

Shanghai Haixun Technology

qichetimes\[.\]com

Shanghai Haixun Technology

digitaljapan\[.\]info

Shanghai Haixun Technology

baobeimama\[.\]com\[.\]cn

Shanghai Haixun Technology

moderntimes\[.\]com\[.\]cn

Shanghai Haixun Technology

jakartapost\[.\]org

Shanghai Haixun Technology

uscitynews\[.\]com

Shanghai Haixun Technology

russiabbs\[.\]com

Shanghai Haixun Technology

turkishdaily\[.\]org

Shanghai Haixun Technology

dertagesspiegel\[.\]com

Shanghai Haixun Technology

spaintour\[.\]club

DURINBRIDGE

dailydispatcher\[.\]com

DURINBRIDGE

easterntribunal\[.\]com

DURINBRIDGE

suratkhabar\[.\]com

DURINBRIDGE

buzzingasia\[.\]com

DURINBRIDGE

internasionalkini\[.\]com

DURINBRIDGE

thinkbusinesstoday\[.\]com

DURINBRIDGE

stargazersarchive\[.\]com

DURINBRIDGE

starsgazette\[.\]com

DURINBRIDGE

glamorousnews\[.\]com

DURINBRIDGE

lifevoyageurs\[.\]com

DURINBRIDGE

heartofmalaysia\[.\]com

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/glassbridge-pro-prc-influence-operations/)

<br/>
---
