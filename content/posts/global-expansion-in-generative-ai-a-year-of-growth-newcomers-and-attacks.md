---
title: "Global expansion in Generative AI: a year of growth, newcomers, and attacks"
date: 2025-03-19
---

AI (Artificial Intelligence) is a broad concept encompassing machines that simulate or duplicate human cognitive tasks, with Machine Learning (ML) serving as its data-driven engine. Both have existed for decades but gained fresh momentum when Generative AI, AI models that can create text, images, audio, code, and video, surged in popularity following the release of OpenAI’s ChatGPT in late 2022. In this blog post, we examine the most popular Generative AI services and how they evolved throughout 2024 and early 2025. We also try to answer questions like how much traffic growth these Generative AI websites have experienced from Cloudflare’s perspective, how much of that traffic was malicious, and other insights.

To accomplish this, we use aggregated data from our 1.1.1.1 DNS resolver to measure the popularity of specific Generative AI services. We typically do this for our Year in Review and now also on the DNS domain rankings page of Cloudflare Radar, where we aggregate related domains for each service and identify sites that provide services to users. For overall traffic growth and attack trends, we rely on aggregated data from the cohort of Generative AI customers that use Cloudflare for performance (including AI inference) and security.

Key takeaways:

- **ChatGPT maintains the top spot:** OpenAI’s ChatGPT remains #1 in Generative AI popularity, hovering around the top 50 Internet domains overall, up from #200 in late 2023.
    
- **Rapid traffic growth:** Monthly traffic to Generative AI services grew by 251% over the past year, between February 1, 2024, and March 1, 2025.
    
- **New entrants on the rise:** Chinese chatbot DeepSeek and Grok/xAI quickly climbed the ranks, illustrating how fast newcomers can gain traction in the AI space.
    
- **Global reach with regional variations:** The U.S. leads with 23% of Generative AI visitors, but Asia dominates certain platforms like poe.com. Brazil also shows up as a strong user of multiple AI services.
    
- **Targeted by cyberattacks:** Over 197 billion potential attack requests were blocked by Cloudflare in the past year, with 39 billion part of DDoS attack campaigns — particularly affecting general AI chatbots and image-generation sites.
    

### Generative AI services popularity ranking: new kids in town

We begin by looking at Generative AI service popularity using the new AI tab on Cloudflare Radar. The newest entrant to our Top 10 is DeepSeek, a Chinese chatbot launched on January 10, 2025. It debuted at #9 on January 26, 2025, climbed to #3 on January 29 (coinciding with Lunar/Chinese New Year), and maintained that position until February 4, before settling at its current position of #6. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/s9pGGWSp87CqvIOcbZ4gb/8a5a83deabdf8a0ed7f9898fc384c74d/image1.png)

Also highlighted here is another AI chatbot that has recently gained popularity — X’s Grok/xAI. This Generative AI service released its Android app in February and gained attention after February 17, 2025, when it launched the Grok-3 model. In our Generative AI ranking, it first entered the top 10 on February 21, 2025, at #9, briefly reached Claude’s typical spot at #8, and is now fluctuating between #9 and #10.

Here is the current Generative AI Top 10 from the Cloudflare Radar AI page, as of March 9, 2025, with ChatGPT/OpenAI as #1 since the start of the year (a trend also observed in previous years, as the table below shows).

