---
title: "<div>Beautifying our UI: Enhancing GitLab's deployment experience</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

At GitLab, we’ve implemented an innovative approach to improving our experience called Beautifying our UI. This unique initiative pairs one product designer with a frontend engineer for a milestone or two, and empowers them to make self-directed usability improvements across the platform. Ultimately, this helps build a more polished product experience, as these pairs can quickly address pain points, refine interactions, and deliver thoughtful improvements that make the platform more efficient and enjoyable to use.

In this iteration, Anna Vovchenko and I decided to focus on the continuous deployment (CD) area of the product. Here is how we did it and what we learned.

## Trying something new

As this was our second round going through the process, we wanted to make several small adjustments that in the end helped us deliver even more quality improvements to the product. These process improvements included:

- **Extended timeline:** We decided this time around we wanted to extend the initiative to span two milestones. This gave us the time to tackle more complex problems, but also gave us space for additional planning at the start.
- **Structured planning:** While it was encouraged in the past to work directly in merge requests, we found it helped to use the initial issue as a place to plan and seek out problems ahead of time. Rather than purely focusing on the ad-hoc, we incorporated a planning phase similar to milestone planning, helping the partnership identify and prioritize potential improvements beforehand.
- **Product manager integration:** As we focused on one area for this round of the project, we also decided to involve the product manager of the team more actively in the process. This ensured alignment on larger changes, reduced surprises when MRs were merged and allowed us to gather valuable feedback throughout the implementation.
- **Engaging the community:** We expanded our improvement efforts by inviting contributions from community members, accelerating our ability to implement fixes and enhancements across the platform.
- **Strategic timing:** We chose to run this iteration during a traditionally slower period, allowing teams to focus more deeply on these improvements without competing priorities.

These refinements maintained the initiative's core strength of direct designer-engineer collaboration, while adding structure that helped our pair work more effectively.

## What were the main improvements?

During the two milestones, our pairing implemented several significant improvements that enhance the user experience across the CD space. Here's a look at what we accomplished:

### Enhanced environment list view

One of the larger changes made during this cycle of "Beautifying our UI" was a redesigned Environment List page to make deployment information more accessible. Previously, users had to click through collapsible sections to view crucial deployment details, and viewing important details at a glance was difficult. Now, this information is immediately visible, bringing the most important deployment information to the forefront where users need it.

