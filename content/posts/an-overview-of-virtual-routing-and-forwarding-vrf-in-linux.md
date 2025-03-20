---
title: "An overview of virtual routing and forwarding (VRF) in Linux"
date: 2025-01-08
tags: 
  - "tech"
---

Virtual routing and forwarding (VRF) is a network virtualization technique allowing traffic isolation at the layer 3 (L3) of the OSI model by creating independent routing and forwarding domains. It is essentially the combination of a separate routing table and associated network interfaces.

Other networking techniques exist but are operating at a different levels—such as VLAN and network namespaces, respectively at the layer 2 and at the device level. Those techniques can be combined, e.g., a VRF can be part of a network namespace and/or VLANs can be attached to a VRF. Note that a network namespace might look like a way to emulate a VRF but they differ in how traffic is handled, notably VRF does not impact layer 2 (L2) traffic.

In the Linux kernel VRF is supported under the `CONFIG_NET_VRF` build configuration option.

## Use cases

As we saw, a VRF is used to isolate L3 traffic. As such it can be used in multi-tenancy environments to isolate customers or to support overlapping networks, as L3 addresses and routes are scoped to a given VRF. When VRF was introduced, it was used in combination with MultiProtocol Label Switching (MPLS), which can be used to allow overlapping traffic to be routed in the same backbone. Because of this, strictly speaking, when VRF is used without MPLS it is called "VRF Lite". We'll keep referring to "VRF" in this article.

Another use case is to have a clear separation between the management network and the data plane. In short and depending on the requirements, VRF has value for both routing packets and at the endpoints.

## Linux implementation

In Linux, a VRF domain is represented by a virtual L3 network device and packets (ingress and egress) are flowing through it. This has the advantage of providing a single point to attach TC and netfilter rules, to dump non-forwarding traffic for debugging or for making applications "VRF-aware" by binding sockets to the VRF device. The VRF device also acts as a parent for interfaces part of the domain (note that a given network interface can only be part of a single domain). It can also be nested under a network namespace to combine both features.

In addition to the VRF interface, a number of techniques are used which are not specific to VRF: a dedicated routing table per VRF domain and routing policy (ip rules), for instance.

This design—mixing VRF specific techniques and non-VRF ones—is a key point to understand how VRF is implemented in Linux: the VRF implementation is not entirely isolated from the rest of the stack. As such, configuring a VRF implies managing its associated network interface but also handling policy rules and routing tables. The combination of all those techniques provides the routing and forwarding domains.

### Under the hood

When a packet is received on an interface, the route lookup is redirected to the right table using policy routing. When a packet is sent, the device is set to the VRF parent device which will ensure the right table is used for the lookup.

## VRF in action

As a VRF domain is represented by a virtual interface on Linux, adding a new VRF domain is similar and as simple as adding other kinds of virtual interfaces. The usual tools can be used, such as `iproute2` or `nmcli`. When adding a new VRF device, a dedicated routing table and (if it does not exist yet) special L3 FIB rule (policy routing) are automatically added. The policy routing rule will direct lookups for matching packets to the routing table associated with the VRF domain.

L3-enabled network devices that should be part of the VRF domain are then added by making them children of the VRF device. Their local and connected routes are automatically moved to the VRF associated routing table but other routes depending on the device are dropped. Note that because of backward compatibility, global IPv6 addresses are lost by default (this is controlled by the `net.ipv6.conf.all.keep_addr_on_down` parameter).

Let's add a VRF domain and see what happens:

```plaintext
$ ip rule show
0:        from all lookup local
32766:    from all lookup main
32767:    from all lookup default
$ ip -6 rule show
0:        from all lookup local
32766:    from all lookup main

$ ip link add vrf42 type vrf table 42
$ ip link set vrf42 up
$ ip -d link show vrf42
4: vrf42: <NOARP,MASTER,UP,LOWER_UP> mtu 65575 qdisc noqueue state UP mode DEFAULT group default qlen 1000
   link/ether be:d1:bb:4e:6f:be brd ff:ff:ff:ff:ff:ff promiscuity 0 allmulti 0 minmtu 1280 maxmtu 65575
   vrf table 42 ...
   
$ ip rule show
0:        from all lookup local
1000:     from all lookup [l3mdev-table]
32766:    from all lookup main
32767:    from all lookup default
$ ip -6 rule show
0:        from all lookup local
1000:     from all lookup [l3mdev-table]
32766:    from all lookup main
```

