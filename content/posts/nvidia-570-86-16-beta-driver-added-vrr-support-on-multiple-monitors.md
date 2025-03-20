---
title: "NVIDIA 570.86.16 Beta Driver added VRR Support on Multiple Monitors"
date: 2025-02-06
---

![](https://ubuntuhandbook.org/wp-content/uploads/2021/06/nvidia-logo-250x250.webp)

NVIDIA released first Beta of the 570 driver series for Linux users a few days ago. See what’s new in this new driver.

NVIDIA Linux driver introduced VRR support for Wayland since 560 driver series. It’s a feature that adjust the monitor’s refresh rate on the fly, to match the frame rate of output signal from the graphics card.

The feature is useful for games to eliminate screen tearing and also lowers power consumption. And the new 570.86.16 driver enhanced it by supporting **variable refresh rate (VRR) on systems with multiple monitors**.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/nvidia-settings570-700x517.webp)

NVIDIA Settings 570

To improve Wayland support, the nvidia-settings control panel now uses NVML instead of NV-CONTROL to control GPU clocks and fan speed. As result, some operations may now require elevated privileges.

For GPU boards that support programmable clock control, nvidia settings now by default show GPU overclocking control, while it was previously only available when bit 3 was set in the “Coolbits” X config option.

For gaming, it added an application profile to improve performance on Indiana Jones and the Great Circle, and resolve a corruption issue on Assassin’s Creed Valhalla and Assassin’s Creed Mirage. As well, it fixed a bug that could cause some multi-threaded OpenGL applications (e.g., Civilization 6) to crash when running on Xwayland.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/ac-full-width_combat_desktop-700x396.webp)

The new driver release also added systemd **suspend-then-hibernate** sleep method, though needs systemd version 248 or newer. It implemented support for the VK\_KHR\_incremental\_present extension, allowing to specify changed regions of the display for updating, rather than resorting to updating the entire screen. And, it enabled 32 bit compatibility support for the NVIDIA GBM backend.

Other changes in the NVIDIA 570.86.16 include:

- Disable a power saving feature on Ada and above generation GPUs for surfaces allocated with the DRM Dumb-Buffers API.
- New nvidia-modeset kernel module parameter `conceal_vrr_caps`
- Support for querying Dynamic Boost status via the ‘power’ file in /proc/driver/nvidia/gpus/\*.
- Add `/sandboxutils-filelist.json` which lists all the driver files used by container runtime environments
- Enable `nvidia-drm fbdev=1` option by default.
- Allow low latency display interrupts to be serviced even when system is under heavy contention.
- Fix bug that some DVI outputs would not work with HDMI monitors.
- For more, see the official release note.

### How to Get NVIDIA Driver 570.86.16

**NOTE: This is a Beta driver, which MAY have bugs! Don’t use it on production machine.**

NVIDIA provides official `.run` installers for all supported driver releases via the link below:

NVIDIA Driver for Unix

For Ubuntu, it’s however recommended to either use the packages from system repository or add the Graphics Drivers Team PPA, which contains the NVIDIA 570 driver for Ubuntu 22.04, Ubuntu 24.04, Ubuntu 24.10.

To add the PPA, simply open terminal (Ctrl+Alt+T) and run command:

```
sudo add-apt-repository ppa:graphics-drivers/ppa
```

Then, you should be able to install the driver using “Additional Drivers” utility.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/02/additionsdrivers-nvidia570-700x429.webp)

Go to Source
