---
title: "Cloud Ransomware Developments | The Risks of Customer-Managed Keys"
date: 2025-01-29
---

Learn how to protect against the abuse of AWS Server-Side Encryption with Customer-Provided Keys (SSE-C) in ransomware campaigns.

Ransomware actors are increasingly abusing native cloud features to target critical data. A recent threat actor campaign, as detailed in the Halcyon blog, was observed abusing Amazon Web Services (AWS) Server-Side Encryption with Customer-Provided Keys (SSE-C). By encrypting S3 objects with their own keys, attackers render data irretrievable, abusing AWS features and services through the use of stolen credentials in an attempt to simplify their efforts with less infrastructure to manage.

This blog explores how the attack unfolds, why SSE-C is an attractive tool for ransomware campaigns, and provides actionable mitigation strategies. In addition to the official guidelines from AWS to protect against this emerging threat, key recommendations include restricting SSE-C usage, adopting least privilege principles, and enabling advanced logging to detect anomalous activities. Organizations must proactively secure their AWS environments to stay ahead of these threats.

![](https://www.sentinelone.com/wp-content/uploads/2025/01/Cloud-Ransomware-Developments-The-Risks-of-Customer-Managed-Keys-11.jpg)

## Server-Side Encryption with Customer-Provided Keys (SSE-C)

Amazon Web Services (AWS) Server-Side Encryption with Customer-Provided Keys (SSE-C) is a feature that enables customers to maintain control over their encryption keys when storing objects in Amazon S3. This capability ensures that data is encrypted server-side while allowing customers to provide and manage their own encryption keys, rather than relying on AWS to generate or manage them. Alternatively, AWS also generates and manages a separate data key to encrypt the data during storage, keeping the customer-provided keys confidential.

### How SSE-C Works

1. Customer Provides the Key: When uploading an object to S3, the customer provides their own encryption key in the request.
2. AWS Encrypts the Object: AWS uses the provided key as a Key Encryption Key (KEK) to encrypt a Data Encryption Key (DEK). The DEK is then used to perform AES-256 encryption on the object before storing it.
3. Key Management by the Customer: AWS does not store the customer’s encryption key. It retains only a cryptographic hash (HMAC) of the key for validation purposes during subsequent operations (e.g., downloading or overwriting the object).
4. Decryption on Retrieval: To retrieve an object encrypted with SSE-C, the customer must provide the same encryption key used during the upload. AWS validates the key using the stored HMAC and decrypts the object before returning it.

### Key Features of SSE-C

- Customer-Controlled Keys: Customers retain full control of the encryption keys and can rotate or revoke them as needed.
- No Key Storage in AWS: AWS does not store the encryption key, minimizing the risk of compromise from AWS-side breaches.
- Data Encryption in Transit and at Rest: Objects are encrypted before being stored in S3, and encryption keys are transmitted securely using HTTPS.
- HMAC Key Validation: AWS stores only a cryptographic hash of the key for validation purposes, ensuring that the provided key matches during decryption.

## Turning Encryption Against You | Attackers Abuse SSE-C

Encryption is widely regarded as a cornerstone of data security. However, in the wrong hands, even robust encryption mechanisms can become tools for attackers. A recent trend in ransomware attacks demonstrates how malicious actors can abuse AWS Server-Side Encryption with Customer-Provided Keys (SSE-C) to encrypt organizations’ data in Amazon S3 buckets. This misuse showcases the unintended risks of granting flexibility in encryption while highlighting the dual-edged nature of powerful cloud-native features.

Cloud platforms often host business-critical information, making them a prime target for extortion. If threat actors combine strategies and gain access to additional services, they are able to maximize the impact of their attacks. Abusing these cloud tools means actors can encrypt data, escalate privileges, delete backups, and disrupt organizations to the point where they feel compelled to pay the ransom.

### The Weaponization of SSE-C in Ransomware Attacks

In traditional ransomware attacks, adversaries encrypt local data and demand a ransom for the decryption key. With the rise of cloud storage, attackers have adapted their tactics, leveraging cloud-native features like SSE-C to encrypt cloud data instead. Here’s how they do it:

**Compromising Credentials**

Attackers gain access to AWS credentials through:

- Phishing campaigns targeting employees
- Credential stuffing using leaked passwords
- Public repositories inadvertently exposing AWS access keys
- Purchasing / Obtaining credentials via Infostealer and cloud logs

**Abusing SSE-C**

Once inside, attackers use the compromised credentials to:

- Access the target’s S3 buckets
- Upload their own encryption keys to re-encrypt the Data Encryption Key (DEK) using SSE-C, which indirectly re-encrypts the objects and renders the data unrecoverable without the attacker’s key.
- Modify lifecycle policies to delete files after a short window of 7 days, creating a sense of urgency

Since SSE-C requires the customer to provide their own encryption keys for each upload, they are not stored by AWS, which ensures that customers maintain full control over key management. However, this also makes recovery impossible without the attacker’s cooperation should they successfully gain access to the AWS account.

**Demanding Ransom**

The final step involves placing ransom notes in affected directories, providing payment instructions and dire warnings. The message is simple: pay up or lose your data forever.

### Why SSE-C Is an Attractive Target

**Customer-Controlled Keys**

SSE-C’s design puts Key Encryption Keys (KEKs) in the hands of customers, which are used to encrypt the Data Encryption Keys (DEKs). Since AWS does not store these KEKs, it is impossible for AWS to recover the DEKs or the encrypted data without the customer-provided KEKs. While this enhances security for legitimate users, it also enables attackers to use the feature to encrypt data with impunity.

**Minimal Permissions Required**

Abusing SSE-C requires basic S3 permissions, such as:

- `s3:GetObject` to read existing data.
- `s3:PutObject` to overwrite data with attacker-encrypted versions. These permissions are often overly broad in real-world configurations.

**Lack of Built-In Abuse Detection**

AWS services like CloudTrail log encryption and decryption activities but do not inherently flag malicious use of SSE-C. This creates a blind spot for organizations without robust monitoring in place.

## Mitigating the Risks

Beyond the official guidelines provided by AWS to protect against this emerging threat, organizations must rethink their cloud security practices and focus on both prevention and detection.

### Bucket Versioning and MFA Delete

Bucket Versioning: When enabled on an S3 bucket, versioning retains all versions of an object, including older ones, even if an attacker overwrites or encrypts the file (e.g., using SSE-C). Organizations can recover previous unencrypted versions of their data without relying on the compromised files. Even if the attacker encrypts the data and uploads new versions, the older versions remain intact and accessible, providing a safeguard against data loss due to ransomware.

MFA Delete: Multi-Factor Authentication (MFA) Delete requires an additional layer of security by enforcing MFA credentials to delete object versions or disable bucket versioning. If an attacker gains access to the account but doesn’t have the required MFA token, they won’t be able to permanently delete objects or versions. This ensures that even if they attempt to delete your data after encrypting it, the older versions remain secure, allowing recovery.

Together, these features make it difficult for attackers to execute a successful ransomware attack, as they cannot irreversibly overwrite or delete data without leaving recoverable versions.

### Limit SSE-C Usage

For organizations that do not use versioning and MFA Delete, implementing strict identity access management (IAM) policies can provide an additional layer of security against ransomware attacks targeting S3 buckets. This policy can also be applied account- or organization-wide via resource control policies (RCPs) and for buckets that do not grant cross-account access via service control policies (SCPs).

One effective approach is to control and limit the use of server-side encryption with customer-provided keys (SSE-C), which is often abused in such attacks. By enforcing bucket policies that restrict SSE-C usage to only necessary workflows and mandate specific encryption methods for all objects, organizations can significantly reduce the risk of unauthorized or malicious encryption actions. Below is an example policy to achieve this:

```
{

"Version": "2012-10-17",

"Statement": [

{

"Effect": "Deny",

"Principal": "*",

"Action": "s3:PutObject",

"Resource": "arn:aws:s3:::your-bucket-name/*",

"Condition": {

"Null": {

"s3:x-amz-server-side-encryption": "true"

}

}

},

{

"Effect": "Deny",

"Principal": "*",

"Action": "s3:PutObject",

"Resource": "arn:aws:s3:::your-bucket-name/*",

"Condition": {

"StringEquals": {

"s3:x-amz-server-side-encryption": "AES256"

},

"StringExists": {

"s3:x-amz-server-side-encryption-customer-key": "true"

}

}

},

{

"Effect": "Deny",

"Principal": "*",

"Action": "s3:PutObject",

"Resource": "arn:aws:s3:::your-bucket-name/*",

"Condition": {

"StringNotEquals": {

"s3:x-amz-server-side-encryption": [

"AES256",

"aws:kms"

]

}

}

}

]

}
```

**Explanation of the Policy**

1. First Statement: Denies any request where the `s3:x-amz-server-side-encryption` header is missing, ensuring all uploads are encrypted.
2. Second Statement: Denies requests that specify `s3:x-amz-server-side-encryption: AES256` (used by both SSE-S3 and SSE-C) **but also include** `s3:x-amz-server-side-encryption-customer-key`, which is specific to SSE-C. This effectively blocks SSE-C while allowing SSE-S3.
3. Third Statement: Denies requests where the encryption method is neither AES256 (SSE-S3) nor aws:kms (SSE-KMS). This ensures that only these two secure encryption methods are allowed.

**Behavior of the Policy**

- SSE-S3 (AES256): Allowed, as it does not include the `s3:x-amz-server-side-encryption-customer-key` header.
- SSE-KMS (aws:kms): Allowed.
- SSE-C (AES256 with customer key headers): Blocked, as the presence of the `s3:x-amz-server-side-encryption-customer-key` header differentiates SSE-C from SSE-S3.
- No Encryption: Blocked, as the `s3:x-amz-server-side-encryption` header is required.

## How S1 Protects You Against Cloud Ransomware Abusing SSE-C

SentinelOne provides advanced security capabilities designed to protect cloud environments from sophisticated threats, including ransomware attacks abusing AWS Server-Side Encryption with Customer-Provided Keys (SSE-C). Here’s how SentinelOne can help:

### SentinelOne Singularity![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) Platform | Native & Open Protection, Detection & Response

SentinelOne Storyline Active Response (STAR) is a cloud-based detection and response engine within the Singularity platform. It allows security teams to create custom rules, automate responses, and enhance threat visibility. STAR empowers enterprises to proactively detect and respond to threats tailored to their unique environments. SentinelOne’s platform can detect object overwrites in S3 buckets using SSE-C encryption, which may indicate ransomware activity. To detect such actions natively, ensure CloudTrail data events are enabled for your S3 buckets.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/alert_sse-c_encryptedobject.jpg)

