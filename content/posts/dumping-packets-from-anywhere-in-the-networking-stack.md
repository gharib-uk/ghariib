---
title: "Dumping packets from anywhere in the networking stack"
date: 2025-01-10
---

Dumping traffic on a network interface is one of the most performed steps while debugging networking and connectivity issues. On Linux, tcpdump is probably the most common way to do this, but some use Wireshark too.

## Where does tcpdump get the packets from?

Internally, both `tcpdump` and `Wireshark` use the Packet Capture (`pcap`) library. When capturing packets, a socket with the `PF_PACKET` domain is created (see `man packet`) which allows you to receive and send packets at the layer 2 from the OSI model.

From libpcap:

```plaintext
sock_fd = is_any_device ?
       socket(PF_PACKET, SOCK_DGRAM, 0) :
       socket(PF_PACKET, SOCK_RAW, 0);
```

Note that the last parameter in the socket call is later set to a specific protocol, or `ETH_P_ALL` if none is explicitly provided. The latter makes all packets to be received by the socket.

This allows to get packets directly after the device driver in ingress, without any change being made to the packet, and right before entering the device driver on egress. Or to say it differently packets are seen between the networking stack and the NIC drivers.

## Limitations

While the above use of `PF_PACKET` works nicely, it also comes with limitations. As packets are retrieved from a very specific and defined place of the networking stack, they can only be seen in the state they were at that point, e.g., on ingress packets are seen before being processed by the firewall or qdiscs, and the opposite is true on egress.

## Offline analysis

By default, `tcpdump` and `Wireshark` process packets live at runtime. But they can also store the captured packets data to a file for later analysis (`-w` option for `tcpdump`). The `pcap` file format (`application/vnd.tcpdump.pcap`) is used. Both tools (and others, e.g., tshark), support reading `pcap` formatted files.

## How to capture packets from other places?

Retrieving packets from other places of the networking stack using `tcpdump` or `Wireshark` is not possible. However, other initiatives emerged and targeted monitoring traffic within a single host, like Retis (documentation).

Retis is a recently released tool aiming at improving visibility into the Linux networking stack and various control and data paths. It allows capturing networking-related events and providing relevant context using eBPF, with one notable feature being capturing packets on any (packet-aware—AKA socket buffer) kernel function and tracepoint.

To capture packets from the `net:netif_receive_skb` tracepoint:

```plaintext
$ retis collect -c skb -p net:netif_receive_skb
4 probe(s) loaded
4581128037918 (8) [irq/188-iwlwifi] 1264 [tp] net:netif_receive_skb
 if 4 (wlp82s0) 2606:4700:4700::1111.53 > [redacted].34952 ttl 54 label 0x66967 len 79 proto UDP (17) len 71
```

Note that Retis can capture packets from multiple functions and tracepoints by using the above `-p` option multiple times. It can even identify packets and reconstruct their flow! To get a list of compatible functions and tracepoints, use `retis inspect -p`.

Also it should be noted that by default `tcpdump` and `Wireshark` put devices on promiscuous mode when dumping packets from a specific interface. This is not the case with Retis. An interface can be set in this mode manually by using `ip link set <interface> promisc on`.

In addition to the above, another tool provides a way to capture packets and convert them to a `pcap` file: bpftrace. It is a wonderful tool but is more low-level and requires to you write the probe definitions by hand and for compilation of the BPF program to take place on the target. Here the `skboutput` function can be used, as shown in the help.

## Making the link

That's nice, but while Retis is a powerful tool when used standalone, we might want to use the existing `tcpdump` and `Wireshark` tools but with packets captured from other places of the networking stack.

This can be done by using the Retis `pcap` post-processing command. This works in two steps: first Retis can capture and store packets, and then post-process them. The `pcap` sub-command allows converting Retis saved packets to a `pcap` format. This can then be used to feed existing `pcap`\-aware tools, such as `tcpdump` and `Wireshark`:

```plaintext
$ retis collect -c skb -p net:netif_receive_skb -p net:net_dev_start_xmit -o
$ retis print
4581115688645 (9) [isc-net-0000] 12796/12797 [tp] net:net_dev_start_xmit
 if 4 (wlp82s0) [redacted].34952 > 2606:4700:4700::1111.53 ttl 64 label 0x79c62 len 59 proto UDP (17) len 51
4581128037918 (8) [irq/188-iwlwifi] 1264 [tp] net:netif_receive_skb
 if 4 (wlp82s0) 2606:4700:4700::1111.53 > [redacted].34952 ttl 54 label 0x66967 len 79 proto UDP (17) len 71

$ retis pcap --probe net:net_dev_start_xmit | tcpdump -nnr -
01:31:55.688645 IP6 [redacted].34952 > 2606:4700:4700::1111.53: 28074+ [1au] A? redhat.com. (51)

$ retis pcap --probe net:netif_receive_skb -o retis.pcap
$ wireshark retis.pcap
```

As seen above, Retis can collect packets from multiple probes during the same session. All packets seen on a given probe can then be filtered and converted to the `pcap` format.

When generating `pcap` files, Retis adds a comment in every packet with a description of the probe the packet was retrieved on:

```plaintext
$ capinfos -p retis.pcap
File name:           retis.pcap
Packet 1 Comment:    probe=raw_tracepoint:net:netif_receive_skb
```

# Conclusion

In many cases, tools like `tcpdump` and `Wireshark` are sufficient. But, due to their design, they can only dump packets from a very specific place of the networking stack, which in some cases can be limiting. When that's the case it's possible to use more recent tools like Retis, either standalone or in combination with the beloved pcap aware utilities to allow using familiar tools or easily integrate this into existing scripts.

The post Dumping packets from anywhere in the networking stack appeared first on Red Hat Developer.  
  

Go to Source
