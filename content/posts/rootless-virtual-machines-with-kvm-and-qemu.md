---
title: "Rootless virtual machines with KVM and QEMU"
date: 2025-01-02
categories: 
  - "general"
---

Since containers became hot, there are fewer scenarios where IT professionals have to deal with virtual machines (VMs) in their day jobs, but from time to time you need something that’s hard or impossible to replicate inside a container and virtualization tools remain a crucial part of a developer’s toolbox.

Early container tools created huge security concerns: any user would be an effective root user, and those tools made it easy to run arbitrary code and circumvent protections provided by mechanisms such as SELinux and Linux kernel capabilities. Later container tools, such as Podman, emphasized rootless modes of operation to prevent such security concerns.

On the other hand, the most popular Linux virtualization tool, libvirt, was designed with a security first mindset. Given the lack of simple to execute but devastating security incidents, many professionals never realized that having unrestricted access to a virtualization stack _also_ comes with significant security concerns. Arguably, those security concerns are not as serious as from a rootful monolithic container engine, but you should not dismiss them without proper consideration.

## Libvirt system and session interfaces

Most professionals use libvirt from its system interface (`qemu:///system`), which allows many forms of interaction between VMs and host network and storage resources. Malicious VMs, such as VMs that were compromised by remote attacks, could damage their hypervisor hosts and networks attached to those hosts.

Few professionals are aware that they could instead use libvirt from its session interface (`qemu:///session`), which runs VMs completely isolated from their host’s storage and network resources. Session VMs run completely rootless, by default, with user mode networking and user’s files as virtual disks.

In fact, creating a session VM can be easier than creating a system VM because you don’t need to move QCOW2 and ISO files around, nor grant file system permissions to libvirt daemons for accessing files on your home directory.

## Session VMs are rootless

Maybe you didn’t know that Linux VMs are “just processes,” the same way that Linux containers are. If you can run containers rootless, you can also run VMs rootless.

Libvirt system VMs do not run as root. They run as the QEMU system user. They have only access to host resources that were previously set up by libvirt. That makes system VMs isolated from session VMs, which run as their creator user. And makes session VMs from different users isolated from each other.

**Note:** QEMU is the hardware emulator in the libvirt virtualization stack. No virtualization technology works purely on hardware-assisted virtualization, which provides virtual CPUs, virtual memory, and maybe virtual device buses. All hypervisors need some degree of hardware emulation to provide virtual video, disk, network, keyboard, and other kinds of devices. KVM provides only hardware-assisted virtualization, and QEMU provides everything else. It’s interesting to note that more of libvirt’s capabilities relate to QEMU than to KVM.

System VMs require a number of helpers from libvirt, which run with elevated privileges, to attach to host storage and networking. That means system VMs are not entirely rootless. Those helpers reduce the attach surface of libvirt and avoid the need of granting root privileges directly to any QEMU process or VM process. Libvirt will not run those helpers for session VMs, except in a few special cases which must be explicitly allowed, so session VMs are completely rootless by default.

## Networking with session VMs

The caveat with rootless VMs is that current tools do not expose the full capabilities of QEMU’s user mode networking stack. Simple tasks such as port forwarding require fiddling with libvirt XML configuration files. That means it is easy to run rootless VMs with access to external networks, but it’s harder to run rootless VMs which accept network connections from anywhere, including their own hosts.

Maybe the former is sufficient for you, and you don’t need to open network connections to anything running inside your VMs. But if it’s not, I propose a compromise, in the name of ease of use: run session VMs with access to a single virtual bridge managed by libvirt. You can connect to network ports on your VMs through that bridge, as long as you run your client software from the same host as your VM.

My proposal implies that you do NOT connect that bridge to any physical network interface of your host, so those VMs cannot accept connections from anything except from their own hosts. That way you prevent your VMs from causing major damage to networks attached to your host.

My proposal is also a temporary workaround: in due time, libvirt front-ends will support easy ways of configuring port-forwarding, and those updates will land in your preferred Linux distribution (Fedora Linux, of course) and later in enterprise distributions such as RHEL.

## Creating a rootless VM

If your Linux system is already configured with libvirt, you should be able to run informational commands, such as:

```plaintext
$ virsh version
$ virsh nodeinfo
$ virsh help
```

