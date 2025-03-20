---
title: "From log analysis to rule creation: How AWS Network Firewall automates domain-based security for outbound traffic"
date: 2025-03-19
---

> **March 6, 2025:** We’ve updated this post to clarify that generated domain reports contain only HTTP and HTTPS traffic domain log data collected from the moment traffic analysis mode is enabled, with a maximum of 30 days.

* * *

When it comes to controlling incoming (ingress) and outgoing (egress) network traffic, organizations typically focus heavily on inbound traffic controls—carefully restricting what traffic can enter their network perimeter. However, this approach addresses only inbound security challenges. Modern applications rely heavily on third-party code through operating systems, libraries, and packages. This dependency can create potential security vulnerabilities. If these components are compromised, affected workloads might attempt to connect to unauthorized command and control servers or send sensitive data to unauthorized destinations on the internet.

This is why implementing strong outbound traffic controls—particularly through domain-based allowlisting—has become a critical security best practice. Rather than allowing unrestricted outbound access or maintaining an ever-growing denylist of low-reputation domains, many organizations are shifting to domain-based allowlisting. This approach restricts outbound communications to explicitly trusted domains, reduces potential risk surfaces, and helps to protect against both known and unknown threats. However, manually identifying and maintaining these allowlists has traditionally been a complex and time-consuming process.

AWS Network Firewall automated domain lists improve visibility into network traffic patterns and simplify outbound traffic control management. This feature provides analytics for HTTP and HTTPS network traffic, helping organizations understand domain usage patterns. It also automates firewall log analysis to create rules based on your network traffic. By combining increased visibility with automation, this feature enhances your security awareness and helps to improve the effectiveness of your firewall rules.

In this blog post, we’ll guide you through the implementation of the AWS Network Firewall automated domain list feature, providing a detailed overview, step-by-step instructions, and best practices to optimize your network security.

## Overview of automated domain lists and traffic insights

Domain-based security allows you to control network traffic based on the domain names that your applications and users are trying to access. This approach offers a more intuitive and flexible way to create firewall rules, focusing on the destinations your network is trying to reach rather than just IP addresses. However, effectively configuring and managing firewall rules remains challenging for some customers, especially in large environments where connected devices, applications, and traffic patterns are continuously growing and changing. Organizations might struggle to keep up with these changes, leading to outdated or ineffective firewall rules and policies that are either too permissive, exposing the network to risks, or too restrictive, blocking legitimate traffic.

Let’s explore how automated domain lists address these challenges through various use cases and benefits:

#### Preventive and detective security controls

1. **Domain control through allowlisting** – Establishing domain allowlists aligns with the security principle of least privilege for network traffic. A least-privilege model adjusts the scope of what a workload can do across the network, from infinite and undefined to scoped-down and well-defined, enabling better insight into potentially risky behaviors. By limiting outbound connections to only approved domains, organizations can more effectively control and monitor workload communications.
2. **Rule audit and compliance** – Domain allowlisting makes it clear which domains are allowed, supporting alignment with standards like the Payment Card Industry Data Security Standard (PCI DSS), Health Insurance Portability and Accountability Act (HIPAA), Cybersecurity Maturity Model Certification (CMMC), and General Data Protection Regulation (GDPR).
3. **Preventive controls enable detection** – Preventive controls also act as detective controls, establishing a baseline for normal domain access patterns. With a domain allowlist in place, security teams can better detect workloads that show signs of unauthorized activity.
4. **Incident response support** – Domain reporting provides the latest list of workload domains accessed, enabling quick identification of potentially malicious domains during security incidents. This information helps teams prioritize which workloads may need immediate attention.

#### Operational value

1. **Initial firewall setup and management** – Automated allowlisting involves analyzing existing traffic patterns and recommending domain-based rules, which simplifies the process of establishing baseline firewall rules. This helps organizations quickly deploy effective security policies, potentially reducing the time and expertise needed for initial firewall configuration and ongoing management.
2. **Application modernization** – Allowlisting supports adjusting firewall rules to accommodate rapidly changing traffic patterns in microservices and containerized environments, helping security to keep pace with evolving architectures.
3. **Cross-environment consistency** – Allowlisting enables consistent firewall rule creation and management across multi-cloud and hybrid environments, regardless of where applications or data reside.

## How the automated domain list feature works

