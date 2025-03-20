---
title: "AWS Network Firewall Geographic IP Filtering launch"
date: 2025-01-02
categories: 
  - "security"
---

AWS Network Firewall is a managed service that provides a convenient way to deploy essential network protections for your virtual private clouds (VPCs). In this blog post, we discuss Geographic IP Filtering, a new feature of Network Firewall that you can use to filter traffic based on geographic location and meet compliance requirements.

Customers with internet-facing applications are constantly in need of advanced security features to protect their applications from threat actors. This includes restricting traffic to and from their workloads in Amazon Web Services (AWS) to certain geographies because of security risk. Customers operating in highly regulated industries—such as banking, public sector, or insurance—might have specific security requirements that can be addressed by Geographic IP Filtering.

Previously, customers had to rely on third-party tools for retrieving an IP address list of specific countries and updating their firewall rules on a regular basis to meet applicable requirements. Now, with Geographic IP Filtering on Network Firewall, you can protect your application workloads based on the geolocation of the IP address. As new IP addresses are assigned by the Internet Assigned Numbers Authority (IANA), the Geographic IP database underneath Network Firewall is automatically updated so that the service can consistently filter inbound and outbound traffic from specific countries based on country codes. It supports IPv4 and IPv6 traffic.

Geographic IP Filtering is supported in all AWS Regions where Network Firewall is available today, including the AWS GovCloud (US) Regions.

## Set up Geographic IP Filtering in Network Firewall

You can use Network Firewall to inspect network traffic and protect your VPCs using layer 3–7 rules (network layer to application layer of the OSI model). When traffic reaches Network Firewall, it will identify the location of the source and destination IP address from the Geographic IP database and block traffic if you have a firewall rule to block that location. You can choose to pass, drop, reject, or create an alert for traffic coming from or going to specific countries.

Before setting up Geographic IP Filtering rules, you need to deploy Network Firewall and attach a firewall policy. You can learn more about these steps in the Network Firewall Getting Started guide. You can configure Network Firewall Geographic IP Filtering in minutes using the AWS Management Console, AWS Command Line Interface (AWS CLI), AWS SDK, or the Network Firewall API.

To configure Geographic IP Filtering rules using the console:

1. Sign in to the AWS Management Console and open the Amazon VPC console.
2. In the navigation pane, under **Network Firewall**, choose **Network Firewall rule groups**.
3. Choose **Create rule group**.
4. In the **Create rule group page**, for the **Rule group type**, select **Stateful rule group**.
5. For the **Rule group format**, select **Standard stateful rule**.
6. For **Rule evaluation order**, select either Strict order (recommended) or Action order.
7. Enter a name for the stateful rule group.
8. For **Capacity**, enter the maximum capacity you want to allow for the stateful rule group.
9. Under **Standard stateful rules**, for **Geographic IP Filtering**, select whether you want to **Disable Geographic IP filtering**, **Match only selected countries,** or **Match all but selected countries**.
10. If you opt for Geographic IP Filtering, then select the Geographic IP traffic direction and Country codes that you want to filter the traffic for.
11. Enter the appropriate values for **Protocol**, **Source**, **Source port**, **Destination**, and **Destination port**.
12. For **Action**, select the action that you want Network Firewall to take when a packet matches the rule settings.
    
    ![Figure 1: Standard stateful rule](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/05/img1-1-1024x952.png)
    
    Figure 1: Standard stateful rule
    
13. Click **Add rule** and then review the rule to create the rule group.
    
    ![Figure 2: Geographic IP Filtering rules](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/05/img2-1024x191.png)
    
    Figure 2: Geographic IP Filtering rules
    

## Suricata compatibility

You can also use Geographic IP filtering with Suricata-compatible rule strings using the geoip keyword.

To create a Suricata compatible rule string:

1. Follow steps 1 through 4 of the previous procedure.
2. For the **Rule group format**, select **Suricata compatible rule string**.
3. For **Rule evaluation order**, select either Strict order (recommended) or Action order.
4. Enter a name for the stateful rule group.
5. For **Capacity**, enter the maximum capacity you want to allow for the stateful rule group.
6. Under **Suricata compatible rule string**, enter an appropriate string based on your source and destination along with the country code to filter traffic for. To use a Geographic IP filter, provide the geoip keyword, the filter type, and the country codes for the countries that you want to filter for.
7. Suricata supports filtering for source and destination IPs. You can filter on either of these types by itself, by specifying `dst` or `src`. You can filter on the two types together with AND or OR logic, by specifying `both` or `any`.

For example, the following sample Suricata rule string drops traffic originating from Japan:

```
drop ip any any -> any any (msg:"Geographic IP from JP,Japan"; geoip:src,JP; sid:55555555; rev:1;)
```

Note that Suricata determines the location of requests using MaxMind GeoIP databases. MaxMind reports very high accuracy of their data at the country level, although accuracy varies according to factors such as country and type of IP. For more information about MaxMind, see MaxMind IP Geolocation.

If you think any of the Geographic IP data is incorrect, you can submit a correction request to MaxMind at MaxMind Correct GeoIP Data.

## Logging Geographic IP Filtering

You can configure Network Firewall logging for your firewall’s stateful engine to get detailed information about the packet and any stateful rule action taken against the packet. There are no changes to the logging and monitoring mechanism with the introduction of the Geographic IP Filtering feature. However, by explicitly specifying the `msg` and `metadata` keywords, you can see additional geographic information in the alert logs that can help with troubleshooting. If these keywords aren’t specified in the Suricata rule string, the log event will not show any geographic information.

## Suricata rule examples

In this section, you will find examples of Suricata rule strings to pass, block, reject, and alert on traffic to or from a specific country.

### Example 1: To pass ingress traffic from a specific country

The following example passes ingress traffic from India.

> **Note**: The rule evaluation order should be set to `Strict` for alert logs to be generated in this example. If the rule evaluation order is set to `Action`, then although the traffic will pass, alert logs will not be generated.

```
alert ip $EXTERNAL_NET any -> $HOME_NET any (msg:"Ingress traffic from IN allowed"; flow:to_server; geoip:src,IN; metadata:geo IN; sid:202409301;)
pass ip $EXTERNAL_NET any -> $HOME_NET any (msg:"Ingress traffic from IN allowed"; flow:to_server; geoip:src,IN; metadata:geo IN; sid:202409302;)
```

The following are the alert and flow logs for Example 1.

**Alert logs:**

```
{
    "firewall_name": "Test-NFW",
    "availability_zone": "eu-north-1a",
    "event_timestamp": "1731102856",
    "event": {
        "src_ip": "13.127.20.X",
        "src_port": 56630,
        "event_type": "alert",
        "alert": {
            "severity": 3,
            "signature_id": 202409301,
            "rev": 0,
            "metadata": {
                "geo": ["IN"]
            },
            "signature": "Ingress traffic from IN allowed",
            "action": "allowed",
            "category": ""
        },
        "flow_id": 234143298308779,
        "dest_ip": "172.31.2.4",
        "proto": "TCP",
        "verdict": {
            "action": "pass"
        },
        "dest_port": 80,
        "pkt_src": "geneve encapsulation",
        "timestamp": "2024-11-08T21:54:16.972019+0000",
        "direction": "to_server"
    }
}
```

**Flow logs from source to destination:**

```
{
    "firewall_name": "Test-NFW",
    "availability_zone": "eu-north-1a",
    "event_timestamp": "1731102918",
    "event": {
        "tcp": {
            "tcp_flags": "13",
            "syn": true,
            "fin": true,
            "ack": true
        },
        "app_proto": "unknown",
        "src_ip": "13.127.20.X",
        "src_port": 56630,
        "netflow": {
            "pkts": 4,
            "bytes": 216,
            "start": "2024-11-08T21:54:16.972019+0000",
            "end": "2024-11-08T21:54:17.263030+0000",
            "age": 1,
            "min_ttl": 112,
            "max_ttl": 112
        },
        "event_type": "netflow",
        "flow_id": 234143298308779,
        "dest_ip": "172.31.2.4",
        "proto": "TCP",
        "dest_port": 80,
        "timestamp": "2024-11-08T21:55:18.257416+0000"
    }
}
```

