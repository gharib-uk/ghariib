---
title: "Red Hat Connectivity Link now generally available"
date: 2025-01-23
---

Red Hat has officially announced the general availability of Red Hat Connectivity Link, a solution designed to simplify and secure traffic management across hybrid cloud environments. In this article, we will explore what Connectivity Link offers and how it can enhance your hybrid cloud strategy by streamlining connectivity and boosting security.

## What is Connectivity Link 

Connectivity Link provides Kubernetes\-native traffic management in multi-cluster environments. It leverages the power of the Kubernetes Gateway API and Envoy proxy to provide a unified, efficient approach to managing ingress traffic and related policies in multi-cluster Kubernetes environments. With Connectivity Link, platform engineers and application developers can collaborate to connect, secure, and protect distributed services and applications. 

## How Connectivity Link resolves challenges

Increasing adoption of hybrid cloud environments and the need for flexible workload deployment across diverse platforms present the challenge of maintaining secure connectivity between distributed applications. Platform engineers and developers need a simple, repeatable way to implement secure traffic management and policies for all their services spread across different clusters. 

Connectivity Link builds on the Gateway API, offering DNS management, TLS certificate lifecycle management, authentication, and rate limiting across all gateway instances in multi-cluster environments (Figure 1). These policies, along with included observability metrics, standardize ingress traffic management in both single- and multi-cluster setups.

<figure>

