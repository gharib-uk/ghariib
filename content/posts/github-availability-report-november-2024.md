---
title: "GitHub Availability Report: November 2024"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

In November, we experienced one incident that resulted in degraded performance across GitHub services.

The post GitHub Availability Report: November 2024 appeared first on The GitHub Blog.

In November, we experienced one incident that resulted in degraded performance across GitHub services.

**November 19 10:56 UTC (lasting 1 hour and 7 minutes)**

On November 19, 2024, between 10:56:00 UTC and 12:03:00 UTC, the notifications service was degraded and stopped sending notifications to all our dotcom customers. On average, notification delivery was delayed by about one hour. This was due to a database host coming out of a regular maintenance process in read-only mode.

We mitigated the incident by making the host writable again. Following this, notification delivery recovered, and any delivery jobs that had failed during the incident were successfully retried. All notifications were fully recovered and available to users by 12:36 UTC.

To prevent similar incidents moving forward, we are working to improve our observability across database clusters to reduce our time to detection and improve our system resilience on startup.

* * *

Please follow our status page for real-time updates on status changes and post-incident recaps. To learn more about what we’re working on, check out the GitHub Engineering Blog.

The post GitHub Availability Report: November 2024 appeared first on The GitHub Blog.

Go to Source
