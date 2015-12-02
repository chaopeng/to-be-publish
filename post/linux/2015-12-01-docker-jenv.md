{
layout: "pages",
title: "给docker镜像减肥",
category: "linux",
description: "定制docker镜像，如何减少其体积",
tags: ["linux","docker","java"]
}

## 引子

这星期有位前端同事需要做一些简单的后端工作，所以就使用了我在docker-hub上的后端镜像，镜像是私有的当然是不方便透露了，但是是基于[chaopeng/jenv](https://hub.docker.com/r/chaopeng/jenv/)制作的。然后那天办公室的网速又不好，足足下了2分钟才下完。然后我那位同事就向我投诉了我做的镜像太大了。所以我就开始给镜像做减肥。

## 忘记清理缓存（800M -> 565M）

看了下镜像的大小

```
➜  ~  sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
chaopeng/jenv        latest              5b752e05eee4        3 weeks ago         801.2 MB
centos               latest              e9fa5d3a0d0e        6 weeks ago         172.3 MB
```

`chaopeng/jenv`果然很大了，赶紧进去看看，原来jenv的下载缓存忘记清理，赶紧跑一下`jenv clean`。立马减肥176M。顺手跑一下`yum clean all`，再省60M。成功减肥到565M，其中355M属于java，210M属于centos及其依赖。如果再想优化还能删什么呢？可以删java下面的jre、src、man这些文件。

## 更换依赖os

既然不考虑优化java，下面能减肥的就只有os这块了，在docker里，其实系统镜像承担的就仅仅是系统目录以及软件包管理的作用。所以我pull了下传统的系统镜像来比对。

```
➜  ~  sudo docker images       
REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
debian               latest              a604b236bcde        11 days ago         125.1 MB
opensuse             latest              1f5ee29cd30e        3 weeks ago         96.14 MB
centos               latest              e9fa5d3a0d0e        6 weeks ago         172.3 MB
base/archlinux       latest              a6e872e6025d        5 months ago        278.7 MB
ubuntu               latest              e9ae3c220b23        3 weeks ago         187.9 MB
```

体积都不小，而且像centos连curl都还没装，究竟为啥这么大，进去看看centos这个镜像其实还挺奇葩的。curl都不装，倒是默认装了个python，可以猜想大多数传统镜像都做了类似的事情。刚好前天在[瓜总的微博下看到有人安利alpine](http://weibo.com/1609119537/D51JkrqYQ?from=page_1005051609119537_profile&wvr=6&mod=weibotime&type=comment)，顺手试试吧。

```
➜  ~  sudo docker images       
REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine               latest              8a648f689ddb        11 weeks ago        5.244 MB
```

这玩意这够小啊，当然也就是什么都没有的状态了，不过有包管理器，自己写个Dockerfile试试build安装jenv的依赖看看。

```Dockerfile
FROM alpine:latest

RUN apk add --update bash curl zip && \
    rm -rf /var/cache/apk/*
```

build出来的结果是

```
➜  ~  sudo docker images       
REPOSITORY           TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine               latest              8a648f689ddb        11 weeks ago        5.244 MB
chaopeng/alpine      latest              8998bdbddf9a        About an hour ago   9.813 MB
```

所以说使用alpine将近可以又节省190M。

当然这样的东西必然有些不足的地方，比如说源里面的东西比较少，第三方支持几乎没有，这样就导致很多软件并不适合使用alpine来做os包。但是我的这个jenv就很合适了，软件上的依赖很简单。注意oracle-jdk对glibc有依赖，需要找个alpine的glibc包安装。

## 总结

1. docker镜像不是自己用的os，有缓存用完就清理掉
2. 选择合适的系统镜像

---------

减肥结果，jenv从800M减肥到313M(centos)或者159M(alpine)。[imagelayers](https://imagelayers.io/?images=chaopeng%2Fjenv:latest-mini,chaopeng%2Fjenv:latest,chaopeng%2Fjenv:jdk7,chaopeng%2Fjenv:jdk8)
