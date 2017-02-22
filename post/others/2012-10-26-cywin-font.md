{
layout: "pages",
title: "奇葩的Cygwin",
category: "cygwin",
description: "修复cygwin字体很丑",
tags: ["cygwin","windows"]
}

一直都很不爽cygwin的字体，一开始以为是win的console的字体就是这么丑的。

今天无意中用了一下win的console发现原来win的没有那么糟糕。无意中试了一下再win的console运行cygwin，发现字体没有变丑。真是奇葩啊。赶紧写了个bat去运行cygwin。

```bat
@echo off
C:
cd C:\cygwin
run Cygwin.bat
```

