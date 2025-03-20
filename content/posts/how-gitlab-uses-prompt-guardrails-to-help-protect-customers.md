---
title: "<div>How GitLab uses prompt guardrails to help protect customers</div>"
date: 2025-02-01
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Imagine introducing a powerful new AI tool that boosts your team's productivity — accelerating code development, resolving issues faster, and streamlining workflows. The excitement is palpable, but questions about security and compliance quickly arise. How do you manage the risk of AI inadvertently exposing sensitive data or responding to malicious prompts? This is where prompt guardrails play a crucial role.

Prompt guardrails are structured safeguards – combining instructions, filters, and context boundaries – designed to guide AI models toward secure and reliable responses. Think of them as safety rails on a bridge, working to keep data and interactions on the correct path while supporting your organization's security protocols. In this article, we'll explore how GitLab implements these guardrails, the risks they address, and their importance for security-conscious enterprises and compliance-focused teams.

## Why prompt guardrails matter

AI models have transformed how organizations work, offering powerful tools to enhance productivity and innovation. However, this power comes with inherent risks. Without safeguards, AI systems may unintentionally disclose sensitive information, such as personally identifiable information (PII) or proprietary business data, or potentially act on malicious instructions. Prompt guardrails address these challenges by creating boundaries for AI models to access and process approved content, contributing to reduced risk of unintended data exposure or manipulation.

For businesses operating under strict regulations like GDPR, prompt guardrails serve as essential protection mechanisms. More importantly, they build trust among decision-makers, end users, and customers, demonstrating GitLab's commitment to secure and responsible AI usage. With prompt guardrails in place, teams can embrace AI's potential while maintaining focus on protecting their critical assets.

## GitLab’s approach to prompt guardrails

At GitLab, we're building AI features with security, transparency, and accountability in mind because we understand these elements are critical for our enterprise customers and their auditors.

Here’s how we’re putting that into practice.

### Structured prompts and context boundaries

Our system utilizes tags – like `<selected_code>` or `<log>` – to define boundaries for AI model interactions. When users ask GitLab Duo to troubleshoot a job failure, relevant logs are encapsulated in `<log>` tags. This structure guides the model to focus on specific data while working to prevent the influence from unauthorized or out-of-scope information.

### Filtering and scanning tools

We employ tools like Gitleaks to scan inputs for secrets (API keys, passwords, etc.) before transmission to the AI. This filtering process helps minimize the potential for exposing confidential information or sending credentials into a model's prompt.

### Role-based insights

Our guardrails support focused AI discussions while contributing to customers' compliance efforts through controlled data handling and clear documentation. Organizations can adopt AI solutions designed to align with enterprise policies and risk tolerances.

## Different approaches to prompt guardrails

Prompt guardrails aren't one-size-fits-all solutions. Different strategies offer unique advantages, with effectiveness varying by use case and organizational requirements. GitLab combines multiple approaches to create a comprehensive system designed to balance security with usability.

### System-level filters: The first line of defense

System-level filters serve as a proactive barrier, scanning prompts for restricted keywords, patterns, or potentially harmful content. These filters work to identify and block potential risks — such as profanity, malicious commands, or unauthorized requests — before they reach the AI model.

This approach requires continuous updates to maintain effectiveness. As threats evolve, maintaining current libraries of restricted keywords and patterns becomes crucial. GitLab integrates these filters into its workflows to address potential risks at the earliest stage.

### Model instruction tuning: Teaching the AI to stay on track

Instruction tuning involves configuring AI behavior to align with specific guidelines. Our AI models are designed to reduce potentially problematic behaviors like role play, impersonation, or generating inappropriate content.

This foundation supports responses that remain informative, professional, and focused. When summarizing discussions or analyzing code, the AI maintains focus on the provided context, ideally mitigating potential deviation into unrelated topics.

### Sidecar or gateway solutions: Adding a layer of protection

Sidecar or gateway solutions function as security checkpoints between users and AI models, processing both inputs and outputs. Like a customs officer reviewing luggage, these components help ensure only appropriate content passes through.

