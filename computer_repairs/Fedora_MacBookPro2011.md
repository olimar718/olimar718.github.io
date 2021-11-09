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
### This gpu is also known to fail and render the laptop unusable 

original links for reference
# https://www.reddit.com/r/linux_on_mac/comments/ejheg4/macbook_pro_2011_fedora_31_outb_disable_radeon/
# https://bugzilla.redhat.com/show_bug.cgi?id=765954#c65
# https://orville.thebennettproject.com/articles/installing-ubuntu-14-04-lts-on-a-2011-macbook-pro/

Install the `outb`command from the `ioport` package
`dnf install ioport`

Install grub2-efi-x64-modules which is noarch
`dnf install grub2-efi-x64-modules`
 
 Copy all, or just iorw.mod, to the EFI system partition
`cp -r /usr/lib/grub/x86_64-efi /boot/efi/EFI/fedora/`


add this to `GRUB_CMDLINE_LINUX` in `/etc/default/grub`
`i915.lvds_channel_mode=2 i915.modeset=1 i915.lvds_use_ssc=0`
the line will then look like 
`GRUB_CMDLINE_LINUX="rhgb quiet i915.lvds_channel_mode=2 i915.modeset=1 i915.lvds_use_ssc=0"`
At the end of `/etc/default/grub` add this line :
`GRUB_PRELOAD_MODULES="iorw"`
Edit /etc/grub.d/00_header as follows
```
   119	# If all_video.mod isn't available load all modules available
   120	# with versions prior to introduction of all_video.mod
   121	cat <<EOF
   122	  if [ x\$feature_all_video_module = xy ]; then
   123	    outb 0x728 1
   124	    outb 0x710 2
   125	    outb 0x740 2
   126	    outb 0x750 0
   127	    insmod all_video
   128	  else
   129	    outb 0x728 1
   130	    outb 0x710 2
   131	    outb 0x740 2
   132	    outb 0x750 0
   133	    insmod efi_gop
   134	    insmod efi_uga
   135	    insmod ieee1275_fb
   136	    insmod vbe
   137	    insmod vga
   138	    insmod video_bochs
   139	    insmod video_cirrus
```
maybe edit the `/etc/grub.d/10_linux` file by placing the following before (or after ?) this line 
`echo "	insmod gzio" | sed "s/^/$submenu_indentation/"`

The following:
```
echo "	outb 0x728 1" | sed "s/^/$submenu_indentation/"
echo "	outb 0x710 2" | sed "s/^/$submenu_indentation/"
echo "	outb 0x740 2" | sed "s/^/$submenu_indentation/"
echo "	outb 0x750 0" | sed "s/^/$submenu_indentation/"
```


then run theses
`sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg`
`sudo grub2-mkconfig -o /boot/grub2/grub.cfg`
then reboot
`systemctl reboot`