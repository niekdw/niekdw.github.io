---
title: "Disable dGPU on ThinkPad T490 in Linux"
date: 2022-07-20T21:07:49+02:00
---

To ensure acceptable battery usage on a ThinkPad T490 with a NVIDIA MX250 in Linux, the dGPU needs to be turned off manually.

Check power current power state of dGPU. D0 means it is consuming power right now.

```
lspci | grep -i nvidia
cat /sys/bus/pci/devices/0000:3c:00.0/firmware_node/power_state
```

Add to **/etc/X11/nvidia-xorg.conf**

```
Section "Device"
  Identifier "nvidia"
    Driver "nvidia"
      BusID "PCI:60:0:0"
      EndSection
````

Load bbswitch modules at boot

```
echo bbswitch > /etc/modules-load.d/bbswitch.conf
grub-mkconfig -o /boot/grub/grub.cfg
```

Turn on/off, and check state

```
echo OFF > /proc/acpi/bbswitch
echo ON > /proc/acpi/bbswitch 
cat /proc/acpi/bbswitch
```

Disable at boot

```
echo options bbswitch load_state=0 unload_state=1 > /etc/modprobe.d/bbswitch.conf
```

Add to **/etc/modprobe.d/blacklist.conf**
```
blacklist nouveau
blacklist nvidia-drm
blacklist nvidia-modeset
blacklist nvidia-uvm
blacklist nvidia
