---
layout: post
title:  "Installing Linux 4 kernel on Banana Pi with Bananian"
date:   2018-01-13 13:51:38 +0100
categories: linux
---

This blog post is copy of an old notes how to update Linux kernel on my Banana Pi M1+ which is still running Bananian.
It's rather obsolete now, since Bananian went [EOL][eol] last year and it's recommended to switch to [Armbian][armbian].


# Get kernel sources

Download archive with kernel sources or clone repository somewhere.
Make symlink /usr/src/linux to the directory with kernel sources:

```
ln -sfn $DIR /usr/src/linux
```

# Get config
To build functional kernel it's good to start from some working configuration.

Depending on your kernel, you can find current configuration in /proc/config or /proc/config.gz.
If there are no such files, try to execute  `modprobe configs` and if you get no error message, configuration file should appear in /proc.

You can also check /boot or mount boot partition /dev/mmcblk0p1.
Otherwise check internet (Bananian.org, GitHub,...)

# Building

Copy configuration file it to `/usr/src/linux/.config`

Run `make menuconfig` and change whatever you need.

When you are satisfied with the result, execute following steps.

```
make LOADADDR=0x40008000 all
make module_install firmware_install


mkdir -p /mnt/p1
mount /dev/mmcblk0p1 /mnt/p1

cp -v /usr/src/linux/arch/arm/boot/uImage /mnt/p1/uImage-next

umount /mnt/p1

```

Please note, that by default the boot partition is really small, so probably there will not be even place to backup original kernel file.

Final note: to verify that you have correct format of image, you can check it with `file`.
You should see something like this:

```
root@banan:/mnt/p1# file uImage uImage-next

uImage:      u-boot legacy uImage, Linux-3.4.113-bananian, Linux/ARM, OS Kernel Image (Not compressed), 4810944 bytes, Sat May  6 14:21:43 2017, Load Address: 0x40008000, Entry Point: 0x40008000, Header CRC: 0x9A836903, Data CRC: 0x5A1B6361
uImage-next: u-boot legacy uImage, Linux-4.12.0-b2, Linux/ARM, OS Kernel Image (Not compressed), 6060512 bytes, Mon Oct  2 13:07:08 2017, Load Address: 0x40008000, Entry Point: 0x40008000, Header CRC: 0xFCD22857, Data CRC: 0x5DD28C3B
```

[armbian]: https://github.com/armbian/build/issues/648
[eol]: https://www.bananian.org/news#the_end_-_2017-04-02
