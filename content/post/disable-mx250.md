---
title: "Disable dGPU on T490 in Linux"
date: 2022-07-20T21:07:49+02:00
---

Check power current power state of dGPU. D0 means it is consuming power right now.

```
lspci | grep -i nvidia
cat /sys/bus/pci/devices/0000:3c:00.0/firmware_node/power_state
```

Add to ```/etc/X11/nvidia-xorg.conf```

```
Section "Device"
  Identifier "nvidia"
    Driver "nvidia"
      BusID "PCI:60:0:0"
      EndSection
``
kefofkef 

asdfsdfasf
