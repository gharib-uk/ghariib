---
title: "Global elections in 2024: Internet traffic and cyber threat trends"
date: 2025-01-02
categories: 
  - "general"
---

Elections define the course of democracies (even as there are several types of democracies), and 2024 was a landmark year, with over 60 countries — plus the European Union — holding national elections, impacting half the world’s population. As highlighted in Pew Research’s global elections report, this was a year of “political disruption,” where the Internet was a relevant stage for both democratic engagement and cyber threats.

At Cloudflare, with our presence in over 330 cities and 120 countries and interconnection with 12,500 networks, we’ve witnessed firsthand the digital impact of these elections. From monitoring Internet traffic patterns to mitigating cyberattacks, we’ve observed trends that reveal how elections increasingly play out online. As detailed in our just-published Cloudflare Impact report, we’ve also worked to protect media outlets, political campaigns, and help elections worldwide.

Here’s the map of countries with national elections that took place in 2024, from our elections report.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3lto8GbhqRNphtxGcSipqj/9422932d5766cd35a050b499161c874f/BLOG-2648_2.png)

We’ve been monitoring 2024 elections worldwide on our blog and in the 2024 Election Insights report available on Cloudflare Radar.

In terms of Internet patterns, we’ve observed how cyber activity in 2024 continues to intersect with real-world events. Online attacks are clearly a significant part of elections, even when unsuccessful in disrupting candidates or election-related websites due to strong protections. Additionally, Internet traffic patterns often vary on election day depending on the country, and government-directed Internet shutdowns continue, including ones related to elections. Email activity is also influenced, especially for more popular candidates in “polarized battles.”

Let’s start our review with attacks. 

## Rising threats: political and election-related cyberattacks in 2024

During 2024, elections saw a rise in DDoS attacks targeting political campaigns, parties, and election infrastructure.

In the **United States**, over 6 billion malicious requests were blocked between November 1-6. A set of DDoS attacks leading up to Election Day on November 5 targeted one of the campaigns with multiple days of attacks, peaking at 700,000 requests per second and sustaining 8 Gbps during major strikes. Key attack tactics included cache-busting, geodiverse patterns, and randomized user agents.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2LUqmbf6tgYWBxAUhux8tf/401681b4b325ff8a90acae824060cd32/BLOG-2648_3.png)

State and local websites also faced increased threats, with 290 million malicious requests blocked since September under Cloudflare’s Athenian Project. Compared to 2020, attacks in 2024 were far more intense, underscoring the growing need for robust cybersecurity to protect elections from disruption.

In **France**, DDoS attacks plagued multiple political parties, with peaks reaching 96,000 requests per second (rps) on election day, July 7. Additional details are available in our related blog post.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2vXeTFoMMMprBcFQHtGT4E/d799e896740ebd74a2803e6d21e6b1d1/BLOG-2648_4.png)

In the **United Kingdom**, DDoS attacks targeted political parties, with the most severe incident affecting a campaign website, reaching 156,000 rps shortly after the results were announced on election day. Additional details are available in our related blog post.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/43DeCSFeJHsPMK3MqgGhMJ/b0cf8050e2751e4fc2cb745ccb93f839/BLOG-2648_5.png)

During the European parliamentary elections in early June, cyberattacks targeted several political websites around election days. Notably, a significant DDoS attack focused on two politically-related websites in the **Netherlands** on June 5–6 (with June 6 being election day), peaking at 73,000 rps.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1Z5F68CqmuIftAd7mNyzht/e5181cdf2fc64bd2f5b3c3a61fbf0374/BLOG-2648_6.png)

In **Romania**, the weeks leading up to the election cycle culminating in the December 1 parliamentary elections saw DDoS attacks targeting political party websites and news organizations.

In **South Africa**, where the general election took place on May 29, there was a relevant DDoS attack in the weeks leading up to the election, targeting a major news site within the country for several days, with a peak on May 7 of 54,000 requests per second.

In **Portugal**, several DDoS attacks targeted political party websites on election day, March 10, particularly after polling stations closed. One political party’s websites experienced a peak of 69,000 rps on May 11 at 00:50 UTC.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3Bh3PXP8WCMOz2hW1ndHJO/84f89609bd8d232104ee682ed04d5fe8/BLOG-2648_7.png)

