---
title: "How Red Hat and Dynatrace intelligently automate your production environment"
date: 2025-01-07
categories: 
  - "ci-cd"
  - "devops"
  - "devops-cloud"
  - "linux"
  - "open-source"
tags: 
  - "ansible"
  - "dynatrace"
  - "dynatrace-version-1-291"
  - "problem-remediation"
  - "red-hat"
---

![Red Hat and Dynatrace integration](https://dt-cdn.net/wp-content/uploads/2024/05/Blog_-OTP-0100_-high-res-version-300x169.png)

Red Hat and Dynatrace are joining forces to revolutionize day-one and day-two operations. A tight integration between Red Hat Ansible Automation Platform, Dynatrace Davis® AI, and the Dynatrace observability and security platform enables closed-loop remediation to automate the process from:

- Detecting a problem
- Managing incidents in corresponding tools
- Identifying the root cause and proper countermeasures
- Executing corrective actions
- Keeping relevant stakeholders in the loop

This way, disruptions are minimized, MTTR is significantly decreased, and DevSecOps and SREs collaborate efficiently to boost productivity.

## Problem remediation is too time-consuming

According to the DevOps Automation Pulse Survey 2023, on average, a software engineer takes nine hours to remediate a problem within a production application. This is too long if you’re in a highly competitive market with 15,000 cargo bookings per hour, where each downtime costs you up to USD 5 million per hour, or if a critical zero-day vulnerability is actively being exploited.

<figure>

![Challenges organizations face in using observability and security data to drive automation.](https://dt-cdn.net/wp-content/uploads/2024/05/P1-Challenges-organizations-faces-in-using-oberservability-data.png)

<figcaption>

Figure 1. Challenges organizations face in using observability and security data to drive automation.

</figcaption>

</figure>

With 85% of organizations facing challenges using observability and security data to drive problem remediation and DevOps automation \[1\], overcoming the burden of siloed data is imperative. Turning data into actions and precisely mapping problems, root causes, and entities to remediation scenarios is critical.

## Red Hat and Dynatrace integration overview

The strategic partnership and integration between Red Hat and Dynatrace are game changers that solve each mentioned pain point:

- Easily ingest (and gain precise insights into) your logs, metrics, traces, and business data.
- In-context topology identification.
- A holistic view of your data and environments with Grail™ data lakehouse.
- Davis AI root cause analysis is used to pinpoint the problem, entity, and root cause.
- Davis AI predictive analysis can be used to decrease downtime by remediating problems before they hit production.
- Integration with Red Hat Automation Controller to trigger job templates for automation and remediation, retrieve job status for smart post-checks and rerun a job if it fails.
- Integration with Red Hat Event-Driven-Ansible will also leverage Red Hat’s flexible rulebook system to map event data, such as problem categories or vulnerability identification, to the correct job template.

Two implementation scenarios cover different use cases and needs.

<figure>

![Integration options of Red Hat and Dynatrace to drive automation and remediation with context. ](https://dt-cdn.net/wp-content/uploads/2024/05/P2-Red-Hat-and-Dynatrace-integration-overview.png)

<figcaption>

Figure 2: Integration options of Red Hat and Dynatrace to drive automation and remediation with context.

</figcaption>

</figure>

## Example implementation scenario #1

The diagram below illustrates configuration-event-based remediation with Dynatrace and Red Hat Ansible Automation Controller for a failing canary release.

Any change in an environment, like the deployment of a new service version, the activation of a feature flag, or, as in the example below, a faulty canary release, is accompanied by a configuration change event. A developer or SRE embeds the config event in the deployment pipeline, which includes the job description, the source, and the corresponding Red Hat Ansible job template to remediate or roll back the change.

Dynatrace Davis AI identifies the problem and maps the configuration change event to the root cause and the correct entity.

<figure>

![Dynatrace Davis AI recognizes a failure rate increase after a faulty canary release and maps the configuration event to the root cause. ](https://dt-cdn.net/wp-content/uploads/2024/05/P3-Problem-w-configuration-change-event-1.png)

<figcaption>

Figure 3. Dynatrace Davis AI recognizes a failure rate increase after a faulty canary release and maps the configuration event to the root cause.

</figcaption>

</figure>

Dynatrace workflows app orchestrates the auto-remediation with Red Hat Ansible Automation Platform, incident management with tools like ServiceNow, and messaging via Slack, Microsoft Teams, or Email to reduce downtime and MTTR:

- Davis AI problem trigger for workflows allows for the setting of a fine-grained filter to run a workflow for an event with an active state, error category, filters for additional tags like release stage or environment, and an identified root cause entity.
- Ownership information is leveraged to inform the correct team via Slack (left side of the workflow).
- A ServiceNow ticket is created and updated with remediation details for compliance and auditing purposes.
- The Red Hat Ansible Automation Controller workflow action triggers the corresponding job template identified via Davis AI problem event. It so initiates the remediation scenario to reset the canary weighting.
- ExtraVars optionally allows the handing over of parameters for the Ansible job template execution.
- Remediation details are linked to the problem in Dynatrace and documented in ServiceNow.

<figure>

![Dynatrace workflows leverage Davis AI insights to trigger the corresponding job template in Red Hat Ansible Automation Controller.](https://dt-cdn.net/wp-content/uploads/2024/05/P4-Workflow-triggering-job-template.png)

<figcaption>

Figure 4. Dynatrace workflows leverage Davis AI insights to trigger the corresponding job template in Red Hat Ansible Automation Controller.

</figcaption>

</figure>

Within Red Hat Ansible Automation Controller, the corresponding job template remediates the problem: in this example, the canary weighting is reset. A second validation workflow is triggered after a specific interval or when the problem is closed to validate the Ansible job status, relaunch the job if an error occurs, inform stakeholders, and update or close the ServiceNow incident.

<figure>

![The job template remediates the problem; a second workflows validates the status after certain time or when the problem is closed.](https://dt-cdn.net/wp-content/uploads/2024/05/P5-Closing-ticket-and-Job-overview.png)

<figcaption>

Figure 5. The job template remediates the problem; a second workflow validates the status after a certain time or when the problem is closed.

</figcaption>

</figure>

## Example implementation scenario #2

Flexible rulebook-based remediation with Dynatrace and Red Hat Event-Driven-Ansible

- The integration of Dynatrace workflows with Red Hat Event-Driven Ansible provides the highest flexibility to remediate detected problems or to mitigate security vulnerabilities:
- Dynatrace Query Language (DQL) is a powerful tool for exploring data and discovering patterns, identifying anomalies and outliers, creating statistical modeling, and more based on data stored in Dynatrace Grail storage. With DQL, the workflow trigger to initiate a required automation and remediation process can be defined. The event payload contains problem details, vulnerability details, root causes, and affected entities.
- Beyond anomalies and root cause analysis for existent problems, Dynatrace Davis predictive and causal AI can forecast situations with high load and initiate change and remediation scenarios before downtime occurs.
- With Dynatrace Ownership information, relevant stakeholders can be easily identified and informed, and high-risk security or infrastructure problems can be escalated. Context-rich tickets can be created in systems like Jira or ServiceNow for traceability and compliance.
- Dynatrace Workflows app allows data enrichment according to all needs, filters for relevant security vulnerabilities or problems, informs the correct users, and hands relevant data to the Red Hat Ansible Automation Platform.
- Red Hat Event-Driven Ansible capability leverages rulebooks, which provide a simple yet powerful event source and category mapping to the correct playbook. With that, Software engineers, SREs, and DevOps can define a broad automation and remediation mapping.
- The remediation template can foresee many options, like a service restart, a faulty deployment rollback, a vulnerable component patch, or a preemptive up or down-scaling mechanism to prevent problems before they occur or to save costs when the load is expected to decrease significantly.

<figure>

![The workflow leverages Davis predictive analysis, informs relevant stakeholders and enriches data being sent to Event Driven Ansible for exact rulebook to playbook matching.](https://dt-cdn.net/wp-content/uploads/2024/05/P6-Workflow-trigger-Red-Hat-Event-Driven-Ansible.png)

<figcaption>

Figure 6: The workflow leverages Davis predictive analysis, informs relevant stakeholders, and enriches data being sent to Event Driven Ansible for the exact rulebook to playbook matching.

</figcaption>

</figure>

Combined with the Dynatrace Collection for Red Hat Automation Platform,  Ansible rulebooks provide the framework for Event-Driven Ansible automation. With a simple yet powerful mapping structure, incoming event categories and types like failure events, custom alerts based on metric thresholds for CPU usage or disk usage can be mapped to the corresponding job template for flexible infrastructure automation and remediation.

<figure>

![Red Hat Event Driven Ansible provides the highest flexibility by mapping problems to remediation scenarios with rulebooks.](https://dt-cdn.net/wp-content/uploads/2024/05/P7-Red-Hat-Event-Driven-Ansible-Controller-and-rulebook.png)

<figcaption>

Figure 7: Red Hat Event Driven Ansible provides the highest flexibility by mapping problems to remediation scenarios with rulebooks.

</figcaption>

</figure>

## Are you a Dynatrace Managed customer? Don’t worry we’ve got you covered!

- Dynatrace provides Full-Stack Monitoring insights into your complete IT operation and automatically detects if any part of your deployment doesn’t fulfill the required quality in terms of performance or error rates. Whenever Dynatrace detects such abnormal system behavior, it creates a single problem that contains all incidents that share the same root cause, which can be configured to trigger an Ansible Automation Controller job template.
- The Red Hat Event Source plugin for Dynatrace enables Dynatrace Managed customers to combine Davis AI anomaly detection and root cause analysis with Red Hat Event Driven Ansible. Download the Dynatrace collection from the Red Hat Ansible Automation Hub and follow the installation guide. The Dynatrace Managed plugin pulls data from Dynatrace every 10 min, filters for open events and available root cause and prevents duplication by adding comments to each event processed.

<figure>

![Exemplary use case-based overview for SaaS and managed customers.](https://dt-cdn.net/wp-content/uploads/2024/05/P8-Saas-and-Managed-Overview.png)

<figcaption>

Figure 8: Exemplary use case-based overview for Dynatrace SaaS and Dynatrace Managed customers.

</figcaption>

</figure>

## What’s next

Collection for closed-loop communication will improve E2E-remediation scenarios:

- Retrieve additional data from Dynatrace within your Ansible playbook; for example, retrieve a Java process ID to restart the correct service, query Dynatrace Grail with DQL, or validate the problem status after a job execution.
- Post comments or events to Dynatrace to track playbook executions or trigger a Dynatrace workflow.

Which implementation scenario is the right one for you? For more details, see the documentation, sign up for a trial, and download examples from the sample repository. There, you’ll find the workflow definition, a notebook that guides you through the setup and ingest of a configuration event, and a simulation event to trigger the workflow. Got any more questions? Leave your comments in our Community or upload your examples to the sample repository.

Get started by downloading samples from our workflow samples repository.  

Download collection here

The post How Red Hat and Dynatrace intelligently automate your production environment appeared first on Dynatrace news.

Go to Source
