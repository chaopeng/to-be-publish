{
layout: "pages",
title: "制作grub2 linux安装u盘",
category: "linux",
description: "制作grub2 linux安装u盘",
tags: ["linux","grub2"]
}

由于本人已经没有光驱多年，所以安装系统全靠u盘。之前我一直用UltraIso这款工具制作安装u盘，但是经常会遇到某个发行版莫名其妙就不能安装的情况，一天与[王sir](http://weibo.com/prinseer) 喝咖啡，被王sir发现我还用这么落后的方法，被鄙视了一顿，再王sir的提示下我也开始制作我的grub2 u盘了。

​##设备准备

1. 将u盘分区成一只fat32 bootable u盘，简答不表。
2. `sudo blkid` 查看分区UUID记下了，有用。

##安装grub2

```
grub-install --force --no-floppy --root-directory=u盘的挂载目录 /dev/sd?(u盘)
```

##配置

1. 将你要安装的iso文件放到u盘的目录
2. 在`u盘/boot/grub/`下编写grub.cfg

```
# global setting
set UUID=DDFB-35D7

# Arch Linux
menuentry "Arch Setup" {
	set isofile="/archlinux-2014.03.01-dual.iso"
	set label=ARCH_201403

	loopback loop $isofile
	linux (loop)/arch/boot/x86_64/vmlinuz  archisolabel=$label img_dev=/dev/disk/by-uuid/$UUID img_loop=$isofile earlymodules=loop
	initrd (loop)/arch/boot/x86_64/archiso.img
}
```

**这里注意一下很多发行版都需要特殊的配置，不清楚的话最好去google一下**