And especially the following, which tells if your system has the default configuration, which connects to the session instance, or if it was changed to connect to the system instance:

```plaintext
$ virsh uri
```

You should see `qemu:///session` as the result. If not, Ask a sysadmin to help you configure libvirt back to its default.

If you can, compare the output of the previous command with sudo and without it. With sudo, the result should be `qemu:///system` indicating that, as root, you access the system instance of libvirt.

If your system is not configured as a virtualization host, you only need to install a few packages (assuming a Fedora, CentOS, or Red Hat Enterprise Linux (RHEL) system):

```plaintext
$ sudo dnf install qemu-kvm libvirt virt-install virt-viewer libvirt-nss
```

Assuming you downloaded the RHEL installation ISO from Red Hat Developer, using a free subscription, you can create a VM and perform an interactive installation of RHEL with a command similar to the following:

```plaintext
$ virt-install --name myvm --osinfo rhel9.4 --memory 2048 --vcpus 2 --disk size=20 --location ~/Downloads/rhel-9.4-x86_64-boot.iso
```

It should start a virtual console viewer automatically, but if it doesn’t, you can start one and attach to your new VM with the following command:

```plaintext
$ virt-viewer myvm
```

If anything happens and you must start over, first make sure you stop your VM:

```plaintext
$  virsh destroy myvm
```

And then delete it and its virtual disk file:

```plaintext
$ virsh undefine myvm --remove-all-storage --nvram
```

Depending on your Linux distribution, you could also use the **Virt Manager** GUI interface or the **Cockpit** web console to create and manage your session VMs. For the sake of brevity, I’ll continue using the command-line and omit those alternatives.

## Outbound networking for session VMs

Your session VMs have access to the internet and can do things such as downloading RPM packages from Red Hat and third-party repositories. They could access GitHub and other internet services, as you wish.

It works because libvirt configures a user mode networking stack that forwards packets from virtual network interfaces of your session VM to the outside world and also forwards reply packets back to your session VMs. Their network connections are perceived, by the Internet or your local area network, as coming from your physical machine.

But, if you need to try server software on your session VMs, such as opening SSH connections to them, or a database server, there’s no connectivity from the outside world to your VMs, not even from the machine running your session VMs.

## Enabling libvirt’s default network

The easiest way of enabling inbound network connectivity to session VMs is allowing them to use the default virtual bridge device managed by libvirt.

Notice that, to perform these configurations, you need root privileges, but you won’t need it later to create and manage session VMs connected to that bridge.

First, check that your system is configured as a network router. It should be, by default:

```plaintext
$ sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1
```

Then create the `/etc/qemu/bridge.conf` configuration file, if it doesn’t exist, with the following contents:

```plaintext
allow virbr0
```

And make sure it is world-readable:

```plaintext
$ ls -l /etc/qemu/bridge.conf
-rw-rw-r--. 1 root root 14 Nov  8 20:41 /etc/qemu/bridge.conf
```

Finally, enable and start the libvirt network daemon:

```plaintext
$ sudo systemctl enable --now virtnetworkd
```

If all is good, you should be able to connect to the system instance of libvirt and see that there’s a network named default:

```plaintext
$ sudo virsh net-list
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
```

You should also see a new network device named virbr0 with an IP address of 192.168.122.1. This is the address your computer uses to access a private virtual network for its VMs.

```plaintext
$ ip -br addr show
```

I also recommend that you enable the libvirt NSS module. It enables resolving IP addresses from the names of your VMs.

On a recent Fedora system you would use the `authselect` utility to configure your `/etc/nsswitch.conf` file:

```plaintext
$ sudo authselect enable-feature with-libvirt
```

Unfortunately, RHEL does not yet support the `with-libvirt` feature yet, so you are advised to make your edits to `/etc/authselect/user-nsswitch.conf` instead of editing `/etc/nsswtich.conf` directly.

In the end, the hosts entry of your `/etc/nsswitch.conf` file should look as follows:

```plaintext
hosts:   files myhostname libvirt libvirt_guest mdns4_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] dns
```

Notice that there are _two_ libvirt NSS modules: the first relies on VMs correctly configured to provide their internal hostnames to DHCP servers; the second uses the libvirt name of a VM, instead of its hostname, but it does not work with session VMs.

