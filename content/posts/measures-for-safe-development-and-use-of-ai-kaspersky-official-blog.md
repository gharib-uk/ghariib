---
title: "Measures for safe development and use of AI | Kaspersky official blog"
date: 2025-01-03
categories: 
  - "cybersecurity"
  - "security"
---

Technical and organizational precautions when deploying existing AI systems and developing new ones

Today, AI-based technologies are already being used in every second company — with another 33% of commercial organizations expected to join them in the next two years. AI, in one form or another, will soon be ubiquitous. The economic benefits of adopting AI range from increased customer satisfaction to direct revenue growth. As businesses deepen their understanding of AI systems’ strengths and weaknesses, their effectiveness will only improve. However, it’s already clear that the risks associated with AI adoption need to be addressed proactively.

Even early examples of AI implementation show that errors can be costly — affecting not only finances but also reputation, customer relationships, patient health, and more. In the case of cyber-physical systems like autonomous vehicles, safety concerns become even more critical.

Implementing safety measures retroactively, as was the case with previous generations of technology, will be expensive and sometimes impossible. Just consider the recent estimates of global economic losses due to cybercrime: $8 trillion in 2023 alone. In this context, it’s not surprising that countries claiming 21st century technological leadership are rushing to set up AI regulation (for example, China’s AI Safety Governance Framework, the EU’s AI Act, and the US Executive Order on AI). However, laws rarely specify technical details or practical recommendations — that’s not their purpose. Therefore, to actually apply regulatory requirements such as ensuring the reliability, ethics, and accountability of AI decision-making, concrete and actionable guidelines are required.

To assist practitioners in implementing AI today and ensuring a safer future, Kaspersky experts have developed a set of recommendations in collaboration with Allison Wylde, UN Internet Governance Forum Policy Network on AI team-member; Dr. Melodena Stephens, Professor of Innovation & Technology Governance from the Mohammed Bin Rashid School of Government (UAE); and Sergio Mayo Macías, Innovation Programs Manager at the Technological Institute of Aragon (Spain). The document was presented during the panel “Cybersecurity in AI: Balancing Innovation and Risks” at the 19th Annual UN Internet Governance Forum (IGF) for discussion with the global community of AI policymakers.

Following the practices described in the document will help respective engineers — DevOps and MLOps specialists who develop and operate AI solutions — achieve a high level of security and safety for AI systems at all stages of their lifecycle. The recommendations in the document need to be tailored for each AI implementation, as their applicability depends on the type of AI and the deployment model.

## Risks to consider

The diverse applications of AI force organizations to address a wide range of risks:

- The risk of not using AI. This may sound amusing, but it’s only by comparing the potential gains and losses of adopting AI that a company can properly evaluate all other risks.
- Risks of non-compliance with regulations. Rapidly evolving AI regulations make this a dynamic risk that needs frequent reassessment. Apart from AI-specific regulations, associated risks such as violations of personal-data processing laws must also be considered.
- ESG risks. These include social and ethical risks of AI application, risks of sensitive information disclosure, and risks to the environment.
- Risk of misuse of AI services by users. This can range from prank scenarios to malicious activities.
- Threats to AI models and datasets used for training.
- Threats to company services due to AI implementation.
- The resulting threats to the data processed by these services.

“Under the hood” of the last three risk groups lie all typical cybersecurity threats and tasks involving complex cloud infrastructure: access control, segmentation, vulnerability and patch management, creation of monitoring and response systems, and supply-chain security.

## Aspects of safe AI implementation

To implement AI safely, organizations will need to adopt both organizational and technical measures, ranging from staff training and periodic regulatory compliance audits to testing AI on sample data and systematically addressing software vulnerabilities. These measures can be grouped into eight major categories:

- **Threat modeling** for each deployed AI service.
- **Employee training.** It’s important not only to teach employees general rules for AI use, but also to familiarize business stakeholders with the specific risks of using AI and tools for managing those risks.
- **Infrastructure security**. This includes identity security, event logging, network segmentation, and XDR.
- **Supply-chain security.** For AI, this involves carefully selecting vendors and intermediary services that provide access to AI, and only downloading models and tools from trusted and verified sources in secure formats.
- **Testing and validation.** AI models need to be evaluated for compliance with the industry’s best practices, resilience to inappropriate queries, and their ability to effectively process data within the organization’s specific business process.
- **Handling vulnerabilities.** Processes need to be established to address errors and vulnerabilities identified by third parties in the organization’s system and AI models. This includes mechanisms for users to report detected vulnerabilities and biases in AI systems, which may arise from training on non-representative data.
- **Protection against threats specific to AI models,** including prompt injections and other malicious queries, poisoning of training data, and more.
- **Updates and maintenance.** As with any IT system, a process must be built for prioritizing and promptly eliminating vulnerabilities, while preparing for compatibility issues as libraries and models evolve rapidly.
- **Regulatory compliance.** Since laws and regulations for AI safety are being adopted worldwide, organizations need to closely monitor this landscape and ensure their processes and technologies comply with legal requirements.

For a detailed look at the AI threat landscape and recommendations on all aspects of its safe use, download Guidelines for Secure Development and Deployment of AI Systems.

Go to Source