In **Taiwan**, a local fact-checking website faced a DDoS attack three days before the election, on January 10.

In **Japan**, a DDoS attack targeted a website used to report scams and misinformation a week before the October 27 election.

While some of these rates may seem small to Cloudflare, they can be devastating for websites not well-protected against such high levels of traffic. DDoS attacks not only overwhelm systems but also serve, if successful, as a distraction for IT teams while attackers attempt other types of breaches.

### Election-related Internet shutdowns 

Several times in 2024, election-related Internet shutdowns were imposed by authorities for various reasons, such as in the Comoros and Pakistan.

**Comoros**, a small archipelago country in Southeastern Africa with a population of less than 1 million, held presidential elections on January 14, which led to protests against the re-election of President Azali Assoumani. Authorities shut down the Internet on January 17, causing a 50% drop in traffic compared to the previous week, lasting for two days.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/72ba6O4SEVjDesw4W9hUTP/7a5e06d815f8dcefc91599d030ef5e99/BLOG-2648_8.png)

**Pakistan’s** general election day on February 8 was marked by an Internet shutdown targeting mobile networks. The outage began around 02:00 UTC, reducing Internet traffic by 50% compared to the previous week. Traffic only began recovering after 15:00, highlighting the severe impact of government-initiated shutdowns on Internet connectivity.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7J1emwVOlzOXIqgQicFF0j/76b7410baba969406a138a9a2ce444d9/BLOG-2648_9.png)

In **Mauritius**, an island nation in the Indian Ocean with under 2 million residents, the government suspended access to social media platforms from November 1 to November 11 ahead of the November 10 parliamentary elections. 

## Other election-related Internet traffic trends 

Election-day Internet traffic patterns often reflect a country’s dominant device usage, with mobile-first nations like Indonesia, Mozambique, and Ghana experiencing noticeable traffic drops after polling stations closed. While mobile-friendly countries generally see steady or higher weekend traffic compared to desktop-focused regions like Europe and the Americas, no consistent trend emerged linking device preference to overall election-day traffic increases or decreases.

Here’s a world map from our Year in Review 2024 showing countries where mobile (purple) or desktop (green) dominates Internet traffic.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/PkHEsQJb6AQXtjWkRYfXP/f3d1c9d2d5190f9bd76cbbb1491b3e05/BLOG-2648_10.png)

Now, let’s explore a selection of relevant elections with Internet traffic impacts, ordered by election dates:

**Taiwan (January 13)** Taiwan’s presidential election saw traffic drop slightly during polling hours, especially in the morning with an 8% drop. Traffic returned to usual levels after 17:00 local time. Post-election, traffic rose by 5% the next morning compared to the previous week.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6C1idfrGkUZFDdZY8NJXm4/4cc00af6e267508a622505fccac9571a/BLOG-2648_11.png)

**Finland (January 28)** On January 28, Finland held its presidential election. Internet traffic dropped by 24% at 11:00 local time, coinciding with higher voter turnout in the morning. A second noticeable drop of 13% occurred at 20:00 when polling stations closed and TV stations broadcast initial projections, though traffic was slightly higher than usual afterward.

**Indonesia (February 14)**  Indonesia held its general election on February 14. With over 200 million voters spread across 17,000 islands, it likely had the highest number of voters on a single day, unlike India’s multi-week election. During polling hours (08:00 to 13:00 local time), Internet traffic dropped by up to 15%. Traffic remained lower than the previous week for the rest of the day, with drops ranging from 8% to 16% throughout the night. Mobile device usage surged to 77%, the highest of the year, reflecting Indonesia’s mobile-first Internet culture. Traffic recovered the next morning, surpassing the previous week’s levels.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6GW0q1qUfbJcPbFjd21Vv/965626dabdc6ca4e3eb9e961cdec858a/BLOG-2648_12.png)

**Portugal (March 10)** Portugal’s parliamentary election on March 10 saw a sharp 16% traffic drop at 20:00 local time when TV stations began broadcasting projections. Traffic picked up after that and remained stable during the day.

**Russia (March 17)** Russia’s presidential election showed steady Internet traffic throughout the day but experienced a 7% decrease after polls closed as results and reactions were broadcast on TV. Unlike other countries, where post-election traffic surges are common, Russia’s pattern reflects the strong influence of broadcast media on election coverage.