Automated domain lists work by analyzing your HTTP and HTTPS traffic, generating reports on frequently accessed domains, and providing a convenient way to create rules based on actual network traffic patterns. To begin using automated domain lists in AWS Network Firewall, sign in to the AWS Management Console, access the Network Firewall service, and either work with an existing firewall or create a new one. Then follow the rest of the steps in this post.

### Step 1: Enable traffic analysis mode to capture HTTP and HTTPS traffic domain logs

After you’ve selected a firewall, in the left navigation pane, choose **Configure advanced settings**. Select the **Enable traffic analysis mode** checkbox to enable it, as shown in Figure 1. Network Firewall uses this logging mode to collect data on observed domains for HTTP and HTTPS traffic to create domain reports for up to 30 days from the date traffic analysis mode is enabled. This means domain reports contain data collected only from the moment traffic analysis mode is enabled.

![Figure 1: Enabling traffic analysis mode for a firewall](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img1-2.png)

Figure 1: Enabling traffic analysis mode for a firewall

To stop collecting data on frequently accessed domains in your network traffic, clear the checkbox to disable traffic analysis mode, as shown in Figure 2. Note that if you disable traffic analysis mode, you won’t be able to generate domain reports.

![Figure 2: Disabling traffic analysis mode](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img2-1.png)

Figure 2: Disabling traffic analysis mode

Once traffic analysis mode is enabled, you’re ready to generate a domain report based on observed network traffic. These reports include metrics collected from the moment you activated the setting, with data available for up to 30 days. Next, you can go to the **Monitoring and observability** tab and choose **Create report**.

![Figure 3: Traffic analysis mode enabled: Now you’re ready to generate domain-based reports](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img3.png)

Figure 3: Traffic analysis mode enabled: Now you’re ready to generate domain-based reports

### Step 2: Create a domain report

The domain report summarizes the HTTP and HTTPS traffic observed by your firewall for up to 30 days (or for the duration since firewall activation if less than 30 days). Select the checkbox next to each traffic analysis type you want to include in the report—HTTP, HTTPS, or both.

> **Important:** Use your monthly domain report to examine 30 days of traffic behavior. Each report type (HTTP, HTTPS) is available once every 30 days at no additional cost.

![Figure 4: Create a domain report that includes traffic analysis types HTTP, HTTPS, or both](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img4.png)

Figure 4: Create a domain report that includes traffic analysis types HTTP, HTTPS, or both

To see the status of your domain report, go to the **Reports** section in the console for your specific firewall. When the report is ready, you can review the report directly in the console or download it, as shown in Figure 5.

![Figure 5: The list of domain reports in the Reports section of the console for your specific firewall](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img5.png)

Figure 5: The list of domain reports in the Reports section of the console for your specific firewall

### Step 3: Review the report details

The report details include the traffic type (HTTP or HTTPS) and the observation period (start and end dates). By default, the report covers the last 30 days, or the entire period since traffic analysis was enabled if that is less than 30 days. The report also shows these details:

- The **Domain** list shows domains that are a fully qualified domain name (FQDN) observed in the network traffic, such as aws.com or subdomain.aws.com.
- The **Access attempt** count refers to the overall count of connection requests to the domain, including both successful and failed attempts.
- The **Unique sources** field shows the number of distinct source IP addresses connected to the domain, indicating its popularity. For example, if one workload connects to aws.com, then count = 1; if 1000 workloads connect to aws.com, then count = 1,000.
- The **First accessed** field shows when the domain was first seen in your traffic, while **Last accessed** shows when it was most recently seen. This includes both successful and failed attempts to access the domain.
- The **Protocol** field indicates how the domain was observed—through either HTTP or HTTPS traffic (in other words, HTTP headers or a TLS handshake).

An example report is shown in Figure 6.

![Figure 6: Example domain report details: 30-day analysis](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img6.png)

Figure 6: Example domain report details: 30-day analysis

### Step 4: (Optional) Create a domain list rule group

You can copy the list of observed domains from the report to a stateful domain list rule group and update your firewall policy. To do so, in the **Report details** section, choose **Create domain list group** to use the firewall policy wizard to create or update your firewall rules. The selected domains are automatically copied to a domain list rule group, as shown in Figure 7. For detailed instructions, see the AWS Network Firewall documentation.

![Figure 7: Option to copy over the observed domain lists and create a domain list rule group using the firewall policy wizard](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/21/img7.png)