![Beautifying UI - Environments page before](https://images.ctfassets.net/r9o86ar0p03f/3PNIW6wr7PjXj56eIckxYR/118bd0134f6ef21dd4876e50842ba5d6/Before_Environments_Page.png)

**Before:** The original design relied on collapsible sections, requiring users to click to reveal deployment information. This meant that users couldn't immediately see the status of their deployments, making it harder to quickly assess the state of their environments.

![Beautifying UI - Environments page after](https://images.ctfassets.net/r9o86ar0p03f/5IXx6EwEFpIlU5IpvlASUV/ab333877441cf3c2ea8dd722f095bac0/After_Environments_Page.png)

**After:** The new design surfaces critical deployment information directly in the list view, including:

- Deployment status with clear visual indicators
- Who triggered the deployment along with timestamps
- Commit information and version tags
- Actions to take on the environment
- Latest deployment indicators

This redesign eliminates the need for extra clicks and gives users immediate visibility into their deployment and environment statuses. The new layout maintains a clean interface while presenting more actionable information upfront.

### Improved deploy keys filtering

Another larger enhancement was made to our deploy keys interface to improve searchability while maintaining performance. This change addresses a critical user need for quickly finding specific deploy keys in large repositories, which was broken when pagination was introduced earlier last year.

![Beautifying UI - Deploy key before](https://images.ctfassets.net/r9o86ar0p03f/0wZRhTpbwYkWBBLB8HhYn/83724f401fc61a85ea5d7f0cf9ebf1ee/Deploy_Key_Before.png)

**Before:** The previous interface displayed deploy keys in a paginated list without a dedicated search function. While pagination helped with performance when handling thousands of keys, users had lost the ability to quickly search through their deploy keys using the browser search functionality, forcing them to manually scan through multiple pages.

![Beautifying UI - Deploy key after](https://images.ctfassets.net/r9o86ar0p03f/63bgcl0bSSoGQXZW8Dx4Rk/7a6bb7e089be6d297690a29ff5e4b39a/Deploy_Key_After.png)

**After:** The new design introduces a dedicated search field at the top of the deploy keys list, allowing users to:

- Quickly filter deploy keys by name or SHA
- Maintain the performance benefits of pagination
- Find specific keys without browsing through multiple pages

This improvement strikes the right balance between performance and usability, especially beneficial for teams managing numerous deploy keys across multiple projects.

### Better Kubernetes agent management

We made significant improvements to the Kubernetes agent experience by simplifying the registration process and providing better visibility into agent status. These enhancements work together to create a smoother onboarding experience for teams getting started.

Our first area of focus was streamlining how users register agents when they have configuration files ready to use. Previously, this process had several pain points that we wanted to address.

![Beautifying UI - Agent before](https://images.ctfassets.net/r9o86ar0p03f/4nQ26pVg2pullsURbdUuUO/2ba71a65e0ff6227c611e806daeb4c5f/Agent_Before.png)

**Before:**

- Only showed connected and previously connected agents
- Connection status was limited to "Never connected" or "Not connected"
- No clear path to register new agents

![Beautifying UI - Agent after](https://images.ctfassets.net/r9o86ar0p03f/1khmWXmfgnZxLkImL98jqX/5d50e88e8398b292af8159c25c22865d/Agent_After.png)

**After:**

- Added a new Available configurations tab showing all potential agent configurations
- Clear "Register an agent" call-to-action button for each available configuration

Next, we turned our attention to making the agent registration modal more intuitive. The previous design created some confusion that we wanted to resolve.

![Beautifying UI - Registration before](https://images.ctfassets.net/r9o86ar0p03f/6UGHFXFRTWtMEOMRNnMC3n/308c56bff1ff25512962c9fa2a78ca38/Registration_Before.png)

**Before:**

- Users faced a confusing dual-purpose search box that both found existing agents and created new ones
- The workflow had too many decision points instead of a clear path forward
- The process for creating vs. selecting an agent wasn't clearly separated

![Beautifying UI - Registration after](https://images.ctfassets.net/r9o86ar0p03f/5IqK5YIFaVuVXh2p1zMf4t/575dfe8a2827e61204fe8322639c1294/Registration_After.png)

**After:**

- Separated the interface into two clear options: bootstrap with Flux or create an agent through the UI
- Streamlined the workflow into a more linear process
- Made the distinction between creating new agents and selecting existing ones more obvious
- Added a success message that clearly shows where to create the optional config file

These improvements make it immediately clear which agents need attention and provide a straightforward path to register new agents. The reorganized interface better supports both new users setting up their first agent and experienced users managing multiple agents.

## Additional usability enhancements

While working on major interface improvements, we also addressed several focused usability issues that significantly improve the day-to-day experience:

- **Enhanced Kubernetes pod search:** Added search functionality for Kubernetes pods on the environment page, making it easier to locate specific pods in large deployments. This was showcased in the GitLab 17.8 release post.
- **Improved Flux status visibility:** Added a "stopped" badge to the dashboard view when Flux sync is stopped, providing immediate visibility into sync status. This was also showcased in the GitLab 17.8 release post.
- **Better release information:** Implemented a clear view of deployments related to a release, improving deployment tracking and visibility.
- **Streamlined environment search:** Fixed an issue where users couldn't effectively search the Environments page, improving navigation in large environment lists.
- **Enhanced error message display:** Resolved issues with viewing Flux details when long error messages were present, making troubleshooting more straightforward.

## Looking forward

The success of these improvements demonstrates the value of empowering our teams to make direct, meaningful changes to our experience. Beyond the product enhancements, one of the most valuable outcomes has been the strengthened relationship between our Frontend and Design teams. Working together closely on these improvements has fostered better understanding of each other's perspectives, workflows, and constraints, leading to more effective collaboration.

This deepened partnership has created a foundation for even better collaboration in our regular workflow, as team members now have stronger working relationships and shared understanding of each other's domains. We're excited to continue this initiative in future iterations, not just for the product improvements it generates, but also for its role in building stronger, more cohesive teams.

> Follow along with the "Beautifying our UI" project as we continue to make improvements to GitLab.

## Read more

- How we overhauled GitLab navigation
- GitLab dark mode is getting a new look
- Beautifying our UI: Giving GitLab build features a fresh look

At GitLab, we’ve implemented an innovative approach to improving our experience called Beautifying our UI. This unique initiative pairs one product designer with a frontend engineer for a milestone or two, and empowers them to make self-directed usability improvements across the platform. Ultimately, this helps build a more polished product experience, as these pairs can quickly address pain points, refine interactions, and deliver thoughtful improvements that make the platform more efficient and enjoyable to use.

In this iteration, Anna Vovchenko and I decided to focus on the continuous deployment (CD) area of the product. Here is how we did it and what we learned.

## Trying something new

As this was our second round going through the process, we wanted to make several small adjustments that in the end helped us deliver even more quality improvements to the product. These process improvements included:

- **Extended timeline:** We decided this time around we wanted to extend the initiative to span two milestones. This gave us the time to tackle more complex problems, but also gave us space for additional planning at the start.
- **Structured planning:** While it was encouraged in the past to work directly in merge requests, we found it helped to use the initial issue as a place to plan and seek out problems ahead of time. Rather than purely focusing on the ad-hoc, we incorporated a planning phase similar to milestone planning, helping the partnership identify and prioritize potential improvements beforehand.
- **Product manager integration:** As we focused on one area for this round of the project, we also decided to involve the product manager of the team more actively in the process. This ensured alignment on larger changes, reduced surprises when MRs were merged and allowed us to gather valuable feedback throughout the implementation.
- **Engaging the community:** We expanded our improvement efforts by inviting contributions from community members, accelerating our ability to implement fixes and enhancements across the platform.
- **Strategic timing:** We chose to run this iteration during a traditionally slower period, allowing teams to focus more deeply on these improvements without competing priorities.

These refinements maintained the initiative's core strength of direct designer-engineer collaboration, while adding structure that helped our pair work more effectively.

## What were the main improvements?

During the two milestones, our pairing implemented several significant improvements that enhance the user experience across the CD space. Here's a look at what we accomplished:

### Enhanced environment list view

One of the larger changes made during this cycle of "Beautifying our UI" was a redesigned Environment List page to make deployment information more accessible. Previously, users had to click through collapsible sections to view crucial deployment details, and viewing important details at a glance was difficult. Now, this information is immediately visible, bringing the most important deployment information to the forefront where users need it.

![Beautifying UI - Environments page before](https://images.ctfassets.net/r9o86ar0p03f/3PNIW6wr7PjXj56eIckxYR/118bd0134f6ef21dd4876e50842ba5d6/Before_Environments_Page.png)

**Before:** The original design relied on collapsible sections, requiring users to click to reveal deployment information. This meant that users couldn't immediately see the status of their deployments, making it harder to quickly assess the state of their environments.

![Beautifying UI - Environments page after](https://images.ctfassets.net/r9o86ar0p03f/5IXx6EwEFpIlU5IpvlASUV/ab333877441cf3c2ea8dd722f095bac0/After_Environments_Page.png)

**After:** The new design surfaces critical deployment information directly in the list view, including:

- Deployment status with clear visual indicators
- Who triggered the deployment along with timestamps
- Commit information and version tags
- Actions to take on the environment
- Latest deployment indicators

This redesign eliminates the need for extra clicks and gives users immediate visibility into their deployment and environment statuses. The new layout maintains a clean interface while presenting more actionable information upfront.

### Improved deploy keys filtering

Another larger enhancement was made to our deploy keys interface to improve searchability while maintaining performance. This change addresses a critical user need for quickly finding specific deploy keys in large repositories, which was broken when pagination was introduced earlier last year.

![Beautifying UI - Deploy key before](https://images.ctfassets.net/r9o86ar0p03f/0wZRhTpbwYkWBBLB8HhYn/83724f401fc61a85ea5d7f0cf9ebf1ee/Deploy_Key_Before.png)

**Before:** The previous interface displayed deploy keys in a paginated list without a dedicated search function. While pagination helped with performance when handling thousands of keys, users had lost the ability to quickly search through their deploy keys using the browser search functionality, forcing them to manually scan through multiple pages.

![Beautifying UI - Deploy key after](https://images.ctfassets.net/r9o86ar0p03f/63bgcl0bSSoGQXZW8Dx4Rk/7a6bb7e089be6d297690a29ff5e4b39a/Deploy_Key_After.png)

**After:** The new design introduces a dedicated search field at the top of the deploy keys list, allowing users to:

- Quickly filter deploy keys by name or SHA
- Maintain the performance benefits of pagination
- Find specific keys without browsing through multiple pages

This improvement strikes the right balance between performance and usability, especially beneficial for teams managing numerous deploy keys across multiple projects.

### Better Kubernetes agent management

We made significant improvements to the Kubernetes agent experience by simplifying the registration process and providing better visibility into agent status. These enhancements work together to create a smoother onboarding experience for teams getting started.

Our first area of focus was streamlining how users register agents when they have configuration files ready to use. Previously, this process had several pain points that we wanted to address.

![Beautifying UI - Agent before](https://images.ctfassets.net/r9o86ar0p03f/4nQ26pVg2pullsURbdUuUO/2ba71a65e0ff6227c611e806daeb4c5f/Agent_Before.png)

**Before:**

- Only showed connected and previously connected agents
- Connection status was limited to "Never connected" or "Not connected"
- No clear path to register new agents

![Beautifying UI - Agent after](https://images.ctfassets.net/r9o86ar0p03f/1khmWXmfgnZxLkImL98jqX/5d50e88e8398b292af8159c25c22865d/Agent_After.png)

**After:**

- Added a new Available configurations tab showing all potential agent configurations
- Clear "Register an agent" call-to-action button for each available configuration

Next, we turned our attention to making the agent registration modal more intuitive. The previous design created some confusion that we wanted to resolve.

![Beautifying UI - Registration before](https://images.ctfassets.net/r9o86ar0p03f/6UGHFXFRTWtMEOMRNnMC3n/308c56bff1ff25512962c9fa2a78ca38/Registration_Before.png)

**Before:**

- Users faced a confusing dual-purpose search box that both found existing agents and created new ones
- The workflow had too many decision points instead of a clear path forward
- The process for creating vs. selecting an agent wasn't clearly separated

![Beautifying UI - Registration after](https://images.ctfassets.net/r9o86ar0p03f/5IqK5YIFaVuVXh2p1zMf4t/575dfe8a2827e61204fe8322639c1294/Registration_After.png)

**After:**

- Separated the interface into two clear options: bootstrap with Flux or create an agent through the UI
- Streamlined the workflow into a more linear process
- Made the distinction between creating new agents and selecting existing ones more obvious
- Added a success message that clearly shows where to create the optional config file

These improvements make it immediately clear which agents need attention and provide a straightforward path to register new agents. The reorganized interface better supports both new users setting up their first agent and experienced users managing multiple agents.

## Additional usability enhancements

While working on major interface improvements, we also addressed several focused usability issues that significantly improve the day-to-day experience:

- **Enhanced Kubernetes pod search:** Added search functionality for Kubernetes pods on the environment page, making it easier to locate specific pods in large deployments. This was showcased in the GitLab 17.8 release post.
- **Improved Flux status visibility:** Added a "stopped" badge to the dashboard view when Flux sync is stopped, providing immediate visibility into sync status. This was also showcased in the GitLab 17.8 release post.
- **Better release information:** Implemented a clear view of deployments related to a release, improving deployment tracking and visibility.
- **Streamlined environment search:** Fixed an issue where users couldn't effectively search the Environments page, improving navigation in large environment lists.
- **Enhanced error message display:** Resolved issues with viewing Flux details when long error messages were present, making troubleshooting more straightforward.

## Looking forward

The success of these improvements demonstrates the value of empowering our teams to make direct, meaningful changes to our experience. Beyond the product enhancements, one of the most valuable outcomes has been the strengthened relationship between our Frontend and Design teams. Working together closely on these improvements has fostered better understanding of each other's perspectives, workflows, and constraints, leading to more effective collaboration.

This deepened partnership has created a foundation for even better collaboration in our regular workflow, as team members now have stronger working relationships and shared understanding of each other's domains. We're excited to continue this initiative in future iterations, not just for the product improvements it generates, but also for its role in building stronger, more cohesive teams.

> Follow along with the "Beautifying our UI" project as we continue to make improvements to GitLab.

## Read more

- How we overhauled GitLab navigation
- GitLab dark mode is getting a new look
- Beautifying our UI: Giving GitLab build features a fresh look

Go to Source
