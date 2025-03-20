---
title: "When the world logs off: Christmas, New Year’s, and the Internet’s holiday rhythm"
date: 2025-01-02
categories: 
  - "general"
---

As January approaches and the year comes to a close, distinct changes in global Internet usage emerge. Year-end traditions — ranging from Christmas feasts to New Year’s Eve (NYE) countdowns — shape online behavior across continents and cultures. Looking back at Christmas and NYE 2023 offers insights into how these trends may repeat this year, and by January 2025, we’ll be able to directly compare patterns. Examining data from 50 countries and regions reveals how people celebrated in 2023-2024, providing a timely reminder of typical holiday trends.

With Cloudflare’s global reach, we observe planet-wide and local Internet habits during the holiday season. In the past, unintended trends during Christmas and New Year’s Eve have surfaced through our Outage Center, which uses automatic traffic anomaly alerts to detect Internet outages or unusual patterns. In the 2023 overview below, traffic dropped enough on those days to trigger dozens of anomaly alerts (orange and pink bubbles):

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6pPt1Ccp2AVqdhCqhOsEEk/424f5cddd931ae86a5a2e21dbabb2eda/image6.png)

While Christmas dominates in many regions, other cultural and religious holidays — like Hanukkah or regional festivities — shape online rhythms in places where Western traditions are less central.

In countries and regions where Christmas is deeply rooted, Internet traffic slows during Christmas Eve dinners, midnight masses, morning gift exchanges, and Christmas Day lunches.

This blog post focuses exclusively on non-bot-related Internet traffic requests, filtering out automated activity to provide a clearer view of genuine human behavior during the holiday season. Before going into location-specific perspectives, here’s a global hourly view of Internet traffic during Christmas and New Year’s Eve 2023 from Cloudflare Radar Data Explorer, highlighting notable drops (in UTC, so it captures impacts across more days due to time zones spanning over 23 hours, from New Zealand to Hawaii in the US):

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2K08u0Q3BgqtwNAQEZCinh/d1eead7068ce053173fa2aca9fb031c9/image5.png)

## Global Christmas and New Year’s Eve daily trends

Let’s start with a ranking of countries and regions and their top low-traffic holiday dates, showing each day’s percentage drop. Many locations like the US see clear dips on December 24 and 25 as people celebrate Christmas Eve and Christmas Day offline, and some show smaller declines (compared to Christmas) around December 31 as the New Year approaches. Still, the exact order and magnitude differ, reflecting cultural nuances — some nations experience greater drops on Christmas Eve, others on Christmas Day, and others signal unique patterns tied to New Year’s Eve or January 1 celebrations.

In the next table, locations are listed first (in the left column) by those with the lowest traffic on December 24 (and the highest percentage of traffic drop), followed by December 25, and finally December 31 (in the right column).

**Top days with the lowest Internet traffic in December 2023 - January 2024**

(with respective percentage drops, if any, from the previous week)

