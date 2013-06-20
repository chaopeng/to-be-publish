{
layout: "pages",
title: "通过eclipse.ini加速eclipse",
category: "java",
description: "通过调整jvm参数加速eclipse启动",
tags: ["java","eclipse"]
}

这仅仅是一个eclipse加速的笔记，是参考《深入理解java虚拟机》一书修改eclipse.ini的。具体过程不表。下面是主要修改的部分：

```{ini}
在-vmargs下面加：

-Xverify:none
-Xms512m
-Xmx512m
-Xmn128m
-XX:PermSize=128m
-XX:MaxPermSize=256m
-XX:+DisableExplicitGC
-Xnoclassgc
-XX:+UseParNewGC
-XX:+UseConcMarkSweepGC
-XX:CMSInitiatingOccupancyFraction=85
```
