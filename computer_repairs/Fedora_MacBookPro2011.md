---
title: Fedora 35 Linux install note on a 2011 MacBook Pro
---
## 2011 MacBook Pro


## WIFI
- Install wifi driver
`dnf install broadcom-wl`
- Install kernel header
`sudo dnf install kernel-devel-$(uname -r)`
- Run akmods
`sudo akmods`
- reboot
`systemctl reboot`

## Disable useless amd gpu the uses battery and produces heat
add this to `GRUB_CMDLINE_LINUX` in `/etc/default/grub`
- doesn't work (system doesn't boot)

- to test 
`i915.lvds_channel_mode=2 i915.modeset=1 i915.lvds_use_ssc=0`

then run 
`sudo grub2-mkconfig -o /etc/grub "$(readlink -e /etc/grub2.cfg)"`
then reboot
`systemctl reboot`