**South Korea (April 10)** South Korea held legislative elections on April 10. Traffic was higher than usual before 05:00 local time but dropped 14% by 07:15 after polling stations opened at 06:00. By 11:45, traffic had rebounded above typical levels. After polling stations closed at 18:00, traffic dropped again, with a 7% decline compared to the previous week.

**India (April 19–June 1) -** **related blog post** India’s seven-phase general election saw significant Internet traffic fluctuations. May 7 recorded the largest nationwide traffic dip of 6%, with populous states like Uttar Pradesh seeing a 9% drop and Maharashtra experiencing a 17% decline. On the final election day (June 1), mobile device usage peaked at 68%, the highest of the year. These patterns underscore India’s mobile-first Internet habits and its diverse election timelines.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3G1p1RGYaAJy2t85MQkOmf/1b9e666380db8ca6cb5a241092466e3f/BLOG-2648_13.png)

**North Macedonia (April 24 & May 8)** North Macedonia’s two-round presidential election featured a 56% traffic increase after 11:00 local time on May 8, sustained throughout the day. Similar, albeit smaller, trends were observed during the first round on April 24.

**Panama (May 5)** On May 5, Panama’s presidential and parliamentary election day, Internet traffic dropped significantly while voting stations were open, with a 23% decrease in the afternoon and 25% lower traffic at 21:30 local time as results were announced. Traffic picked up after that.

**South Africa (May 29) -** **related blog post** On May 29, South Africa’s general election saw Internet traffic decrease by 16% at 05:45 and remain lower throughout polling hours. Traffic surged by 25% the night before the election, peaking at midnight. Post-election, traffic increased by up to 12% early on May 30, highlighting the transition from offline to online engagement.

**Mexico (June 2) -** **related blog post** Mexico’s general election on June 2 saw a 3% daily traffic drop, with hourly dips of up to 11% during polling hours (08:00–20:00 local time). Traffic surged by 14% at 01:30 the following day as results were announced, peaking at 8% above the previous week by 22:00 local time.

**Iceland (June 1)** Iceland’s presidential election on June 1 saw minor Internet traffic drops, including a 12% dip between 14:00 and 16:00 local time, but traffic increased at night by as much as 11% at 20:00. The day after, traffic rose by 26% compared to the previous week. Iceland elected Halla Tómasdóttir as its second female president.

**European Union (June 6–9) -** **related blog post** The 2024 European Parliament elections showed notable Internet traffic shifts and cybersecurity challenges. The Czech Republic and Slovakia experienced traffic drops of over 10%, while Finland and Ireland saw moderate declines. Key speeches, such as Belgian Prime Minister Alexander De Croo’s resignation and French President Macron’s snap election announcement, also caused traffic fluctuations.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7s9Gc0c394zNszAUuc23Jb/e63c55e6477c875b8427469b881f0ec8/BLOG-2648_14.png)

_Source: Cloudflare; created with Datawrapper_

**Iran (June 28)** Iran’s presidential election saw significant traffic fluctuations, with traffic falling by 16% after 17:30 local time. Extended polling hours (including at night) led to continued drops, falling to 24% lower by 22:30. After midnight, traffic rebounded, showing a 13% increase compared to the previous week.

**France (June 30 & July 7) -** **related blog post** France’s legislative elections brought significant Internet and cybersecurity activity. On July 7, Internet traffic dropped 16% at 20:00 local time as polling stations closed and TV broadcasts announced results. Mobile device usage surged to 58%, and DNS traffic to news outlets spiked by 250% during the first round and by 244% on runoff day, reflecting heightened public interest.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3X3WZo3RMQu9944oUtc23V/2e2c61c3cc0d77045ff8fcd7ff0af8e2/BLOG-2648_15.png)

**United Kingdom (July 4) -** **related blog post** The UK’s general election on July 4 saw the Labour Party win a majority after 14 years of Conservative rule. Internet traffic declined slightly during voting hours, with a 2% drop at noon, before surging in the evening as results were announced. Northern Ireland experienced the sharpest traffic drop (10%), compared to 6% in Scotland and 5% in Wales. DNS traffic to election-related domains peaked with increases of 600% at 22:00 and 671% at 04:00 the following day.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1O4VzkC2RZXUoSiaULXHir/b74a889b506a40ba0ff8f5894055b0b0/BLOG-2648_16.png)