|   **Denmark**  #1 December 24 (-35%)  #2 December 25 (-11%)  #3 December 31   |   **South Africa**  #1 December 25 (-27%)  #2 December 24 (-15%)  #3 December 31 (-5%)   |
| --- | --- |
|   **Norway**  #1 December 24 (-32%)  #2 December 25 (-12%)  #3 December 31   |   **United Kingdom**  #1 December 25 (-26%)  #2 December 24 (-19%)  #3 December 31   |
|   **Portugal**  #1 December 24 (-32%)  #2 December 25 (-24%)  #3 December 31   |   **Italy**  #1 December 25 (-25%)  #2 December 24 (-25%)  #3 December 31   |
|   **Poland**  #1 December 24 (-31%)  #2 December 25 (-21%)  #3 December 31   |   **Australia**  #1 December 25 (-25%)  #2 December 24 (-15%)  #3 December 31 (-1%)   |
|   **Spain**  #1 December 24 (-28%)  #2 December 25 (-25%)  #3 December 31   |   **Ireland**  #1 December 25 (-24%)  #2 December 24 (-22%)  #3 December 23   |
|   **Sweden**  #1 December 24 (-26%)  #2 December 25 (-6%)  #3 December 31   |   **New Zealand**  #1 December 25 (-22%)  #2 December 24 (-8%)  #3 December 31 (-4%)   |
|   **Chile**  #1 December 24 (-23%)  #2 December 25 (-24%)  #3 December 31 (-3%)   |   **Canada**  #1 December 25 (-19%)  #2 December 24 (-15%)  #3 December 31   |
|   **Finland**  #1 December 24 (-23%)  #2 December 25 (-16%)  #3 December 31   |   **Nigeria**  #1 December 25 (-18%)  #2 December 24 (-19%)  #3 January 1   |
|   **France**  #1 December 24 (-22%)  #2 December 25 (-19%)  #3 December 23   |   **Philippines**  #1 December 25 (-16%)  #2 December 24 (-7%)  #3 December 31   |
|   **Germany**  #1 December 24 (-21%)  #2 December 25 (-9%)  #3 December 31   |   **Hong Kong**  #1 December 25 (-9%)  #2 December 24 (-6%)  #3 December 23   |
|   **Mexico**  #1 December 24 (-21%)  #2 December 25 (-19%)  #3 December 31   |   **Belgium**  #1 December 31 (-1%)  #2 December 24 (-20%)  #3 December 25 (-17%)   |
|   **Belgium**  #1 December 24 (-20%)  #2 December 25 (-17%)  #3 December 31 (-1%)   |   **Indonesia**  #1 December 31 (-1%)  #2 December 25 (-7%)  #3 December 24   |
|   **Romania**  #1 December 24 (-20%)  #2 December 25 (-14%)  #3 December 31 (-3%)   |   **Netherlands**  #1 December 31 (-10%)  #2 December 24 (-10%)  #3 December 25 (-20%)   |
|   **United States**  #1 December 24 (-16%)  #2 December 25 (-21%)  #3 December 31   |   **Ukraine**  #1 December 31 (-10%)  #2 December 24 (-5%)  #3 December 30   |
|   **Brazil**  #1 December 24 (-14%)  #2 December 25 (-26%)  #3 December 31   |   **Thailand**  #1 December 31 (-6%)  #2 January 1 (-2%)  #3 December 25 (-2%)    |
|   **Colombia**  #1 December 24 (-14%)  #2 December 25 (-26%)  #3 December 31 (-4%)   |    |

The data shows that in many European countries — such as Denmark, Norway, the United Kingdom, Portugal, Italy, Poland, Spain, Ireland, Sweden, Finland, France, Germany, Belgium, the Netherlands, and Romania — Christmas Eve (December24) and Christmas Day (December25) consistently register the biggest drops in Internet traffic. These dips suggest that in much of Europe, Christmas traditions take people firmly offline, whether it’s for family gatherings, festive meals, or religious observances. Outside Europe, similar patterns appear in predominantly Christian-influenced regions, including Australia, New Zealand, Canada, the United States, and several Latin American countries (like Brazil, Chile, and Colombia), confirming that the holiday’s cultural importance is mirrored in their online habits.

In contrast, locations less influenced by Western Christmas traditions, such as those in Asia, show subtler or different patterns. For example, Hong Kong and the Philippines do show declines in traffic, reflecting a hybrid of local and global influences, while places like Thailand and Indonesia present smaller dips on Christmas compared to other days or emphasize different holidays altogether. These variations highlight that while Christmas exerts a strong pull offline in many parts of the world, its impact on Internet usage is shaped by local cultural contexts.

As an example, here’s the US perspective from Cloudflare Radar Data Explorer, where the drop in traffic during Christmas and New Year 2023 is evident:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2Z2ODYM1L5vlBdZHhEdq4W/84b2144c586b01db3e3e99bb3bd05bc6/image3.png)

## Where Christmas isn’t central

Not every country’s December revolves around Christmas. In Israel, for example, Hanukkah’s timing changes year to year, influencing when people log off. In 2023, Hanukkah started on December 7, leading to an 8% traffic drop that day and 7% on the following days through December 10. Interestingly, in some years like 2024, Hanukkah begins closer to December 25, potentially overlapping with Western Christmas.

**Countries where Christmas didn’t have a clear impact**

