---
title: "Manage authorization within a containerized workload using Amazon Verified Permissions"
date: 2025-03-19
---

Containerization offers organizations significant benefits such as portability, scalability, and efficient resource utilization. However, managing access control and authorization for containerized workloads across diverse environments—from on-premises to multi-cloud setups—can be challenging.

This blog post explores four architectural patterns that use Amazon Verified Permissions for application authorization in Kubernetes environments. Verified Permissions is a scalable permissions management and fine-grained authorization service for your applications.

In this blog post, we cover the following patterns and discuss their trade-offs:

- Calling Verified Permissions from an Amazon API Gateway API fronting your application in Kubernetes
- Calling Verified Permissions from a Kubernetes Ingress controller component
- Calling Verified Permissions from a sidecar container running in the same pod as the application container
- Calling Verified Permissions from the application container

Understanding these patterns and their implications can help you implement secure and consistent authorization mechanisms across your entire infrastructure without compromising the scalability, portability, and resource efficiency of your containerized workloads.

### Consistent authorization through centralized policy management

Access to application resources can be secured more effectively with a centralized and consistent approach to authorization. Especially in containerized environments with distributed architectures and shared resources, traditional access control methods, like embedding authorization logic within individual application code or relying on local access control policies, can become difficult to manage and prone to errors. This becomes even more challenging when you have a combination of on-premises and cloud setups.

A centralized authorization solution empowers developers to implement consistent access control across individual components of an application efficiently. Benefits include reduced duplicate work, an improved security posture, and lower complexity in managing and enforcing access control policies.

### Verified Permissions benefits in a containerized environment

Amazon Verified Permissions provides several key benefits as an external authorization service:

- **Benefits for the platform engineering team** – Centralized authorization enables platform engineering teams to implement, maintain, and govern authorization policies across the organization without requiring changes to individual applications. This aligns with modern platform engineering practices, where platform engineering teams can provide authorization as a service to application teams, promoting consistent security standards while reducing the operational burden on development teams.
- **Consistent authorization across environments** – With Verified Permissions, you can define and manage access control policies in a centralized location. This makes it easier to apply consistent authorization rules across your entire infrastructure, including on-premises deployments and different cloud environments.
- **Simplified application development** – Externalizing authorization logic from applications reduces development complexity. Developers can focus on core application functionality without having to implement and maintain authorization mechanisms within each service or component. This separation of concerns promotes code modularity, reusability, and faster iteration cycles.
- **Scalable and highly available** – Verified Permissions is a managed service, designed to be scalable and highly available out of the box. As your containerized workloads grow in scale and complexity, Verified Permissions can handle increasing authorization request volumes while maintaining performance and availability.
- **Fine-grained access control** – Verified Permissions supports attribute-based access control (ABAC) and role-based access control (RBAC). This allows you to define granular policies in the open source Cedar language based on various attributes like user roles, resource properties, environmental factors, and more.

## Integration patterns for authorization

Kubernetes provides many options for architecting applications. Therefore, there are multiple locations in a typical architecture where authorization decisions can be enforced, as shown in Figure 1.

![Figure 1: Integration points for authorization in containerized workloads](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img1-2.png)

Figure 1: Integration points for authorization in containerized workloads

The workflow is as follows:

1. **API Gateway**. Organizations can use entry points to the application outside of the Kubernetes cluster, such as an API gateway, to obtain an authorization decision. In AWS, Amazon API Gateway enables customers to use authorizer Lambda functions to send an authorization request to Verified Permissions.
2. **Ingress controller**. The Kubernetes API defines Ingress objects, which provide load balancing and routing functions on layer 7. Common Ingress controllers like Traefik offer the option to integrate external authorization services.
3. **Sidecar proxy container**. You can intercept every request routed to the application by using a sidecar container running in the same pod as the application container. This sidecar container calls Verified Permissions for authorization decisions.
4. **Application container**. Developers can use the Amazon SDK to communicate with Verified Permissions from inside the application when an authorization decision is needed.

In the following sections, we explore each of these patterns in detail, examining their implementation, use cases, and specific considerations. At the end of our discussion, we provide a comprehensive comparison table to help you choose the most appropriate pattern based on factors such as scalability, performance, maintenance overhead, and specific use case requirements. This will help you make an informed decision about which pattern best suits your application’s needs.