This approach proves particularly valuable in environments requiring strict information control, such as regulated industries or compliance-driven workflows.

### Why GitLab combines these approaches

No single strategy addresses all potential risks. GitLab's hybrid approach combines system-level filters, instruction tuning, and sidecar solutions to create a robust security framework while maintaining usability.

System-level filters provide initial screening, while instruction tuning aligns AI behavior with security standards. Sidecar solutions offer additional oversight, supporting transparency and control over data flow.

This combination creates a framework designed to support confident AI adoption while aiming to protect sensitive data and maintain compliance requirements.

## Lessons learned

While prompt guardrails help to significantly reduce risks, no system is infallible. Here are some lessons we have learned along the way:

- Overly restrictive rules might hamper legitimate usage, frustrate developers, or slow down workflows. Striking the right balance between protecting data and providing real value is key.
- Threat landscapes change, as do the ways people use AI. Regular updates to guardrails support alignment with current requirements and potential threats
- At GitLab, we understand that no system can promise absolute security. Instead of making guarantees, we emphasize how our guardrails are designed to reduce risks and strengthen your defenses. This transparent approach builds trust by acknowledging that security is an ongoing process — one that we continuously refine to help support your organization’s evolving needs.
- We gather feedback from actual user scenarios to iterate on our guardrails. Real-world insights help us refine instructions, tighten filters, and improve scanning tools over time.

## Summary

Prompt guardrails go beyond being a technical solution — they represent GitLab’s commitment to prioritizing AI security for our customers. By helping to reduce exposure, block harmful inputs, and ensure clear traceability of AI interactions, these guardrails aim to provide your teams with the confidence to innovate securely.

With GitLab Duo, our structured prompts, scanning tools, and carefully tuned instructions work together to help keep AI capabilities aligned with compliance standards and best practices. Whether you’re a developer, auditor, or decision-maker, these safeguards aim to enable you to embrace AI confidently while staying true to your organization’s security and compliance goals.

> Learn more about GitLab Duo and get started with a free, 60-day trial today!

Imagine introducing a powerful new AI tool that boosts your team's productivity — accelerating code development, resolving issues faster, and streamlining workflows. The excitement is palpable, but questions about security and compliance quickly arise. How do you manage the risk of AI inadvertently exposing sensitive data or responding to malicious prompts? This is where prompt guardrails play a crucial role.

Prompt guardrails are structured safeguards – combining instructions, filters, and context boundaries – designed to guide AI models toward secure and reliable responses. Think of them as safety rails on a bridge, working to keep data and interactions on the correct path while supporting your organization's security protocols. In this article, we'll explore how GitLab implements these guardrails, the risks they address, and their importance for security-conscious enterprises and compliance-focused teams.

## Why prompt guardrails matter

AI models have transformed how organizations work, offering powerful tools to enhance productivity and innovation. However, this power comes with inherent risks. Without safeguards, AI systems may unintentionally disclose sensitive information, such as personally identifiable information (PII) or proprietary business data, or potentially act on malicious instructions. Prompt guardrails address these challenges by creating boundaries for AI models to access and process approved content, contributing to reduced risk of unintended data exposure or manipulation.

For businesses operating under strict regulations like GDPR, prompt guardrails serve as essential protection mechanisms. More importantly, they build trust among decision-makers, end users, and customers, demonstrating GitLab's commitment to secure and responsible AI usage. With prompt guardrails in place, teams can embrace AI's potential while maintaining focus on protecting their critical assets.

## GitLab’s approach to prompt guardrails

At GitLab, we're building AI features with security, transparency, and accountability in mind because we understand these elements are critical for our enterprise customers and their auditors.

Here’s how we’re putting that into practice.

### Structured prompts and context boundaries

Our system utilizes tags – like `<selected_code>` or `<log>` – to define boundaries for AI model interactions. When users ask GitLab Duo to troubleshoot a job failure, relevant logs are encapsulated in `<log>` tags. This structure guides the model to focus on specific data while working to prevent the influence from unauthorized or out-of-scope information.

