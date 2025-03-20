---
title: "New Boards Hub Rollout Expectations"
date: 2025-01-07
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
tags: 
  - "agile"
  - "azure-cloud"
  - "community"
---

Although the process may seem slow, we are steadily progressing toward rolling out the New Boards Hub to all customers. Our plan is to deprecate the old Boards experience for all Azure DevOps service users by the end Q1 2025.

The rollout is advancing on two fronts. First, we are setting the New Boards Hub as the default experience. Second, for customers who already have the New Boards Hub enabled as the default, we are transitioning groups to exclusively use the New Boards Hub. Removing the option to revert to the old Boards experience.

Currently, 60% of customers have the New Boards Hub set as their default experience. For 20% of customers, the New Boards Hub is now the only option, with no ability to revert to the old experience.

![Image new boards 1](https://devblogs.microsoft.com/devops/wp-content/uploads/sites/6/2024/12/new-boards-1.png)

## ![🚀](https://s.w.org/images/core/emoji/15.0.3/72x72/1f680.png) How we release updates

Before diving into the details of rolling out the New Boards Hub, it’s helpful to explain how Azure DevOps releases updates and features to customers. We use a ring-based release process, dividing Azure DevOps customers into six distinct rings.

The process begins with **Ring 0**, our canary ring, which is exclusively used by the Azure DevOps engineering team. **Ring 1** includes early adopter customers who receive new features early and aggressively share feedback to help shape features. Each subsequent ring contains a progressively larger group of customers, ending in **Ring 5**, which includes the broadest audience.

This ring-based approach ensures that updates are thoroughly tested as they progress through each ring. By the time updates reach Ring 5, they have undergone extensive usage and testing in earlier rings, allowing us to identify and address issues. As a result, problems rarely make it to Rings 4 and 5.

## ![⏳](https://s.w.org/images/core/emoji/15.0.3/72x72/23f3.png) Why is the rollout taking so long?

As we enable the New Boards Hub for each new ring, customers often identify edge cases and provide valuable feedback on issues that need to be addressed. To ensure a smooth transition, we allow the New Boards Hub to “bake” for a period of time with each ring. This gives us the opportunity to thoroughly address customer feedback and resolve any issues before moving to the next ring.

While this process is time-consuming, it allows us to be deliberate and minimizes the potential disruption customers might experience if the rollout were more aggressive.

## ![🤚](https://s.w.org/images/core/emoji/15.0.3/72x72/1f91a.png) Can I turn New Boards Hub off?

It’s important to emphasize that the old Boards experience is on a path to deprecation, to be fully replaced by the New Boards Hub. The timing of this transition depends on the ring your organization is in.

For customers in Rings 1 and 2, reverting to the old Boards is no longer an option. For those who still have the ability to revert, we encourage you to remain on the New Boards and share your feedback with us. Doing so not only ensures your input helps shape the experience but also makes the transition smoother for you and your team.

![🔗](https://s.w.org/images/core/emoji/15.0.3/72x72/1f517.png) Learn more about the features exclusive to the New Boards Hub.

## ![⏭](https://s.w.org/images/core/emoji/15.0.3/72x72/23ed.png) What’s next?

Starting in early January 2025, we will make the New Boards Hub the only experience for the next set of customers that reside in Ring 3. At the same time, we will enable the New Boards Hub as the default experience for the remaining 40% of customers that are in Ring 5.

As with previous stages, we will allow a short period for feedback and issue resolution to ensure a smooth transition. However, our goal is to fully convert all customers to the New Boards Hub, removing the option to revert, by April of 2025.

## ![🐞](https://s.w.org/images/core/emoji/15.0.3/72x72/1f41e.png) What happens if I find an issue?

If you find a problem, please don’t turn off the New Boards Hub. Report the issue via Feedback Ticket or email me directly. We need to know if there is an issue so we can fix it. All feedback is appreciated.

The post New Boards Hub Rollout Expectations appeared first on Azure DevOps Blog.

Go to Source