**Sri Lanka (September 21)** Sri Lanka’s presidential election caused a 9% morning traffic dip and an 18% post-election surge after polls closed. Results triggered a 109% traffic increase at 03:00 local time on September 22.

**Tunisia (October 6)** Tunisia’s presidential election saw a 15% traffic dip at 17:00, followed by a 13% decline at 19:30 when results started arriving. The steady traffic decrease highlights the evening focus on offline engagement and result tracking.

**Mozambique (October 9)** Mozambique’s election drove an Internet traffic drop throughout the day, falling as much as 51% by 20:30 local time, and continuing lower than usual after that. A post-election surge of 16% occurred at 01:30. The election, held on a public holiday, resulted in a 31% daily traffic drop compared to the previous week.

**Georgia (October 26)** When Georgia held its parliamentary election on October 26, Internet traffic was 11% higher than the previous week, peaking at 67% above normal around 23:00 when results were announced. Unlike other countries, traffic only dipped slightly (2%) in the afternoon during polling hours.

**Japan (October 27)** Japan’s House of Representatives election saw Internet traffic decrease by 4% at 20:00 after polling stations closed, but it rose later in the evening.

**Botswana (October 30)** A traffic drop was observed throughout the day of Botswana’s general election, with a 42% decrease around 21:30 local time compared to the previous week.

**United States (November 5) -** **related blog post** The US elections saw a 15% spike in Internet traffic, particularly after polls closed, with the Midwest leading. There were also specific spikes related to key moments during election night, as the next chart shows: 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1pSMZDroAQWViUyhwvvLEs/ccb47a566c67069350ca19ef6b241ce6/BLOG-2648_17.png)

DNS traffic surged by 756% to polling services and 325% to news sites. As highlighted in our recent Internet Services Year in Review blog post, the US election also boosted DNS traffic and ranking positions for CNN, Fox News, and The New York Times, underscoring the Internet’s critical role during major political events.

In the US, beyond election day, we also reported in 2024 on trends surrounding the first Biden vs. Trump debate, the attempted assassination of Trump and the Republican National Convention, the Democratic National Convention, and the Harris-Trump presidential debate.

**Ghana (December 7)** Ghana’s general election caused mid-morning traffic drops of 11%, followed by declines of 13% and 14% after polling stations closed at 17:00. These patterns indicate offline focus during results announcements.

**Romania (December 1)** Romania’s parliamentary election showed minimal traffic fluctuations during the day, though its November 24 presidential election remains disputed.

## Email perspectives on the US presidential election

From a cybersecurity perspective, trending events, topics, and individuals often attract more emails, including malicious, phishing, and spam messages. In our analysis earlier this year, we focused on the US presidential elections and the two major party candidates.

From June 1 to November 5, 2024, Cloudflare processed over 19 million emails mentioning "Donald Trump" or "Kamala Harris," with Trump appearing more frequently and in higher rates of spam (12%) and malicious emails (1.3%) compared to Harris (0.6% spam, 0.2% malicious). Nearly half were sent after September, with a surge in the final 10 campaign days.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6uAzBIxewULmhPPjNaWLYu/4464d2e3953d3a527651d78252f8aa8b/BLOG-2648_18.png)

## Conclusion: the election cycle doesn’t stop

As a global election year, 2024 underscored how deeply the Internet is woven into the democratic process, serving both as a tool for engagement and a target for disruption. From relevant DDoS attacks to government-imposed Internet shutdowns, the challenges faced during these elections reflect a growing need for robust cybersecurity measures to safeguard critical infrastructure and ensure free, fair electoral processes.

In this context, Germany has announced an anticipated federal election for February 23, 2025, following the collapse of its governing coalition during the 2024 government crisis. This snap election joins others in France and the UK, reflecting a growing trend of political instability requiring urgent electoral responses.

Looking ahead, the increasing frequency and complexity of cyber threats, such as DDoS attacks on campaigns and election infrastructure, demand proactive defenses. Shutdowns like those in Pakistan and Comoros, along with surges in phishing and misinformation, highlight the need for closer collaboration between governments, technology providers, and civil society to safeguard democracy in the digital era.

If you want to follow more trends and insights about the Internet and elections in particular, you can check Cloudflare Radar, and more specifically our new 2024 Elections Insights report.

Go to Source