|   **Turkey**  #1 December 31 (-18%)  #2 December 29  #3 December 30   |   **Israel**  #1 December 29  #2 January 5  #3 December 30   |
| --- | --- |
|   **Japan**  #1 December 31 (-8%)  #2 January 1  #3 December 30 — December 24 with -3%   |   **Vietnam**  #1 January 1 (-7%)  #2 December 31 (-3%)  #3 January 2   |
|   **Russia**  #1 December31 (-23%)  #2 January 1 (-15%)  #3 December 30   |   **Singapore**  #1 December 16  #2 December 17  #3 December 18   |
|   **India**  #1 December 17  #2 December 16  #3 December 24   |   **Bangladesh**  #1 December 15  #2 December 16  #3 December 18   |
|   **Saudi Arabia**  #1 January 5  #2 January 6  #3 January 8   |   **China**  #1 December 19  #2 December 15  #3 December 18   |

Now, let’s focus on a more granular perspective of these trends, showing the impact of Christmas dinners and lunches, and also New Year’s Eve drops in traffic.

_Note: Unless otherwise noted, all times used in this blog post are local ones; in countries with several timezones, we’re using the timezone where more people live (for the US, Eastern time is used)._

## A more granular perspective of Christmas: offline feasts and morning quiet

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/AnOIfryr3c8zSAc23SRKD/f3ee2dd0ed6f8d4f54a951663e2916e7/image4.png)

### Europe

In Europe, Christmas traditions dominate, leading to the most significant Internet traffic drops. Christmas Eve dinner is a near-universal offline moment, with countries like Spain (-70% at 21:45), Portugal (-70% at 20:30), and Denmark (-68% at 19:45) experiencing the steepest declines. On Christmas Day, mornings are quieter as people relax or attend religious services, while festive lunches drive further drops, with traffic down 43% at 13:45 in Portugal and 44% at 07:15 in France.

By Boxing Day (December 26), digital activity rebounds as people return online for sales or socializing. For instance, the UK shows a 16% increase at 13:00, while Canada records a 19% rise at 08:15. In Australia, traffic climbs by 20% at 09:30, illustrating regional differences in how the day is celebrated.

### Americas

In the Americas, holiday patterns reflect a mix of cultural traditions. In the United States, Christmas Eve traffic drops by 29% at 20:15, aligning with evening family gatherings, and Christmas Day sees a 32% decline at 09:15, reflecting quieter mornings.

In Latin America, Christmas Eve (Nochebuena) takes center stage, with significant traffic declines aligning with late-night traditions like the Midnight Toast (in Argentina, the late-night feast is quite popular) and Misa de Gallo (Midnight Mass). For example:

- Colombia: -48% at 21:45
    
- Argentina: -58% at 22:00; -67% at midnight
    
- Chile: -64% at 22:45
    
- Mexico: -50% at 21:45
    
- Brazil: -22% at 21:45
    

These late-night traffic dips highlight the region’s emphasis on midnight celebrations, family feasts, and religious observances.

### Asia Pacific

Asian locations influenced by Western traditions, such as the Philippines and Hong Kong, experience moderate Christmas dips but shift focus to New Year’s celebrations — more on NYE below.

In the Southern Hemisphere, Australia and New Zealand experience their steepest traffic drops during Christmas lunch, with Australia seeing a 43% decrease at 13:45 and New Zealand recording a 42% decline.

### Middle East and Africa

In regions less influenced by Christmas, holiday traffic patterns vary significantly. For example, Nigeria sees a 26% drop at lunchtime on Christmas Day, while South Africa records a 37% decline at 14:15, reflecting offline family gatherings.

In predominantly non-Christian countries like Egypt and Saudi Arabia, December 24-25 does not show significant dips; instead, other cultural holidays drive offline moments. For example, as we’ve noted, Israel experienced up to an 8% drop in 2023 during Hanukkah, particularly in the first four days after December 7. In previous blog posts, we have shown how events like Ramadan clearly impact Internet traffic in countries with large Muslim populations. One example from our Year in Review 2024 highlights Indonesia and the United Arab Emirates, where traffic dropped during Eid al-Fitr, the festival marking the end of Ramadan (April 9-10, 2024).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2NCDJ1EQZaBbqAn5KKeN7s/1fe50400f25c144f24292bfbf4fe821e/image2.png)

### The Boxing Day revival

Boxing Day on December 26 marks a significant digital rebound in countries like the UK, Canada, Australia (where there is a higher increase from the previous week, with daily traffic growing 9%), and New Zealand, as people return online after the Christmas break. Traditionally associated with charitable activities, family gatherings, and shopping, the day sees traffic spikes across these regions:

|   **Location**   |   **December 26 increase in daily traffic**   |   **Higher traffic increase on December 26**   |
| --- | --- | --- |
|   Australia   |   +9%   |   December 26; 09:30: +20%   |
|   United Kingdom   |   +2%   |   December 26; 13:00: +16%   |
|   Canada   |   +1%   |   December 26, 08:15: +19%   |

Here is the list of locations that saw a clear drop in traffic on Christmas Eve or Christmas Day morning or lunch. We selected the time (morning or lunch) with the bigger drop compared to the previous week for further analysis. The list is ordered by the Christmas Eve dinner drop. Countries like Russia (where Orthodox Christians celebrate Christmas later, on January 7), Japan, China, Indonesia, Turkey, Israel, Thailand, Egypt, Singapore, Vietnam, and Bangladesh showed no impact during Christmas Eve dinner or Christmas Day morning or lunch.

|   **Location**   |   **Christmas Eve Dinner Drop**   |   **Christmas Day Morning/Lunch Drop**   |
| --- | --- | --- |
|   Spain   |   \-70% at 21:45   |   \-51% at 08:00 (morning)   |
|   Portugal   |   \-70% at 20:30   |   \-43% at 13:45 (lunch)   |
|   Denmark   |   \-68% at 19:45   |   \-43% at 06:15 (morning)   |
|   Chile   |   \-64% at 22:45; (-65% at 00:00, December 25)   |   \-49% at 09:00 (morning)    |
|   Norway   |   \-63% at 18:45   |   \-50% at 06:45 (morning)   |
|   Czech Republic   |   \-60% at 18:15   |   \-43% at 06:30 (morning)   |
|   Poland   |   \-59% at 17:15   |   \-51% at 07:15 (morning)   |
|   Argentina   |   \-58% at 22:00 (-67% at 00:00, December 25)   |   \-52% at 09:00 (morning)    |
|   Italy   |   \-55% at 21:15   |   \-44% at 07:00 (morning)   |
|   France   |   \-55% at 20:45   |   \-44% at 07:15 (morning)   |
|   Mexico   |   \-50% at 21:45   |   \-38% at 08:15 (morning)   |
|   Belgium   |   \-50% at 20:15   |   \-46% at 07:15 (morning)   |
|   Switzerland   |   \-50% at 19:45   |   \-46% at 06:30 (morning)   |
|   Austria   |   \-50% at 19:15   |   \-42% at 06:15 (morning)   |
|   Nigeria   |   \-49% at 18:00   |   \-26% at 12:30 (lunch)   |
|   Colombia   |   \-48% at 21:45   |   \-49% at 08:00 (morning)   |
|   Germany   |   \-47% at 19:15   |   \-36% at 07:15 (morning)   |
|   Sweden   |   \-47% at 16:30   |   \-36% at 07:00 (morning)   |
|   Finland   |   \-42% at 17:45   |   \-42% at 08:00 (morning)   |
|   Ireland   |   \-40% at 18:15   |   \-36% at 15:15 (lunch)   |
|   South Africa   |   \-37% at 19:00   |   \-37% at 14:15 (lunch)   |
|   Romania   |   \-34% at 20:45   |   \-34% at 06:30 (morning)   |
|   United Kingdom   |   \-34% at 18:00   |   \-38% at 14:45 (lunch)   |
|   Canada   |   \-32% at 20:30   |   \-31% at 09:30 (morning)   |
|   Netherlands   |   \-30% at 20:45   |   \-35% at 06:45 (morning)   |
|   United States   |   \-29% at 20:15   |   \-32% at 09:15 (morning)   |
|   Australia   |   \-23% at 20:45   |   \-43% at 13:45 (lunch)   |
|   New Zealand   |   \-23% at 18:30   |   \-42% at 13:15 (lunch)   |
|   Brazil   |   \-22% at 21:45   |   \-42% at 08:00 (morning)   |
|   Philippines   |   \-22% at 21:30   |   \-29% at 06:45 (morning)   |

## New Year’s Eve: A planetary offline moment

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6fK4TEIFANRI6YxNUH6toS/e2ea778748511440f8a4f2103519c651/image1.png)

Midnight, December 31 is a shared offline moment worldwide, as people step away from their screens to celebrate. To provide a more accurate assessment of New Year’s Eve’s impact, we compare traffic at 00:00 on January 1 with 00:00 on December 18, avoiding distortions caused by Christmas-related patterns. This approach highlights the distinct drop in Internet activity due to New Year’s celebrations.