### Authorization workflow

Independent of which of the four mentioned options for authorization you choose, the overall authorization workflow, shown in Figure 2, will stay the same.

![Figure 2: Authorization workflow with Amazon Verified Permissions](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img2-2.png)

Figure 2: Authorization workflow with Amazon Verified Permissions

The workflow is as follows:

1. **Authentication**. The user first authenticates with an identity provider to obtain a JSON Web Token (JWT). You can configure the identity provider to write relevant information like user roles, tenant ID, or other needed user attributes into the JWT. You can then use this information later to make an authorization decision.
2. **API request**. The user makes a request to your application that includes the JWT.
3. **Authorization information**. Your application extracts the relevant information that is needed to make an authorization decision from the request. This can include _principal_ information from the JWT, information about the _resource_ that the user requests, and what _action_ the user wants to perform.
4. **(Optional) Policy information point lookup**. Depending on your policies, you might need additional information in order for Verified Permissions to make an authorization decision. For example, you can query ownership details for a document from a database.
5. **Authorization decision**. You then send the relevant information to Verified Permissions, which returns a decision stating whether the request is permitted or forbidden.
6. **Authorization enforcement**. You then enforce the decision from Verified Permissions in your application by allowing or denying an action. For a REST API, this would result in sending back an `HTTP 403 forbidden` status if the request was denied, or processing the request if it was allowed and sending an `HTTP 200 OK` status.

### Authorization outside of the cluster by using Amazon API Gateway

In this pattern, authorization decisions are made at the API gateway layer before requests reach the Kubernetes cluster. When a request arrives at the API gateway, it triggers an authorization check with Verified Permissions to evaluate the request against defined policies. Based on the Verified Permissions response, the gateway either forwards the request to the containerized application or denies access.

This pattern excels in scenarios where you need coarse-grained access control that can be enforced with information accessible at the API level (such as an HTTP header or ID or access token) and that supports RBAC and ABAC. Consider a document management application where different users have access to different documents based on group membership or identity attribute.

This approach to authorization works consistently regardless of whether your application runs in containers, virtual machines, or serverless environments. The API gateway acts as a unified control point for enforcing access policies across backend services.

For implementations that use Amazon API Gateway specifically, you can use Lambda authorizers to integrate with Verified Permissions. For each incoming API request, API Gateway invokes the authorizer Lambda function, which makes a call to Verified Permissions to evaluate the request against the defined authorization policies, as shown in Figure 3.

![Figure 3: Integration of Amazon Verified Permissions in Amazon API Gateway](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img3-2.png)

Figure 3: Integration of Amazon Verified Permissions in Amazon API Gateway

AWS provides a quick-start solution that demonstrates this integration by using Amazon API Gateway and Amazon Cognito, making it easier to implement this pattern. The setup process is detailed in the blog post Authorize API Gateway APIs using Amazon Verified Permissions and Amazon Cognito or bring your own identity provider.

### Authorization in a Kubernetes Ingress

Another option to implement coarse-grained access control in use cases as described in the previous section is to use a Kubernetes Ingress layer. Some customers prefer Kubernetes-native solutions, especially if they need to run Kubernetes clusters within and outside of AWS.

Kubernetes provides an API to create and maintain Ingress objects, operating at layer 7 (the application layer), which enables routing decisions based on HTTP attributes. This layer 7 capability makes Ingress controllers ideal for implementing authorization checks.

One Kubernetes Ingress controller that supports external authorization is Traefik Proxy. With this feature, you can delegate authorization decisions to an external service like Verified Permissions before routing requests to the application container.

Assuming that the authorization endpoint is backed by a service in the same Kubernetes cluster, the architecture looks as shown in Figure 4.

![Figure 4: Integration of Amazon Verified Permissions in a Kubernetes Ingress](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img4-2.png)

Figure 4: Integration of Amazon Verified Permissions in a Kubernetes Ingress

The workflow is as follows:

1. Authenticated users access the service through an Elastic Load Balancer of type Network Load Balancer (NLB).
2. The NLB—operating at layer 4—exposes a Kubernetes Ingress inside the cluster that provides layer 7 capabilities. The Ingress object is implemented by an Ingress controller that supports external authorization, as described earlier.
3. The Ingress forwards the request—or parts of it—that needs authorization to a local authorization service in the cluster. We use a dedicated authorization service in this architecture because the Ingress backend service allows an external endpoint to be called for authorization.
4. The authorization service is deployed into its own Kubernetes namespace with a dedicated Kubernetes service account. EKS Pod Identity provides the ability to link the service account in this namespace to an AWS Identity and Access Management (IAM) role that grants access to Verified Permissions by injecting temporary AWS access credentials into the pod at runtime.
5. The authorization service extracts relevant information from the request and sends it to Verified Permissions for an authorization decision.
6. The Ingress for the backend service awaits the response of the authorization service and forwards it to the backend service, if access is granted. The Ingress expects the authorization service to respond with `HTTP status code 200` for authorized requests. If the Ingress receives `HTTP status code 403`, the requester is not allowed to access the requested resource, and the Ingress will block the request at this stage.
7. Only authorized requests are forwarded to registered backend pods.

Because integration with external authorization services is not part of the Kubernetes Ingress API, you need to consult the documentation of the Ingress controller that you decide to use to determine the availability of this feature and its implementation details. Forward authentication of the Traefik Kubernetes Ingress supports this pattern and can be configured with the vendor-specific annotations described in the Traefik documentation.

### Authorization in sidecar containers

Not all Ingress controllers support integration with an external authorization service. Amazon Elastic Kubernetes Service (Amazon EKS) customers might prefer the AWS Load Balancer Controller to manage the lifecycle of NLBs and ALBs for their services. Customers can continue using their existing Ingress controller, even if it does not support calling external authorization services today. You can move the authorization of requests behind the Ingress layer with the sidecar container pattern.

Sidecar containers are a common pattern for extending an application’s functionality in Kubernetes. A sidecar is a container running in the same pod as the application it relates to. This means that the sidecar and application follow the same lifecycle and share resources, such as the network ID. This pattern is a good fit when the authorization logic is service-specific. Because the authorization service is deployed alongside the application, this pattern also provides better support in situations where changes to the application demand changes in the authorization logic.

Consider a document management system where access control depends on document metadata and team structures. When a user attempts to edit a document, the sidecar queries the document’s metadata, such as the classification level, tags, and department ownership. The sidecar can also check the organizational team hierarchy to understand reporting relationships and access privileges. This context enables fine-grained authorization decisions that consider not just who the user is, but also, for example, their organizational context or the individual document’s metadata.

Although it’s possible to configure sidecar proxies such as Envoy for individual pods manually, the more convenient option is to introduce a _service mesh_. A service mesh provides a control plane to manage proxies, including centralized configuration, automated injection of sidecars, and an Ingress layer for traffic routing. Istio is a popular option for a service mesh in Kubernetes.

The diagram in Figure 5 shows the deployment architecture to implement authorization with Verified Permissions in a service mesh.

![Figure 5: Integration of Amazon Verified Permissions in a Kubernetes Ingress](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img5-2.png)

Figure 5: Integration of Amazon Verified Permissions in a Kubernetes Ingress

The workflow is as follows:

1. Authenticated users access the application through an NLB.
2. The request is routed through an Ingress in the Kubernetes cluster.
3. The Ingress forwards the request directly to the backend service.
4. Pods of the backend service consist of multiple containers. Each request is routed through an Envoy proxy first.
5. The Envoy proxy forwards the request to a co-located container running the authorization service.
6. Pod Identity is used to map an IAM role to a Kubernetes service account bound to the pod, which enables the authorization sidecar to invoke Verified Permissions for an authorization decision. Note that each container in this pod has access to the IAM credentials that are mapped to the service account.
7. The Envoy proxy awaits the response of the authorization sidecar and blocks or forwards the request to the backend container, depending on the Verified Permissions authorization decision.

When Istio is deployed into a Kubernetes cluster, it introduces Custom Resource Definitions (CRDs) for managing the service mesh. The authorization workflow can be implemented using the ServiceEntry CRD and an Istio Authorization Policy. The authorization service running as a local container in the application pod becomes a registered service entry in the mesh. This service entry can then be configured in an authorization policy as the target for request authorization in the proxy. For more details, see the External Authorization section in the Istio documentation.