<figcaption>

Alert for identifying SSE-C encrypted object overwrites

</figcaption>

</figure>

### Singularity![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) Cloud Native Security | Our Agentless CNAPP Solution

Cloud Native Security (CNS) is an agentless solution within SentinelOne’s Cloud Native Application Protection Platform (CNAPP), seamlessly integrated into the Singularity Platform. It empowers cloud security teams by adopting an attacker’s perspective to focus on abuse-prone issues. CNS offers a comprehensive suite of features, including rapid onboarding, cloud asset inventory and mapping, vulnerability and secrets scanning, cloud security posture management (CSPM), infrastructure as code (IaC) scanning, container image security, Kubernetes posture management, and cloud detection and response. Its unique Offensive Security Engine provides proof of abuse with each alert, helping teams prioritize critical issues and reduce false positives.

SentinelOne CNS can identify misconfigurations in bucket hardening, such as disabled AWS S3 bucket versioning and lack of MFA delete. These detections enable organizations to validate their bucket configurations, reducing the likelihood of ransomware attacks. Below are example policies that detect enforcement of MFA Delete and Bucket Versioning for better bucket security:

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/alert_bucketversnotenabled.jpg)

<figcaption>

Alert on bucket versioning not enabled

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/graph_miscon_bucketvers-scaled.jpg)

