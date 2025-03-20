---
title: "Record-breaking 5.6 Tbps DDoS attack and global DDoS trends for 2024 Q4"
date: 2025-01-22
---

Welcome to the 20th edition of the Cloudflare DDoS Threat Report, marking five years since our first report in 2020.

Published quarterly, this report offers a comprehensive analysis of the evolving threat landscape of Distributed Denial of Service (DDoS) attacks based on data from the Cloudflare network. In this edition, we focus on the fourth quarter of 2024 and look back at the year as a whole.

## Cloudflare’s unique vantage point

When we published our first report, Cloudflare’s global network capacity was 35 Terabits per second (Tbps). Since then, our network’s capacity has grown by 817% to 321 Tbps. We also significantly expanded our global presence by 65% from 200 cities in the beginning of 2020 to 330 cities by the end of 2024.

Using this massive network, we now serve and protect nearly 20% of all websites and close to 18,000 unique Cloudflare customer IP networks. This extensive infrastructure and customer base uniquely positions us to provide key insights and trends that benefit the wider Internet community.

## Key DDoS insights

- In 2024, Cloudflare’s autonomous DDoS defense systems blocked around 21.3 million DDoS attacks, representing a 53% increase compared to 2023. On average, in 2024, Cloudflare blocked 4,870 DDoS attacks every hour.
    
- In the fourth quarter, over 420 of those attacks were hyper-volumetric, exceeding rates of 1 billion packets per second (pps) and 1 Tbps. Moreover, the amount of attacks exceeding 1 Tbps grew by a staggering 1,885% quarter-over-quarter.
    
- During the week of Halloween 2024, Cloudflare’s DDoS defense systems successfully and autonomously detected and blocked a 5.6 Terabit per second (Tbps) DDoS attack — the largest attack ever reported.
    

_To learn more about DDoS attacks and other types of cyber threats, visit our_ _Learning Center__, access_ _previous DDoS threat reports_ _on the Cloudflare blog, or visit our interactive hub,_ _Cloudflare Radar__. There's also a_ _free API_ _for those interested in investigating these and other Internet trends. You can also learn more about the_ _methodologies_ _used in preparing these reports._

## Anatomy of a DDoS attack

In 2024 Q4 alone, Cloudflare mitigated 6.9 million DDoS attacks. This represents a 16% increase quarter-over-quarter (QoQ) and 83% year-over-year (YoY).

Of the 2024 Q4 DDoS attacks, 49% (3.4 million) were Layer 3/Layer 4 DDoS attacks and 51% (3.5 million) were HTTP DDoS attacks.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/33qc2yEBIE4Tmq6ke3dOIY/398216db2fb03e6093f55dac35394568/image13.png)

_Distribution of 6.9 million DDoS attacks: 2024 Q4_

## HTTP DDoS attacks

The majority of the HTTP DDoS attacks (73%) were launched by known botnets. Rapid detection and blocking of these attacks were made possible as a result of operating a massive network and seeing many types of attacks and botnets. In turn, this allows our security engineers and researchers to craft heuristics to increase mitigation efficacy against these attacks.

An additional 11% were HTTP DDoS attacks that were caught pretending to be a legitimate browser. Another 10% were attacks which contained suspicious or unusual HTTP attributes. The remaining 8% “Other” were generic HTTP floods, volumetric cache busting attacks, and volumetric attacks targeting login endpoints.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/27nsCB9HReu48XtiJKufwg/cb8814d1cc390e4cd1ffea9316fd589e/image19.png)

_Top HTTP DDoS attack vectors: 2024 Q4_

These attack vectors, or attack groups, are not necessarily exclusive. For example, known botnets also impersonate browsers and have suspicious HTTP attributes, but this breakdown is our attempt to categorize the HTTP DDoS attacks in a meaningful way.

### Top user agents

As of this report’s publication, the current stable version of Chrome for Windows, Mac, iOS, and Android is 132, according to Google’s release notes. However, it seems that threat actors are still behind, as thirteen of the top user agents that appeared most frequently in DDoS attacks were Chrome versions ranging from 118 to 129.

