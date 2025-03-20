---
title: "<div>Self-service security alert handling with GitLab's UAM</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The GitLab Security Operations team prioritizes automation that enables security engineers to focus on high-impact work rather than routine tasks that can be automated. A key innovation in this automation strategy is creation of the User Attestation Module (UAM), which allows GitLab team members to directly respond to and verify security alerts flagged as potentially malicious. When the GUARD framework detects suspicious activity, it routes the alert to the relevant team member for review. The team member can then attest whether they recognize and authorize the activity. Their response is recorded for audit purposes, and, based on their input, the system either closes the alert or escalates it to the Security Incident Response Team (SIRT).

In this article, you'll learn about the UAM and how it can benefit your DevSecOps environment.

## How the User Attestation Module works

The UAM streamlines security alert handling through a comprehensive workflow that includes:

- Alert verification by team members
- Collection and documentation of supporting evidence
- Option to request additional support from GitLab SecOps
- Secure storage of team member responses
- Automated alert resolution or incident escalation
- Team member feedback collection for continuous improvement

We created UAM to help us:

1. Route low priority alerts (such as administrative activities) to the relevant team members who performed them.
2. Reduce alert fatigue by first checking with the team member who completed the activity before routing to SIRT if necessary.
3. Collect and store team member responses to maintain an audit trail and rich metrics.
4. Create a response tier between **SIRT needs to triage this alert** and **This is an informational signal that does not need to be reviewed directly**.

## UAM's design principles

The UAM is a Slack-first automation that reaches out to team members to validate activity directly in Slack, reducing effort and increasing participation. Today, 40% of all security alerts are delivered to team members through the UAM, saving SIRT valuable time to focus on higher importance alerts and incidents.

A robust escalation workflow in the UAM ensures that all alerts are validated by team members or escalated to SIRT. When a UAM alert reaches a team member, they have a period of time to respond attesting to the activity or stating they do not recognize the activity. If no response is recorded, the UAM alert is auto-escalated to SIRT for handling.

Comprehensive metrics collection is a core GUARD design principle, which extends to how we designed UAM. All user interactivity with triggered UAM alerts is logged in a metrics database, which enables comprehensive measurement to identify problematic alerts, opportunities for process improvement, and overall UAM health.

UAM enables a third alert tier, bridging the gap between alerts that always needed to be investigated, and lower importance informational signals that are grouped by entity for escalation and correlation.

- Stable alerts (must be triaged and investigated by SIRT)
- UAM alerts (routed to team members to attest to the activity)
- Informational signal (low-importance events that are interesting and correlated by entity grouping)

## UAM components

The UAM framework consists of multiple components:

- GitLab: Fetches a user email address based on user\_id via user’s API and stores user's responses - Slack: Searches each user by email using Slack API and posts a UAM notification to the end user as well as collects responses from users using Slack modals
- Tines: Processes and orchestrates user responses and alerts
- Devo: Receives alert payload and alert notifications
- Metrics DB: Records metrics for triggered UAM alerts

The workflow integrates with following modules:

- GitLab API for user identification
- Slack API for user communication
- Webhook configuration for alert reception
- Audit trail storage in GitLab

## UAM workflow

The diagram below illustrates the workflow of the UAM module:

![UAM - flow chart](https://images.ctfassets.net/r9o86ar0p03f/540ED2ZZYEgsio1IWEkC09/7a50bf8361079b9e20f1239550dd9b6f/UAM_detection_edited.png)

## Following along with GUARD

We are still unveiling parts of GUARD and how it works, so follow along to learn how we automate our security detections from end to end.

## Read more about the GUARD framework

- Unveiling the GUARD framework to automate security detections at GitLab
- Automating cybersecurity threat detections with GitLab CI/CD
- Open Source Security at GitLab

The GitLab Security Operations team prioritizes automation that enables security engineers to focus on high-impact work rather than routine tasks that can be automated. A key innovation in this automation strategy is creation of the User Attestation Module (UAM), which allows GitLab team members to directly respond to and verify security alerts flagged as potentially malicious. When the GUARD framework detects suspicious activity, it routes the alert to the relevant team member for review. The team member can then attest whether they recognize and authorize the activity. Their response is recorded for audit purposes, and, based on their input, the system either closes the alert or escalates it to the Security Incident Response Team (SIRT).

In this article, you'll learn about the UAM and how it can benefit your DevSecOps environment.

## How the User Attestation Module works

The UAM streamlines security alert handling through a comprehensive workflow that includes:

- Alert verification by team members
- Collection and documentation of supporting evidence
- Option to request additional support from GitLab SecOps
- Secure storage of team member responses
- Automated alert resolution or incident escalation
- Team member feedback collection for continuous improvement

We created UAM to help us:

1. Route low priority alerts (such as administrative activities) to the relevant team members who performed them.
2. Reduce alert fatigue by first checking with the team member who completed the activity before routing to SIRT if necessary.
3. Collect and store team member responses to maintain an audit trail and rich metrics.
4. Create a response tier between **SIRT needs to triage this alert** and **This is an informational signal that does not need to be reviewed directly**.

## UAM's design principles

The UAM is a Slack-first automation that reaches out to team members to validate activity directly in Slack, reducing effort and increasing participation. Today, 40% of all security alerts are delivered to team members through the UAM, saving SIRT valuable time to focus on higher importance alerts and incidents.

A robust escalation workflow in the UAM ensures that all alerts are validated by team members or escalated to SIRT. When a UAM alert reaches a team member, they have a period of time to respond attesting to the activity or stating they do not recognize the activity. If no response is recorded, the UAM alert is auto-escalated to SIRT for handling.

Comprehensive metrics collection is a core GUARD design principle, which extends to how we designed UAM. All user interactivity with triggered UAM alerts is logged in a metrics database, which enables comprehensive measurement to identify problematic alerts, opportunities for process improvement, and overall UAM health.

UAM enables a third alert tier, bridging the gap between alerts that always needed to be investigated, and lower importance informational signals that are grouped by entity for escalation and correlation.

- Stable alerts (must be triaged and investigated by SIRT)
- UAM alerts (routed to team members to attest to the activity)
- Informational signal (low-importance events that are interesting and correlated by entity grouping)

## UAM components

The UAM framework consists of multiple components:

- GitLab: Fetches a user email address based on user\_id via user’s API and stores user's responses - Slack: Searches each user by email using Slack API and posts a UAM notification to the end user as well as collects responses from users using Slack modals
- Tines: Processes and orchestrates user responses and alerts
- Devo: Receives alert payload and alert notifications
- Metrics DB: Records metrics for triggered UAM alerts

The workflow integrates with following modules:

- GitLab API for user identification
- Slack API for user communication
- Webhook configuration for alert reception
- Audit trail storage in GitLab

## UAM workflow

The diagram below illustrates the workflow of the UAM module:

![UAM - flow chart](https://images.ctfassets.net/r9o86ar0p03f/540ED2ZZYEgsio1IWEkC09/7a50bf8361079b9e20f1239550dd9b6f/UAM_detection_edited.png)

## Following along with GUARD

We are still unveiling parts of GUARD and how it works, so follow along to learn how we automate our security detections from end to end.

## Read more about the GUARD framework

- Unveiling the GUARD framework to automate security detections at GitLab
- Automating cybersecurity threat detections with GitLab CI/CD
- Open Source Security at GitLab

Go to Source
