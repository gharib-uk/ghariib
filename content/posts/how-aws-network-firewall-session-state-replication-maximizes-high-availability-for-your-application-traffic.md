---
title: "How AWS Network Firewall session state replication maximizes high availability for your application traffic"
date: 2025-02-06
---

AWS Network Firewall is a managed, stateful network firewall and intrusion protection service that you can use to implement firewall rules for fine grained control over your network traffic. With Network Firewall, you can filter traffic at the perimeter of your virtual private cloud (VPC); including filtering traffic going to and coming from an internet gateway, NAT gateway, over VPN or AWS Direct Connect. In this post, we show you how AWS Network Firewall uses session state replication to help maintain high availability for your application traffic.

## Network Firewall session state replication

The stateful engine used by Network Firewall applies network security policies according to customer-defined rules. When inspecting stateful flows, Network Firewall maintains a reconstructed state—stored in a flow table—for each connection. Network Firewall is a distributed service, spreading traffic and the connection state over many backend firewall hosts using Gateway Load Balancer endpoints.

There are several operational reasons why backend hosts might be brought in and out of service, such as autoscaling events to adjust for changing traffic levels or installing software for security and other service updates. During these operations, if the traffic contains long-lived traffic flows, Network Firewall allows these flows to drain for several minutes before replacing the hosts and re-balancing them to the newer hosts.

When an existing flow is rebalanced onto a new host that doesn’t contain its connection state, the connection is handled according to the firewall policy’s configured stream exception policy. Network Firewall will either drop or reject these connections, or Network Firewall can be configured to continue applying the firewall policy’s rules without the context from earlier in the connection. Both choices have implications: using the `drop` or `reject` action maintains security by forcing connections to be restarted and re-inspected but at the cost of some broken connections, while the `continue` action requires writing firewall rules that can accept connections that are broken midstream.

In December 2024, AWS introduced the ability for Network Firewall to replicate the session state between backend hosts, reducing the number of cases where the stream exception policy needs to be applied to broken connections. Now, the majority of these failed-over flows go to a new host that already contains the correct flow state, allowing those connections to continue without interruption. This feature is automatically enabled by default on all firewalls and no action is required by you. The stream exception policy will continue to be applied in rare cases where the state cannot automatically be replicated or when connections are broken for other reasons such as routing changes in the network.

![Figure 1: Session state replication flow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/04/img1.png)_Figure 1: Session state replication flow_

Figure 1 shows the sequence of events to maintain persistent connection during operations to replace a backend firewall host:

1. A network flow arrives at the Network Firewall endpoint and is forwarded to a firewall backend host (firewall host 1) by Gateway Load Balancer.
2. Firewall host 1 is de-registered from the Gateway Load Balancer target group, which causes Gateway Load Balancer to stop assigning new flows to the host but maintains existing ones.
3. The service exports the remaining session state table from backend firewall host 1.
4. The service replicates the session table data to other healthy backend hosts.
5. A flow flush operation causes Gateway Load Balancer to reassign the remaining flows on host 1 to other in-service hosts, where the ongoing flows will continue being inspected by the stateful inspection rules configured on the firewall.

Some of the key considerations for your own workloads:

- This new feature only applies to the firewall stateful engine.
- Both IPv4 and IPv6 connection states are supported.
- Network Firewall doesn’t support asymmetric routing. See Avoiding asymmetric routing with AWS Network Firewall for more information..
- The configured stream exception policy will continue to be applied in rare cases where state cannot be replicated and when connections are broken for other reasons. The StreamExceptionPolicyPackets Amazon CloudWatch metric continues to reflect the number of times that the exception policy is applied to arriving packets. For more information, see Stream exception policy options in your AWS Network Firewall firewall policy .

## Conclusion

In this post, we outlined how AWS Network Firewall uses its ability to replicate connection state across multiple backend firewall hosts to maintain high availability for your application traffic. This feature is enabled by default for existing and new customers and there are no additional costs or configuration changes required to use this feature.

To learn more about AWS Network Firewall, see the AWS Network Firewall product page and the service documentation. To see which AWS Regions AWS Network Firewall is available in, see AWS Services by Region.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Tushar Jagdale](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/04/jagdalet.jpg) Tushar Jagdale  
Tushar is a Specialist Solutions Architect focused on Networking at AWS, where he helps customers build and design scalable, highly available, secure, resilient, and cost effective networks. He has over 15 years of experience building and securing data center and cloud networks.

![Amish Shah](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/05/amishsh.jpg) Amish Shah  
Amish is a seasoned product leader with over 15 years’ experience developing innovative and scalable solutions for networking, security, and cloud use cases. He currently leads the AWS Network Firewall service, where he helps develop security solutions that protect AWS workloads. Outside of work, Amish enjoys playing cricket and soccer, loves to travel, and has recently started collecting niche fragrances.

![Vikram Saurabh](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/04/viksau.jpg) Vikram Saurabh  
Vikram is an experienced engineering leader with 20 years of experience in software engineering, primarily in building firewall products and services. He currently leads the AWS Network Firewall engineering team and has previously led the engineering team of Route53 DNS Firewall. Outside of work, Vikram enjoys playing cricket, hiking, and solving math puzzles.

![Jamie Lavigne](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/04/lavignen.png) Jamie Lavigne  
Jamie is a Principal Software Dev Engineer with over 10 years of experience building and operating highly resilient network security services at AWS. Jamie has been a technical lead of the AWS Network Firewall service since its inception and continues to focus on ensuring that it meets the security, compliance, and availability needs of its internal and external customers.

Go to Source