The HITV\_ST\_PLATFORM user agent had the highest share of DDoS requests out of total requests (99.9%), making it the user agent that’s used almost exclusively in DDoS attacks. In other words, if you see traffic coming from the HITV\_ST\_PLATFORM user agent, there is a 0.1% chance that it is legitimate traffic.

Threat actors often avoid using uncommon user agents, favoring more common ones like Chrome to blend in with regular traffic. The presence of the HITV\_ST\_PLATFORM user agent, which is associated with smart TVs and set-top boxes, suggests that the devices involved in certain cyberattacks are compromised smart TVs or set-top boxes. This observation highlights the importance of securing all Internet-connected devices, including smart TVs and set-top boxes, to prevent them from being exploited in cyberattacks.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5uUCjjPdGu63u7OmgRE6Yw/4b15c1e88cfe86ae0bc5824346908b24/image18.png)

_Top user agents abused in DDoS attacks: 2024 Q4_

The user agent hackney came in second place, with 93% of requests containing this user agent being part of a DDoS attack. If you encounter traffic coming from the hackney user agent, there is a 7% chance that it is legitimate traffic. Hackney is an HTTP client library for Erlang, used for making HTTP requests and is popular in Erlang/Elixir ecosystems.

Additional user agents that were used in DDoS attacks are uTorrent, which is associated with a popular BitTorrent client for downloading files. Go-http-client and fasthttp were also commonly used in DDoS attacks. The former is the default HTTP client in Go’s standard library and the latter is a high-performance alternative. fasthttp is used to build fast web applications, but is often exploited for DDoS attacks and web scraping too.

## HTTP attributes commonly used in DDoS attacks

### HTTP methods

HTTP methods (also called HTTP verbs) define the action to be performed on a resource on a server. They are part of the HTTP protocol and allow communication between clients (such as browsers) and servers.

The GET method is most commonly used. Almost 70% of legitimate HTTP requests made use of the GET method. In second place is the POST method with a share of 27%.

With DDoS attacks, we see a different picture. Almost 14% of HTTP requests using the HEAD method were part of a DDoS attack, despite it hardly being present in legitimate HTTP requests (0.75% of all requests). The DELETE method came in second place, with around 7% of its usage being for DDoS purposes.

The disproportion between methods commonly seen in DDoS attacks versus their presence in legitimate traffic definitely stands out. Security administrators can use this information to optimize their security posture based on these headers.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1fD5aUHaIkRMUNPZJI0LKW/d5856e7ce13cb7d1e28727401b885b1a/image10.png)

_Distribution of HTTP methods in DDoS attacks and legitimate traffic: 2024 Q4_

### HTTP paths

An HTTP path describes a specific server resource. Along with the HTTP method, the server will perform the action on the resource.

For example, GET https://developers.cloudflare.com/ddos-protection/ will instruct the server to retrieve the content for the resource /ddos-protection/.

DDoS attacks often target the root of the website (“/”), but in other cases, they can target specific paths. In 2024 Q4, 98% of HTTP requests towards the /wp-admin/ path were part of DDoS attacks. The /wp-admin/ path is the default administrator dashboard for WordPress websites.

Obviously, many paths are unique to the specific website, but in the graph below, we’ve provided the top _generic_ paths that were attacked the most. Security administrators can use this data to strengthen their protection on these endpoints, as applicable. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/I9SweJVs4sLYjgHy469NN/b7d0e76648b0ec26af32143a45dc1dd6/image21.png)

 _Top HTTP paths targeted by HTTP DDoS attacks: 2024 Q4_

## HTTP vs. HTTPS

In Q4, almost 94% of legitimate traffic was HTTPS. Only 6% was plaintext HTTP (not encrypted). Looking at DDoS attack traffic, around 92% of HTTP DDoS attack requests were over HTTPS and almost 8% were over plaintext HTTP.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1grfbkXvzjh8nXJYtrhiJP/8ff46ac59d296fcad89475f2bc242184/unnamed__2_.png)

