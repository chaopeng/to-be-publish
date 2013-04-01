{
layout: "pages",
title: "Play! Framework 的使用",
category: "java",
description: "playframework的使用笔记",
tags: ["java","playframework"]
}


## 安装jdk
## 安装python
## 安装二进制包

* 二进制包
    
    1. 下载[进制包](http://downloads.typesafe.com/releases/play-1.2.4.zip)
    2. 添加目录到环境变量PATH
    3. 命令行输入`play`检查是否安装成功
    
**注意坑**

linux下如果安装了sox的话play的符号已经被占用，需重命名play脚本。

## 建立play项目

* `play new 项目名`
* 导入到eclipse `play eclipsify 项目名`

**注意坑**

在eclipse debug的时候会出现

    Error occurred during initialization of VM
    agent library failed to init: jdwp
    ERROR: Cannot load this JVM TI agent twice, check your java command line for duplicate jdwp options.

需要删除`eclispe/helloworld.launch`中的`-Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n`。

## 参考网站

* [Play! Framework 学习笔记（一）：初识Play](http://djb4ke.iteye.com/blog/662240)
* [Play! Framework 的模板引擎](http://xbgd.iteye.com/blog/813281)
* [Play! Framework 中文文档](http://play-framework.herokuapp.com/zh/home)
* [玩转 Java Web 应用开发：Play 框架](http://www.ibm.com/developerworks/cn/java/j-lo-play/)
 
**还有任何疑惑的话看官方文档吧**