---
title: "Adapting Security Strategies for the Gen AI Era"
date: 2025-01-29
categories: 
  - "ai"
  - "ci-cd"
  - "cloud"
  - "cloudnative"
  - "data"
  - "development"
  - "devops"
  - "devops-cloud"
  - "devsecops"
  - "digital-transformation"
  - "education"
  - "expert-pov"
  - "featured"
  - "ndc"
  - "security"
---

![](https://devopsonline.co.uk/wp-content/uploads/2024/11/AI-webpost-1.jpg)

## **Generative AI is Evolving—Is Your Security Strategy Keeping Pace?**

**Generative AI** has quickly become a pivotal element in **digital transformation** across industries, driving automation, enhancing customer interactions, and accelerating software development. But with these advancements come new and complex security challenges. Companies are rushing to adopt **generative AI** to stay competitive, yet many haven’t adapted their cybersecurity practices to handle the emerging threats. When companies designed their cybersecurity policies and controls, few, if any, had generative AI on their bingo card. As a result, we’re seeing organisations unprepared for the unique vulnerabilities this technology introduces.

## **The intersection of generative AI and cybersecurity**

There’s a critical intersection between AI and cybersecurity that we can’t ignore. On the positive side, generative AI offers significant opportunities to strengthen defences. For instance, it can automate threat detection, streamline code scanning, and quickly pinpoint vulnerabilities. AI tools are helping blue teams—the defensive security teams—by recognising patterns and detecting anomalies far faster than we could with traditional methods. This ultimately helps organisations respond more quickly to incidents and limit damage from potential breaches.

However, there’s a flip side. Just as we are using AI to enhance security, threat actors are leveraging it to improve their attacks. For example, we’ve seen how traditional **cyberattacks** like phishing are becoming more sophisticated, thanks to generative AI. Attackers can now automate the creation of highly convincing fake emails and messages, which makes it easier to trick users into revealing sensitive information.  Or more sophisticated attacks such as malware that used to require a detailed understanding of low-level software layers and years of study can now be generated from a prompt. What’s even more concerning is the potential for AI to create entirely new types of attacks, like prompt injection, which we have little experience handling. We’re essentially chasing a moving target that’s evolving at an almost unprecedented rate in the industry. Our **cybersecurity strategies** need to keep pace, but we’re lagging behind.

<figure>

![Security strategies for the Gen AI era](https://devopsonline.co.uk/wp-content/uploads/2024/11/Jason-Goth.png)

<figcaption>

_**Learn how to adapt security strategies for the Gen AI on the 31 Media Podcast**_

</figcaption>

</figure>

## **Emerging threats in AI cybersecurity**

Some of the most dangerous AI-driven threats we’re seeing today are prompt injection and data poisoning. Prompt injection is particularly tricky because generative AI models process such a wide variety of inputs—anything from human language to code—which makes it harder to spot harmful instructions. A simple prompt can be manipulated to extract sensitive data, bypass security measures, or execute malicious commands without the user even realising it.

Data poisoning is another serious issue. Attackers can feed corrupt data into AI models, altering their outputs in subtle but harmful ways. I’ve referenced the example from Microsoft’s AI, where bad actors introduced toxic inputs that caused the model to exhibit harmful behaviours. This clearly illustrates how easy it is for attackers to “poison the well” and manipulate AI systems to produce unintended, damaging outcomes. Data poisoning can also be difficult to detect, as images and videos can be subtly altered in ways that the human eye can’t detect.

Red teams—offensive security teams—are already using generative AI to create more advanced attack simulations, while blue teams are struggling to keep up. This growing disparity between offensive and defensive AI applications is a big concern for me. If we don’t act fast, the gap will only widen, leaving many organisations vulnerable to AI-driven cyberattacks.

Confidentiality and privacy concerns also persist. Even if there is no malicious intent to poison models, sensitive data—such as trade secrets, internal communications, or personally identifiable information (PII)—can still find its way into models. For example, companies may use AI to search and summarise documents from their internal SharePoint, not realising that offer letters with salary information are included. This data can unexpectedly appear in responses from an LLM.

## **How can companies securely implement and manage generative AI while mitigating the unique cybersecurity risks it introduces?**

- **Strengthen your data governance.** Data is the backbone of any AI model, so you need to know exactly where it’s coming from and ensure it’s clean, reliable, and secure before using it. If you’re unsure about your data’s integrity, provenance, or how it’s been handled, you’re opening the door to major risks. Start by auditing your data sources and setting up a governance framework to track data lineage. Build in regular data quality checks, and make sure you’re reviewing permissions—who has access to it and why? As I often say, “If you don’t understand your data’s integrity and origins, you’ll run into problems.” Begin by mapping your data flow: identify the sources, review how it’s collected, and validate its accuracy before feeding it into any AI models.

_Note that this doesn’t imply a long, slow manual effort. Quite the contrary. As mentioned above, many data issues are unrecognisable to the human eye. Large volumes of documents can’t be scanned manually, and even if they could, it’s hard for humans to detect subtle biases or data anomalies. Automation and machine learning are key to modern data governance._

- **Train your team to recognise AI-specific threats.** Cyberattacks are evolving, and AI is making phishing and social engineering attacks more convincing. It’s no longer enough to give employees a general cybersecurity overview—they need targeted training that includes AI-generated threats. Make sure your staff knows what AI-powered phishing attempts look like and how to respond. Offer hands-on training with real-world examples of AI-driven cyber-attacks and update these regularly as new threats emerge. It’s about getting your team to think critically and recognise patterns that might go unnoticed. Develop specific training sessions on AI-related cybersecurity threats, such as AI-generated phishing and prompt injection attacks. Incorporate simulated attacks as part of your phishing tests.
- **Set clear guardrails for AI use.** Without proper boundaries, generative AI can expose sensitive company data or even introduce security risks. You need to establish clear rules for how your teams are allowed to use AI. Draft an AI usage policy that defines where generative AI can be applied in your company. Be specific about data it can access, who can use it, and implement approval processes for higher-risk areas. At Credera, we’ve put strict policies in place to control where and how AI can be used, so we’re not exposing ourselves to unnecessary risks. We’ve also automated many of these guardrails and exposed them as APIs so that development teams can use them without slowing innovation.
- **Be careful about directly exposing AIs to users.** LLMs are still evolving rapidly. While we have learned about some threats to AI-based systems, such as prompt injection and data poisoning, we also recognise that there are likely more threats yet to be discovered. For this reason, we recommend that companies avoid allowing direct user interactions with LLMs. The analogy we use is a database: we don’t let users read and write to databases directly. Instead, we introduce software to abstract this interaction and enforce business and security rules. These layers can make use of the automated guardrails discussed above to provide a more consistent and safer experience.
- **Thoroughly vet AI-generated code.** AI-generated code can be a huge time-saver, but it should never go straight into production without careful security reviews. Code produced by AI could have hidden flaws or vulnerabilities that aren’t immediately obvious. Continue using your current security measures, such as DAST (Dynamic Application Security Testing) and SAST (Static Application Security Testing), to catch potential risks in the AI-generated code before deployment. This ensures your pipeline remains secure, regardless of the speed AI brings to development. Require that all AI-generated code goes through both automated and manual code reviews and integrate these processes into your CI/CD pipeline to ensure no code, AI-generated or otherwise, bypasses your security reviews.

As we move forward, it’s clear we’re entering an “AI arms race,” where both companies and cybercriminals will leverage increasingly advanced AI models. Just as AI is revolutionising customer service and business operations, it’s also giving rise to more sophisticated and dangerous cyber threats. From AI-generated zero-day vulnerabilities to self-evolving malware, the risks are escalating. As I often emphasise, the red team’s creativity will always push boundaries, but it’s crucial for blue teams to meet that challenge with equal innovation and agility in their defences. The future of cybersecurity will depend on how quickly and effectively companies adapt to this AI-driven landscape.

* * *

### **Get in touch**

For event sponsorship enquiries, please get in touch with **calum.budge@31media.co.uk**  
For media enquiries, please get in touch with **vaishnavi.nashte@31media.co.uk**

<figure>

![Digital Transformation Awards 2025](https://devopsonline.co.uk/wp-content/uploads/2024/09/DTA_header.png)

<figcaption>

Enter the Digital Transformation Awards 2025 here.

</figcaption>

</figure>

The post Adapting Security Strategies for the Gen AI Era first appeared on DevOps Online.

Go to Source
