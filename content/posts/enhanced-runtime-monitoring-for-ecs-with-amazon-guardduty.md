---
title: "Enhanced Runtime Monitoring for ECS with Amazon GuardDuty"
date: 2025-01-25
---

With the majority of our applications now being cloud-native and containerized, ensuring security has become paramount. While static security measures, such as image scanning with Amazon Inspector, play a crucial role, monitoring container security during runtime is equally important. This is where ECS Runtime Monitoring with Amazon GuardDuty comes into play. GuardDuty Runtime Monitoring, now over a year in general availability, has proven its effectiveness in detecting runtime security threats across EC2 instances, ECS Clusters, and EKS Clusters. In this blog, we'll walk through enabling runtime monitoring for your ECS Cluster, generating GuardDuty findings, and setting up alerts for both runtime monitoring health and GuardDuty Findings to enhance your security posture.

## Enabling the fully managed GuardDuty Agent

![Enabling the GuardDuty Agent](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fku1aie6zl7964wdllkea.png)

When we deploy the GuardDuty security agent, GuardDuty will create a VPC Endpoint for the security agent to deliver runtime security events to GuardDuty. Alongside it will also create a new security group that will control the traffic that's allowed to reach the resources using inbound rules of the security group and will adapt to vpc cidr range changes.

## ECS Cluster

I started with an existing ECS Cluster with a single task running on AWS Fargate.

![ECS Fargate Cluster](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxvi5kdbl9g3q06bz9bwl.png)

Within the task configuration, you'll notice two containers running:

- Main Application Container
- Sidecar Container launched by AWS to run the Amazon GuardDuty agent

![ECS Task Configuration](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fr1a26u422baj2v0prh3s.png)

**GuardDuty actively monitoring the ECS Cluster**

![GuardDuty Runtime Monitoring](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fscb24uw9ht39wpla1h1b.png)

## GuardDuty Runtime Monitoring Alerts

It is essential to configure alerts for when GuardDuty Runtime Monitoring enters an unhealthy state or when a Runtime Monitoring Finding is detected.

To achieve this, I have configured EventBridge rules with Amazon SNS as the target to trigger email notifications for both.

**GuardDuty Runtime Monitoring Unhealthy State Alert**

I manually scaled down the ECS service from 1 to 0, so that the GuardDuty agent is no longer able to communicate with Amazon GuardDuty and the Runtime Monitoring status is pushed to an unhealthy state.

Event Pattern for Eventbridge Rule:  

```
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Runtime Protection Unhealthy"]
}
```

![Unhealthy GuardDuty Runtime Monitoring](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fybuc181938hfg9xuu83e.png)

![Unhealthy Runtime Monitoring Notification](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe7mxm2mijffwv6mjxw14.png)

**GuardDuty Runtime Monitoring Findings Alert**

I generated sample findings in GuardDuty to test and validate the alerting mechanism.

Event Pattern for Eventbridge Rule:  

```
{
  "source": ["aws.guardduty"],
  "detail": {
    "type": ["Backdoor:Runtime/C&CActivity.B", "PrivilegeEscalation:Runtime/DockerSocketAccessed"]
  }
}
```

You can find the full list of GuardDuty Runtime Monitoring Finding Types here.

**Sample Findings Generated**

![Sample Findings](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7z6i21hoezffn6sxta4l.png)

![Alerts on Runtime Findings](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdgl303nxi0f2gjm6v0pq.png)

## Conclusion

I hope this blog serves as a great starting point for exploring this exciting feature. Below, I've compiled a few additional resources that will help you dive deeper and make the most of it.

- Using Amazon GuardDuty ECS runtime monitoring with Fargate and Amazon EC2
- GuardDuty Alerting using Eventbridge
- GuardDuty Runtime Monitoring

Go to Source
