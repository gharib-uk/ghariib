---
title: "<div>Improve AI security in GitLab with composite identities</div>"
date: 2025-02-01
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Artificial intelligence (AI) is quickly becoming the backbone of modern software development, fueling developer efficiency and accelerating innovation. With the emergence of AI agents implementing code based on instructions from humans, we are learning that implementing AI-based features has its own unique set of security challenges. **How do we protect access to the resources AI needs, protect confidentiality, and avoid privilege escalation**? Few organizations are ready to answer these questions today. At GitLab, we are. We are introducing a new paradigm for identity management: composite identities.

When AI agents are integrated into your DevSecOps workflows, previously simple questions become difficult to answer: Who authored this code? Who is the author of this merge request? Who created this Git commit? We found we had to start asking new questions: Who instructed an AI agent to generate this code? What context did AI need to build this feature? What were the resources AI had to access and read to generate the answer?

To answer these questions, we need to understand some fundamental aspects of AI’s identity:

- Does an AI agent have its own distinct identity?
- What is the representation of this identity?
- How do we make it all secure?

### Authentication and AI identity management

We are at the beginning of a paradigm shift in identity management in the software delivery lifecycle. Before the AI era, identity management was simpler. We had human user-based identities and machine-type identities using separate accounts.

With the emergence of AI and agentic workflows, the distinction between these two core types of identities has blurred. AI agents are supposed to work in an autonomous way, so it makes sense to think about them as machine-type accounts. On the other hand, AI agents are usually being instructed by human users, and require access to resources the human users have access to in order to complete their tasks. This introduces significant security risks — for example, the AI may provide human users with information they should not have access to. How do we avoid privilege escalation, provide auditability, and protect confidentiality in a world with AI agents?

### The solution: Composite identities

A composite identity is our new identity principal, representing an AI agent’s identity that is linked with the identity of a human user who requests actions from the agent. **This enhances our ability to protect resources stored in GitLab**. Whenever an AI agent with a composite identity attempts to access a resource, we will not only authenticate the agent itself, but also link its principal with a human user who is instructing the agent, and will try to authorize both principals before granting access to a resource. Both principals need access; otherwise, the access will be denied. If an AI agent by itself can access a project, but a human user who instructed the agent to do so cannot, GitLab will deny the access.

The inverse is true as well — if a human user can access a confidential issue, but an AI agent can’t, then its service account will not be able to read the issue. We authorize access to every API request and for each resource an agent attempts to access this way. Composite identity without a request-scoped link to a human account will not be authorized to access any resource. For fully autonomous workloads we are also considering adding support for linking composite identities with other principals.

#### Composite identity and service accounts

We redesigned our authorization framework to support composite identities, allowing multiple principals to be evaluated simultaneously when determining access rights to a resource. We enhanced our security infrastructure by implementing scoped identities across our entire system — from API requests to CI jobs and backend workers. These identities are linked to an AI agent's composite identity account also through OAuth tokens and CI job tokens. This project yielded unexpected security benefits, particularly in GitLab CI, where we upgraded job tokens to signed JSON web tokens (JWTs). Additionally, we contributed code to several open source libraries to add support for scoped identities.

### Composite identity with GitLab Duo with Amazon Q

In the GitLab 17.8 release, we made composite identity for service accounts support available for customers through our GitLab Duo with Amazon Q integration. Amazon Q Developer agent will have composite identity enforced, which will protect your confidential GitLab resources from unauthorized access.

### What’s next?

To learn more, check out our composite identity docs.

Artificial intelligence (AI) is quickly becoming the backbone of modern software development, fueling developer efficiency and accelerating innovation. With the emergence of AI agents implementing code based on instructions from humans, we are learning that implementing AI-based features has its own unique set of security challenges. **How do we protect access to the resources AI needs, protect confidentiality, and avoid privilege escalation**? Few organizations are ready to answer these questions today. At GitLab, we are. We are introducing a new paradigm for identity management: composite identities.

When AI agents are integrated into your DevSecOps workflows, previously simple questions become difficult to answer: Who authored this code? Who is the author of this merge request? Who created this Git commit? We found we had to start asking new questions: Who instructed an AI agent to generate this code? What context did AI need to build this feature? What were the resources AI had to access and read to generate the answer?

To answer these questions, we need to understand some fundamental aspects of AI’s identity:

- Does an AI agent have its own distinct identity?
- What is the representation of this identity?
- How do we make it all secure?

### Authentication and AI identity management

We are at the beginning of a paradigm shift in identity management in the software delivery lifecycle. Before the AI era, identity management was simpler. We had human user-based identities and machine-type identities using separate accounts.

With the emergence of AI and agentic workflows, the distinction between these two core types of identities has blurred. AI agents are supposed to work in an autonomous way, so it makes sense to think about them as machine-type accounts. On the other hand, AI agents are usually being instructed by human users, and require access to resources the human users have access to in order to complete their tasks. This introduces significant security risks — for example, the AI may provide human users with information they should not have access to. How do we avoid privilege escalation, provide auditability, and protect confidentiality in a world with AI agents?

### The solution: Composite identities

A composite identity is our new identity principal, representing an AI agent’s identity that is linked with the identity of a human user who requests actions from the agent. **This enhances our ability to protect resources stored in GitLab**. Whenever an AI agent with a composite identity attempts to access a resource, we will not only authenticate the agent itself, but also link its principal with a human user who is instructing the agent, and will try to authorize both principals before granting access to a resource. Both principals need access; otherwise, the access will be denied. If an AI agent by itself can access a project, but a human user who instructed the agent to do so cannot, GitLab will deny the access.

The inverse is true as well — if a human user can access a confidential issue, but an AI agent can’t, then its service account will not be able to read the issue. We authorize access to every API request and for each resource an agent attempts to access this way. Composite identity without a request-scoped link to a human account will not be authorized to access any resource. For fully autonomous workloads we are also considering adding support for linking composite identities with other principals.

#### Composite identity and service accounts

We redesigned our authorization framework to support composite identities, allowing multiple principals to be evaluated simultaneously when determining access rights to a resource. We enhanced our security infrastructure by implementing scoped identities across our entire system — from API requests to CI jobs and backend workers. These identities are linked to an AI agent's composite identity account also through OAuth tokens and CI job tokens. This project yielded unexpected security benefits, particularly in GitLab CI, where we upgraded job tokens to signed JSON web tokens (JWTs). Additionally, we contributed code to several open source libraries to add support for scoped identities.

### Composite identity with GitLab Duo with Amazon Q

In the GitLab 17.8 release, we made composite identity for service accounts support available for customers through our GitLab Duo with Amazon Q integration. Amazon Q Developer agent will have composite identity enforced, which will protect your confidential GitLab resources from unauthorized access.

### What’s next?

To learn more, check out our composite identity docs.

Go to Source
