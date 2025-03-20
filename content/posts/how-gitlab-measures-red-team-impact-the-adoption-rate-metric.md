---
title: "<div>How GitLab measures Red Team impact: The adoption rate metric</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

In early 2024, we started a journey to implement better metrics for our internal Red Team. Our first iteration focused on what we now call the adoption rate metric, which measures how often the recommendations our team makes are accepted and implemented.

Choosing this metric was very deliberate. While there are many ways to measure a Red Team's impact, we wanted to start with something fundamental: Are we actually driving meaningful security improvements? The adoption rate directly ties our work to real security outcomes, and we could measure it using tools and processes we already had in place.

In this article, you'll discover how we used GitLab to track these results end-to-end, some lessons we learned (including what we would have done differently), and our plans to tackle the next set of metrics.

## How we implemented the adoption rate metric

We use GitLab extensively for our Red Team planning, execution, and reporting. Every operation wraps up with a report that's written in markdown in a dedicated GitLab project. Each report contains a section called "Recommendations" with a list of suggestions to make GitLab more secure.

Those recommendations are always linked to a dedicated issue, which we open in the project closest to the team who can address it. If we're suggesting a product feature, it goes directly in that tracker. If it's a detection capability, it goes into the detections as code repository. We always assign a directly responsible individual (DRI) in the group that owns that space, and we use this issue template to ensure consistency in describing the problem, the risk, and potential solutions.