In the above example we can see adding a new VRF device automatically added a new routing table (table 42) and rules for VRF devices were added in the routing policy with a special `[l3mdev-table]` lookup. A single rule (per-protocol) is used for all VRF devices.

To add existing devices to the VRF domain, we can then use `ip link set dev <device> master vrf42`.

For an example on how to do the above using `nmcli`, see this article.

### Note on the default route order

As shown previously, the policy routing rules for VRF route lookups are added with a priority of 1000. As a consequence, rules with an higher priority (lower number) can match before the VRF one. By default this happens for the local table rule. See below:

```plaintext
$ ip -6 rule
0:        from all lookup local
1000:     from all lookup [l3mdev-table]
32766:    from all lookup main
$ ip -6 route show table local
local ::1 dev lo proto kernel metric 0 pref medium
```

Packets sent to `::1` will be delivered to the `lo` interface in the default domain even if `::1` is also set to an interface in the VRF:

```plaintext
$ ip -6 address add ::1/128 dev vrf42
$ ip -6 address show lo
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
   inet6 ::1/128 scope host
      valid_lft forever preferred_lft forever
$ ip -6 address show vrf42
32: vrf42: <NOARP,MASTER,UP,LOWER_UP> mtu 65575 qdisc noqueue state UP group default qlen 1000
   inet6 ::1/128 scope host
      valid_lft forever preferred_lft forever
$ ip -6 route get ::1
local ::1 from :: dev lo table local proto kernel src ::1 metric 0 pref medium
$ ip -6 route get ::1 dev vrf42
local ::1 from :: dev lo table local proto kernel src ::1 metric 0 pref medium
```

Similar behavior can be seen for all policy routing rules matching before the VRF one. This can be leveraged to have a fine-grained configuration: rules can be added before, per-VRF rules can be used instead of the global one, or the VRF specific rule can be deleted or reordered.

## Making applications VRF-aware

By default applications are not bound to any routing domain. Programs we want to be VRF-aware must explicitly do so. This can be done in various ways, as stated in `man ip vrf`: "A process can specify a VRF using several APIs -- binding the socket to the VRF device using SO\_BINDTODEVICE, setting the VRF association using IP\_UNICAST\_IF or IPV6\_UNICAST\_IF, or specifying the VRF for a specific message using IP\_PKTINFO or IPV6\_PKTINFO."

Sometimes existing applications do not implement the above. For those cases, all sockets of a program can be bound to the VRF device (`SO_BINDTODEVICE`) using `ip vrf exec <vrf device> <cmd>`:

```plaintext
$ ip vrf exec vrf42 python3 -m http.server &
$ ip vrf pids vrf42
 788  python3
```

By default the scope of unbound sockets and bound (to a VRF device) ones do not cross for TCP and UDP. This means that packets received on interfaces in a VRF domain won't be delivered to unbound sockets outside the domain. This behavior is controllable using two `sysctl` parameters:

```plaintext
$ sysctl -w net.ipv4.tcp_l3mdev_accept=1
$ sysctl -w net.ipv4.udp_l3mdev_accept=1
```

For RAW sockets a parameter exists as well but is enabled by default (for backward compatibility): `net.ipv4.raw_l3mdev_accept`.

## Routing between VRFs

On a system hosting multiple VRF domains, if the networks aren't overlapping, routing between the domains can be as simple as adding a route entry. E.g., in order to route packets `1111:1::/64 -> 1111:2::/64` from `vrf1` to `vrf2` (and vice versa):

```plaintext
$ sysctl -w net.ipv6.conf.all.forwarding=1
$ ip -6 route add 1111:2::/64 dev vrf2 table 41
$ ip -6 route add 1111:1::/64 dev vrf1 table 42
```

## Conclusion

VRF domains are a lightweight solution for isolating L3 traffic and are useful in a number of cases in both routers and endpoints. On Linux, the feature is built on top and reusing existing features (routing tables, policy routing) which allows great flexibility in configuring the desired behavior. However, this also requires to understand the big picture and how everything play with the rest.

The post An overview of virtual routing and forwarding (VRF) in Linux appeared first on Red Hat Developer.  
  

Go to Source