### Application container

When it comes to integrating Verified Permissions directly within your application container, you have the advantage of fine-grained control over authorization decisions at the application level. This approach allows for more context-aware authorization checks and can be useful when you need to make authorization decisions based on application-specific data that you can query from a policy information point.

Unlike the sidecar pattern, where authorization happens before the request reaches your application code, this approach lets you gather the necessary context from your application state, databases, or other services before making the authorization call. This is particularly valuable when the authorization logic is deeply intertwined with business logic or requires data that’s only available within the application context. This pattern also supports minimizing the number of authorization requests, if, for example, only a subset of requests processed by a monolithic service require authorization.

However, it’s important to note that this tight coupling between authorization and business logic makes the system more brittle and susceptible to breakage when functional or business logic changes occur. This means that modifications to your application code might require careful consideration of their impact on the authorization logic, potentially increasing maintenance complexity.

The architecture for authorization requests from the application container is shown in Figure 6.

![Figure 6: Integration of Amazon Verified Permissions in the application container](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/img6.png)

Figure 6: Integration of Amazon Verified Permissions in the application container

The workflow is as follows:

1. Authenticated users access the application through an Elastic Load Balancer—either Application Load Balancer (ALB) or NLB depending on workload requirements.
2. The Kubernetes service or Ingress for the backend application is directly registered at the ALB or NLB by the AWS Load Balancer Controller.
3. Requests are directly routed to a pod that is backing the service.
4. The backend application’s logic is responsible for identifying whether a request needs authorization. The backend uses an IAM role injected at runtime through Pod Identity, when an authorization decision from Verified Permission is needed. The backend application returns HTTP status code 403 if the decision is a deny; otherwise it will continue processing the request.

See the Simplify fine-grained authorization with Amazon Verified Permissions and Amazon Cognito blog post for details on calling Verified Permissions within an application.

## Choosing the right pattern for your app

You now have a set of patterns to introduce authorization into your containerized workloads. You need to consider multiple factors and understand the trade-offs that come with each pattern to identify the best option for a given scenario. In the following table, we list certain areas with influence on your architectural decisions.

