---
title: "No hallucinations here: track the latest AI trends with expanded insights on Cloudflare Radar"
date: 2025-02-06
---

During 2024’s Birthday Week, we launched an AI bot & crawler traffic graph on Cloudflare Radar that provides visibility into which bots and crawlers are the most aggressive and have the highest volume of requests, which crawl on a regular basis, and more. Today, we are launching a new dedicated “AI Insights” page on Cloudflare Radar that incorporates this graph and builds on it with additional metrics that you can use to understand AI-related trends from multiple perspectives. In addition to the traffic trends, the new section includes a view into the relative popularity of publicly available Generative AI services based on 1.1.1.1 DNS resolver traffic, the usage of robots.txt directives to restrict AI bot access to content, and open source model usage as seen by Cloudflare Workers AI.

Below, we’ll review each section of the new AI Insights page in more detail.

### AI bots and crawlers traffic trends

Tracking traffic trends for AI bots can help us better understand their activity over time. Initially launched in September 2024 on Radar’s Traffic page, the **AI bot & crawler traffic** graph has moved to the AI Insights page and provides visibility into traffic trends gathered globally over the selected time period for the top five most active AI bots & crawlers. The associated list of user agents tracked here is based on the ai.robots.txt list, and will be updated with new entries as they are identified. The time series and summary data for this graph is available from the Radar API, and traffic trends for the full set of AI bots & crawlers we see traffic from can be viewed in the Data Explorer.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6EicYZIfSdeRMBCIID5Fbr/0213b9501e22033ac5315bbef48c5a7a/image3.png)

### Popularity of Generative AI services

Over the last several years, the Cloudflare Radar Year in Review has analyzed request traffic data from our 1.1.1.1 DNS resolver to present rankings of the most popular Internet services, both generally and across several categories. In both 2023 and 2024, this section included rankings for publicly-available Generative AI services, with ChatGPT topping the list both years. While an accompanying blog post provides a more detailed look at how the rankings shifted over the course of the year, it too is looking through the rearview mirror. That is, it doesn’t provide visibility into the changes as they are occurring. The new **Generative AI services popularity** graph shows the relative rankings of these services and platforms based on DNS request traffic for domains associated with these services aggregated at a daily level. The underlying time series data is available through the Radar API, using the `serviceCategory=Generative%20AI` parameter.

The graph below shows that as of the end of January 2025, the top five services were fairly stable over the preceding four weeks, but there was regular movement among those ranked #6-10. We expect that the rankings will continue to change over time. DeepSeek, a Generative AI service that took the industry by storm at the end of January, can be seen making its initial appearance at #9 on January 26, rising rapidly to #3 on January 29, just three days later. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/fzh8oz8ZhybkKlJXBE0qq/4f695ddd38dd676c3b418d5ceac939fb/image5.png)

### Analysis of robots.txt files

Content providers can attempt to control access to their full site, or specific portions of it, through the use of Allow or Disallow directives in a robots.txt file. However, successful access control is dependent on the bots respecting the listed directives. Cloudflare's AI Audit gives you visibility and control into how AI bots are interacting with your website, and now Cloudflare Radar gives you insights into how other sites are handling them.

On a weekly basis, we analyze Radar’s top 10,000 domains to determine which associated sites publish robots.txt files, as well as aggregating the AI-specific directives within those files. In our new **AI user agents found in robots.txt** graph, seen below, we are now providing insights into actions that these top sites are taking with respect to AI bots. These actions are specified by directives that allow or disallow access by a given user agent (bot identifier) for either all content on the site (Fully Allowed/Disallowed) or certain sections (Partially Allowed/Disallowed).

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/16U4GdEyxsUlzqjKd4Y1jH/25535296e710ae31aa8658b4c338296e/image6.png)

In addition, we have also organized these domains by category (for example, Ecommerce or News & Media), highlighting the specific bots that the sites within those categories have listed in their directives. For example, the News & Media domain category graph shown below illustrates that these types of sites almost universally fully disallow access to their sites by AI user agents.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7i1a23p2FfasbJvrS65S7l/0f476352a9573f9822b5ca9d351795d7/image4.png)

Changing the directive to “Allow” shows a much smaller set of user agents, with a drastically smaller set of sites explicitly allowing full or partial access. (Note that if a user agent is not listed in a robots.txt file, and a wildcard “\*” user agent is not specified, then access is fully allowed by default.)

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5I7OgW10PrX8wKVtRRWmnQ/193d5be5f5211b32c29b8c4601ee38ba/image2.png)

In addition to appearing on the AI Insights page, the underlying data is available for further exploration and analysis through the Radar API and the Data Explorer. 

### Popularity of models and tasks on Workers AI

The AI model landscape is rapidly evolving, with providers regularly releasing more powerful models, capable of tasks like text and image generation, speech recognition, and image classification. Cloudflare works closely with AI model providers to ensure that Workers AI supports these models as soon as possible following their release. On the new AI Insights page, Radar now provides visibility into the popularity of publicly available supported models (**Workers AI model popularity**) as well as the types of tasks (**Workers AI task popularity**) that these models perform, based on customer account share. Extended insights, including share trends and summary shares for the full list of models and tasks, as well as the ability to compare model and task shares across time periods, are available through the Data Explorer. The underlying model popularity and task popularity data is also available through API endpoints.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5c7YE87EdMsoN4bYELM4Rw/556abd2ebb70cbc7839fa98c653e816d/image7.png)

### Conclusion

The AI space is extremely dynamic, with new platforms, services, and models regularly appearing. In some cases, these new entrants even have the power to upset the market as they see rapid growth in interest and usage. And over two years since ChatGPT was announced, there continues to be tension between content providers and AI platforms about scraping content for model training. The new “AI Insights” page on Cloudflare Radar provides timely trends and information about this dynamic space, enabling industry observers and participants to better understand how it is changing and evolving over time.

If you share AI Insights graphs on social media, be sure to tag us: @CloudflareRadar (X), noc.social/@cloudflareradar (Mastodon), and radar.cloudflare.com (Bluesky). You can also reach out on social media, or contact us via email, with suggestions for AI metrics that we can explore adding to the page in the future.

Go to Source
