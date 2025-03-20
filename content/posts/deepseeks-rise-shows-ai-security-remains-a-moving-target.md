---
title: "DeepSeek’s Rise Shows AI Security Remains a Moving Target"
date: 2025-02-01
---

If you’ve been following tech news in the last few days, you’ve heard of DeepSeek. This large language model (LLM) is threatening to disrupt current AI market leaders and fundamentally change the economics of AI-powered applications.

Released by a 200-person Chinese startup, the model appears as capable as state-of-the-art tools offered by OpenAI and Google with the benefit of being significantly faster and less expensive to run. What’s more, DeepSeek has been released to open source and is lightweight enough to run on commodity hardware – any developer can start tinkering with it without having to access costly GPUs. DeepSeek has been heralded as a “Sputnik moment” for AI and has sent shockwaves through financial markets.

Palo Alto Networks Unit 42 also uncovered concerning vulnerabilities in DeepSeek, revealing that it can be easily jailbroken to produce nefarious content with little to no specialized knowledge or expertise. Unit 42 researchers recently uncovered two novel and effective jailbreaking techniques, Deceptive Delight and Bad Likert Judge. Given their success against other LLMs, Unit 42 tested these two jailbreaks and another multistage jailbreaking technique, called Crescendo, against DeepSeek models, finding high rates in bypassing safeguards. These techniques enabled explicit guidance on malicious activities, including keylogger creation, data exfiltration and even instructions for incendiary devices, demonstrating the tangible security risks.

## What Does All This Mean for Security Leaders Like You?

Every organization will have its policies about new AI models. Some will ban them completely; others will allow limited, experimental and heavily guardrailed use. Still others will rush to deploy it in production, looking to eke out that extra bit of performance and cost optimization.

But beyond your organization’s need to decide on a new specific model, DeepSeek’s rise offers several lessons about AI security in 2025. AI’s pace of change, and the surrounding sense of urgency, can’t be compared to other technologies. How can you plan ahead when a somewhat obscure model (and the more than 500 derivatives already available on Hugging Face) becomes the number-one priority seemingly out of nowhere?

The short answer: you can’t. AI security remains a moving target and is going to stay that way for a while. And things are changing quickly. DeepSeek isn’t going to be the last model that catches the world by surprise. It will take time before AI technologies are fully understood and clear leaders emerge. Until then, you have no choice but to expect the unexpected.

Organizations can switch LLMs at little to no cost, which allows development teams to move quickly. Replacing software that relies on OpenAI, Google or Anthropic’s models with DeepSeek (or whatever model comes out tomorrow) usually requires updating just a few lines of code. The temptation for product builders to test the new model to see if it can solve a cost issue or latency bottleneck or outperform on a specific task is huge. And if the model turns out to be the missing piece that helps bring a potentially game-changing product to market, you don’t want to be the one who stands in the way.

## Secure AI by Design

While it can be challenging to guarantee complete protection against all adversarial techniques for a specific LLM, organizations can implement security measures that can help monitor when and how employees are using LLMs. This becomes crucial when employees are using unauthorized third-party LLMs. That’s the goal we had in mind when we rolled out our Secure AI by Design portfolio last year. It was all about empowering organizations to allow their employees to safely adopt AI tools and deploy enterprise AI applications.

To do this, we developed AI Access Security to empower our customers to enable secure use of third party AI tools by employees:

- Gain real-time visibility into what GenAI applications are being used across an organization and by whom.
- Streamline access controls with the ability to block unsanctioned apps, apply InfoSec policies, and protect against threats.
- Protect sensitive data from unauthorized access and leakage to risky GenAI applications and AI training models.

We built AI Runtime Security and AI-SPM to enable organizations to secure their own AI-powered applications:

- Understand what AI assets are in the environment and where they are located, as well as which models and data sources they are connected to, and who has access to this ecosystem.
- Protect apps and models against supply chain, configuration and runtime risks for all AI apps and models.
- Secure the data within applications and models from leaks, whether through intentional actions or inadvertent exposures.

## Great Technology Meets Good Governance

In our whitepaper Establishing AI Governance for AI-Powered Applications, we suggest that organizations adopt frameworks for **visibility and control** over AI usage – from model training to application deployment.

#### Four Steps to Manage Risks Related to New LLMs

**1\. Create centralized visibility into AI model usage in the organization –** Setting controls at the model provider level will quickly become a game of whack-a-mole and is impossible to do with open-source models. Instead, look to establish cross-cloud and cross-organizational visibility into the existing model inventory, with systems in place to monitor when new models are deployed. Sign up for a free demo of AI Runtime Security and learn how you can perform a no-cost, risk-free AI discovery.

**2\. Maintain clear policies regarding sanctioned and unsanctioned models** **–** While every organization will have a different tolerance for risk when it comes to new technologies, it’s best not to make these decisions on a completely ad hoc basis. Having a well-established process for vetting, evaluating and approving new models will prove useful in these situations.

**3\. Decide on relevant guardrails for models in production** **–** Again, there’s a question of risk tolerance here, but the decision should be made in advance. Guardrails can be applied on model input and/or model output. AI Runtime Security allows you to inspect and block AI-specific threats, such as prompt injections, malicious URLs and insecure data outputs.

**4\. Reduce opportunities for data exfiltration –** Once models have access to sensitive data (for training or inference purposes), you’re playing a much higher stakes game. Adopting a unified security perspective that gives you a single view into data, potential attack paths, code and applications will help you understand where sensitive data is at risk. AI Runtime Security can detect thousands of predefined data patterns to secure against data exfiltration through intentional actions or inadvertent exposures.

![Visibility on models, data, use cases, access, compliance.](https://www.paloaltonetworks.com/blog/wp-content/uploads/2025/01/word-image-333700-1.png)

## A Wake Up Call

The public launch of DeepSeek has been a wake-up call for financial markets and industries across the board looking to adopt AI. If this has been a wake-up call for your organization, let us help you navigate the road ahead.

Want to know what AI applications are in your cloud environments and how you can secure them? Sign up for a free demo of AI Runtime Security.

The post DeepSeek’s Rise Shows AI Security Remains a Moving Target appeared first on Palo Alto Networks Blog.

Go to Source
