---
title: "GitHub Availability Report: February 2025"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

In February, we experienced two incidents that resulted in degraded performance across GitHub services.

The post GitHub Availability Report: February 2025 appeared first on The GitHub Blog.

In February, we experienced two incidents that resulted in degraded performance across GitHub services.

**February 25 14:25 UTC (lasting 2 hours and 19 minutes)**

On February 25, 2025, between 14:25 UTC and 16:44 UTC, email and web notifications experienced delivery delays. At the peak of the incident, the delay resulted in ~10% of all notifications taking over 10 minutes to be delivered, with the remaining ~90% being delivered within 5-10 minutes. This incident was caused by worker pools running too close to capacity at peak times, resulting in a delay in queue processing.

We mitigated the incident by scaling out the service to meet the demand. We’ve since established a higher baseline capacity to ensure no further delays occur, and we are improving our capacity planning to proactively manage our pool going forward.

**February 3 18:01 UTC (lasting 30 minutes)**

On February 3, 2025, at 18:01 UTC, an incident was declared due to failures in our Migration Tools. The root cause was traced to a deployment of a system component, which led to missing Docker images, causing a 100% outage for all users attempting migrations in this window. The issue was mitigated by rolling back to the previous stable version, restoring service within approximately 30 min.

We have enhanced our test coverage and our workflows to ensure validation of critical dependencies.

* * *

Please follow our status page for real-time updates on status changes and post-incident recaps. To learn more about what we’re working on, check out the GitHub Engineering Blog.

The post GitHub Availability Report: February 2025 appeared first on The GitHub Blog.

Go to Source