**Flow logs from destination to source:**

```
{
    "firewall_name": "Test-NFW",
    "availability_zone": "eu-north-1a",
    "event_timestamp": "1731102918",
    "event": {
        "tcp": {
            "tcp_flags": "13",
            "syn": true,
            "fin": true,
            "ack": true
        },
        "app_proto": "unknown",
        "src_ip": "172.31.2.4",
        "src_port": 80,
        "netflow": {
            "pkts": 2,
            "bytes": 112,
            "start": "2024-11-08T21:54:16.972019+0000",
            "end": "2024-11-08T21:54:17.263030+0000",
            "age": 1,
            "min_ttl": 126,
            "max_ttl": 126
        },
        "event_type": "netflow",
        "flow_id": 234143298308779,
        "dest_ip": "13.127.20.X",
        "proto": "TCP",
        "dest_port": 56630,
        "timestamp": "2024-11-08T21:55:18.257449+0000"
    }
}
```

### Example 2: To block ingress traffic from a specific country

The following example blocks ingress traffic from Japan.

```
drop ip $EXTERNAL_NET any -> $HOME_NET any (msg:"Ingress traffic from JP blocked"; flow:to_server; geoip:any,JP; metadata:geo JP; sid:202409303;)
```

### Example 3: To block ingress SSH traffic from a specific country

The following example blocks ingress SSH traffic from Russia.

```
drop ssh $EXTERNAL_NET any -> $HOME_NET any (msg:"Ingress SSH traffic from RU blocked"; flow:to_server; geoip:src,RU; metadata:geo RU; sid:202409304;)
```

### Example 4: To reject egress TCP traffic to a specific country:

The following example rejects egress TCP traffic to Iran.

```
reject tcp $HOME_NET any -> $EXTERNAL_NET any (msg:"Egress traffic to IR rejected"; flow:to_server; geoip:dst,IR; metadata:geo IR; sid:202409305;)
```

### Example 5: To alert on traffic originating from or destined to specific country

The following example alerts on traffic that originates from Venezuela.

```
alert ip any any -> any any (msg:"Geographic IP is from VE, Venezuela"; geoip:any,VE; sid: 202409306;)
```

## Conclusion

You can use the new Geographic IP Filtering feature in AWS Network Firewall to enhance your security posture by controlling traffic based on geographic locations. In this post, you learned about the key concepts, configuration steps, and examples for implementing the Geographic IP Filtering feature in Network Firewall. By using this feature, businesses can protect their networks from potentially harmful traffic and control which geographic locations can interact with their infrastructure. As cyber threats continue to evolve, the Geographic IP Filtering feature serves as a vital tool for strengthening network security.

If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Prasanjit Tiwari](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/05/prasanjt.jpg) Prasanjit Tiwari  
Prasanjit is a Cloud Support Engineer II based out of Virginia, USA. He has a Master of Science in Telecommunications Engineering from the University of Maryland. He is a WAF and Route 53 subject matter expert and enjoys working on networking and perimeter security services. He is enthusiastic about using innovative solutions to address customer challenges.

![Dhiren Patel](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/05/dhrnptl.jpg) Dhiren Patel  
Dhiren is a Cloud Support Engineer-II based out of Virginia, USA. He has a Master of Science in Electrical and Computer Engineering from New York University. As a WAF and Route 53 subject matter expert, he specializes in AWS networking and security services. He’s passionate about helping customers solve their AWS issues and get the best cloud experience possible through AWS.

Go to Source