To make ranking changes and trends easier to spot, the table below shows the February 1 - March 1, 2025 (monthly average) standings on the left, with color-coded comparisons to 2024’s list: services that dropped since 2024 appear in red, while new or higher-ranked ones appear in green. For reference, the second column presents the top 10 from our 2024 Year in Review (including comparisons to the previous year), and the third column displays the 2023 Top 10.

  
| **Top 10 Generative AI services**    **in February 2025**   ChatGPT / OpenAI (=)   Character.AI (=)   QuillBot (**#4 in 2024**)   Codeium (**#3**)   GitHub Copilot (**#7**)   DeepSeek (**new**)   Perplexity **(#6**)   Claude / Anthropic (**#5**)   Hugging Face (**new**)   Suno AI (**new**) | **Top 10 Generative AI services in 2024 (****Radar Year in Review****)**      ChatGPT / OpenAI (=)   Character.AI (=)   Codeium (**new**)   QuillBot (#3 in 2023)   Claude / Anthropic (**new**)   Perplexity (=)   GitHub Copilot (**new**)   Wordtune (**#7**)   Poe (**#5**)   Tabnine (**new**) | **Top 10 Generative AI services in 2023 (****Radar Year in Review****)**   ChatGPT / OpenAI   Character.AI   QuillBot   Hugging Face   Poe   Perplexity   Wordtune   Google Bard   ProWritingAid   Voicemod |
| --- | --- | --- |

Other than the previously mentioned DeepSeek, Grok/xAI and ChatGPT/OpenAI, the top 10 includes other chatbots like Anthropic’s Claude, as well as other types of Generative AI services. Character.AI — a specialized platform for creating and interacting with character-based personalities — is #2, then there’s Perplexity (#7) that functions as an AI search engine, while QuillBot (#3) is an AI-powered writing assistant for paraphrasing, grammar, and summarizing. Codeium (#4), which includes developer productivity services like Windsurf AI, and GitHub Copilot (#5) serve as AI coding assistants.

There’s also Hugging Face (#9), an open-source hub for AI models (we’re including it here as a Generative AI platform, just as we do for other AI model enablers like Replicate and Stability AI), and Suno AI (#10), a music generator that creates songs from text prompts.

We saw that Grok/xAI entered the top 10 during the last days of February, but since we’re using February’s monthly average, it appears at #11 here. Curious about the rest of the February 2025 Top 20? Here it is, with AI coding services having a strong presence — beyond Codeium and GitHub Copilot, Sider AI and Tabnine also make the list.

11 Grok / xAI

12 Poe

13 Sider AI

14 Civitai

15 Tabnine

16 Google Gemini

17 Voicemod

18 GliaCloud

19 Runway ml

20 Midjourney

We have published Generative AI popularity rankings in both the 2023 and 2024 Cloudflare Radar Year in Review, and in both, OpenAI’s ChatGPT has consistently held the #1 spot. In 2024, as explained in our blog post, ChatGPT also moved in our overall rankings, nearly breaking into the top 50 by the end of the year. (It was just outside the top 100 in 2023).

### ChatGPT's influence in the overall ranking 

A recent addition to Cloudflare Radar is the updated domains ranking page in our DNS section, which includes a number of detailed trends. There, we now show the top 100 overall Internet services ranking next to a top 100 domains list. ChatGPT / OpenAI, the leading Generative AI service, is typically ranked in the mid-50’s on weekdays and close to #60 on weekends (based on early March 2025 insights), next to non-AI services like Temu, eBay, or Disney Plus.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4miBGzkADr0y6XTYzyFpCR/cea822eb3c9b7c8c25d27858a72762ca/image5.png)

Looking at previous trends, as noted in our  Year in Review blog, ChatGPT / OpenAI ranked around #200 in early 2023 and climbed to near the top 100 by the end of the year. In 2024, it started just outside the top 100, reached the top 60 in May with the release of the 4o model, and has been near the top 50 since September 2024, aligning with the return of employees and students to their routines.

### Visitor location distribution: Americas, Europe and Asia

The Domain Information page on Cloudflare Radar enables users to look at the location popularity of a specific domain (from the last seven days), derived from Cloudflare 1.1.1.1 resolver traffic data in a period of 48 hours (Radar’s default) on March 3-4, 2025.

In this case, the chatgpt.com domain has most of its DNS traffic from the United States (17%), followed by Germany(7%), Brazil (4%), Indonesia (4%), and India (4%).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/MsCFiTsGRVF1N3c1oa87b/d5e02c68d7558b16e4fdb518ef5c776a/image2.png)

In the case of the new kid in town, deepseek.com, the U.S. is #1 location, with 14% of that domain’s DNS traffic, followed by China (11%), Germany (10%), Brazil (7%), and Hong Kong (5%).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/YKbSmz0ksNrbef2Xt7UE0/3fd694750f2cb413cd0e6e5001dfb248/image10.png)

Grok.com, on the other hand, has 20% of its traffic from the U.S., 8% from Hong Kong, 6% from Germany, 6% from Japan, and 6% from Vietnam, reflecting a strong presence in Asia within its top 5 locations. Asia is even more dominant for another well-known Generative AI chatbot domain, poe.com, with Hong Kong ranking #1 (29% of traffic), followed by the U.S. (13%), Japan (6%), China (6%), and Singapore (5%).

Hugging Face (huggingface.co), the Generative AI models platform, also has the U.S. as its top location (34% of traffic), but its top 5 includes four European countries: France (6%), the United Kingdom (6%), Germany (4%), and Sweden (4%).

Looking more specifically at AI-powered coding tools, DNS traffic for githubcopilot.com is primarily driven by the United States (22%), followed by Germany (6%), Hong Kong (5%), India (5%), and Japan (5%). A similar pattern appears for codeium.com, where the U.S. leads with 15%, followed by Hong Kong (8%), Japan (7%), Brazil (5%), and the Netherlands (5%). Likewise, cursor.com has 20% of its DNS traffic from the U.S., followed by Hong Kong (10%), India (6%), China (6%), and Japan (5%). Tabnine.com, another AI code completion tool, has its highest traffic from the U.S. (15%), followed by India (6%), Brazil (5%), Germany (5%), and Hong Kong (5%).

The DNS traffic data from Cloudflare Radar highlights strong U.S. usage across all major Generative AI and AI coding tools, with regional adoption varying by platform. (It is worth noting that 1.1.1.1 has a larger user base in the U.S., but these specific trends vary depending on the domains.)

- Asia dominates poe.com and AI coding tools like Codeium and Cursor.
    
- Europe plays a significant role in Hugging Face and GitHub Copilot.
    
- Brazil emerges as a notable player, particularly in DeepSeek and Tabnine.
    

### Generative AI general traffic growth 

Cloudflare, in terms of Generative AI customers, has a unique perspective on the industry. We power many Generative AI services, both large and small. From a cohort of Generative AI customers — some recently popular, others established chatbots or image AI generators, and some just starting — we’ve aggregated both HTTP request data over the past months and application-layer attack trends.

Let’s start with HTTP requests traffic growth in the past year. From February 1, 2024, through March 1, 2025 (a 13-month period to compare February 2024 with February 2025), **monthly traffic grew a total of 251%**, and over 2% of the requests processed by Cloudflare were mitigated as potential attacks.

Note that there was an increase over most of the entities in the cohort of Generative AI websites, and this 251% growth also includes recent Generative AI customers, although those mostly don’t influence the growth trend that much — if we exclude Generative AI customers that onboarded to Cloudflare in late 2024 and early 2025, year growth is 234%.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4323TeVxE3VqSEoFjopa8t/640305a2b4b3583218115f7a9c35cd60/image11.png)

