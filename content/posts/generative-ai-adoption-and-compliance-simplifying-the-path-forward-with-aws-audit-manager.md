---
title: "Generative AI adoption and compliance: Simplifying the path forward with AWS Audit Manager"
date: 2025-01-02
categories: 
  - "security"
---

As organizations increasingly use generative AI to streamline processes, enhance efficiency, and gain a competitive edge in today’s fast-paced business environment, they seek mechanisms for measuring and monitoring their use of AI services.

To help you navigate the process of adopting generative AI technologies and proactively measure your generative AI implementation, AWS developed the AWS Audit Manager generative AI best practices framework. This framework provides a structured approach to evaluating and adopting generative AI technologies and addresses important aspects such as strategy alignment, governance, risk assessment, and security and operational best practices. You can use the framework within AWS Audit Manager as you implement generative AI workloads, to measure and monitor existing workloads through Audit Manager capabilities such as automated evidence collection and customized assessment reports.

In this blog post, we’ll cover the AWS Audit Manager generative AI best practices framework and how it can help you during your generative AI journey. We’ll highlight key considerations to prioritize when deploying generative AI workloads, and discuss how the framework can facilitate auditing and compliance with generative AI-specific controls using Audit Manager.

## Starting the generative AI Journey

An important consideration in preparing for the introduction of generative AI in your organization is the need to align your risk management strategies with robust mitigation measures. Examples of potential risks include the following:

- **Data quality, reliability, and bias:** Poor source-data quality used to train models might lead to inconsistent, inaccurate, or biased outputs, which can have significant financial and regulatory impact for organizations. For example, a language model trained on biased data might generate text that reinforces harmful stereotypes or propagates misinformation. Similarly, training AI on biased product reviews or ratings might lead to product suggestions that don’t accurately reflect product quality or user preferences.
- **Model explainability and transparency:** The opaque nature of many generative AI models makes it challenging to understand how they arrive at specific outputs or decisions. For example, if a model is used to generate creative content, such as stories or learning materials, it could be difficult to understand why certain outputs are generated, including potential biases or inappropriate content.
- **Data privacy and security:** Generative AI models are trained on vast amounts of data, which might inadvertently include sensitive or personal information. For example, a model trained to generate text could potentially produce sentences that contain personal details from its training data.

AWS empowers organizations to use this technology responsibly while helping them to align with best practices. As part of enabling organizations to create a comprehensive risk management strategy for generative AI systems, AWS has built the AWS Audit Manager generative AI best practices framework which is mapped to Amazon Bedrock and Amazon SageMaker in AWS Audit Manager.

Amazon Bedrock is a managed service that enables you to create, manage, and scale machine learning (ML) and AI services while facilitating adherence to security and defined compliance requirements. Amazon SageMaker is a fully managed ML service that can build, train, and deploy ML models for extended use cases that require deep customization and model fine-tuning.

You can use this framework to facilitate your auditing and compliance requirements by taking advantage of controls for more responsible, ethical, and effective deployment of generative AI models.

The framework is organized into four pillars, as follows:

- **Data Governance:** Data is the foundation of generative AI models, and the quality and diversity of the training data can significantly impact the model’s performance and output. The Data Governance pillar focuses on facilitating data management practices such as data sourcing, data quality, data privacy, and data bias.
- **Model Development:** This pillar focuses on the responsible development and testing of generative AI models and covers aspects such as model architecture selection, model training, and model evaluation.
- **Model Deployment:** This pillar addresses the challenges associated with deploying generative AI models in production environments and covers aspects such as model deployment strategies, infrastructure considerations, and access controls.
- **Monitoring & Oversight:** This pillar focuses on the ongoing monitoring and governance of generative AI models in production environments and addresses aspects such as model performance monitoring and incident response planning.

You can also use Amazon Bedrock Guardrails to provide an additional level of control on top of the protections built into foundation models (FMs) to help deliver relevant and safe user experiences that align with your organization’s policies and principles.

Each organization’s generative AI journey is unique, influenced by factors such as industry-specific regulations, risk appetite, and scale of generative AI deployment. By integrating the framework with Amazon Bedrock or Amazon SageMaker, you can customize the controls to your organization’s unique needs, aligning your generative AI deployments with your specific risk management strategies. This customization is especially valuable for highly regulated sectors, such as the financial sector.

For example, you can map the risk of inaccurate outputs to controls related to data quality and model validation. Similarly, you can map data security risks to controls related to access management and encryption.

Let’s consider an example that uses a subset of these risks to understand how you could perform this mapping. A financial services firm decides to use generative AI models to develop a chatbot capable of understanding complex customer inquiries and providing accurate and tailored responses for their customer portal. Although chatbots can greatly enhance customer experiences and operational efficiency, they also introduce risks that you need to understand and measure, so that you can develop a corresponding mitigation strategy.

