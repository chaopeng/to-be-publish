{
layout: "pages",
title: "linux mint 笔记",
category: "linux",
description: "我使用linux的笔记，也适用于部分ubuntu的发行版",
tags: ["linux","ubuntu","linux mint"]
}

[TOC]

## home目录切换回英文 ##

```{bash}
export LANG=en_US
xdg-user-dirs-gtk-update
```

选上不再提示，点继续。然后换回中文：

```{bash}
export LANG=zh_CN
```

## 安装windows字体 ##

1. 把需要的字体ttf文件放到一个文件夹并复制到`/usr/share/fonts`
2. 修改权限`sudo chmod 644 /usr/share/fonts/winfonts/*`
3. 刷新字体缓存

```{bash}
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fsv
```

## 安装texlive ##

1 安装perl-tk 

2 挂载texlive镜像 

```{bash}
sudo mkdir /mnt/iso
sudo mount -o loop texlive2010.iso /mnt/iso
```

3 安装

```{bash}
sudo /mnt/iso/install-tl --gui
```

最后那里有项“自动创建链接”选上。生活就会很简单了。

## 开机自启 ##
安装这个`sysv-rc-conf`。告别手动改rc。 