In this next perspective, shown at a daily level, the expected drop during Christmas and the end of the year holidays is quite clear. Another trend surfaces: the cohort of Cloudflare’s Generative AI customers definitely see more use during weekdays than weekends, suggesting a workplace focus. The clear drop during the holidays also includes the summer in the Northern Hemisphere — there's a slight drop in peak traffic in July, for example (similar to what we typically see in terms of general traffic in most countries).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1NvzlVxeom6omVtgFjwauB/a3088b03dc870581c9b6b1f86a810f6d/image12.png)

We also have a perspective on the top visitor locations to Generative AI websites, where the U.S. ranks #1, with 23% of all requests in this category, followed by India (8%), Brazil (5%), Indonesia (4%), and Philippines (4%) in the top 5. European countries, such as the U.K. and Germany, come next in the ranking. Below, we show the top 50 for further exploration. Note that Egypt is the first African country appearing in the ranking, at #32, with the same 0.7% as South Africa.

**Top locations by share of traffic to Generative AI websites**

|   **Rank**   |   **Country**   |   **Percentage of total**   |   **Rank**   |   **Country**   |   **Percentage of total**   |
| --- | --- | --- | --- | --- | --- |
|   1   |   United States   |   22.7%   |   26   |   Singapore   |   1.1%   |
|   2   |   India   |   8.3%   |   27   |   Ukraine   |   1%   |
|   3   |   Brazil   |   4.9%   |   28   |   Taiwan   |   0.9%   |
|   4   |   Indonesia   |   4.2%   |   29   |   Thailand   |   0.9%   |
|   5   |   Philippines   |   4%   |   30   |   Chile   |   0.8%   |
|   6   |   United Kingdom   |   3.8%   |   31   |   United Arab Emirates   |   0.7%   |
|   7   |   Germany   |   3.7%   |   32   |   Egypt   |   0.7%   |
|   8   |   Canada   |   3.2%   |   33   |   Saudi Arabia   |   0.7%   |
|   9   |   France   |   3%   |   34   |   South Africa   |   0.7%   |
|   10   |   Mexico   |   2.7%   |   35   |   Sweden   |   0.6%   |
|   11   |   Japan   |   2.4%   |   36   |   Belgium   |   0.6%   |
|   12   |   Russian Federation   |   2.2%   |   37   |   Bangladesh   |   0.6%   |
|   13   |   Spain   |   2%   |   38   |   Switzerland   |   0.6%   |
|   14   |   Australia   |   2%   |   39   |   Morocco   |   0.6%   |
|   15   |   South Korea   |   1.8%   |   40   |   Ecuador   |   0.6%   |
|   16   |   Vietnam   |   1.6%   |   41   |   Israel   |   0.5%   |
|   17   |   Italy   |   1.5%   |   42   |   Nigeria   |   0.5%   |
|   18   |   Malaysia   |   1.5%   |   43   |   Romania   |   0.5%   |
|   19   |   Turkey   |   1.4%   |   44   |   Portugal   |   0.5%   |
|   20   |   Poland   |   1.4%   |   45   |   Kazakhstan   |   0.5%   |
|   21   |   Netherlands   |   1.4%   |   46   |   Austria   |   0.4%   |
|   22   |   Argentina   |   1.2%   |   47   |   Czech Republic   |   0.4%   |
|   23   |   Colombia   |   1.2%   |   48   |   Hong Kong   |   0.4%   |
|   24   |   Pakistan   |   1.2%   |   49   |   Algeria   |   0.4%   |
|   25   |   Peru   |   1.1%   |   50   |   Denmark   |   0.4%   |