_HTTP vs. HTTPS in legitimate traffic and DDoS attacks: 2024 Q4_

## Layer 3/Layer 4 DDoS attacks

The top three most common Layer 3/Layer 4 (network layer) attack vectors were SYN flood (38%), DNS flood attacks (16%), and UDP floods (14%).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1hXTXtKe2kVD9fjw26aIN8/7bbd5ef01b04a3bba28232cdcf876c3a/image1.png)

_Top L3/4 DDoS attack vectors: 2024 Q4_

An additional common attack vector, or rather, botnet type, is Mirai. Mirai attacks accounted for 6% of all network layer DDoS attacks — a 131% increase QoQ. In 2024 Q4, a Mirai-variant botnet was responsible for the largest DDoS attack on record, but we’ll discuss that further in the next section.

## Emerging attack vectors

Before moving on to the next section, it’s worthwhile to discuss the growth in additional attack vectors that were observed this quarter. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7Hz074MxtzzdG4uvCM8P93/af6c86b023160f66acf0fe209386acf7/image8.png)

_Top emerging threats: 2024 Q4_

Memcached DDoS attacks saw the largest growth, with a 314% QoQ increase. Memcached is a database caching system for speeding up websites and networks. Memcached servers that support UDP can be abused to launch amplification or reflection DDoS attacks. In this case, the attacker would request content from the caching system and spoof the victim's IP address as the source IP in the UDP packets. The victim will be flooded with the Memcache responses, which can be up to 51,200x larger than the initial request.

BitTorrent DDoS attacks also surged this quarter by 304%. The BitTorrent protocol is a communication protocol used for peer-to-peer file sharing. To help the BitTorrent clients find and download the files efficiently, BitTorrent clients may utilize BitTorrent Trackers or Distributed Hash Tables (DHT) to identify the peers that are seeding the desired file. This concept can be abused to launch DDoS attacks. A malicious actor can spoof the victim’s IP address as a seeder IP address within Trackers and DHT systems. Then clients would request the files from those IP addresses. Given a sufficient number of clients requesting the file, it can flood the victim with more traffic than it can handle.

## The largest DDoS attack on record

On October 29, a 5.6 Tbps UDP DDoS attack launched by a Mirai-variant botnet targeted a Cloudflare Magic Transit customer, an Internet service provider (ISP) from Eastern Asia. The attack lasted only 80 seconds and originated from over 13,000 IoT devices. Detection and mitigation were fully autonomous by Cloudflare’s distributed defense systems. It required no human intervention, didn’t trigger any alerts, and didn’t cause any performance degradation. The systems worked as intended.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/kx3Uj4y4G4KZ6yNritxg4/d47e8f1b51a630bce28e8b036a4e7b64/image16.png)

_Cloudflare’s autonomous DDoS defenses mitigate a 5.6 Tbps Mirai DDoS attack without human intervention_

While the total number of unique source IP addresses was around 13,000, the average unique source IP addresses per second was 5,500. We also saw a similar number of unique source ports per second. In the graph below, each line represents one of the 13,000 different source IP addresses, and as portrayed, each contributed less than 8 Gbps per second. The average contribution of each IP address per second was around 1 Gbps (~0.012% of 5.6 Tbps).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2biclYyny81QnJxQpP3PcF/8e1ec9c4b227043b1bd05914c1f543b1/image14.png)

_The 13,000 source IP addresses that launched the 5.6 Tbps DDoS attack_

## Hyper-volumetric DDoS attacks

In 2024 Q3, we started seeing a rise in hyper-volumetric network layer DDoS attacks. In 2024 Q4, the amount of attacks exceeding 1 Tbps increased by 1,885% QoQ and attacks exceeding 100 Million pps (packets per second) increased by 175% QoQ. 16% of the attacks that exceeded 100 Million pps also exceeded 1 Billion pps.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3L3X48ztfIeGRVe3Su009z/b6798328b8926b33ea78b0617ee3aad5/image6.png)

