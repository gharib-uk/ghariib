---
title: "Amazon Redshift enhances security by changing default behavior in 2025"
date: 2025-02-01
---

Today, I’m thrilled to announce that Amazon Redshift, a widely used, fully managed, petabyte-scale data warehouse, is taking a significant step forward in strengthening the default security posture of our customers’ data warehouses. Some default security settings for newly created provisioned clusters, Amazon Redshift Serverless workgroups, and clusters restored from snapshots have changed. These changes include disabling public accessibility, enabling database encryption, and enforcing secure connections.

Amazon Redshift already supports encryption in transit and at rest. Database encryption is crucial because it helps safeguard sensitive data from unauthorized access. Furthermore, restricting public access can be advantageous because it limits the threat surface and helps prevent unauthorized access to the database. By confining the Amazon Redshift cluster within the customer’s virtual private cloud (VPC), the cluster is isolated from the public internet, which significantly reduces the possibility of unauthorized parties discovering and accessing the data warehouse.

Enforcing secure connections is another essential security measure. This enforces encryption of communication between the applications and the database, reducing the risk of eavesdropping and man-in-the-middle exploits, which helps protect the confidentiality and integrity of the data being transmitted.

By implementing additional security defaults for newly created provisioned clusters, Serverless workgroups, and clusters restored from snapshots, Amazon Redshift helps customers adhere to best practices in data security without requiring additional setup, reducing the risk of potential misconfigurations.

These new security enhancements include three key changes:

1. **Disabling public access by default** – Public accessibility will be disabled by default for newly created or restored provisioned clusters. This means that the newly created clusters will be accessible only within your VPC and not accessible from the public internet. With this change, if you create a provisioned cluster from the AWS Management Console, then the cluster is created with public access disabled by default. Specifically, the `PubliclyAccessible` parameter will be set to `false` by default. This change will also be reflected in the `CreateCluster` and `RestoreFromClusterSnapshot` API operations and the corresponding console, AWS CLI, and AWS CloudFormation By default, connections to clusters will only be permitted from client applications within the same VPC. To access your data warehouse from applications in another VPC, you have to configure cross-VPC access.
    
    If you still need public access, you must explicitly override the default and set the `PubliclyAccessible` parameter to `true` when you run the `CreateCluster` or `RestoreFromClusterSnapshot` API operations. With a publicly accessible cluster, we recommend that you always use security groups or network access control lists (network ACLs) to restrict access.
    
2. **Enabling encryption by default** – With this change, the ability to create unencrypted clusters will no longer be available in the Amazon Redshift console. When you use the console, CLI, API, or CloudFormation to create a provisioned cluster without specifying an AWS Key Management Service (AWS KMS) key, the cluster will automatically be encrypted with an AWS-owned key. The AWS-owned key is managed by AWS.
    
    This update might impact you if you are creating unencrypted clusters by using automated scripts or using data sharing with unencrypted clusters. If you regularly create new unencrypted consumer clusters and use them for data sharing, review your configurations to verify that the producer and consumer clusters are both encrypted to reduce the chance that you will experience disruptions in your data-sharing workloads.
    
3. **Enforcing secure connections by default** – With this change, a new default parameter group named `default.redshift-2.0` will be introduced for newly created or restored clusters, with the `require_ssl` parameter set to `true` by default. New clusters created without a specified parameter group will automatically use the `default.redshift-2.0` parameter group. When you create a cluster through the console, the new `default.redshift-2.0` parameter group will be automatically selected. This change will also be reflected in the `CreateCluster` and `RestoreFromClusterSnapshot` API operations, as well as in the corresponding console, AWS CLI, and AWS CloudFormation operations.
    
    For customers who are using existing or custom parameter groups, the service will continue to honor the `require_ssl` value specified in your parameter group. However, we recommend that you update the `require_ssl` parameter to `true` in order to enhance the security of your connections. You continue to have the option to change the `require_ssl` value in your custom parameter groups as needed. You can follow the procedure in this topic in the Amazon Redshift Management Guide to configure security options for connections.
    

We recommend that all Amazon Redshift customers review their current configurations for this service and consider implementing the new security measures across their applications. These security enhancements could impact existing workflows that rely on public access, unencrypted clusters, or non-SSL connections. We recommend that you review and update your configurations, scripts, and tools to align with these new defaults.

If you have feedback about this post, submit comments in the Comments section below. If you have questions about this post, contact AWS Support.  
 

![Yanzhu Ji](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/29/yanzhuji.jpg) Yanzhu Ji  
Yanzhu Ji is a Senior Product Manager on the Amazon Redshift team. She has extensive experience in database security and developing product vision and strategy for industry-leading data products and platforms. She excels at building robust software products using web development, system design, database, and distributed programming techniques.

Go to Source
