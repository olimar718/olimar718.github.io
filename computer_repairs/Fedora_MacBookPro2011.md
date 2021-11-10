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


Keep it turned of after suspend then resume :
original source is https://www.variadic.xyz/2020/06/28/switching-off-dgpu/
Compile and run this code after every resume

`gcc -Wall outbfix.c -o outbresumefix`
```
#include <stdio.h> 
#include <sys/io.h>

#define PORT_SWITCH_DISPLAY     0x710
#define PORT_SWITCH_SELECT      0x728
#define PORT_SWITCH_DDC         0x740
#define PORT_DISCRETE_POWER     0x750

static int gmux_switch_to_igd()
{
    outb(1, PORT_SWITCH_SELECT);
    outb(2, PORT_SWITCH_DISPLAY);
    outb(2, PORT_SWITCH_DDC);
    return 0;
}

static void mbp_gpu_power(int state)
{
    outb(state, PORT_DISCRETE_POWER);
}

static void mb_gpu_print()
{
    printf("SELECT:  %hhu\n", inb(PORT_SWITCH_SELECT));
    printf("DISPLAY: %hhu\n", inb(PORT_SWITCH_DISPLAY));
    printf("DDC:     %hhu\n", inb(PORT_SWITCH_DDC));
    printf("POWER:   %hhu\n", inb(PORT_DISCRETE_POWER));
}

int main(int argc, char **argv)
{
    if (iopl(3) < 0) {
        perror ("No IO permissions");
        return 1;
    }
    int state=0;
    if (argc > 1) state = atoi(argv[1]);
    printf("Before:\n");
    mb_gpu_print();
    mbp_gpu_power(state);
    gmux_switch_to_igd();
    printf("After:\n");
    mb_gpu_print();
    return 0;
}

```
Using this article https://www.addictivetips.com/ubuntu-linux-tips/run-scripts-and-commands-on-suspend-and-resume-on-linux/
you can setup the program to run everytime the system resumes from sleep.

copy your `outbresumefix` executable to `/usr/lib/systemd/system-sleep/`
Then write this into this **new** file `/usr/lib/systemd/system-sleep/pre-suspend.sh`
```
#!/bin/sh
if [ "${1}" == "pre" ]; then
	:
else
	/usr/lib/systemd/systemd-sleep/outbresumefix
fi

```
make the file executable and your good to go
`chmod +x /usr/lib/systemd/system-sleep/pre-suspend.sh`


you might want to do that as well because now it's failing:
`sudo systemctl disable systemd-backlight@backlight\:acpi_video0.service`

# sensor conf 
for `lm_sensors`
Taken from https://www.variadic.xyz/2020/06/03/lmsensors-config/

save this file as `/etc/sensors.d/applesmc.conf`

```
# this is my MacBook Pro 2011 8,2
chip "applesmc-isa-0300"
    label temp1 "Battery"
    label temp27 "Palm Rest"
    label temp4 "CPU Die Core Temp"
    label temp8 "CPU Proximity Temp"
    label temp9 "CPU Core 1"
    label temp10 "CPU Core 2"
    label temp11 "CPU Core 3"
    label temp12 "CPU Core 4"
    label temp16 "GPU Die"
    label temp24 "Heatpipe 1"
    label temp25 "Heatpipe 2"
    ignore temp2
    ignore temp3
    ignore temp5
    ignore temp6
    ignore temp7
    ignore temp13
    ignore temp14
    ignore temp15
    ignore temp17
    ignore temp18
    ignore temp19
    ignore temp20
    ignore temp21
    ignore temp22
    ignore temp23
    ignore temp26
    ignore temp28
```