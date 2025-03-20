---
title: "<div>DigitalOcean VPC Peering: A Technical Deep Dive</div>"
date: 2025-02-01
---

DigitalOcean has recently launched VPC peering GA, enabling private network connectivity between VPC Networks. This release unlocks some major customer requests including:

- Ability to connect VPCs within and across data centers
    
- VPC Native DigitalOcean Kubernetes
    
- VPC peering for DBaaS (MySQL, Postgres, Redis, MongoDB, Vector DB)
    

In this technical deep dive, we’ll explore the architecture, implementation details, and key considerations behind this new feature.

## VPC Peering Architecture Overview

The VPC peering implementation leverages the existing VPC building blocks as described in the VPC: Behind the Scenes blog. On top of that, we built a new L3 routing component to enable routing between different VPC networks. This architecture enables transit of different kinds of networks and hence we named it as the Transit Gateway. The core routing software on the HV is enabled by Open vSwitch (OVS). OvS provides the capability to express the VPC datapath with OpenFlow specification.

![vpc peering architecture](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/VPC%20Peering%20Architecture%20-%20E2E%20Routing%20(5).png)

## Core Components

### Transit Gateway (TGW)

The Transit Gateway (TGW) can be looked at as a system of loosely coupled, single purpose components that work together to enable transit of different kinds of networks. In the initial version of TGW, its one and only responsibility is to enable routing between different VPC networks within and across data centers. With incremental upgrades, more transit functionality can be added to the system to enable transit of other kinds of networks into and out of DO VPC Networks. For example, connecting an AWS VPC to DO VPC over an interconnect connection or stretching VPC connectivity into a third party NFS vendor’s private network.

### Network State Management

The TGW system is built on commodity x86 hosts with routing functionality developed using XDP and eBPF constructs for fast packet processing. Network state management is done using eBPF maps which are looked up by the eBPF router code for VXLAN routing.

Let’s look at the BPF map definitions for modeling peering connections and peering memberships. The overall idea here is to create two separate maps - a peering map and a forwarding map. The peering map holds peering relationships between VPCs and helps key into the right VPC as packets arrive on the TGW, while the forwarding map holds detailed endpoint information for packet rewriting and forwarding. An example may make this more clear -

Peering Map: (holds the peering relationships)

If VPC with an ID 1 and subnet 10.104.0.0/20 is peered with VPC with an ID 2 and subnet, 192.168.10.0/24, the peering map would need the following entries.

(1, 192.168.10.0/24) -> 2

(2, 10.104.0.0/20) -> 1

Forwarding Map: (holds the virtual chassis members)

(1, 10.104.0.2) -> (mac1, hv\_ip1)

(1, 10.104.0.3) -> (mac2, hv\_ip2)

(1, 10.104.0.4) -> (mac3, hv\_ip3)

(1, 10.104.0.5) -> (mac4, hv\_ip4)

(1, 10.104.0.6) -> (mac5, hv\_ip5)

Forwarding Logic:

Packet going from 10.104.0.2 -> 192.168.10.3. Will arrive at the transit gateway with VNI = 1 in the vxlan header.

Lookup peering map: (1, 192.168.10.3) -> 2

Lookup forwarding map: (2, 192.168.10.3) -> (mac, hv\_ip)

After the forwarding map lookup, the packet’s inner ethernet header is modified to have the destination Droplet’s MAC address as returned by the forwarding map and the outer destination IP is updated to be the destination hypervisor IP so that the packet can be transmitted to the destination HV.

### Control Plane

As we discussed in the previous blog, the VPC datapath is implemented with VXLAN with Open vSwitch (OVS) being used to program the required packet flows. Every VPC is isolated in its own unique VXLAN tunnel and therefore the primary responsiblity for the control plane to express the VPC peering datapath boils down to two high level items -

- Programming the OvS flows to steer traffic to resources in peered VPCs via the transit gateway
    
- Programming the transit gateway for peered VPCs to enable inter VXLAN routing
    

![control plane architecture](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/VPC%20Peering%20Architecture%20-%20Transit%20Gateway%20(9).png)

Regional Networking Service (RNS), is the core control plane service for VPC. RNS is responsible for persisting peering connections. More details on RNS. As the VPC Peering control plane, RNS needs to know the state of peering relationships as this information is needed to properly program the Transit Gateway cluster. When a user request comes in to create a peering connection, the primary thing that needs to happen is to transactionally persist the peering connection details in the RNS DB so we track the source of truth (desired state) and then program the Transit Gateways cluster with peering relationships. A peering data structure (eBPF map) that holds the peering relationships between VPCs exists on the TGW. This peering map remains constant for the lifetime of a peering relationship. When a peering connection is made, the peering map needs to be programmed on the gateway cluster. Each gateway listens to VPC changes using a stream RPC provided by RNS.