Across Europe, countries like Portugal (-60%) and Romania (-60%) see dramatic traffic drops, reflecting widespread offline gatherings. Spain (-56%) and Germany (-49%) also experience steep declines, emphasizing the importance of this tradition across the region. Even Northern Europe mirrors this trend, with Denmark (-41%), Norway (-39%), and Sweden (-29%) showing significant dips.

In the Americas, this offline moment is particularly pronounced in Latin America, where family and communal gatherings dominate. Argentina (-66%) and Chile (-74%) lead the region, with Brazil (-46%) and Colombia (-44%) following closely. In North America, the impact is less dramatic due to time zone variations — in this case, with millions of people spread out in distinct time zones. Canada records a 14% drop, and the United States shows a modest 12% decline compared to December 18.

In Asia and the Pacific, New Year’s Eve celebrations heavily influence Internet trends. Thailand saw a 31% drop, Indonesia 23%, and Japan 16%, also reflecting this region’s focus on communal gatherings and celebrations. Australia (-21%) and New Zealand (-11%), among the first countries to welcome the New Year, also show noticeable declines as midnight festivities take center stage.

In the Middle East and Africa, Turkey (-23%), South Africa (-32%), and Nigeria (-15%) exhibit significant offline engagement at midnight. Israel records a smaller but notable 6% dip before midnight, reflecting localized variations in celebration styles.

Of course, this offline intermission doesn’t last long. After a few hours, people return to their devices. France sees a 37% surge at 3:15 on January 1, while Turkey experiences a 36% upswing in the early hours.

Next, we present the list of locations with clear drops in traffic at midnight on New Year’s Eve, compared to December 18, ordered by percentage of drop. 

|   **Locations**   |   **January 1, 00:00 drop compared to December 18**   |   **Locations**   |   **January 1, 00:00 drop compared to December 18**   |
| --- | --- | --- | --- |
|   Chile   |   \-74%   |   Thailand   |   \-31%   |
|   Argentina   |   \-66%   |   Italy   |   \-30%   |
|   Romania   |   \-60%   |   Sweden   |   \-29%   |
|   Portugal   |   \-60%   |   Vietnam   |   \-27%   |
|   Spain   |   \-56%   |   United Kingdom   |   \-25%   |
|   Germany   |   \-49%   |   Ukraine   |   \-25%   |
|   Brazil   |   \-46%   |   Indonesia   |   \-23%   |
|   Mexico   |   \-44%   |   Turkey   |   \-23%   |
|   Colombia   |   \-44%   |   Australia   |   \-21%   |
|   Philippines   |   \-43%   |   Hong Kong   |   \-21%   |
|   Netherlands   |   \-42%   |   Ireland   |   \-19%   |
|   Poland   |   \-41%   |   France   |   \-17%   |
|   Denmark   |   \-41%   |   Japan   |   \-16%   |
|   Austria   |   \-40%   |   South Korea   |   \-16%   |
|   Switzerland   |   \-39%   |   Nigeria   |   \-15%   |
|   Norway   |   \-39%   |   Canada   |   \-14%   |
|   Czech Republic   |   \-33%   |   Finland   |   \-14%   |
|   Russia   |   \-32%   |   Singapore   |   \-13%   |
|   Belgium   |   \-32%   |   United States   |   \-12%   |
|   South Africa   |   \-32%   |   China   |   \-12%   |

## Conclusion: A mosaic of traditions and digital habits

What emerges from these patterns is a rich tapestry of cultural habits. While Christmas Eve and Day are central offline moments in Europe and the Americas, other regions mark their quiet days on different dates, shaped by unique holidays and customs. The insights from 50 countries and regions confirm how cultural traditions guide when people step away from screens.

As the Gregorian calendar year comes to a close, the universal appeal of stepping offline becomes clear. Whether raising glasses at the stroke of midnight, exchanging greetings, or lighting candles for festivals like Hanukkah, these moments remind us that while the Internet connects billions, cultural rhythms still shape our relationship with technology. Whether feasting with loved ones or counting down to a new year, humans everywhere find reasons to unplug — if only for a moment.

If you’re interested in more trends and insights about the Internet, check out Cloudflare Radar. Follow us on social media at @CloudflareRadar (X), https://noc.social/@cloudflareradar (Mastodon), and radar.cloudflare.com (Bluesky), or contact us via email.

Go to Source
