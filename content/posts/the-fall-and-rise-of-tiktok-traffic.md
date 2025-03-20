---
title: "The fall and rise of TikTok (traffic)"
date: 2025-01-22
---

The United States ban on TikTok went into effect on January 19, 2025, and although service began to be restored after just 14 hours, it was only close to the inauguration of Donald Trump as the 47th President of the United States that associated DNS traffic started to recover to closer to previous levels. In this post, we analyze the events of January 19 and 20, and what they meant for TikTok-related DNS traffic, but also other competitors (including their growth outside the US).

For context, we wrote an initial blog post about the TikTok ban on Sunday, January 19, 2025. The ban was part of the "Protecting Americans from Foreign Adversary Controlled Applications Act," proposed in Congress, which ordered ByteDance to divest due to alleged security concerns. The bill was signed into law by Congress and President Biden in April 2024, and was upheld by the Supreme Court on January 17, 2025.

Aggregated data from our 1.1.1.1 DNS resolver shows — as we’ve posted on social media — that the TikTok shutdown in the US began to impact DNS traffic to TikTok-related domains on January 19, just after 03:30 UTC (22:30 ET on January 18). This includes DNS traffic not only for TikTok, but also for other ByteDance-owned platforms, such as the CapCut video editor. Here’s the timeline focused on DNS traffic for TikTok related domains (with the respective line chart), as we’ve observed it:

- **January 19, just after 03:30 UTC (22:30 ET on January 18):** DNS traffic to TikTok-related domains dropped by as much as 85% compared to the previous week, and showed signs of further decline in the following hours.
    
- **January 19, 17:30 UTC (12:30 ET):** After a 14-hour shutdown, TikTok announced it was starting service restoration following assurances from Donald Trump. DNS traffic began to recover slightly after 18:00 UTC but stayed near "shutdown" levels for several hours. Traffic from AS396986 (ByteDance) showed a similar trend.
    
- **January 20, 06:00 UTC (01:00 ET):** A short-lived spike in DNS traffic for TikTok-related domains occurred, with traffic still 25% below the previous week.
    
- **January 20, 14:00–15:00 UTC (09:00–10:00 ET):** DNS traffic picked up, moving from 27% to 18% below pre-shutdown levels.
    
- **January 20, 17:00 UTC (12:00 ET):** During Donald Trump’s inauguration ceremony, DNS traffic increased to 12% below pre-shutdown levels, with a trend of continued growth, reaching 10% below previous levels at 18:00 UTC (13:00 ET).
    
- **January 21, 05:00 UTC (00:00 ET):** DNS traffic was 7% below pre-shutdown levels.
    

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5P0HXhOaJtBClOnZyAifNx/d66b69eb511bf0670d9136a0da4f74cc/image6.png)

On January 19, around 17:30 UTC (12:30 ET), TikTok released a statement: “In agreement with our service providers, TikTok is in the process of restoring service. We thank President Trump for providing the necessary clarity and assurance to our service providers that they will face no penalties.” A message indicating the TikTok ban was over appeared for US users (image on the left). However, a few hours later, some users reported difficulties accessing the app (image on the right).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/3nI5tzedgXiQOpj9UJYAN5/5a2e1b0ad470807dc475f8f23e65744a/Screenshot_2025-01-21_at_13.49.55.png)

Analyzing data from autonomous system\-level data, traffic from TikTok owner ByteDance’s network (AS396986) in the US to Cloudflare experienced a sharp decline, dropping by as much as 95% after 03:30 UTC on January 19 (22:30 ET on January 18).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4fgbhrtt4ROVLvshup0lG2/f27c0cd29e6320deac0c7dcfac5877b8/image4.png)

Our data shows that traffic within ByteDance’s network (AS396986) never fully recovered, remaining around 80% below pre-shutdown levels. This suggests that ByteDance may have used other solutions after the shutdown.

## Alternatives like RedNote (Xiaohongshu) 

As mentioned previously, DNS traffic in the US for TikTok alternatives, driven by RedNote (Xiaohongshu or Little Red Book), has been steadily increasing since January 13. It surged on January 19 by up to 74% around 04:00 UTC (23:00 ET on January 18) compared to the previous week, with lower growth seen later that day in the US (around 52% at 17:00 UTC (12:00 ET)). Traffic subsequently declined, and was only 17% higher than the previous week after TikTok announced it was beginning to restore its services in the US around 22:00 UTC (17:00 ET), and it lost even more growth momentum after that.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4nXqvN8EE0kvvnO2h1t5c6/c1394571b7ac3f833d48ecc25b0ec7b8/image2.png)

Daily DNS traffic in the US for TikTok alternatives has been rising since January 13, reaching 116% higher than the previous week on January 15. On Sunday, January 19, the day of the TikTok ban, it peaked with a 291% increase compared to the previous week.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4OxdAjwocA13LmKIWcZME6/6acd7370c818dd6fa133015998fdd03d/image12.png)

## RedNote impacting other countries

DNS traffic for TikTok alternatives, driven by RedNote, has also been increasing in other countries, with a noticeable rise in daily DNS traffic to these platforms. Below is the breakdown of the most impacted countries, with a few updates from our most recent blog post. We highlight the peak day of DNS traffic and the percentage growth compared to the previous week.

- Mexico (+1200% on January 19)
    
- Brazil (+185% on January 20)
    
- France (+165% on January 19)
    
- Germany (+142% on January 19)
    
- Canada (+119% on January 19)
    
- Spain (+106% on January 19)
    
- Portugal (+97% on January 19)
    
- The UK (+86% on January 19)
    
- Australia (+19% on January 15)
    
- Japan (+18% on January 18)
    

(Note: In many cases, DNS traffic had been growing for more than a week, so countries with recent growth may show higher percentages.)

Those trends are consistent with apps like RedNote rising on top of the Android and iOS App Stores, according to Data.ai.

The rapid increases in DNS traffic can be clearly seen in the graphs below:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/619Z2yoZlVuATOgN3DHBXX/6d3266656dfcea4cce601a2a4b6306bb/image10.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/71fJCQSX0K7aG072m6muva/3c96f32a6be1595cbeb9d03249baac86/image11.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6RBoRKMmKva2E1pHEeQytf/6e98cd7586370e34af8b73814a2718e4/image8.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4aFxiYyzMD8w1JugtBLVvx/e824cb4c4f2d4beb8fcdcfe01ed3393a/image7.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/28NKyTWZ1L0YWObIBlfiSv/f0d2e14ec39a90e20751f58ef4b953b9/image13.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7u3ekAZgdlOMkmyvpAvxZx/6c5b8fdb1fe590cc67498cf41d7907fb/image5.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6vFqDPx17F2LjgUDA922px/dabce8ce69fae17779241aa69b47db4c/image1.png)![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4BalkDrhzcYMfgikD3qNSB/6afae6e46ffc3feff41fbb129c4b5408/image15.png)

If you’re interested in more trends and insights about the Internet, check out Cloudflare Radar. Follow us on social media at @CloudflareRadar (X), noc.social/@cloudflareradar (Mastodon), and radar.cloudflare.com (Bluesky).

Go to Source
