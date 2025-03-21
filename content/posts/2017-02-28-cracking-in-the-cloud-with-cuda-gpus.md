---
title: "Cracking in the Cloud with CUDA GPUs"
date: Tue, 28 Feb 2017 00:00:00 +0000
draft: false
type: posts
---
# Cracking in the Cloud with CUDA GPUs
https://www.kali.org/blog/cloud-cracking-with-cuda-gpu/images/kali-cuda-cracking-cloud.jpg
<br/>

<br/>
Due to increasing popularity of cloud-based instances for password cracking, we decided to focus our efforts into streamlining Kali&rsquo;s approach. We&rsquo;ve noticed that Amazon&rsquo;s AWS P2-Series and Microsoft&rsquo;s Azure NC-Series are focused on Windows and Ubuntu. The corresponding blog posts and guides followed suit. Although these instances are limited by
<br/>
Due to increasing popularity of cloud-based instances for password cracking, we decided to focus our efforts into streamlining Kali’s approach. We’ve noticed that Amazon’s AWS [P2-Series](https://aws.amazon.com/ec2/instance-types/p2/) and Microsoft’s Azure [NC-Series](https://azure.microsoft.com/en-us/blog/azure-n-series-general-availability-on-december-1/) are focused on Windows and Ubuntu. The corresponding blog posts and guides followed suit. Although these instances are limited by the NVIDIA Tesla K80’s hardware capabilities, the ability to quickly deploy a Kali instance with CUDA support is appealing.

Installing proprietary graphics drivers has always been a source of frustration; fortunately, improvements in packaging have made this process much more seamless. Although we’ve done the work for you in the cloud offerings, we’d like to help simplify installation for your own use.

### Prerequisites

First, you’ll need to ensure that your system is fully upgraded and that your card supports [CUDA](https://developer.nvidia.com/cuda-gpus). **Note:** GPUs with a [CUDA compute capability](https://developer.nvidia.com/cuda-gpus) > 5.0 are recommended, but GPUs with less will still work:

```sh
apt-get update && apt-get dist-upgrade -y
```

Once we’ve updated the system, we need to check for the **nouveau kernel modules**, and if enabled, blacklist them:

```console
root@kali:~# lsmod |grep -i nouveau
nouveau 1499136 1
mxm_wmi 16384 1 nouveau
wmi 16384 2 mxm_wmi,nouveau
video 40960 1 nouveau
root@kali:~#
root@kali:~# echo -e "blacklist nouveau\noptions nouveau modeset=0\nalias nouveau off" > /etc/modprobe.d/blacklist-nouveau.conf
```

After modifying kernel parameters, we’ll need to update our **initramfs**, then reboot:

```sh
update-initramfs -u && reboot
```

### Installation on a Local Computer

Once we have rebooted and have determined that the nouveau modules have not loaded, we will proceed to install the **OpenCL ICD Loader**, **Drivers**, and the **CUDA toolkit**:

```sh
apt-get install -y ocl-icd-libopencl1 nvidia-driver nvidia-cuda-toolkit
```

During installation of the drivers the system created new kernel modules, so another reboot is required.

### Verify Driver Installation

Now that our system should be ready to go, we need to verify the drivers have been loaded correctly. We can quickly verify this by running the [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) tool:

```console
root@kali:~# nvidia-smi
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 375.26 Driver Version: 375.26 |
|-------------------------------+----------------------+----------------------+
| GPU Name Persistence-M| Bus-Id Disp.A | Volatile Uncorr. ECC |
| Fan Temp Perf Pwr:Usage/Cap| Memory-Usage | GPU-Util Compute M. |
|===============================+======================+======================|
| 0 Tesla K80 Off | 0000:00:1E.0 Off | 0 |
| N/A 28C P0 53W / 149W | 0MiB / 11439MiB | 65% Default |
+-------------------------------+----------------------+----------------------+
```

With the output displaying our driver and GPU correctly, we can now dive into password cracking. Before we get too far ahead, let’s double check to make sure hashcat and CUDA are working together:

```console
root@kali:~# hashcat -I
OpenCL Info:
Platform ID #1
Vendor : NVIDIA Corporation
Name : NVIDIA CUDA
Version : OpenCL 1.2 CUDA 8.0.0
Device ID #1
Type : GPU
Vendor ID : 32
Vendor : NVIDIA Corporation
Name : Tesla K80
Version : OpenCL 1.2 CUDA
Processor(s) : 13
Clock : 823
Memory : 2047/11439 MB allocatable
OpenCL Version : OpenCL C 1.2
Driver Version : 375.26
```

**Note:** If you receive the error c\_lGetDeviceIDs(): CL\_DEVICE\_NOT\_FOUND\_ with Platform ID labeled _Vendor: Mesa_ run:

```sh
apt-get remove mesa-opencl-icd
```

It appears everything is working, let’s go ahead and run a benchmark test.

### Benchmarking

```console
root@kali:~# hashcat -b
OpenCL Platform #1: NVIDIA Corporation
======================================
* Device #1: Tesla K80, 2047/11439 MB allocatable, 13MCU
Hashtype: MD5
Speed.Dev.#1.....: 4247.2 MH/s (102.66ms)
Hashtype: SHA1
Speed.Dev.#1.....: 1850.5 MH/s (58.64ms)
Hashtype: SHA256
Speed.Dev.#1.....: 785.1 MH/s (69.41ms)
```

### Cracking

Now let’s crack some hashes. We are going to use the example NetNTLMv2 hash found on the [hashcat wiki.](https://hashcat.net/wiki/doku.php?id=example_hashes)

```console
root@kali:~# hashcat -a 0 -m 5600 ntlmv2.hash dict.txt
OpenCL Platform #1: NVIDIA Corporation
======================================
* Device #1: Tesla K80, 2047/11439 MB allocatable, 13MCU
ADMIN::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030:hashcat
Session..........: hashcat
Status...........: Cracked
Hash.Type........: NetNTLMv2
Hash.Target......: ADMIN::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
Input.Base.......: File (dict.txt)
Input.Queue......: 1/1 (100.00%)
Speed.Dev.#1.....: 0 H/s (0.10ms)
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 101/101 (100.00%)
```

**Success!** We’ve cracked the example hash and proven our installation is functional. There are a multitude of configurations to improve cracking speed, not mentioned in this guide. However, we encourage you to take a look at the [hashcat documentation](https://hashcat.net/wiki/) for your specific cases.

### Running a GPU Instance in AWS

We’ve registered new CUDA enabled [Kali Rolling images with Amazon](https://aws.amazon.com/marketplace/pp/B08LL91KKB) which work out of the box with P2 AWS images. With virtually no additional setup required, you can get up and running with a Kali GPU instance in less than 30 seconds. All you need to do is choose a P2 instance, and you’re ready to start cracking!

#### [Source](https://www.kali.org/blog/cloud-cracking-with-cuda-gpu/)

<br/>
---
