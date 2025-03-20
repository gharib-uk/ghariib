---
title: "The Graphics Drivers PPA updated with NVIDIA 570 for Ubuntu / Linux Mint"
date: 2025-03-19
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/06/nvidia-logo-250x250.webp)

For users who want to try out the new NVIDIA 570.124.04 driver, the Graphics Drivers Team PPA finally updated with the packages for Ubuntu 20.04, Ubuntu 22.04, Ubuntu 24.04, Ubuntu 24.10, and Ubuntu 25.04.

NVIDIA 570.124.04 is the latest production branch driver for Linux, which was released in last month.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/nvidiasettings570.webp)

The new driver features **variable refresh rate (VRR) with multiple monitors support**, systemd **suspend-then-hibernate** sleep, and an application profile to improve performance on Indiana Jones and the Great Circle.

Other changes include:

- added new `conceal_vrr_caps` parameter to the nvidia-modeset kernel module.
- use NVML instead of NV-CONTROL to control GPU clocks and fan speed for Wayland.
- show GPU overclocking control by default for GPU boards that support the feature.
- support for the VK\_KHR\_incremental\_present extension.
- 32-bit compatibility support for NVIDIA GBM backend.
- And more, see the official release note for details.

### Install NVIDIA 570 from Ubuntu PPA

For Linux Mint 21/22, Ubuntu 20.04, Ubuntu 22.04, Ubuntu 24.04 and higher, the new driver can be easily installed from Graphics Drivers PPA by running the commands below one by one.

**NOTE: This PPA is well-known, but NOT recommended for production use!! Use it at your own risk.**

**1.** First, press `Ctrl+Alt+T` on keyboard to open up a terminal window. When it opens, run command to add the PPA:

```
sudo add-apt-repository ppa:graphics-drivers/ppa
```

_Type user password when it asks (no asterisk feedback in Ubuntu) and hit Enter to continue._

![](https://ubuntuhandbook.org/wp-content/uploads/2024/08/graphics-driver-ppa-700x535.webp)

**2.** Linux Mint users need to manually fresh package cache, though it’s done automatically in Ubuntu, by running command:

```
sudo apt update
```

**3.** Finally, either launch “**Additional Drivers**” utility. When it opens, choose nvidia-driver-570 from the list, and finally click on “Apply Changes” button to start installing the new driver.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/03/nvidia570-additionaldrivers-700x418.webp)

For choice, you may run the command below instead to install the driver:

```
sudo apt install nvidia-driver-570
```

**4.** Once installed, **restart computer**. For hybrid graphics, launch `nvidia-settings` and navigate to PRIME Profiles to switch between integrated and dedicated GPU.

**Tips: If `intel (Power Saving mode)` is grayed out, use `sudo prime-select intel` command to choose it and restart computer to apply.**

![](https://ubuntuhandbook.org/wp-content/uploads/2024/05/nvidia-settings-selectgpu.webp)

After restarted you computer, use the command below to tell which GPU is in use:

```
glxinfo |grep -E "OpenGL vendor|OpenGL renderer"
```

![](https://ubuntuhandbook.org/wp-content/uploads/2024/05/verify-gpu-700x300.webp)

If you want to run some apps or games with NVIDIA graphics card, while leaving others rendering with integrated GPU, or add system menu options to switch GPU, then see this tutorial instead.

### Uninstall NVIDIA 570

To uninstall this graphics driver, also launch “**Additional drivers**” utility, choose another driver and click “Apply Changes”.

Then, you may run the command below in a terminal window (Ctrl+Alt+T) to remove the driver package:

```
sudo apt remove --autoremove nvidia-driver-570
```

Go to Source
