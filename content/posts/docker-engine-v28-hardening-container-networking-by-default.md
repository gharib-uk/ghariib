---
title: "Docker Engine v28: Hardening Container Networking by Default"
date: 2025-03-19
tags: 
  - "14041"
  - "22054"
  - "48815"
---

Docker simplifies containerization by removing runtime complexity and making app development seamless. With Docker Engine v28, we’re taking another step forward in security by ensuring containers aren’t unintentionally accessible from local networks. This update isn’t about fixing a single vulnerability — it’s about security hardening so your containers stay safe. 

## What happened?

When you run a container on the default Docker “bridge” network, Docker sets up **NAT (Network Address Translation)** rules using your system’s firewall (via iptables). For example, the following command forwards traffic from port 8080 on your host to port 80 in the container. 

```
docker run -d -p 8080:80 my-web-app
```

However, if your host’s `filter-FORWARD` chain is permissive (i.e., `ACCEPT` by default) **and** `net.ipv4.ip_forward` is enabled, unpublished ports could also be remotely accessible under certain conditions.

This only affects hosts on the **same physical/link-layer network** as the Docker host. In multi-tenant LAN environments or other shared local networks, someone connected on an RFC1918 subnet (such as `192.168.x.x` or `10.x.x.x`) could reach unpublished container ports if they knew (or guessed) its IP address.

## Who’s affected?

This behavior only affects Linux users running Docker versions earlier than 28.0.0 with `iptables`. Docker Desktop is **not** affected.

If you installed Docker on a single machine and used our defaults, without manually customizing your firewall settings, you’re likely unaffected by upgrading. However, you could be impacted if:

- You **deliberately set your host’s `FORWARD` chain to `ACCEPT`** and rely on accessing containers by their IP from other machines on the LAN.
- You **bound containers to `127.0.0.1`** or another loopback interface but still route external traffic to them using advanced host networking tricks.
- You **require direct container access** from subnets, VLANs, or other broadcast domains _without_ explicitly publishing ports (i.e., no `p` flags).

If any of these exceptions apply to you, you might notice that containers previously reachable by direct IP now appear blocked unless you opt out or reconfigure your networking.

## What’s the impact?

This exposure would’ve required being **on the same local network** or otherwise having route-level access to the container’s RFC1918 IP range. It **did not** affect machines across the public internet. However, a malicious user could discover unpublished container ports, within LAN settings or corporate environments, and connect to them. 

For instance, they might add a custom route to the container’s subnet, using the following command:

```
ip route add 172.17.0.0/16 via 192.168.0.10
```

From there, if `192.168.0.10` is your Docker host with a permissive firewall, the attacker could send packets straight to `172.17.0.x` (the container’s IP).

## What are my next steps?

If you’ve been impacted, we recommend taking the following three steps:

#### 1\. Upgrade to Docker Engine 28.0

Docker Engine 28.0 now drops traffic to unpublished ports by default. 

This “secure by default” approach prevents containers from being unexpectedly accessible on the LAN. Most users won’t notice a difference, published ports will continue to function as usual (`-p 8080:80`), and unpublished ports remain private as intended.

#### 2\. Decide if you need the old behavior (opt-out)

If you connect to containers over the LAN without publishing ports, you have a couple of options:

**Option 1: Disable Docker’s `DROP` policy  
**In `/etc/docker/daemon.json`, add the following:

```
{
  "ip-forward-no-drop": true
}
```

Or you can run Docker with the `--ip-forward-no-drop` flag. This preserves a globally open `FORWARD` chain. But keep in mind that Docker still drops traffic for unpublished ports using separate rules. See Option 2 for a fully unprotected alternative.

**Option 2: Create a “nat-unprotected” network**

```
docker network create -d bridge \
  -o com.docker.network.bridge.gateway_mode_ipv4=nat-unprotected \
  my_unprotected_net
```

#### 3\. Consider custom iptables management

Advanced users with complex network setups can manually configure iptables to allow exactly the traffic they need. But this route is only recommended if you’re comfortable managing firewall rules.

## Technical details

In previous Docker Engine versions, Docker’s reliance on a permissive `FORWARD` chain meant that containers on the default bridge network could be reached if:

1. `net.ipv4.ip_forward` was enabled (often auto-enabled by Docker if it wasn’t already).
2. The system-wide `FORWARD` chain was set to `ACCEPT`.
3. Another machine on the same LAN (or an attached subnet) routed traffic to the container’s IP address.