![Red team - recommendation-example](https://images.ctfassets.net/r9o86ar0p03f/73y1KlWLKWijWmoQjPaS9z/6311f485b2a68bd16c64bd79b434c1eb/recommendation-example__1_.png)

Here's where the tracking logistics come in. We use GitLab labels to classify the recommendation across three categories:

- Detections and alerts (`RTRec::Detection`)
- Security controls (`RTRec::Control`)
- Processes and procedures (`RTRec::Process`)

We then use another set of labels to follow the lifecycle of that recommendation – from review all the way through adoption:

- Under review (`RecOutcome::UnderReview`)
- Accepted and actively being worked on (`RecOutcome::InProgress`)
- Accepted but backlogged (`RecOutcome::Backlogged`)
- Accepted but blocked (`RecOutcome::Blocked`)
- Fully adopted and closed (`RecOutcome::Adopted`)
- Partially adopted and closed (`RecOutcome::PartiallyAdopted`)
- Not adopted and closed (`RecOutcome::NotAdopted`)

## How we stay on top of recommendations

We use a new GitLab feature called "GitLab Query Language" (GLQL) to build a dynamic Security Recommendations Dashboard inside a GitLab issue.

This issue allows us to quickly identify things like:

- open recommendations that haven't been updated recently
- open recommendations that have been backlogged for an extended period of time
- closed recommendations that weren't properly labeled with an adoption outcome

We've found this process encourages the Red Team to follow up on stale recommendations, reaching out to the owners and seeing how we can help get them adopted.

GLQL is very cool, and allows us to turn a short code block like this:

```yaml
--- 
display: table  
fields: title, labels("RTRec::*"), labels("RecOutcome::*"), created, updated  
--- 
group = "gitlab-com"  
AND label = "RTRec::*"  
AND opened = true  
```

... into a dynamic table like this:

![Red Team - GLQL table](https://images.ctfassets.net/r9o86ar0p03f/2lPijOJclw45o0Qi6HqdXd/664f11ca26dc8f80e3e1508481ee3abe/glql-table.png)

That table for us is very tactical and we use it to keep things moving. Beyond that, we also visualize the adoption rate trends over time. That allows us to look at things like quarterly adoption rate percentages, how long different types of recommendations take to adopt and implement, and how these figures vary across departments.

## Lessons learned

**1\. Start with metrics in place; don't wait for your program to mature first.**

Early in our Red Team's development, we focused more on how we would execute operations and less on how we would measure them. The idea of using metrics to distill complex operations into simple numbers felt like it might oversimplify our work. But we've learned that thoughtful metrics don't reduce the value of Red Team operations - they help demonstrate our impact and guide our program's growth. Starting with clear metrics earlier would have accelerated this growth.

Implementing these metrics later meant spending significant time reformatting years of historical recommendations to enable consistent analysis. Had we planned for metrics from the start, we could have saved ourselves a lot of time and effort.

We’re keeping this lesson in mind as we start on our next set of metrics, threat resilience, which we talk about below.

**2\. Don't operate in a silo.**

Red Teams aren't the only groups that provide recommendations in a security organization. At GitLab, we have our bug bounty program, our external pentests, product security, security assurance, and security operations.

On the Red Team, we developed our own recommendations process from scratch. It's been fairly effective, but we have noticed some areas for improvement, particularly around prioritization, project management, and alignment with our organization's risk reporting process.

We also noticed that some other teams are really good at these areas such as our bug bounty program and the triaging of findings from our external pentests. Those particular groups are very good at delivering product recommendations, and we've been learning from their approach to improve our own delivery methods.

So we've taken our success with visualizing metrics and are integrating these lessons to create a more standard format that can be used across teams. This will allow us to leverage things that are working well, like our adoption rate metric, and combine them with the more efficiently managed processes used by other groups to ultimately achieve a higher adoption rate and a more secure GitLab.

## Next up: Measuring our threat resilience

Next up for us is implementing metrics around threat resilience. We want to measure how well GitLab can prevent, detect, and respond to the threats most relevant to our organization. We're building a dashboard that will help visualize this data, showing our top threat actors and a series of scores that measure how well we defend against their specific techniques.

Our goal is to have this dashboard drive decisions around what Red Team operations to conduct, what defensive capabilities to improve, and in general where we should be investing time and effort across our entire security division.

We hope to consolidate our existing tools in this process and are currently evaluating solutions. We'll share more info when we've achieved some success here.

## Key takeaways and how to get started

If you're looking to measure your Red Team's impact, here's what we've learned:

1. Start tracking metrics early, even if they're not perfect.
2. Focus on actionable metrics first (like adoption rate).
3. Use your existing tools. We used GitLab and Tableau, but the approach works with any tracking system.
4. Collaborate across security teams to leverage existing processes when possible.

We'd love to hear about your experience with metrics in security so drop a comment below or open an issue in one of our public projects.

## Read more from GitLab's Red Team

- Stealth operations: The evolution of GitLab's Red Team
- How GitLab's Red Team automates C2 testing
- How we run Red Team operations remotely

In early 2024, we started a journey to implement better metrics for our internal Red Team. Our first iteration focused on what we now call the adoption rate metric, which measures how often the recommendations our team makes are accepted and implemented.

Choosing this metric was very deliberate. While there are many ways to measure a Red Team's impact, we wanted to start with something fundamental: Are we actually driving meaningful security improvements? The adoption rate directly ties our work to real security outcomes, and we could measure it using tools and processes we already had in place.

In this article, you'll discover how we used GitLab to track these results end-to-end, some lessons we learned (including what we would have done differently), and our plans to tackle the next set of metrics.

## How we implemented the adoption rate metric

We use GitLab extensively for our Red Team planning, execution, and reporting. Every operation wraps up with a report that's written in markdown in a dedicated GitLab project. Each report contains a section called "Recommendations" with a list of suggestions to make GitLab more secure.

Those recommendations are always linked to a dedicated issue, which we open in the project closest to the team who can address it. If we're suggesting a product feature, it goes directly in that tracker. If it's a detection capability, it goes into the detections as code repository. We always assign a directly responsible individual (DRI) in the group that owns that space, and we use this issue template to ensure consistency in describing the problem, the risk, and potential solutions.

![Red team - recommendation-example](https://images.ctfassets.net/r9o86ar0p03f/73y1KlWLKWijWmoQjPaS9z/6311f485b2a68bd16c64bd79b434c1eb/recommendation-example__1_.png)

Here's where the tracking logistics come in. We use GitLab labels to classify the recommendation across three categories:

- Detections and alerts (`RTRec::Detection`)
- Security controls (`RTRec::Control`)
- Processes and procedures (`RTRec::Process`)

We then use another set of labels to follow the lifecycle of that recommendation – from review all the way through adoption:

- Under review (`RecOutcome::UnderReview`)
- Accepted and actively being worked on (`RecOutcome::InProgress`)
- Accepted but backlogged (`RecOutcome::Backlogged`)
- Accepted but blocked (`RecOutcome::Blocked`)
- Fully adopted and closed (`RecOutcome::Adopted`)
- Partially adopted and closed (`RecOutcome::PartiallyAdopted`)
- Not adopted and closed (`RecOutcome::NotAdopted`)

## How we stay on top of recommendations

We use a new GitLab feature called "GitLab Query Language" (GLQL) to build a dynamic Security Recommendations Dashboard inside a GitLab issue.

This issue allows us to quickly identify things like:

- open recommendations that haven't been updated recently
- open recommendations that have been backlogged for an extended period of time
- closed recommendations that weren't properly labeled with an adoption outcome

We've found this process encourages the Red Team to follow up on stale recommendations, reaching out to the owners and seeing how we can help get them adopted.

GLQL is very cool, and allows us to turn a short code block like this:

```yaml
--- 
display: table  
fields: title, labels("RTRec::*"), labels("RecOutcome::*"), created, updated  
--- 
group = "gitlab-com"  
AND label = "RTRec::*"  
AND opened = true  
```

... into a dynamic table like this:

![Red Team - GLQL table](https://images.ctfassets.net/r9o86ar0p03f/2lPijOJclw45o0Qi6HqdXd/664f11ca26dc8f80e3e1508481ee3abe/glql-table.png)

That table for us is very tactical and we use it to keep things moving. Beyond that, we also visualize the adoption rate trends over time. That allows us to look at things like quarterly adoption rate percentages, how long different types of recommendations take to adopt and implement, and how these figures vary across departments.

## Lessons learned

**1\. Start with metrics in place; don't wait for your program to mature first.**

Early in our Red Team's development, we focused more on how we would execute operations and less on how we would measure them. The idea of using metrics to distill complex operations into simple numbers felt like it might oversimplify our work. But we've learned that thoughtful metrics don't reduce the value of Red Team operations - they help demonstrate our impact and guide our program's growth. Starting with clear metrics earlier would have accelerated this growth.

Implementing these metrics later meant spending significant time reformatting years of historical recommendations to enable consistent analysis. Had we planned for metrics from the start, we could have saved ourselves a lot of time and effort.

We’re keeping this lesson in mind as we start on our next set of metrics, threat resilience, which we talk about below.

**2\. Don't operate in a silo.**

Red Teams aren't the only groups that provide recommendations in a security organization. At GitLab, we have our bug bounty program, our external pentests, product security, security assurance, and security operations.

On the Red Team, we developed our own recommendations process from scratch. It's been fairly effective, but we have noticed some areas for improvement, particularly around prioritization, project management, and alignment with our organization's risk reporting process.

We also noticed that some other teams are really good at these areas such as our bug bounty program and the triaging of findings from our external pentests. Those particular groups are very good at delivering product recommendations, and we've been learning from their approach to improve our own delivery methods.

So we've taken our success with visualizing metrics and are integrating these lessons to create a more standard format that can be used across teams. This will allow us to leverage things that are working well, like our adoption rate metric, and combine them with the more efficiently managed processes used by other groups to ultimately achieve a higher adoption rate and a more secure GitLab.

## Next up: Measuring our threat resilience

Next up for us is implementing metrics around threat resilience. We want to measure how well GitLab can prevent, detect, and respond to the threats most relevant to our organization. We're building a dashboard that will help visualize this data, showing our top threat actors and a series of scores that measure how well we defend against their specific techniques.

Our goal is to have this dashboard drive decisions around what Red Team operations to conduct, what defensive capabilities to improve, and in general where we should be investing time and effort across our entire security division.

We hope to consolidate our existing tools in this process and are currently evaluating solutions. We'll share more info when we've achieved some success here.

## Key takeaways and how to get started

If you're looking to measure your Red Team's impact, here's what we've learned:

1. Start tracking metrics early, even if they're not perfect.
2. Focus on actionable metrics first (like adoption rate).
3. Use your existing tools. We used GitLab and Tableau, but the approach works with any tracking system.
4. Collaborate across security teams to leverage existing processes when possible.

We'd love to hear about your experience with metrics in security so drop a comment below or open an issue in one of our public projects.

## Read more from GitLab's Red Team

- Stealth operations: The evolution of GitLab's Red Team
- How GitLab's Red Team automates C2 testing
- How we run Red Team operations remotely

Go to Source