## Inbound networking for session VMs

Now that there’s a default virtual network managed by libvirt, you can attach your session VMs to it. Just provide the name of the bridge using the `--network` option to create VMs attached to the virbr0 kernel bridge:

```plaintext
$ virt-install --name servervm --osinfo rhel9.4 --network bridge=virbr0 --memory 2048 --vcpus 2 --disk size=20 --location ~/Downloads/rhel-9.4-x86_64-boot.iso
```

Once your VM is created and its OS installed, how do you get its IP address? The common answer would be the `virsh domifaddr` command, but it only works for system VMs.

One alternative would be to just log in to the VM and get its IP addresses, using the `ip addr show` command, as you would do for any Linux system. But what if your VM was designed to not have any user with password login enabled, and requires SSH connections with keys?

One way to get the IP address of a session VM is to query DHCP leases on libvirt default network:

```plaintext
$ sudo virsh net-dhcp-leases default
 Expiry Time           MAC address      Protocol   IP address           Hostname   Client ID or DUID
------------------------------------------------------------------------------------------------------------
 2024-11-11 13:39:44   52:54:00:94:49:81   ipv4    192.168.122.137/24   servervm   01:52:54:00:94:49:81
```

(Notice that the output above includes the internal host name of the VM. If the VM didn’t set its own host name, like when you see a host name of `localhost` on its Bash shell, you will not see a Client ID set. This will be important in a moment.)

And then, assuming your VM has the SSH daemon enabled and a user named “user” with password login enabled:

```plaintext
$ ssh user@192.168.122.137
```

There’s a simpler alternative: if you enabled the libvirt NSS module, as instructed in the previous section, and your VM replies to DHCP servers with their internal host names, you can just connect to the host name of your VMs:

```plaintext
$ ssh user@servervm
```

## Connectivity between VMs

Session VMs attached to the libvirt default network are invisible to your local area network: only their host, and other VMs on the same host, can connect to libvirt’s default network.

Be aware that system VMs could connect with session VMs, and session VMs from different users could connect to each other, if they are also attached to the default network.

If you’re using a shared server, and wish to isolate VMs from different users, it’s a matter of creating additional libvirt networks, or creating Linux bridge devices manually, and grant permissions to different users to use different virtual networks or bridges.

Or, if you actually want to expose your VM to network access from other physical machines, then you need system VMs and learn more about libvirt networking, which is beyond the scope of this article.

## Session VMs with Passt

The default user mode networking for libvirt is Slirp, which Podman users may recognize from its old user mode networking layer named slirp4netns. On Podman, slirp4netns is being replaced by a more performance and capable networking layer, based on Passt and called Pasta.

Recent libvirt releases are also able to use Passt and offer port-forwarding functionality that is similar to Podman. They enable exposing network services on a session VM without attaching it to any virtual bridge. Unfortunately, libvirt CLIs and GUIs have yet to catch up with Passt, and enabling port-forwarding to a session VM requires editing XML files. But using that capability does not require configuring stuff using sudo nor the help of a sysadmin.

## Wrap up

Rootless VMs are a native feature of Linux KVM virtualization, exposed by libvirt on Fedora-based distributions. Using rootless VMs, known as session VMs in libvirt jargon, enables you to test applications and configurations which you couldn’t do with containers alone and avoids the risk of interfering with your physical networks.

Attaching a VM, even a rootless VM, to a physical network device creates some risk: VMs could perform dangerous things such as ARP spoofing and packet fragmentation attacks. Ideally, you would run your rootless VMs with only user-level networking and use port-forwarding to access server applications on those VMs, but that is not easy to do with current libvirt tools and require editing XML configurations manually.

As a compromise, you can enable connections to your rootless VMs, from their hosts only, by attaching them to a virtual bridge that’s not connected to any physical network. With just user-mode networking or with libvirt’s virtual bridge your VMs get egress connectivity by packet forwarding, which constrains their ability to do damage.

Thanks to Andrea Bolognani, Daniel Berrange, and Stefano Brivio who helped me understand how to enable inbound connections to libvirt’s session VMs.

The post Rootless virtual machines with KVM and QEMU appeared first on Red Hat Developer.  
  

Go to Source
