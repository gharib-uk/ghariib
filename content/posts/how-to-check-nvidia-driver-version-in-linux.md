---
title: "How to Check NVIDIA Driver Version in Linux"
date: 2025-01-02
categories: 
  - "security"
---

It is important to know which NVIDIA driver version is installed on your Linux system for compatibility with your hardware and software. The driver version ensures your graphics card works properly and supports the applications you use. Knowing the version also helps when updating or troubleshooting. You can check the installed version using commands like `nvidia-smi` or by checking system settings in your Linux distribution.

This tutorial will help you understand how to check the NVIDIA driver version on your Linux system. It will guide you step-by-step using simple commands and tools.

## Check NVIDIA Driver Version on Linux

Choose one of the below methods to find NVIDIA driver version on your Linux system.

### Method 1: Using `nvidia-smi` Command

The easiest way to check the driver version is by using the nvidia-smi command. This tool comes with the NVIDIA driver package.

Open your terminal and type nvidia-smi and press Enter.

```bash

nvidia-smi
```

Look for the “Driver Version” in the output. It will display the installed driver version.

<figure>

![Check NVIDIA Version using nvidia-smi](https://tecadmin.net/wp-content/uploads/2024/12/check-nvidia-version-ubuntu-with-nvidia-smi.png)

<figcaption>

check NVIDIA version using nvidia-smi

</figcaption>

</figure>

### Method 2: Using `cat` Command

You can also check the version from a file that stores the driver information.

```bash

cat /proc/driver/nvidia/version
```

The output will show the installed driver version.

<figure>

![Check NVIDIA Version from files](https://tecadmin.net/wp-content/uploads/2024/12/check-nvidia-version-from-file-1024x128.png)

<figcaption>

Check NVIDIA Version from files

</figcaption>

</figure>

## Conclusion

Checking your NVIDIA driver version on a Linux system is simple and can be done using commands like `nvidia-smi`, `cat /proc/driver/nvidia/version`, or through the graphical interface. Knowing the version ensures compatibility, helps with troubleshooting, and keeps your system running smoothly. By following this guide, you can easily stay informed and maintain your graphics card’s performance.

The post How to Check NVIDIA Driver Version in Linux appeared first on TecAdmin.