| **Granularity of authorization decisions** |  |
| --- | --- |
| **API gateway** |   - Authorization on API or service level - Decision based on information from HTTP request - Suitable for consistent coarse-grained authorization   |
| **Ingress controller** |   - Authorization on API or service level - Decision based on information from HTTP request - Suitable for consistent coarse-grained authorization   |
| **Sidecar proxy** |   - Authorization on service level - Decision based on information from HTTP request and service domain (such as policy information point or static service-specific rules) - Suitable for service-specific authorization with low or mid-level complexity for decisions   |
| **Application container** |   - Authorization on code level - Decision based on information from HTTP request and arbitrary business logic - Suitable for highly complex decision logic   |
| **Resource overhead** |  |
| **API gateway** |   - No cluster resources needed for authorization - Unauthorized requests don’t consume cluster resources   |
| **Ingress controller** |   - Central Ingress pods consume cluster resources - Unauthorized requests don’t consume application pod resources   |
| **Sidecar proxy** |   - Authorization services increases resource demand of each pod in the cluster - Unauthorized requests consume application pod resources   |
| **Application container** |   - Authorization service consumes resources only when authorization is performed - Unauthorized requests consume CPU cycles of application logic until the authorization logic is triggered   |
| **Scalability** |  |
| **API gateway** |   - Fully managed serverless service   |
| **Ingress controller** |   - Scaling of Ingress pods needs to be defined - Cluster auto-scaling or capacity planning for compute resources in cluster needed   |
| **Sidecar proxy** |   - Existing scaling policies can be leveraged   |
| **Application container** |   - Existing scaling policies can be leveraged   |
| **Performance** |  |
| **API gateway** |   - Invokes Verified Permission in AWS for each request that needs authorization - Supports caching to reduce number of requests to Verified Permission out of the box   |
| **Ingress controller** |   - Invokes Verified Permission in AWS for each request that needs authorization - (Optional) Integration of avp-local-agent to minimize number of requests to Verified Permissions   |
| **Sidecar proxy** |   - Invokes Verified Permissions in AWS for each request that needs authorization - (Optional) Integration of avp-local-agent to minimize number of requests to Verified Permissions   |
| **Application container** |   - Invokes Verified Permissions in AWS only if the business logic processing a request requires authorization - (Optional) Integration of avp-local-agent to minimize number of requests to Verified Permissions   |
| **Cost** |  |
| **API gateway** |   - Consumption-based costs depending on requests received and data transferred out, and optionally cache size - Consumption-based costs to invoke Verified Permissions - Enabling a cache potentially reduces costs per requests   |
| **Ingress controller** |   - Adds to infrastructure costs depending on the underlying compute resources needed to run the fleet of pods for Ingress and authorization service - Consumption-based costs to invoke Verified Permissions, which can be minimized by integrating avp-local-agent   |
| **Sidecar proxy** |   - Adds to infrastructure costs depending on the underlying compute resources needed to run the sidecar containers for the proxy and authorization service in each pod - Consumption-based costs to invoke Verified Permissions, which can be minimized by integrating avp-local-agent   |
| **Application container** |   - Typically minimal additional costs, since authorization code shares the application’s resources - Consumption-based costs to invoke Verified Permissions, which can be minimized by integrating avp-local-agent   |
| **Portability** |  |
| **API gateway** |   - Portability is limited and depends on API Gateway with functionality for custom authorization   |
| **Ingress controller** |   - Portable across Kubernetes environments with Ingress controller supporting external authorization   |
| **Sidecar proxy** |   - Portable across Kubernetes environments   |
| **Application container** |   - Highly portable without dependencies to underlying components   |
| **Complexity** |  |
| **API gateway** |   - Offered as central component by platform engineering team to offload complexity of authorization from product teams - Changes in authorization service impact product teams   |
| **Ingress controller** |   - Offered as central component by platform engineering team to offload complexity of authorization from product teams - Changes in authorization service impact product teams   |
| **Sidecar proxy** |   - Platform engineering teams provide standardized patterns (and optionally implementation) for authorization that can be integrated and implemented by product teams - Increases autonomy of individual teams   |
| **Application container** |   - Full responsibility for authorization lies with the product teams - Increases autonomy of individual teams   |

Not all services have the same requirements for authorization. You can also combine the patterns discussed in this post. You can, for example, put a basic authorization workflow with coarse-grained access control in front of the majority of your services. You can then rely on sidecar proxies with policy information points to inject additional, dynamic context into authorization decisions for specific services. Lastly, if certain use cases of a service demand complex authorization decisions, you can fall back to application-level authorization for specific parts of your code base.

## Conclusion

In this blog post, we explored four patterns for integrating Verified Permissions into a containerized environment. We discussed the benefits and considerations of implementing Verified Permissions at different levels: from an API gateway outside of Kubernetes clusters, by means of Ingress controllers and sidecar proxies as network components inside the Kubernetes cluster, to authorization within the application itself. We saw how each pattern offers unique advantages. We also discussed considerations for finding a suitable option for your situation and when to combine patterns.

By using Verified Permissions, organizations can implement consistent, fine-grained authorization across their containerized workloads, whether they’re running on-premises, in the cloud, or in hybrid environments. This centralized approach to authorization can enhance security and simplify policy management and application development.

To learn more about implementing these patterns and best practices, visit the Amazon Verified Permissions User Guide. For hands-on experience, we recommend exploring the Verified Permissions workshop, which provides practical examples and guided exercises.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Manuel Heinkel](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/02/07/heinkel.jpg) Manuel Heinkel  
Manuel is a Solutions Architect at AWS, working with software companies in Germany to build innovative and secure applications in the cloud. He supports customers in solving business challenges and achieving success with AWS. Manuel has a track record of diving deep into security and SaaS topics. Outside of work, he enjoys spending time with his family and exploring the mountains.

![Markus Kokott](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/03/12/mkokott.jpg) Markus Kokott  
Markus is a Senior Solutions Architect at AWS, specializing in guiding software companies to become successful SaaS providers. With over a decade of experience in consulting as well as designing, building, and operating software products, he excels in bridging the gap between business and technology. Markus is passionate about containers, platform engineering, and DevOps in general, using his expertise to drive innovation and efficiency.

Go to Source
