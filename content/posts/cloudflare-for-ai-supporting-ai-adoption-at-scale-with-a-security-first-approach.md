---
title: "Cloudflare for AI: supporting AI adoption at scale with a security-first approach"
date: 2025-03-19
---

AI is transforming businesses — from automated agents performing background workflows, to improved search, to easier access and summarization of knowledge. 

While we are still early in what is likely going to be a substantial shift in how the world operates, two things are clear: the Internet, and how we interact with it, will change, and the boundaries of security and data privacy have never been more difficult to trace, making security an important topic in this shift.

At Cloudflare, we have a mission to help build a better Internet. And while we can only speculate on what AI will bring in the future, its success will rely on it being reliable and safe to use.

Today, we are introducing Cloudflare for AI: a suite of tools aimed at helping businesses, developers, and content creators adopt, deploy, and secure AI technologies at scale safely.

Cloudflare for AI is not just a grouping of tools and features, some of which are new, but also a commitment to focus our future development work with AI in mind.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4s1CIOnpHnIdWPdFrzS6e1/a29e0ffa7123d69e855b3eac6d0a154f/1.png)

Let’s jump in to see what Cloudflare for AI can deliver for developers, security teams, and content creators…

### For developers

If you are building an AI application, whether a fully custom application or a vendor-provided hosted or SaaS application, Cloudflare can help you deploy, store, control/observe, and protect your AI application from threats.

**Build & deploy**: Workers AI and our new AI Agents SDK facilitates the scalable development & deployment of AI applications on Cloudflare’s network. Cloudflare’s network enhances user experience and efficiency by running AI closer to users, resulting in low-latency and high-performance AI applications. Customers are also using Cloudflare’s R2 to store their AI training data with zero egress fees, in order to develop the next-gen AI models. 

We are continually investing in not only our serverless AI inference infrastructure across the globe, but also in making Cloudflare the best place to build AI Agents. Cloudflare’s composable AI architecture has all the primitives that enable AI applications to have real time communications, persist state, execute long-running tasks, and repeat them on a schedule. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2M1dOiI89ecQAb1y0SJ57E/dcc98c263ea9503aa0aacdf69326fa9d/2.png)

**Protect and control**: Once your application is deployed, be it directly on Cloudflare, using Workers AI, or running on your own infrastructure (cloud or on premise), Cloudflare’s AI Gateway lets you gain visibility into the cost, usage, latency, and overall performance of the application.

Additionally, Firewall for AI lets you layer security on top by automatically ensuring every prompt is clean from injection, and that personally identifiable information (PII) is neither submitted to nor (coming soon) extracted from, the application.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4dDiUMSKDoqyrPgzjiHfkx/22a2a9e159f8efcbf2b2a43f9bdf2a9b/3.png)

### For security teams

Security teams have a growing new challenge: ensure AI applications are used securely, both in regard to internal usage by employees, as well as by users of externally-facing AI applications the business is responsible for. Ensuring PII data is handled correctly is also a growing major concern for CISOs.

**Discover applications:** You can’t protect what you don’t know about. Firewall for AI’s discovery capability lets security teams find AI applications that are being used within the organization without the need to perform extensive surveys.

**Control PII flow and access:** Once discovered, via Firewall for AI or other means, security teams can leverage Zero Trust Network Access (ZTNA) to ensure only authorized employees are accessing the correct applications. Additionally, using Firewall for AI, they can ensure that, even if authorised, neither employees nor potentially external users, are submitting or extracting personally identifiable information (PII) to/from the application.

**Protect against exploits:** Malicious users are targeting AI applications with novel attack vectors, as these applications are often connected to internal data stores. With Firewall for AI and the broader Application Security portfolio, you can protect against a wide number of exploits highlighted in the OWASP Top 10 for LLM applications, including, but not limited to, prompt injection, sensitive information disclosure, and improper output handling.

**Safeguarding conversations:** With Llama Guard integrated into both AI Gateway and Firewall for AI, you can ensure both input and output of your AI application is not toxic, and follows topic and sentiment rules based on your internal business policies.

### For content creators

The advent of AI is arguably putting content creators at risk, with sophisticated LLM models now generating both text, images, and videos of high quality. We’ve blogged in the past about AI Independence, our approach to safeguarding content creators, for both individuals and businesses. If you fall in this category, we have the right tools for you too.

**Observe who is accessing your content:** With our AI Audit dashboard, you gain visibility (who, what, where and when) into the AI platforms crawling your site to retrieve content to use for AI training data. We are constantly classifying and adding new vendors as they create new crawlers.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6s9WmTy7EQyOF6mCZd6KTc/1ba40dd811bb81de26da04155655e6d8/4.png)

**Block access:** If AI crawlers do not follow robots.txt or other relevant standards, or are potentially unwanted, you can block access outright. We’ve provided a simple “one click” button for customers using Cloudflare on our self-serve plans to protect their website. Larger organizations can build fine tune rules using our Bot Management solution allowing them to target individual bots and create custom filters with ease.

### Cloudflare for AI: making AI security simple

If you are using Cloudflare already, or the deployment and security of AI applications is top of mind, reach out, and we can help guide you through our suite of AI tools to find the one that matches your needs.

Ensuring AI is scalable, safe and resilient, is a natural extension of Cloudflare’s mission, given so much of our success relies on a safe Internet.

Go to Source
