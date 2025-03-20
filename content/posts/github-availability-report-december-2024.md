---
title: "GitHub Availability Report: December 2024"
date: 2025-01-17
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

In December, we experienced two incidents that resulted in degraded performance across GitHub services.

The post GitHub Availability Report: December 2024 appeared first on The GitHub Blog.

In December, we experienced two incidents that resulted in degraded performance across GitHub services.

**December 17 14:17 UTC (lasting 17 minutes)**

On December 17, 2024, between 14:33 UTC and 14:50 UTC, users experienced intermittent errors and timeouts when accessing github.com. The error rate was 8.5% on average and peaked at 44.3% of requests.

The increased error rate caused a broad impact across our services, such as the inability to log in, view a repository, open a pull request, and comment on issues. The errors were caused by our web servers being overloaded as a result of planned maintenance that unintentionally caused our live updates service to fail. This service supports the features that provide automatic updates in our user experience, forcing users to manually refresh the page to receive updates. With our live updates service being down, clients refreshed aggressively, further overloading our servers.

We mitigated the incident by rolling back the changes from the planned maintenance and scaling up the service to handle the influx of traffic from WebSocket clients.

We had gaps in our alerting due to the web servers being overloaded, leading to incorrect assessment of services that were impacted. We were able to assess the broad scope of the impact to our services only after the incident was mitigated.

We are working to reduce the impact of the live updates service’s availability on github.com to prevent issues like this one in the future. We have added monitoring higher in the request path to capture this type of failure and are also working to improve our alerting to better detect the scope of impact from incidents.

**December 20 15:57 UTC (lasting 43 minutes)**

On December 20, 2024, between 15:57 UTC and 16:39 UTC, some of our marketing pages became inaccessible due a partial outage issue with one of our third-party service providers. All GitHub users attempting to access the pages would have received 500 errors. There was no impact to any operational product or service area.

At 16:39 UTC, the service provider resolved the outage, restoring access to the affected pages. We are investigating methods to improve error handling and gracefully degrade these pages in case of future outages.

* * *

Please follow our status page for real-time updates on status changes and post-incident recaps. To learn more about what we’re working on, check out the GitHub Engineering Blog.

The post GitHub Availability Report: December 2024 appeared first on The GitHub Blog.

Go to Source
