---
title: "Elasticsearch: Cluster Status is RED"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "ai"
  - "cybersecurity"
  - "security"
  - "soc"
---

![](https://socprime.com/wp-content/uploads/v1-82-400x234.jpg)

It happens very rarely, but sometimes your cluster gets red status.

Red status means that not only has the primary shard been lost but also that the replica has not been upgraded to primary in its place.

However, as in the case of yellow status, you should not panic and start firing commands without finding out what is happening, as Elasticsearch has mechanisms that can restore the situation automatically.

1: Find the cause of the allocation failure:

```
GET _cluster/allocation/explain
```

The API returns: “unassigned\_info” (reason for the shard being unassigned), “node\_allocation\_decision” (list of explanations for each node’s eligibility to receive the shard), and “deciders” (decision with its explanation).

2\. Retry Elasticsearch shard allocation blocked by multiple consecutive allocation failures:

```
POST /_cluster/reroute?retry_failed=true
```

3\. The CAT pending tasks operation displays the progress of all pending tasks, including their priority and time in the queue, as shown in the following example request:

```
GET /_cat/pending_tasks?v
```

  
  

The post Elasticsearch: Cluster Status is RED appeared first on SOC Prime.

Go to Source
