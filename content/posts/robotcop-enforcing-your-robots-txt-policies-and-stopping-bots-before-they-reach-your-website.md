---
title: "Robotcop: enforcing your robots.txt policies and stopping bots before they reach your website"
date: 2025-01-02
categories: 
  - "general"
---

Cloudflare’s AI Audit dashboard allows you to easily understand how AI companies and services access your content. AI Audit gives a summary of request counts broken out by bot, detailed path summaries for more granular insights, and the ability to filter by categories like **AI Search** or **AI Crawler**.

Today, we're going one step further. You can now quickly see which AI services are honoring your robots.txt policies, which aren’t, and then programmatically enforce these policies. 

### What is robots.txt?

Robots.txt is a plain text file hosted on your domain that implements the Robots Exclusion Protocol, a standard that has been around since 1994. This file tells crawlers like Google, Bing, and many others which parts of your site, if any, they are allowed to access. 

There are many reasons why site owners would want to define which portions of their websites crawlers are allowed to access: they might not want certain content available on search engines or social networks, they might trust one platform more than another, or they might simply want to reduce automated traffic to their servers.

With the advent of generative AI, AI services have started crawling the Internet to collect training data for their models. These models are often proprietary and commercial and are used to generate new content. Many content creators and publishers that want to exercise control over how their content is used have started using robots.txt to declare policies that cover these AI bots, in addition to the traditional search engines.

Here’s an abbreviated real-world example of the robots.txt policy from a top online news site:

```
User-agent: GPTBot
Disallow: /

User-agent: ChatGPT-User
Disallow: /

User-agent: anthropic-ai
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: Bytespider
Disallow: /
```

This policy declares that the news site doesn't want ChatGPT, Anthropic AI, Google Gemini, or ByteDance’s Bytespider to crawl any of their content.

### From voluntary compliance to enforcement

Compliance with the Robots Exclusion Protocol has historically been voluntary. 

That’s where our new feature comes in. We’ve extended AI Audit to give our customers both the visibility into how AI services providers honor their robots.txt policies _and_ the ability to enforce those policies at the network level in your WAF. 

Your robots.txt file declares your policy, but now we can help you enforce it. You might even call it … your Robotcop.  

### How it works

AI Audit takes the robots.txt files from your web properties, parses them, and then matches their rules against the AI bot traffic we see for the selected property. The summary table gives you an aggregated view of the number of requests and violations we see for every Bot across all paths. If you hover your mouse over the Robots.txt column, we will show you the defined policies for each Bot in the tooltip. You can also filter by violations from the top of the page. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/o2hHH0Nm68muUzaxmbx7E/0b9c2acfb33f2ca2d59e00625b4d0fc7/BLOG-2619_2.png)

In the “Most popular paths” section, whenever a path in your site gets traffic that has violated your policy, we flag it for visibility. Ideally, you wouldn't see violations in the Robots.txt column — if you do see them, someone's not complying.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/1o5sChT2d6QK8JNPejImVk/79590e1721644a2fd067784bb9ce862e/BLOG-2619_3.png)

But that's not all… More importantly, AI Audit allows you to enforce your robots.txt policy at the network level. By pressing the "Enforce robots.txt rules" button on the top of the summary table, we automatically translate the rules defined for AI Bots in your robots.txt into an advanced firewall rule, redirect you to the WAF configuration screen, and allow you to deploy the rule in our network.

This is how the robots.txt policy mentioned above looks after translation:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/5qYJG3RcvrDxzVDtb28Q2J/d73d7dcea94acb261e9fc525427c2e77/BLOG-2619_4.png)

Once you deploy a WAF rule built from your robots.txt policies, you are no longer simply requesting that AI services respect your policy, you're enforcing it.

### Conclusion

With AI Audit, we are giving our customers even more visibility into how AI services access their content, helping them define their policies and then enforcing them at the network level.

This feature is live today for all Cloudflare customers. Simply log into the dashboard and navigate to your domain to begin auditing the bot traffic from AI services and enforcing your robots.txt directives.

Go to Source
