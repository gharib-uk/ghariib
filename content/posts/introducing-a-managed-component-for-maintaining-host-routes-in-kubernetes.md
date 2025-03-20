---
title: "<div>Introducing a Managed Component for Maintaining Host Routes in Kubernetes</div>"
date: 2025-03-19
---

Our new **DOKS routing agent** is a managed component for configuring static routes on Kubernetes worker nodes. It is a direct response to user feedback on its predecessor, the static route operator, and introduces new features to enhance routing flexibility. Despite being a managed component, the DOKS routing agent is included **at no additional cost** for users.

## Key Features of the DOKS Routing Agent

### 1\. Static Route Management via Custom Resources

The DOKS routing agent enables users to configure IP routes on their Kubernetes worker nodes using a dedicated Kubernetes Custom Resource. This is particularly useful for VPN setups or tunneling egress traffic through specific gateway nodes.

#### Example Configuration:

```
apiVersion: networking.doks.digitalocean.com/v1alpha1
kind: Route
metadata:
  name: basic
spec:
  destinations:
    - "1.2.3.4/5"  # Networks to be routed via the specified gateways
  gateways:
    - "10.114.0.3"  # Gateway IP
```

### 2\. Support for Multiple Gateways and ECMP

The routing agent supports multiple gateways and automatically configures ECMP (Equal-Cost Multi-Path) routing to distribute traffic across them.

#### Example Configuration:

```
apiVersion: networking.doks.digitalocean.com/v1alpha1
kind: Route
metadata:
  name: basic
spec:
  destinations:
    - "1.2.3.4/5"
  gateways:
    - "10.114.0.3"
    - "10.114.0.4"
```

**How ECMP Works:**

- ECMP distributes traffic across multiple gateways based on a hash of source/destination IP and port.
- If a gateway fails, the Linux kernel stops sending traffic to it.
- The routing agent pings gateways every 30 seconds to detect failures and restore traffic flow when a gateway recovers.
- **Note:** Ensure that ICMP traffic is allowed on gateways for health checks to function properly.

### 3\. Overriding Default Routes

The routing agent allows users to override default routes without disrupting cluster connectivity—one of the most requested features.

#### Example Configuration:

```
apiVersion: networking.doks.digitalocean.com/v1alpha1
kind: Route
metadata:
  name: basic
spec:
  destinations:
    - "0.0.0.0/0"  # Default route
  gateways:
    - "10.114.0.3"
    - "10.114.0.4"
```

To prevent issues with Kubernetes components, the routing agent ensures that essential control plane endpoints, metadata services, and DNS servers maintain direct connectivity through the worker node Droplet’s default gateway.

### 4\. Node Selection for Routes

Routes can be applied to specific nodes using Kubernetes label selectors, allowing for fine-grained control over network configurations.

#### Example Configuration:

```
apiVersion: networking.doks.digitalocean.com/v1alpha1
kind: Route
metadata:
  name: basic
spec:
  destinations:
    - "1.2.3.4/5"
  gateways:
    - "10.114.0.3"
  nodeSelector:
    nodeSelectorTerms:
      - matchExpressions:
          - key: doks.digitalocean.com/node-pool
            operator: In
            values:
              - "worker-pool"
```

## Enabling the DOKS Routing Agent

The routing agent can be enabled or disabled using **doctl** or the **DigitalOcean API**.

### Example Commands:

```
doctl kubernetes cluster create --enable-routing-agent ...

doctl kubernetes cluster update --enable-routing-agent ...

```

For API users, the field structure is:

```
{
  "name": "prod-cluster-01",
  "region": "nyc3",
  "routing_agent": {
    "enabled": true
  }
}

```

## Usage for Static Egress IP

With the DOKS routing agent and a self-managed VPC gateway Droplet, users can configure static egress IPs, ensuring outbound traffic from Kubernetes workloads originates from a predictable IP address.

### Why This Matters:

- **Allow-listing:** Secure external services by allow-listing known IPs.
- **Compliance:** Maintain a consistent outbound IP for regulatory requirements.

### Coming Soon: Fully Managed NAT Gateway

We’re also working on a **fully managed NAT gateway**, which will offer a simpler solution for achieving static egress IPs. This feature is on our roadmap and will be available later this year.

## Simplify Static Route Management

**The DOKS routing agent streamlines static route management in Kubernetes, providing:**

- Custom routes using Kubernetes Custom Resources
- Load distribution across multiple gateways with ECMP
- Default route overrides without disrupting cluster connectivity
- Node-specific routing with label selectors

These features are especially useful for VPN setups, custom egress routing, and self-managed VPC gateways.

## Get started today

- Explore our product documentation
    
- Visit our Managed Kubernetes homepage
    
- Log into your DigitalOcean account
    
- Access the release notes
    
- Check out the How-To Guide
    

Go to Source