<figcaption>

Graph view for misconfigured bucket versioning

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/alert_mfadeletedsabled.jpg)

<figcaption>

Alert for MFA delete disabled for S3 bucket

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2025/01/graph_misconfigbucketnomfadelete.jpg)

<figcaption>

Graph view for misconfigured bucket with MFA delete disabled

</figcaption>

</figure>

## Conclusion

Cloud-native features (CNFs) are central to storing and managing sensitive business data, making them an increasingly attractive target for threat actors. Without the right protections in place, API integrations, automated backups, and data synchronization, though designed to aid organizations, can be leveraged by actors to deepen the impact of their attack campaigns.

SentinelOne’s robust capabilities provide a multi-layered defense against ransomware attacks targeting AWS environments. By combining proactive posture management, real-time detection, and automated response, SentinelOne helps organizations prevent the abuse of features like SSE-C and ensures the resilience of their cloud workloads.

Deploying SentinelOne as part of your cloud security strategy means confidently navigating the complexities of cloud-native threats while safeguarding your critical assets. Reach out to us today for an in-depth demo or, contact our team directly to learn more about enterprise-wide cybersecurity built for what’s next.

Singularity![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) Cloud Security

Improve prioritization, respond faster, and surface actionable insights with Singularity![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) Cloud Security, the comprehensive, AI-powered CNAPP from SentinelOne.

Get a Demo

Go to Source