Figure 7: Option to copy over the observed domain lists and create a domain list rule group using the firewall policy wizard

## Best practices for implementing domain allowlists

When you implement domain allowlisting, consider the following guidelines for operational success. We recommend that you also consult your own internal compliance and security policies.

1. Start with a strategy of _generous allowlisting_:
    - Begin with broader and more generous allowlist rules rather than a more refined list, initially, to reduce the risk of accidently blocking legitimate domains.
    - Focus on getting to a Default Deny policy so that you can benefit from its risk surface reduction.
    - Create flexible rules for trusted domains, including second-level domains and top-level domains, such as allowing access to subdomains under your registered second-level domain. Or allow access to second-level domains under top-level domains that your organization trusts—for example, .mil, .gov, or .edu.
    - Use custom Suricata rules with regex capabilities to handle complex traffic efficiently. See Examples of stateful rules for Network Firewall.
    - Remember that even a broad allowlist provides better security than having no allowlist at all.
2. Make iterative improvements:
    - After you establish an initial generous allowlist and Default Deny rules, evaluate the rules to determine which areas you might want to start narrowing down further. Use alert rules before pass rules in order to log the specific domains a pass rule might be allowing access to.
    - Adjust logging levels based on domain trust levels and monitoring requirements.
    - Review and update rules based on operational insights and changing requirements.
    - Take a pragmatic and iterative approach to rule refinement rather than attempting to make the ruleset very strict.
3. Set up robust logging:
    - Enable Network Firewall alert logs to maintain visibility into traffic patterns.
    - Use tools like Amazon CloudWatch Logs Contributor Insights for log analysis.
    - Consider setting up proactive alerts for denied domains accessed by critical workloads.
    - Monitor logs to identify potential allowlist additions or changes.
4. Additional considerations:
    - After you enable traffic analysis mode, the automated domain lists feature provides visibility into your network traffic, reporting on observed connections. Although it doesn’t distinguish between allowed and blocked traffic, the domain list report can help you identify the most critical domains to include in your firewall rules.
    - The domain traffic data used to generate the list of domain recommendations is available for up to the last 30 days after traffic analysis has been enabled. This allows you to focus on the most relevant and recent network activity when optimizing your firewall policies.
    - Data collection for automated domain lists is opt-in and performed independently of the firewall policy and logging configuration. Enabling the feature doesn’t impact the performance of the firewall itself.

## Conclusion

With AWS Network Firewall automated domain lists, you can simplify your firewall management process, create more effective rules based on actual traffic patterns, and maintain a strong security posture with less manual effort. This feature helps you address common challenges such as keeping up with rapidly changing application landscapes, managing security across complex environments, and adhering to compliance requirements. To learn more about Network Firewall and its features, see the product page and service documentation.

If you have feedback about this post, submit comments in the Comments section below. If you have questions about this post, start a new thread on the AWS Network Firewall re:Post forum or contact AWS Support.

![Mary Kay Sondecker](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/20/marykays.jpg) Mary Kay Sondecker  
Mary Kay is a Senior Product Manager at AWS, focused on AWS Network Firewall. With over two decades of experience in the technology industry, she is passionate about helping customers easily implement effective, scalable cloud solutions to drive better business outcomes.

![Jesse Lepich](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2021/02/15/Jesse-Lepich-Author.jpg) Jesse Lepich  
Jesse is a Senior Security Solutions Architect at AWS based in Lake St. Louis, Missouri, focused on helping customers implement native AWS security services. Outside of cloud security, his interests include relaxing with family, barefoot waterskiing, snowboarding and snow skiing, surfing, boating, and mountain climbing.

![Michael Leighty](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/20/mleighty.jpg) Michael Leighty  
Michael is a Senior Security Solutions Architect at AWS, based in Atlanta. He specializes in helping customers design and implement effective network security controls, drawing from extensive experience at leading network security vendors. At AWS, he works closely with service teams to drive continuous improvement in security services based on customer needs and feedback.

![Jason Goode](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/02/20/jmgoode.jpg) Jason Goode  
Jason is a Senior Security GTM Content Specialist at AWS, where he develops content strategies that bridge technical concepts with practical business solutions. Based in Austin, Texas, he leverages his creative background and expertise to help organizations understand and use native AWS security services.

Go to Source