### Filtering and scanning tools

We employ tools like Gitleaks to scan inputs for secrets (API keys, passwords, etc.) before transmission to the AI. This filtering process helps minimize the potential for exposing confidential information or sending credentials into a model's prompt.

### Role-based insights

Our guardrails support focused AI discussions while contributing to customers' compliance efforts through controlled data handling and clear documentation. Organizations can adopt AI solutions designed to align with enterprise policies and risk tolerances.

## Different approaches to prompt guardrails

Prompt guardrails aren't one-size-fits-all solutions. Different strategies offer unique advantages, with effectiveness varying by use case and organizational requirements. GitLab combines multiple approaches to create a comprehensive system designed to balance security with usability.

### System-level filters: The first line of defense

System-level filters serve as a proactive barrier, scanning prompts for restricted keywords, patterns, or potentially harmful content. These filters work to identify and block potential risks — such as profanity, malicious commands, or unauthorized requests — before they reach the AI model.

This approach requires continuous updates to maintain effectiveness. As threats evolve, maintaining current libraries of restricted keywords and patterns becomes crucial. GitLab integrates these filters into its workflows to address potential risks at the earliest stage.

### Model instruction tuning: Teaching the AI to stay on track

Instruction tuning involves configuring AI behavior to align with specific guidelines. Our AI models are designed to reduce potentially problematic behaviors like role play, impersonation, or generating inappropriate content.

This foundation supports responses that remain informative, professional, and focused. When summarizing discussions or analyzing code, the AI maintains focus on the provided context, ideally mitigating potential deviation into unrelated topics.

### Sidecar or gateway solutions: Adding a layer of protection

Sidecar or gateway solutions function as security checkpoints between users and AI models, processing both inputs and outputs. Like a customs officer reviewing luggage, these components help ensure only appropriate content passes through.

This approach proves particularly valuable in environments requiring strict information control, such as regulated industries or compliance-driven workflows.

### Why GitLab combines these approaches

No single strategy addresses all potential risks. GitLab's hybrid approach combines system-level filters, instruction tuning, and sidecar solutions to create a robust security framework while maintaining usability.

System-level filters provide initial screening, while instruction tuning aligns AI behavior with security standards. Sidecar solutions offer additional oversight, supporting transparency and control over data flow.

This combination creates a framework designed to support confident AI adoption while aiming to protect sensitive data and maintain compliance requirements.

## Lessons learned

While prompt guardrails help to significantly reduce risks, no system is infallible. Here are some lessons we have learned along the way:

- Overly restrictive rules might hamper legitimate usage, frustrate developers, or slow down workflows. Striking the right balance between protecting data and providing real value is key.
- Threat landscapes change, as do the ways people use AI. Regular updates to guardrails support alignment with current requirements and potential threats
- At GitLab, we understand that no system can promise absolute security. Instead of making guarantees, we emphasize how our guardrails are designed to reduce risks and strengthen your defenses. This transparent approach builds trust by acknowledging that security is an ongoing process — one that we continuously refine to help support your organization’s evolving needs.
- We gather feedback from actual user scenarios to iterate on our guardrails. Real-world insights help us refine instructions, tighten filters, and improve scanning tools over time.

## Summary

Prompt guardrails go beyond being a technical solution — they represent GitLab’s commitment to prioritizing AI security for our customers. By helping to reduce exposure, block harmful inputs, and ensure clear traceability of AI interactions, these guardrails aim to provide your teams with the confidence to innovate securely.

With GitLab Duo, our structured prompts, scanning tools, and carefully tuned instructions work together to help keep AI capabilities aligned with compliance standards and best practices. Whether you’re a developer, auditor, or decision-maker, these safeguards aim to enable you to embrace AI confidently while staying true to your organization’s security and compliance goals.

> Learn more about GitLab Duo and get started with a free, 60-day trial today!

Go to Source