### Attacks targeting Generative AI websites

On the security front, Generative AI websites have become key targets for DDoS attacks as they have gained attention and grown in popularity. Recently, our Cloudforce One team published a threat analysis on attacks by Anonymous Sudan targeting AI-related companies: Inside LameDuck: Analyzing Anonymous Sudan’s Threat Operations. In this report, they explained how the U.S. Department of Justice indicted two Sudanese brothers behind LameDuck, linking them to 35,000+ DDoS attacks via the Skynet Botnet. The case exposes both political and financial motives behind their operations and underscores the global effort — including Cloudflare’s — to strengthen cybersecurity.

Over the last 13 months, from February 1, 2024, until March 1, 2025, Cloudflare blocked **197 billion requests** as potential attacks. Of that number, **39 billion** requests were part of **DDoS attacks** targeting Generative AI websites.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4R9GfQsKixZ3CcGB4GPazz/2e43fa46125d22a3058e2099a1c8c38e/image6.png)

In terms of malicious requests that were blocked, June 2024 saw the highest number of potential attacks blocked by Cloudflare, followed by January 2025. For DDoS attacks, January 2025 recorded the highest activity, followed by November 2024 and February 2024.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7KtuwFvwQWbjHL464kDbUG/45bc21c813d9a0ab0fb621da4eb4367d/image14.png)

