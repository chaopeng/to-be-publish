{
layout: "pages",
title: "linux下部署cocos2d-x",
category: "linux",
description: "linux下部署android开发环境以及部署cocos2d-x",
tags: ["linux","cocos2d-x","android","移动端"]
}

[TOC]

网上配置cocos2d-x的windows开发部署有不少，可惜很多都不能真正完成安装。linux的配置文章只有聊聊数篇。前天，[GV](http://weibo.com/gvgarven "GV")就搞了一天都没有好。看了一下因为要g++做交叉编译，要装cywin。想想windows做这种东西还是各种坑。算了，还是在linux下开发好了。本文分上下两篇，上篇是linux下Android开发环境配置，下篇是cocos2d-x的配置。本文默认你已经知道：

* linux下怎么装Java的。不会的话，看[这篇](/blog/2012/09/16/ubuntu-sun-jdk.html)。
* 怎么翻X，由于需要翻X的步骤比较多，大家就一直翻着搞吧。
 

安装Android SDK
---

* 下载[Android SDK](https://developer.android.com/sdk/index.html)。解压到`/opt`。
* 打开GUI安装

```bash
cd /opt/android-sdk-linux/tools
chmod +x android
./android update sdk
```

* 配置SDK的环境变量

在`/etc/profile`末尾添加：

```bash
export ANDROID_HOME=/opt/android-sdk-linux
```

安装Android NDK
---

* 下载[Android NDK](https://developer.android.com/sdk/index.html)。解压到`/opt/android-sdk-linux`。

* 配置NDK的环境变量

在`/etc/profile`末尾添加：

```bash
export NDK_ROOT=/opt/android-sdk-linux/android-ndk
export COCOS2DX_ROOT=/home/chao/cworkspace/cocos2d-x
export PATH=$NDK_ROOT:$ANDROID_HOME/platform-tools:$PATH
```

* 解决x64下ndk-build报错
```bash
sudo apt-get install libc6-dev-i386 ia32-libs
```

安装Eclipse开发插件ADT
Eclipse最好下载C/C++版本，或者下Classic装CDT插件。

* Eclipse > Help > Install New Software，插件安装路径是`https://dl-ssl.google.com/android/eclipse/` 。
* Eclipse > Windows > Preferences > Android 填入Android SDK路径。
* Eclipse > Windows > Preferences > Android > NDK 填入Android NDK路径。


至此linux下Android开发环境以及配置好了，想测试一下可以跑一下ndk下面的示例。附[linux下如何用小米手机Debug](http://blog.csdn.net/chenghai2011/article/details/7270664)。

** 参考文献：**

* [Android开发环境搭建(Linux篇)](http://www.linuxsight.com/blog/1808)
* [Linux下NDK的安装配置](http://blog.csdn.net/yxz329130952/article/details/7429124)
* [重回菜地。。记录解决android ndk在ubuntu x64上一坨binary无法执行的问题](http://9esuluciano.iteye.com/blog/842366)

------------------------

下面是cocos2d-x的部署，如果你并不需要支持cocos2d-x可以忽略。

安装cocos2d-x的依赖程序
---

安装glfw和zlib
```bash
sudo apt-get install libglfw-dev zlib
```

编译cocos2d-x
---
在cocos2d-x目录下

```bash
cd cocos2dx／proj.linux
make
sudo cp libcocos2d.so /usr/lib
sudo ldconfig
```

** 以下是一些我遇到的猥琐的地方：**
cocos2d-x依赖于libcurl。在ubuntu的源下，这个包的名字叫`libcurl4-gnutls-dev`，而不是curl官网写的`libcurl3`。

编译CocosDenshion
---
在cocos2d-x目录下

```bash
cd CocosDenshion／proj.linux
make
sudo cp libcocosdenshion.so /usr/lib
sudo ldconfig
```

** 以下是一些我遇到的猥琐的地方：**
到`CocosDenshion/third_party/fmod/lib64/api/lib find libfmodex64.so`的时候都会出错。
由于ubuntu的源没有这个库，只能自己编译了。在[fmod.org](http://www.fmod.org/)下载`fomdex`，回来`make install`。再复制一份到`CocosDenshion/third_party/fmod/lib64/api/lib`如此便可解决问题。

现在试试跑Sample的HelloCpp吧。

** 参考文献：**

* [cocos2d-x for linux的编译和运行](http://blog.csdn.net/laschweinski/article/details/6726311)
* [Archlinux 64 位环境下编译 cocos2d-x 出错的解决办法](http://leenjewel.blog.163.com/blog/static/60193792201222863531203/)

