---
title: "How VXLAN encapsulation affects Arm (aarch64) servers"
date: 2025-02-03
---

In the previous article, Linux on Arm (aarch64) servers..., we examined flat networks. That is, we used only bare network interfaces and nothing else. That's a good start, but it is often not the topology used, especially with solutions like OpenStack. So in this installment, let’s evaluate how impactful VXLAN encapsulation can be for throughput and CPU consumption.

We’ll reuse the hosts and testing procedures, with only the modifications to add VXLAN. For details on the test environment, please refer to the previously mentioned article.

## VXLAN configuration

Each host has a VXLAN tunnel to the other host. For reference, the following is the NMState YAML file used for one of the hosts:

```plaintext
interfaces:
- name: vxlan08-09
  type: vxlan
  state: up
  mtu: 1450
  ipv4:
    address:
    - ip: 192.168.2.1
      prefix-length: 24
    enabled: true
    dhcp: false
  vxlan:
    base-iface: enp1s0f0np0
    id: 809
    remote: 192.168.1.2
routes:
  config:
    - destination: 192.168.2.2/32
      next-hop-interface: vxlan08-09
```

## Tests with ELN kernal

With an ELN kernel, kernel-6.9.0-0.rc4.37.eln136, the results are shown in Figure 1. Each pair of bars is a test. For example, the first pair, the generated traffic is in green while the received traffic by the device under test (DUT) is in purple. For transmission control protocol (TCP), they are the same, but they will vary on user datagram protocol (UDP) tests. The error markers are standard deviation out of 10 runs of 1 minute.

For the CPU/throughput graphs, the total CPU consumption for both cores, application, and interrupt request (IRQ) are summed (theoretical maximum of 200 then) and then divided by the observed throughput. The smaller the number, the better (less CPU used to pump the same amount of traffic).

TCP throughput: Figure 1 shows that encapsulation adds a toll. We can see it when compared to the results in previous article. It goes about 60% of the original throughput. 

<figure>

![Bar graph showing DUT being able to receive 24Gbps and send 40Gbps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-tput-selected.png?itok=Querv_65)

<figcaption>

Figure 1: TCP throughput.

</figcaption>

</figure>

The application CPU, which was just 11.1% free, now gets some more slack because the IRQ one needs to do more work for the same packet, as seen in Figures 2 and 3 and their respective tables. Like before, what limits the throughput is the receiver IRQ CPU usage in all cases. Similar to the test with bare interfaces, it’s interesting to note that DUT was able to send traffic as fast as the traffic generator could receive while still having nearly half of the CPU power left unused.

<figure>

![Bar graph showing DUT using 6.5% of CPU per Gbps on receive](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-cputput-selected-rx.png?itok=M6mm19QM)

<figcaption>

Figure 2: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     1.8+-0.3   
CPU App %sys     54.2+-0.6  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    44.0+-0.5  
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  99.9+-0.1  
CPU S-IRQ %idle  0.1+-0.0
```

<figure>

![Bar graph showing DUT using 2.5% CPU per Gbps sent](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-tput-selected-tx.png?itok=lDlWLwxJ)

<figcaption>

Figure 3: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     0.7+-0.1
CPU App %sys     56.0+-1.1
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    43.3+-1.0
CPU S-IRQ %usr   0.0+-0.0
CPU S-IRQ %sys   0.1+-0.3
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  53.1+-0.6
CPU S-IRQ %idle  46.8+-0.6
```

### UDP 1500 bytes

Figure 4 shows that the throughput is halved compared to the test with bare interfaces, which is expected for this topology. 

<figure>

![Bar graph showing DUT being able to send 3.6GBps and receive 3.7Gbps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-1500-tput.png?itok=oPO4lQnR)

<figcaption>

Figure 4: UDP 1500 bytes throughput.

</figcaption>

</figure>