Looking more closely at DDoS traffic at a daily level, the largest attack occurred on February 23, 2024, when 3.7 billion requests were blocked as part of a DDoS attack. The second largest was a 1.5 billion request DDoS attack on November 13, 2024. Additionally, a series of multiday DDoS attacks took place between January 20 and 31, 2025, with January 29 seeing the highest number of DDoS attack-related requests, at over one billion (7.3 billion in total for the month).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2DpI9bSIUziw2mVCA6WwUh/06d00c656656af711cba830f6380ae98/image3.png)

During the February 23, 2024, DDoS attack, which targeted a specific Generative AI customer, more than 20% of all requests across all Generative AI customers were blocked as part of the attack.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1gh467o7mEZBvm0wu3s9eI/1e2a21f47e3645de4257549937bc5ba0/image8.png)

Taking a more granular view of DDoS attacks against that particular Generative AI customer, the attack began on February 22, 2024, at 22:45 UTC, lasting for over eight hours of continuous traffic spikes, peaking at 270,000 requests per second. Further attacks followed, with the most significant occurring on February 26, 2024, at 03:45 UTC, lasting three minutes and peaking at 309,000 requests per second.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5ccxcb4WCqyoZOpZvJkv8/1748a4044e7de327aff9f289d51e4fdc/image9.png)

Another popular Generative AI customer was targeted in a DDoS campaign from January 25 to January 31, 2025, with traffic peaking on January 30, reaching 523,000 requests per second.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/ZBMBJXBlyk7kHc85TthBG/ba0309a070b3d920b9ba2f9651fb1343/image7.png)

Another perspective to consider over the same February 2024 to February 2025 period is the type of Generative AI websites most targeted by DDoS attacks. General AI chatbots accounted for over 80% of all blocked requests, making them the primary targets.

**DDoS attacks targets by Generative AI category**

|   **Category**   |   **Percentage**   |
| --- | --- |
|   General Chatbots   |   82.7%   |
|   Image AI Generators   |   8.2%   |
|   Code Assistants   |   3.4%   |
|   Other   |   2.6%   |
|   AI Research & Infra   |   1.3%   |
|   AI Music Creation   |   1.2%   |
|   Writing & Content AI   |   0.4%   |
|   Voice & Video AI   |   0.3%   |

However, when looking at the percentage of total traffic blocked as DDoS attacks within each category, image AI-related websites had the highest proportion, with over 50% of their total traffic being blocked.

**Websites category with the highest percentage of traffic blocked as DDoS attacks** 

|   **Category**   |   **Blocked DDoS (%)**   |
| --- | --- |
|   Image AI   |   50.8%   |
|   AI Chatbot   |   31%   |
|   AI Search   |   9.4%   |
|   AI Code Assistant   |   6.8%   |
|   AI Model   |   5.8%   |
|   AI Music   |   3.6%   |
|   AI Company   |   2.9%   |

### Conclusion: AI transformation

Generative AI continues to grow and transform Internet usage, driving traffic growth of over 250% for AI services over the course of the last year. ChatGPT is definitely the most popular service, and nears the top 50 of all Internet services as seen through analysis of traffic from our 1.1.1.1 DNS resolver. New entrants like DeepSeek and Grok/xAI have quickly climbed the popularity rankings, while regional adoption patterns show the U.S., India, and Brazil leading in visitor traffic.

This rapid rise has also drawn cyberattacks, with 39 billion requests identified as DDoS attacks targeting specific Generative AI websites over the past year. While most attacks focus on general AI chatbots, image-generation sites show the highest percentage of blocked requests, at over 50%. As Generative AI evolves, tracking these trends provides a historical record of growth surges, global reach, and emerging threats.

If you’re interested in more trends and insights about the Internet, check out Cloudflare Radar. Follow us on social media at @CloudflareRadar (X), noc.social/@cloudflareradar (Mastodon), and radar.cloudflare.com (Bluesky), or contact us via email.

Go to Source
