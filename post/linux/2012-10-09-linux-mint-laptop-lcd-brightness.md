{
layout: "pages",
title: "Linux Mint笔记本亮度设置",
category: "linux",
description: "Linux Mint使用laptop-mode-tools调节笔记本亮度设置笔记",
tags: ["linux","linux mint","laptop-mode-tools", "笔记本亮度"]
}

** 本文无法解决mate13桌面瞎狗眼的亮度，mate之后的版本我都没有试过。 **

用了Linux Mint这个半月最不爽的就是每次开机都是最高亮度，那一瞬间我的狗眼都要瞎了。所以必须找个办法让开机就是最低亮度。

打开/etc/default/acpi-support ，发现最后有这么一段注释：

```bash
# Note: to enable "laptop mode" (to spin down your hard drive for longer
# periods of time), install the laptop-mode-tools package and configure
# it in /etc/laptop-mode/laptop-mode.conf.
```

1 所以开始安装吧：`apt-get install laptop-mode-tools`

2 然后就开始配置了，打开`/etc/laptop-mode/laptop-mode.conf`修改：
```bash
# 在交流电下也使用笔记本配置
ENABLE_LAPTOP_MODE_ON_AC=1
```

3 最后打开`/etc/laptop-mode/conf.d/lcd-brightness.conf`修改：
```bash
#
# 启动亮度控制
CONTROL_BRIGHTNESS=1
#
# “echo 0” 就是要将亮度值 0 写入 BRIGHTNESS_OUTPUT文件中，亮度值根据自己的情况进行取值。
BATT_BRIGHTNESS_COMMAND="echo 0"
LM_AC_BRIGHTNESS_COMMAND="echo 0"
NOLM_AC_BRIGHTNESS_COMMAND="echo 0"
BRIGHTNESS_OUTPUT="/sys/class/backlight/acpi_video0/brightness"
```

重启完成设置。
