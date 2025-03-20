---
title: "Resilient Internet connectivity in Europe mitigates impact from multiple cable cuts"
date: 2025-01-02
categories: 
  - "general"
---

When cable cuts occur, whether submarine or terrestrial, they often result in observable disruptions to Internet connectivity, knocking a network, city, or country offline. This is especially true when there is insufficient resilience or alternative paths — that is, when a cable is effectively a single point of failure. Associated observations of traffic loss resulting from these disruptions are frequently covered by Cloudflare Radar in social media and blog posts. However, two recent cable cuts that occurred in the Baltic Sea resulted in little-to-no observable impact to the affected countries, as we discuss below, in large part because of the significant redundancy and resilience of Internet infrastructure in Europe.

## BCS East-West Interlink

### Traffic volume indicators

On Sunday, November 17 2024, the BCS East-West Interlink submarine cable connecting Sventoji, Lithuania and Katthammarsvik, Sweden was reportedly damaged around 10:00 local (Lithuania) time (08:00 UTC). A Data Center Dynamics article about the cable cut quotes the CTO of Telia Lietuva, the telecommunications provider that operates the cable, and notes “_The Lithuanian cable carried about a third of the nation's Internet capacity, but capacity was carried via other routes._”

As the Cloudflare Radar graphs below show, there was no apparent impact to traffic volumes in either country at the time that the cables were damaged. The NetFlows graphs represent the number of bytes that Cloudflare sends to users and clients in response to their requests.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7xDllSeyPtet5ovpXI3GMH/6bc5680bbd8219f417e891102c4ffb0e/BLOG-2626_2.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2RE0V8M2CFPt1uxsOjhSBz/dc5c261808c021fc9ff0ab65963fce0b/BLOG-2626_3.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6wjHy79iHDqcuzknxcK14o/a4526787c3fdde54a6627b16717aaec0/BLOG-2626_4.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2zPi0GZ54gGDjCMyivhH0C/f6e50728ae7dc110fd15edc40f43b694/BLOG-2626_5.png)

### Internet quality

Internet quality metrics for both countries show changes in measured bandwidth and latency throughout the day on Sunday, but with no sudden anomalous shifts visible around the time of the cable cut. (The loss of connectivity associated with a cable cut potentially manifests itself as an increase in latency and concurrent decrease in bandwidth due to loss of capacity.) The latency graph for Sweden does show an increase in latency, but it began before the cable cut occurred, is similar to a pattern visible several hours earlier, and is matched by an increase in measured bandwidth, so it is unlikely to be related to the cable cut event.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3aXlBjU08WKKT0OSnWBsIP/eb32b937d1729160dec83204bba06e91/BLOG-2626_6.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5eeR8OlwA5CHROGkqKn1KJ/e372a25ad2a93aaa38339f360f3a7b0e/BLOG-2626_7.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6wGJFP5K0LkEjw8DXbQ45z/516cf3b04ac5c5f2f82398be508fe4b0/BLOG-2626_8.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2ZLyXG9lCsfazoMm1b6c4z/1af296913ff00da991c04d1422bd49fd/BLOG-2626_9.png)

### Visibility in BGP events, announced IP address space unaffected

BGP announcements are a way for network providers to communicate routing information to other networks, and announcement activity observed on Telia Lietuva’s autonomous systems around the time of the cable cut may be related to the re-routing referenced in the article. No change in announced IP address space was visible for any of these autonomous systems, suggesting no loss of connectivity as the capacity was re-routed.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7dWHQYn0cJ3PdivPgI2sDI/696207021bf5e75d061040c33505923a/BLOG-2626_10.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5QPU28IyaW3QPCqaIzTZec/19b3ed7675d23441c9493c2313134a41/BLOG-2626_11.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4zSx3b14HwaBIFX5qc59bq/4f8e2b4951498a2edcae846068927350/BLOG-2626_12.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1P04AQZbfZOTisBPutbLZa/5e6520bfd1782976538c98914134fe94/BLOG-2626_13.png)

Telegeography’s submarinecablemap.com illustrates, at least in part, the resilience in connectivity enjoyed by these two countries. In addition to the damaged cable, it shows that Lithuania is connected to neighboring Latvia as well as to the Swedish mainland. Over 20 submarine cables land in Sweden, connecting it to multiple countries across Europe. In addition to the submarine resilience, network providers in both countries can take advantage of terrestrial fiber connections to neighboring countries, such as those illustrated in a European network map from Arelion (formerly Telia), which is only one of the large European backbone providers.

## C-Lion1

### Traffic volume indicators