An auditor within the internal audit function of the financial organization would like to use the AWS Audit Manager generative AI best practices framework to assess compliance with the following sample of risks associated with the application:

- **Responsible:** Validating that the chatbot adheres to ethical principles, such as fairness and transparency, and avoids perpetuating biases or discrimination against certain customer segments.
- **Accurate:** Verifying the reliability and accuracy of the chatbot’s responses, particularly when handling sensitive financial information or providing advice on complex financial products.
- **Secure:** Protecting the integrity and security of the data being used to train the generative AI model from unauthorized access and validating that sensitive customer data is segregated from data used for training.

## Example mapping

We’ve provided an example mapping here that illustrates how you can use the framework within Audit Manager to develop a risk management strategy. Based on your individual control objectives and organizational requirements, you can further customize controls, and evidence collection can be automated or manually defined. The example mapping is as follows:

- **Responsible: Implement mechanisms for AI model monitoring and explainability to detect and mitigate potential biases or unfair outcomes.**
    - RESPAI3.8: Document Risks and Tolerances: Define, document, and implement specific controls to address identified risks and organizational risk tolerances.
    - RESPAI3.9: Develop AI RACI: Define organizational roles and responsibilities, lines of communication, and ownership of controls to address identified risks. Ensure that this mapping, measuring, and managing of generative AI risks is clear to individuals and teams throughout the organization.
    - RESPAI3.13: Continuous Risk Monitoring: Periodically perform retrospectives and review policies and procedures to determine if new risks should be considered, and if current risks are addressed based on AI performance, incidents, and user feedback.
    - RESPAI3.15: Ethical Guidelines: Develop and adhere to ethical guidelines for the deployment and usage of generative AI models.
- **Accurate: Implement robust data quality checks, model validation processes, and ongoing monitoring to ensure the accuracy and reliability of the generative AI chatbot’s outputs.**
    - ACCUAI3.4: Regular Audits: Conduct periodic reviews to assess the model’s accuracy over time, especially after system updates or when integrating new data sources.
    - ACCUAI3.6: Source Verification: Ensure that the data source is reputable, reliable, and the data is of high quality.
    - ACCUAI3.14: Quality Data Sourcing: The accuracy of generative AI largely depends on the quality of its training data. Ensure that the data is representative, comprehensive, and free from biases or errors.
- **Secure: Implement robust access controls, data encryption, and security monitoring measures to protect the generative AI chatbot system and training data.**
    - SECAI3.2: Data Encryption In Transit: Implement end-to-end encryption for the input and output data of the AI models to minimum industry standards.
    - SECAI3.3: Data Encryption At Rest: Implement data encryption at rest for data that’s stored to train the AI models, and for the metadata that’s produced by AI models.  
        
        > **Note:** This is an example of a control that can be configured with automated evidence collection using AWS Config as the underlying data source, or further customized with additional data sources according to the scope of the control.
        
    - SECAI3.7: Least Privilege: Document, implement, and enforce least privileged principles when granting access to generative AI systems.
    - SECAI3.8: Periodic Reviews: Document, implement, and enforce periodic reviews of users’ access to generative AI systems.  
        
        > **Note:** This is an example of a control that can be configured with manual evidence collection based on the specific policies and procedures defined by each organization.
        
    - SECAI3.15: Access Logging: Require and enable mechanisms that allow users to request access to generative AI models. Ensure that access requests are properly logged, reviewed, and approved.

## Conclusion

It’s important for institutions, especially those in highly regulated sectors, to proactively address new developments that relate to generative AI. Using the AWS Audit Manager generative AI best practices framework as part of a comprehensive risk management strategy can help you stay ahead of the curve and embrace an agile and responsible approach to generative AI.

The guidance provided by the framework, together with the capabilities of Audit Manager, Amazon Bedrock and SageMaker can help you establish secure and controlled environments for generative AI implementation, automate evidence collection and risk assessments, and monitor and mitigate potential risks. By embracing the potential of generative AI while adhering to best practices, you can position your organization at the forefront of innovation while maintaining the trust and confidence of stakeholders and customers.

If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.

![Kurt Kumar](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/06/kurtkmr.jpg) Kurt Kumar  
Kurt is a Security Consultant at AWS Professional Services and is passionate about helping customers implement secure environments. He has over 14 years of experience in security assurance across audit, risk, and compliance functions and currently holds CISA, CISSP, Associate C|CISO, AWS Security Specialty, AWS Certified AI Practitioner and AWS SysOps Administrator – Associate certifications. Outside of work, he enjoys watching Formula 1 and travelling.

Go to Source