In Docker 28.0, we now explicitly drop unsolicited inbound traffic to each container’s internal IP unless that port was explicitly published (`-p` or `--publish`). This doesn’t affect local connections from the Docker host itself, but it does block remote LAN connections to unpublished ports.

### Attacks: a real-world example

Suppose you have two hosts on the same RFC1918 subnet:

- **Attacker** at `10.0.0.2`
- **DockerHost** at `10.0.0.3`

DockerHost runs a container with IP `172.17.0.2`, and Docker’s firewall policy is effectively “ACCEPT.” In this situation, an attacker could run the following command:

```
ip route add 172.17.0.2/32 via 10.0.0.3
nc 172.17.0.2 3306
```

If MySQL was listening on port 3306 in the container (unpublished), Docker might still forward that traffic. The container sees a connection from `10.0.0.2`, and no authentication is enforced by Docker’s network stack alone. 

### Mitigations in Docker Engine 28.0

By enforcing a drop rule for unpublished ports, Docker 28.0 now prevents the above scenario by default.

**1\. Default drop for unpublished ports**

Ensures that local traffic to container IPs is discarded unless explicitly published.

**2\. Targeted adjustments to `FORWARD` policy**

Originally, if Docker had to enable IP forwarding, it would set `DROP` by default. Now, Docker only applies that if necessary and extends the same logic to IPv6. You can opt-out with `--ip-forward-no-drop` or in the config file.

**3\. Unpublished ports stay private**

The container remains accessible from the host itself, but not from other devices on the LAN (unless you intentionally choose “nat-unprotected”).

### Why we’re making this change now

Some users have raised concerns about local-network exposure for years (see issues like #14041 and #22054). But there’s been a lot of change since then:

- **Docker was simpler**: Use cases often revolved around single-node setups where any extra exposure was mitigated by typical dev/test workflows.
- **Ecosystem evolution**: Overlay networks, multi-host orchestration, and advanced routing became mainstream. Suddenly, scenarios once relegated to specialized setups became common.
- **Security expectations**: Docker now underpins critical workloads in complex environments. Everyone benefits from safer defaults, even if it means adjusting older assumptions.

By introducing these changes in Docker Engine 28.0, we align with today’s best practices: **don’t expose anything without explicit user intent**. It’s a shift away from relying on users to configure their own firewalls if they want to lock things down.

### Backward compatibility and the path forward

These changes are **not** backward compatible for users who rely on direct container IP access from a local LAN. For the majority, the new defaults lock down containers more securely without breaking typical `docker run -p` scenarios.

Still, we strongly advise upgrading to 28.0.1 or later so you can benefit from:

- **Safer defaults**: Unpublished ports remain private unless you publish or explicitly opt out.
- **Clearer boundaries**: Published vs. unpublished ports are now unambiguously enforced by iptables rules.
- **Easier management**: Users can adopt the new defaults without becoming iptables experts.

### Quick upgrade checklist

Before you upgrade, here’s a recommended checklist to run through to make sure you’re set up for success with the latest release:

1. **Examine current firewall settings
    
    **Run `iptables -L -n` and `ip6tables -L -n`. If you see `ACCEPT` in the `FORWARD` chain, and you depend on that for multi-subnet or direct container access, plan accordingly.
2. **Test in a controlled environment**Spin up Docker Engine 28.0 on a staging environment. Attempt to reach containers that you previously accessed directly. Then, verify which connections still work and which are now blocked.
3. **Decide on opt-outs
    
    **If you do need the old behavior, set `"ip-forward-no-drop": true` or use a “nat-unprotected” network. Otherwise, enjoy the heightened security defaults.
4. **Monitor logs and metrics**Watch for unexpected connection errors or service downtime. If something breaks, check if it’s caused by Docker’s new drop rules before rolling back any changes.

## Conclusion

By hardening default networking rules in Docker Engine 28.0, we’re reducing accidental container exposure on local networks. Most users can continue without disruption and can just enjoy the extra peace of mind. But if you rely on older behaviors, you can opt out or create specialized networks that bypass these rules.

**Ready to upgrade?** Follow our official installation and upgrade instructions to get Docker Engine 28.0 on your system today.

We appreciate the feedback from the community and encourage you to reach out if you have questions or run into surprises after upgrading. You can find us on GitHub issues, our community forums, or your usual support channels.

### Learn more

- Review the Docker Engine v28 release notes
- Read Docker’s iptables documentation
- Issues that shaped these changes:
    - #14041
    - #22054
    - #48815

​Products, Docker, Docker engine, security