We can observe that the CPU usage pattern on the sending side is similar to the bare interface test with the application CPU being the bottleneck. As seen in Figures 5 and 6 and their respective tables, most of the VXLAN transmit processing is done at the application CPU, while the IRQ CPU is only doing the transmit completion. This means just freeing the sent packets, so it puts a considerable pressure on the sending application CPU. Conversely, on the receiving side, the DUT IRQ CPU gets fully busy, causing 42% idle time to show up now in the DUT application CPU.

<figure>

![Bar graph showing DUT using 42% CPU per Gbps received](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-1500-tput-selected-rx.png?itok=u61k45b7)

<figcaption>

Figure 5: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     6.9+-0.2   
CPU App %sys     50.9+-0.4  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    42.2+-0.4  
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  99.9+-0.0  
CPU S-IRQ %idle  0.0+-0.0 
```

<figure>

![Bar graph showing DUT using 34% CPU per Gbps when sending](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-1500-cputput-selected-tx.png?itok=NVh7wMwV)

<figcaption>

Figure 6: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     4.2+-0.4
CPU App %sys     95.7+-0.4
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    0.1+-0.0
CPU S-IRQ %usr   0.1+-0.2
CPU S-IRQ %sys   0.0+-0.0
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  23.7+-0.9
CPU S-IRQ %idle  76.2+-1.0
```

### UDP 60 bytes

In this test, the encapsulation toll is less expressive because the stack is already under pressure due to the small packet size. The increased overhead due to the VXLAN processing is not as expressive as in the previous test, as seen in Figures 7-9 and their respective tables.

<figure>

![Bar graph showing DUT being able to receive 90Mbps and send 65Mbps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput.png?itok=WKL8SwXJ)

<figcaption>

Figure 7: UDP 60 bytes throughput.

</figcaption>

</figure>

<figure>

![Bar graph showing DUT using 1.7% CPU per Mbps on receive](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput-selected-rx.png?itok=BvxMnvJn)

<figcaption>

Figure 8: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     15.0+-0.5  
CPU App %sys     85.0+-0.5  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    0.0+-0.0   
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  58.9+-1.5  
CPU S-IRQ %idle  41.0+-1.5
```

<figure>

![Bar graph showing DUT using 1.8% CPU per Mbps sent](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput-selected-tx.png?itok=2RO06y_q)

<figcaption>

Figure 9: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     6.5+-0.3
CPU App %sys     93.4+-0.3
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    0.1+-0.0
CPU S-IRQ %usr   0.1+-0.3
CPU S-IRQ %sys   0.0+-0.0
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  19.1+-0.8
CPU S-IRQ %idle  80.9+-0.8
```

## Tests with RHEL 9.4

Red Hat Enterprise Linux 9.4 kernel is based on `kernel-5.14.0-427.el9`. That’s the one we tested here with the same procedure.

### TCP throughput

The results were the same as with the previous ELN kernel, as seen in Figures 10-12 and their respective tables. So, there is nothing else to add here.

<figure>

![Bar graph showing DUT being able to receive 24Gbps and send 40Gbps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-tput-selected_0.png?itok=6J98RSXo)

<figcaption>

Figure 10: TCP throughput.

</figcaption>

</figure>

<figure>

![Bar graph showing DUT using 7% CPU per Gbps received](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-cputput-selected-rx_0.png?itok=bQAlWXOl)

<figcaption>

Figure 11: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     3.0+-0.5   
CPU App %sys     64.1+-0.7  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    32.9+-0.5  
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  100.0+-0.1 
CPU S-IRQ %idle  0.0+-0.0 
```

<figure>

![Bar graph showing DUT using 2.7% CPU per Gbps sent](https://developers.redhat.com/sites/default/files/styles/article_floated/public/tcp-throughput-cputput-selected-tx.png?itok=kVgz60tA)

<figcaption>

Figure 12: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     0.7+-0.2
CPU App %sys     58.4+-1.4
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    40.9+-1.3
CPU S-IRQ %usr   0.0+-0.0
CPU S-IRQ %sys   0.0+-0.0
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  53.6+-0.9
CPU S-IRQ %idle  46.4+-0.9
```