_Distribution of hyper-volumetric L3/4 DDoS attacks: 2024 Q4_

## Attack size

The majority of HTTP DDoS attacks (63%) did not exceed 50,000 requests per second. On the other side of the spectrum, 3% of HTTP DDoS attacks exceeded 100 million requests per second.

Similarly, the majority of network layer DDoS attacks are also small. 93% did not exceed 500 Mbps and 87% did not exceed 50,000 packets per second. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/25TQ7mayQOrr3ZpG1yLADa/ce08756eec2fbb2b213aad1668d59b4f/unnamed.png)

_QoQ change in attack size by packet rate: 2024 Q4_

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1eNqV8gIxZgukwropBeyvs/23f128993b6573a3acb6e2a33306813d/unnamed__1_.png)

_QoQ change in attack size by bit rate: 2024 Q4_

## Attack duration

The majority of HTTP DDoS attacks (72%) end in under ten minutes. Approximately 22% of HTTP DDoS attacks last over one hour, and 11% last over 24 hours.

Similarly, 91% of network layer DDoS attacks also end within ten minutes. Only 2% last over an hour.

Overall, there was a significant QoQ decrease in the duration of DDoS attacks. Because the duration of most attacks is so short, it is not feasible, in most cases, for a human to respond to an alert, analyze the traffic, and apply mitigation. The short duration of attacks emphasizes the need for an in-line, always-on, automated DDoS protection service.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6Yfb7JGpZ2GTXR2HYK5pAS/55a1dbf4eec229e7154cf223d542e3bf/unnamed__3_.png)

_QoQ change in attack duration: 2024 Q4_

## Attack sources

In the last quarter of 2024, Indonesia remained the largest source of DDoS attacks worldwide for the second consecutive quarter. To understand where attacks are coming from, we map the source IP addresses launching HTTP DDoS attacks because they cannot be spoofed, and for Layer 3/Layer 4 DDoS attacks, we use the location of our data centers where the DDoS packets were ingested. This lets us overcome the spoofability that is possible in Layer 3/Layer 4. We’re able to achieve geographical accuracy due to our extensive network spanning over 330 cities around the world.

Hong Kong came in second, having moved up five spots from the previous quarter. Singapore advanced three spots, coming in third place.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4Z7DgqDBlKbd3eDRv7ZVmL/49aabaee6301a3c93bb40851e645dd42/image2.png)

_Top 10 largest sources of DDoS attacks: 2024 Q4_

### Top source networks

An autonomous system (AS) is a large network or group of networks that has a unified routing policy. Every computer or device that connects to the Internet is connected to an AS. To find out what your AS is, visit https://radar.cloudflare.com/ip.

When looking at where the DDoS attacks originate from, specifically HTTP DDoS attacks, there are a few autonomous systems that stand out.

The AS that we saw the most HTTP DDoS attack traffic from in 2024 Q4 was German-based Hetzner (AS24940). Almost 5% of all HTTP DDoS requests originated from Hetzer’s network, or in other words, 5 out of every 100 HTTP DDoS requests that Cloudflare blocked originated from Hetzner.

In second place we have the US-based Digital Ocean (AS14061), followed by France-based OVH (AS16276) in third place.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7pQUunzZ0ioH48lTOJOLVe/8dc42b7904b0f0b838f117ce5f35a35a/image12.png)

_Top 10 largest source networks of DDoS attacks: 2024 Q4_

For many network operators such as the ones listed above, it can be hard to identify the malicious actors that abuse their infrastructure for launching attacks. To help network operators and service providers crack down on the abuse, we provide a **free** DDoS Botnet threat intelligence feed that provides ASN owners a list of their IP addresses that we’ve seen participating in DDoS attacks. 

## Top threat actors