Whenever a Droplet is added or removed from the VPC, the forwarding map on the Transit Gateway needs to be updated for that chassis so that the Transit Gateway is made aware of the member’s routing information (VNI, destination HV and MAC address).

**Data Plane - Transit Gateway Architecture**

![data plane image](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/VPC%20Peering%20Architecture%20data%20plane.png)

The diagram above shows the Transit Gateway architecture in detail. To facilitate inter VXLAN traffic for inter VPC routing, the Transit Gateway has a leg on the network on which hypervisors are running. VPC flows in OVS are configured to send IPv4 traffic for a destination in a different VPC to the TGW. There is a router program that is meant to parse the headers and do the VXLAN routing. This program is attached to the NIC interfaces on the TGW bare metal machines which are set up in a bonded configuration (eth0, eth1 bonded with bond0). This program is designed to ensure we do not interrupt any management traffic such as ssh to the gateway host, router control plane traffic, etc.

A BPF program attached to the netdevs is responsible for forwarding packets to other VXLAN tunnels. The router program makes use of local BPF maps to rewrite the packets for enabling the transit to the peered VXLAN tunnel. Once the packet fields are modified, the packet is sent back to the same interface where it came on using the XDP\_TX construct. This packet then makes its way to the destination HV that hosts the Droplet in the peered VPC.

To summarize, the transit gateway system is composed of the following components:

1. Bare metal x86 host with an XDP compatible NIC (Nvidia CX-6) for hosting the control plane and data plane components
    
2. Gatewayd - A gRPC daemon deployed on the bare metal host to serve control plane APIs for managing BPF maps. It watches for VPC changes emitted by the VPC control plane (RNS)
    
3. VXLAN router eBPF program that is attached to an XDP hook in the NIC driver for VXLAN packet processing.
    

The Transit Gateway uses a pull-based control plane approach for managing the gateway configuration. It makes always-running bi-directional RPC calls to RNS to watch VPC events, making the appropriate changes to its BPF maps.

### VXLAN Routing

Routing was touched on briefly in the previous section as it happens inside the transit gateway. There is a local state that the gateway carries in BPF maps where it keeps track of all the active endpoints and peering connections. To be able to rewrite and properly route the packets, the router program needs to know the peering state and some metadata about the target endpoint like the VXLAN VNI, placement info (HV) and endpoint MAC address. Before we go deep into the structure of the local state that the gateway needs to track, it will help looking at a packet journey as it transits from one VXLAN tunnel to another.

### **Packet Flow**

When a packet travels between peered VPCs, it follows this path:

- Packet egresses the Droplet via VPC interface (commonly eth1 on ubuntu images)
    
- Packet source IP is set to Droplet private IP, dst is set to private IP of Droplet in a peered VPC, dst MAC is set to a static gateway MAC
    
- OVS steers the packet to the Transit Gateway (TGW)
    
- TGW updates the VNI in the VXLAN header to the VNI of the peered VPC
    
- TGW updates the dst IP in the outer IP header to the IP of HV hosting the destination Droplet
    
- TGW updates the src MAC in the inner ethernet header to the static gateway MAC
    
- TGW updates the dst MAC in the inner ethernet header to the MAC of destination Droplet
    
- TGW updates the src IP in the outer IP header to be the IP of the TGW
    
- TGW forwards the packet to the dest HV
    
- OVS on dest HV matches on VNI+dstMAC, decaps the packet and forwards to the VM port (tapint)
    

TGW’s job here is to make the packet compatible to enter the destination VXLAN tunnel. OVS flows on the destination HV remain unchanged as the VNI+dstMAC match is present to allow the packet to reach the VM port.

### OVS

OVS flows need to be added to route the traffic for peered VPCs via the gateway. In this architecture, the OVS responsibilities are limited to properly responding to ARP queries of the gateway as requested by the Droplet and steering IPv4 traffic for peered VPCs to the transit gateway. This keeps our routing stack simple as we do not have to spin up additional OVS ports or services on the HV datapath and all the inter VPC routing heavy lifting is done by the transit gateway layer.

## Conclusion

DigitalOcean’s VPC peering implementation provides a robust foundation for private network connectivity while helping to maintain security and performance. The Transit Gateway architecture, combined with eBPF-based packet processing, delivers a scalable solution that can evolve with future requirements.

Go to Source