Less than a day later, the C-Lion1 submarine cable, which connects Helsinki, Finland and Rostock Germany was reportedly damaged during the early morning hours of Monday, November 18. Cinia, the telecommunications company that owns the cable, said that the cable stopped working at about 02:00 UTC. 

In this situation as well, as the Cloudflare Radar graphs below show, there was no apparent impact to traffic volumes in either country at the time that the cables were damaged. The Finland graphs, week-on-week, show fewer bytes transferred and fewer HTTP requests, but that difference is present before the cable cut at 02:00 UTC. However, the trend of the current line does not change after the cable cut, so the two events would appear unrelated. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4OQtSFBgBzdmnzWz8AdM7Z/3a66cec98698bf6d506d93fc13fe4c74/BLOG-2626_14.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4gqxHsD3ykjWGWhATVU8iw/f74916e1faf186efef94e6dc29bbca58/BLOG-2626_15.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4gh5eLQZNabsz9XnOSgtU1/fb6d0770c62ce016d73c1a3c47ae99f1/BLOG-2626_16.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6FIKh8U2nxHkxMdXho6HsI/9e349d0767df5d34d3b8274710c2cb0b/BLOG-2626_17.png)

### Internet quality

By looking at volume-related metrics, alone, Internet connectivity would appear to be unaffected by the cable cut.

If, however, we change perspective and look at Internet quality, a brief yet interesting change is visible for Finland around the reported time of the cable damage, though it isn’t clear whether it is related in any way. Just after midnight, median measured bandwidth, previously consistent around 50 Mbps begins to grow, peaking just over 200 Mbps around 03:00 UTC. Around that same time, measured median latency also begins to drop, falling from around 30 ms to a low of 13 ms, also around 03:00 UTC. Median bandwidth returned to normal levels around 06:00 UTC, while latency took about two hours longer to return to normal levels.  These observed  improvements in bandwidth and latency could have been due to traffic being re-routed to along paths with better connectivity to measurement endpoints, but because the shifts began before the cable damage occurred, and recovered shortly thereafter, that is unlikely to be the root cause.

In Germany, a brief minor increase in median bandwidth peaked around 02:45 UTC, while no notable changes were observed in latency.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/94V0coi6oFBdUMX1SVyl7/44738b06af2e51b4e436c84dbe6a1a79/BLOG-2626_18.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/Bqy5uQ76FwmmOX92Co4cE/96190329454e264966119a0f9a4533ff/BLOG-2626_19.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1BJCVdjJMILFubi4SW8HR6/7b97343910ab70cc1a4cad3d3565a727/BLOG-2626_20.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3lf5GR9ElhjpW0wYzPieNI/c02a588af54ac36521f901307d9f62f7/BLOG-2626_21.png)

### BGP business as usual

From a routing perspective, there was no notable BGP announcement activity observed for top autonomous systems in either Finland or Germany around 02:00 on November 18, and total announced IP address space aggregated at a country level also demonstrated no change.

Telegeography’s submarinecablemap.com shows that both Finland and Germany also have significant redundancy and resilience from a submarine cable perspective, with over 10 cables landing in Finland, and nearly 10 landing in Germany, including Atlantic Crossing-1 (AC-1), which connects to the United States over two distinct paths. Terrestrial fiber maps from Arelion and eunetworks (as just two examples) show multiple redundant fiber routes within both countries, as well as cross-border routes to other neighboring countries, enabling more resilient Internet connectivity.

## Conclusion

As we have discussed in multiple prior blog posts (Jersey, 2016; AAE-1/SMW5, 2022; WACS/MainOne/SAT3/ACE, 2024; EASSy/Seacom, 2024), cable cuts often cause significant disruptions to Internet connectivity, in many cases because they represent a concentrated point of vulnerability, whether for an individual network provider, city/state, or country. These disruptions are often quite lengthy as well, due to the time needed to marshal repair resources, identify the location of the damage, etc. Although it is not always feasible due to financial or geographic constraints, building redundant and resilient network architecture, at multiple levels, is a best practice. This includes the sending traffic over multiple physical cables (both submarine and terrestrial), connecting to multiple peer and upstream network providers, and even avoiding single points of failure in core Internet resources like DNS servers.

The Cloudflare Radar team continually monitors the status of Internet connectivity in countries/regions around the world, and we share our observations on the Cloudflare Radar Outage Center, via social media, and in posts on blog.cloudflare.com. Follow us on social media at @CloudflareRadar (X), https://noc.social/@cloudflareradar (Mastodon), and radar.cloudflare.com (Bluesky), or contact us via email.

Go to Source