When surveying Cloudflare customers that were targeted by DDoS attacks, the majority said they didn’t know who attacked them. The ones that did know reported their competitors as the number one threat actor behind the attacks (40%). Another 17% reported that a state-level or state-sponsored threat actor was behind the attack, and a similar percentage reported that a disgruntled user or customer was behind the attack.

Another 14% reported that an extortionist was behind the attacks. 7% claimed it was a self-inflicted DDoS, 2% reported hacktivism as the cause of the attack, and another 2% reported that the attacks were launched by former employees.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7gThccj4k75gfFoGBn301W/403bd5cf3984611490e7d90f3435f3c1/image15.png)

_Top threat actors: 2024 Q4_

## Ransom DDoS attacks

In the final quarter of 2024, as anticipated, we observed a surge in Ransom DDoS attacks. This spike was predictable, given that Q4 is a prime time for cybercriminals, with increased online shopping, travel arrangements, and holiday activities. Disrupting these services during peak times can significantly impact organizations' revenues and cause real-world disruptions, such as flight delays and cancellations.

In Q4, 12% of Cloudflare customers that were targeted by DDoS attacks reported being threatened or extorted for a ransom payment. This represents a 78% QoQ increase and 25% YoY growth compared to 2023 Q4.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1BV3NoLbxwzO0ShVyCwQ97/7ccb684195b6efef0db209aefffff476/image17.png)

_Reported Ransom DDoS attacks by quarter: 2024_

Looking back at the entire year of 2024, Cloudflare received the most reports of Ransom DDoS attacks in May. In Q4, we can see the gradual increase starting from October (10%), November (13%), and December (14%) — a seven-month-high.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/EllNHd6iUWkQ6Z481gLss/a20b10f96d4f7a649dfa23beceebad8e/image9.png)

_Reported Ransom DDoS attacks by month: 2024_

## Target of attacks

In 2024 Q4, China maintained its position as the most attacked country. To understand which countries are subject to more attacks, we group DDoS attacks by our customers’ billing country. 

Philippines makes its first appearance as the second most attacked country in the top 10. Taiwan jumped to third place, up seven spots compared to last quarter.

In the map below, you can see the top 10 most attacked locations and their ranking change compared to the previous quarter.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4TosbZ02NmNGbgpwkskUNs/6f96885b4de89c34403551a03a01e634/image5.png)

_Top 10 most attacked locations by DDoS attacks: 2024 Q4_

## Most attacked industries

In the fourth quarter of 2024, the _Telecommunications, Service Providers and Carriers_ industry jumped from the third place (last quarter) to the first place as the most attacked industry. To understand which industries are subject to more attacks, we group DDoS attacks by our customers’ industry. The _Internet_ industry came in second, followed by _Marketing and Advertising_ in third.

The _Banking & Financial Services_ industry dropped seven places from number one in 2024 Q3 to number eight in Q4.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/444JREdNrmb6yePqqfGI4B/a268a1d3d3cd1dd7d9e076ffcf5b06c5/image7.png)

_Top 10 most attacked industries by DDoS attacks: 2024 Q4_

## Our commitment to unmetered DDoS protection

The fourth quarter of 2024 saw a surge in hyper-volumetric Layer 3/Layer 4 DDoS attacks, with the largest one breaking our previous record, peaking at 5.6 Tbps. This rise in attack size renders capacity-limited cloud DDoS protection services or on-premise DDoS appliances obsolete.

The growing use of powerful botnets, driven by geopolitical factors, has broadened the range of vulnerable targets. A rise in Ransom DDoS attacks is also a growing concern.

Too many organizations only implement DDoS protection after suffering an attack. Our observations show that organizations with proactive security strategies are more resilient. At Cloudflare, we invest in automated defenses and a comprehensive security portfolio to provide proactive protection against both current and emerging threats.

With our 321 Tbps network spanning 330 cities globally, we remain committed to providing unmetered and unlimited DDoS protection no matter the size, duration and quantity of the attacks.

Go to Source