### UDP 1500 bytes

The results were quite the same as with the ELN kernel above, as can be seen in Figures 13-15 and their respective tables. Once again, I have nothing else to add here.

<figure>

![Bar graph showing DUT being able to receive 4Gbps and send 3.5GBps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput_0.png?itok=Pz7O6Goe)

<figcaption>

Figure 13: UDP 1500 bytes throughput.

</figcaption>

</figure>

<figure>

![Bar graph showing DUT using 40% CPU per GBps received](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-1500-cputput-selected-rx.png?itok=4f1qllEN)

<figcaption>

Figure 14: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     7.6+-0.4   
CPU App %sys     56.5+-0.7  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    35.9+-0.6  
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  99.9+-0.0  
CPU S-IRQ %idle  0.0+-0.0 
```

<figure>

![Bar graph showing DUT using 35% CPU per Gbps sent](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-1500-tput-selected-tx.png?itok=GRS4njzm)

<figcaption>

Figure 15: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     3.6+-0.2
CPU App %sys     96.3+-0.2
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    0.1+-0.0
CPU S-IRQ %usr   0.0+-0.0
CPU S-IRQ %sys   0.0+-0.0
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  23.3+-0.5
CPU S-IRQ %idle  76.7+-0.5
```

### UDP 60 bytes

Once again, the results were quite the same as with the previous ELN kernel, as seen in Figures 16-18 and their respective tables with nothing else to add.

<figure>

![Bar graph showing DUT being able to receive 85MBps and send 65Mbps](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput_1.png?itok=YOxhB4fO)

<figcaption>

Figure 16: UDP 60 bytes throughput.

</figcaption>

</figure>

<figure>

![Bar graph showing DUT using 1.8% CPU per Mbps received](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-tput-selected-rx_0.png?itok=b7nctqRD)

<figcaption>

Figure 17: DUT CPU % per throughput on receive.

</figcaption>

</figure>

```plaintext
                 DUT rcv    
CPU App %usr     13.6+-0.5  
CPU App %sys     86.4+-0.5  
CPU App %irq     0.0+-0.0   
CPU App %soft    0.0+-0.0   
CPU App %idle    0.0+-0.0   
CPU S-IRQ %usr   0.0+-0.0   
CPU S-IRQ %sys   0.0+-0.0   
CPU S-IRQ %irq   0.0+-0.0   
CPU S-IRQ %soft  55.4+-0.9  
CPU S-IRQ %idle  44.6+-0.9
```

<figure>

![Bar graph showing DUT using 1.8% CPU per Mbps sent](https://developers.redhat.com/sites/default/files/styles/article_floated/public/udp-60-cputput-selected-tx.png?itok=6x19Rrb0)

<figcaption>

Figure 18: DUT CPU % per throughput on send.

</figcaption>

</figure>

```plaintext
                 DUT snd
CPU App %usr     5.8+-0.3
CPU App %sys     94.1+-0.3
CPU App %irq     0.0+-0.0
CPU App %soft    0.0+-0.0
CPU App %idle    0.1+-0.0
CPU S-IRQ %usr   0.0+-0.0
CPU S-IRQ %sys   0.0+-0.0
CPU S-IRQ %irq   0.0+-0.0
CPU S-IRQ %soft  18.1+-0.4
CPU S-IRQ %idle  81.9+-0.4
```

## Conclusions

The DUT server kept up with the traffic generator and sustained datacenter-level traffic, as it was able to with bare interfaces. It is worth mentioning that there are many techniques to scale the application when using UDP sockets, such as UDP GRO, UDP Segmentation Offload, RPS, RFS, XPS, etc. Also, the DUT server was launched mid-2020 and aarch64 designs often have plenty of cores which the applications can use to scale.

The post How VXLAN encapsulation affects Arm (aarch64) servers appeared first on Red Hat Developer.  
  

Go to Source
