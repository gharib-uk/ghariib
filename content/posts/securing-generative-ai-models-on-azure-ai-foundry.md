---
title: "Securing generative AI models on Azure AI Foundry"
date: 2025-03-19
---

New generative AI models with a broad range of capabilities are emerging every week. In this world of rapid innovation, when choosing the models to integrate into your AI system, it is crucial to make a thoughtful risk assessment that ensures a balance between leveraging new advancements and maintaining robust security. At Microsoft, we are focusing on making our AI development platform a secure and trustworthy place where you can explore and innovate with confidence. 

Here we’ll talk about one key part of that: how we secure the models and the runtime environment itself. How do we protect against a bad model compromising your AI system, your larger cloud estate, or even Microsoft’s own infrastructure?  

## How Microsoft protects data and software in AI systems

But before we set off on that, let me set to rest one very common misconception about how data is used in AI systems. Microsoft does _not_ use customer data to train shared models, nor does it share your logs or content with model providers. Our AI products and platforms are part of our standard product offerings, subject to the same terms and trust boundaries you’ve come to expect from Microsoft, and your model inputs and outputs are considered customer content and handled with the same protection as your documents and email messages. Our AI platform offerings (Azure AI Foundry and Azure OpenAI Service) are 100% hosted by Microsoft on its own servers, with no runtime connections to the model providers. We do offer some features, such as model fine-tuning, that allow you to use your data to create better models for your own use—but these are _your_ models that stay in your tenant. 

So, turning to model security: the first thing to remember is that models are just software, running in Azure Virtual Machines (VM) and accessed through an API; they don’t have any magic powers to break out of that VM, any more than any other software you might run in a VM. Azure is already quite defended against software running in a VM attempting to attack Microsoft’s infrastructure—bad actors try to do that every day, not needing AI for it, and AI Foundry inherits all of those protections. This is a “zero-trust” architecture: Azure services do not assume that things running on Azure are safe! 

What is Zero Trust?

Learn more

Now, it _is_ possible to conceal malware inside an AI model. This could pose a danger to you in the same way that malware in any other open- or closed-source software might. To mitigate this risk, for our highest-visibility models we scan and test them before release: 

- **Malware analysis**: Scans AI models for embedded malicious code that could serve as an infection vector and launchpad for malware. 

- **Vulnerability assessment**: Scans for common vulnerabilities and exposures (CVEs) and zero-day vulnerabilities targeting AI models. 

- **Backdoor detection**: Scans model functionality for evidence of supply chain attacks and backdoors such as arbitrary code execution and network calls. 

- **Model integrity**: Analyzes an AI model’s layers, components, and tensors to detect tampering or corruption. 

You can identify which models have been scanned by the indication on their model card—no customer action is required to get this benefit. For especially high-visibility models like DeepSeek R1, we go even further and have teams of experts tear apart the software—examining its source code, having red teams probe the system adversarially, and so on—to search for any potential issues before releasing the model. This higher level of scanning doesn’t (yet) have an explicit indicator in the model card, but given its public visibility we wanted to get the scanning done before we had the UI elements ready. 

## Defending and governing AI models

Of course, as security professionals you presumably realize that no scans can detect all malicious action. This is the same problem an organization faces with any other third-party software, and organizations should address it in the usual manner: trust in that software should come in part from trusted intermediaries like Microsoft, but above all should be rooted in an organization’s own trust (or lack thereof) for its provider.  

For those wanting a more secure experience, once you’ve chosen and deployed a model, you can use the full suite of Microsoft’s security products to defend and govern it. You can read more about how to do that here: Securing DeepSeek and other AI systems with Microsoft Security.

And of course, as the quality and behavior of each model is different, you should evaluate any model not just for security, but for whether it fits your specific use case, by testing it as part of your complete system. This is part of the wider approach to how to secure AI systems which we’ll come back to, in depth, in an upcoming blog. 

## Using Microsoft Security to secure AI models and customer data

In summary, the key points of our approach to securing models on Azure AI Foundry are: 

1. Microsoft carries out a variety of security investigations for key AI models before hosting them in the Azure AI Foundry Model Catalogue, and continues to monitor for changes that may impact the trustworthiness of each model for our customers. You can use the information on the model card, as well as your trust (or lack thereof) in any given model builder, to assess your position towards any model the way you would for any third-party software library. 

2. All models hosted on Azure are isolated within the customer tenant boundary. There is no access to or from the model provider, including close partners like OpenAI. 

3. Customer data is not used to train models, nor is it made available outside of the Azure tenant (unless the customer designs their system to do so). 

## Learn more with Microsoft Security

To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.

The post Securing generative AI models on Azure AI Foundry appeared first on Microsoft Security Blog.

Go to Source