![Red Hat Connectivity Link extends the functionalities of gateway api](https://developers.redhat.com/sites/default/files/rhcl-gatewayapi.png)

<figcaption>

Figure 1: Red Hat Connectivity Link extends the functionalities of Gateway API.

</figcaption>

</figure>

Let’s unpack that a little bit. Gateway API is the emerging ingress and connectivity standard for Kubernetes designed to be expressive, portable, and extensible. Connectivity Link extends Gateway API via the concept of policy attachment to apply the right policies to the right resources. 

To secure the platform, engineers can set up default policies with the role-oriented Gateway API resources and Connectivity Link platform, such as a zero-trust AuthPolicy or RateLimitPolicy. Then developers can create policies specific to their services (HTTRoute) that cater to business needs and use cases, as illustrated in Figure 2.

<figure>

![Connectivity Link Traffic Flow](https://developers.redhat.com/sites/default/files/screenshot_2025-01-14_at_2.27.01_pm.png)

<figcaption>

Figure 2: Connectivity Link traffic flow.

</figcaption>

</figure>

## 7 Connectivity Link core capabilities

Let's examine the seven core capabilities unveiled with the general availability of Connectivity Link:

1. Dynamic DNS and multi-cluster routing  
2. Secure traffic with TLS policy 
3. Controlled access with Auth policy 
4. Rate limit policy
5. Observability 
6. Red Hat OpenShift console integration 
7. API Controller

### 1\. Dynamic DNS and multi-cluster routing

The DNS policy is key to bringing traffic to your gateways. It leverages common cloud DNS providers like Route 53, Azure, and Google DNS, and CoreDNS (coming soon) and auto-populates DNS records based on listener hosts and addresses defined through Gateway API resources. It enables multi-cluster connectivity and routing options, such as GEO and weighted responses.

The following is an example of DNS policy for gateway attachment and load balancing scenarios:

```plaintext
apiVersion: kuadrant.io/v1
kind: DNSPolicy
metadata:
 name: prod-web
 namespace: ingress-gateway
spec:
 targetRef:
   name: prod-web
   group: gateway.networking.k8s.io
   kind: Gateway
   sectionName: listenerName 
 providerRef:
   name: my-aws-credentials 
 loadBalancing:
   weight: 120 
   geo: GEO-EU 
   defaultGeo: true
```

### 2\. Secure traffic with TLS policy

Secure the traffic entering your Kubernetes clusters via gateways with Automated Certificate Management Environment (ACME)-based TLS integration. This works with all major ACME providers, such as Let’s Encrypt. Connectivity Link integrates with cert manager for the lifecycle management of the certificates. 

Here is an example TLS policy to configure TLS to the gateway:

```plaintext
apiVersion: kuadrant.io/v1
kind: TLSPolicy
metadata:
  name: prod-web
spec:
  targetRef:
    name: prod-web
    group: gateway.networking.k8s.io
    kind: Gateway
  issuerRef:
    group: cert-manager.io
    kind: Issuer
    name: selfsigned-issuer
EOF
```

### 3\. Controlled access with auth policy 

Auth policy protects your applications and services by providing authentication and authorization options both at the gateway level or the individual service level. The implementation of the auth policy is based on the Envoy External Authorization protocol. It enables the platform engineering team to set defaults at the organization level until they apply a more specific policy.

The following is a simple key-based authentication:

```plaintext
apiVersion: kuadrant.io/v1
kind: AuthPolicy
metadata:
  name: toystore-authn
spec:
  targetRef:
    group: gateway.networking.k8s.io
    kind: HTTPRoute
    name: toystore
  defaults:
    strategy: merge
    rules:
      authentication:
        "api-key-authn":
          apiKey:
            selector:
              matchLabels:
                app: toystore
          credentials:
            authorizationHeader:
              prefix: APIKEY
```

### 4\. Rate limit policy 

The rate limit policy guards your APIs and services against attack and abuse by implementing flexible and granular rate limits at different levels. Similar to the auth policy, the rate limit policy relies on the Envoy Rate Limit Service (RLS) protocol. Limitador is the component that takes care of rate limiting and receives requests from the gateway to enforce the rate limits.

Rate limits can be applied at the gateway level or the individual service level. The following example shows the rate limits at gateway level:

```plaintext
apiVersion: kuadrant.io/v1
kind: RateLimitPolicy
metadata:
  name: gw-rlp
spec:
  targetRef:
    group: gateway.networking.k8s.io
    kind: Gateway
    name: external
  limits:
    "global":
      rates:
      - limit: 5
        window: 10s
```

### 5\. Observability

Connectivity Link includes observability metrics and templates to integrate with Grafana, Prometheus, and alert manager deployments. Connectivity Link also includes role-based dashboards for platform engineers and developers to monitor various metrics, such as policy compliance, resource usage, error rates, latency, throughput, and multi-cluster insights (Figure 3).

<figure>

![Platform Engineer Grafana dashboard.](https://developers.redhat.com/sites/default/files/grafana-pe-dashabord.png)

<figcaption>

Figure 3: Platform Engineer Grafana dashboard.

</figcaption>

</figure>

### 6\. OpenShift console integration 

Integrated directly into the Red Hat OpenShift console is a consolidated view of all the Gateway API resources (gateways, HTTPRoutes, etc.), including their status and respective policies enforced by Connectivity Link (Figure 4).

<figure>

![OpenShift Console Plugin for Red Hat Connectivity Link](https://developers.redhat.com/sites/default/files/screenshot_2025-01-14_at_3.35.25_pm.png)

<figcaption>

Figure 4: View of the gateways and APIs in the OpenShift Console.

</figcaption>

</figure>

### 7\. API Controller 

Manage the lifecycle of your schemas and API definitions with API Controller (developer preview). While Apicurio Studio helps you design schemas and API definitions, the Apicurio Registry helps you manage and store these definitions and schemas in a centralized repository. 

## Get started with Red Hat Connectivity Link

With these powerful features now available, Red Hat Connectivity Link is set to transform how you manage and secure traffic across hybrid cloud environments. It enables platform engineers and developers alike to streamline connectivity and security in a scalable, efficient manner by leveraging the Gateway API and providing robust policies and observability tools. Whether you're operating in a single- or multi-cluster environment, Connectivity Link provides the flexibility and control needed for seamless connectivity across the hybrid cloud.

You can find more information about Connectivity Link here:

1. Product page
2. Solution Pattern: Connect, Secure and Protect with Red Hat Connectivity Link
3. Interactive demo
4. Documentation

The post Red Hat Connectivity Link now generally available appeared first on Red Hat Developer.  
  

Go to Source
