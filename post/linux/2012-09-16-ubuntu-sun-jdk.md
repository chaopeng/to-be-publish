{
layout: "pages",
title: "Ubuntu安装SUN-JDK",
category: "linux",
description: "ubuntu发行版安装sun-jdk笔记",
tags: ["linux","ubuntu","java", "jdk"]
}

安装sun-jdk，其实一点都不简单。网上其实很多中文教程都很坑。

这是别人的教程：

1. 第一步谁都会的，先去oracle下最新的jdk
2. 解压到/opt/java
3. 添加环境变量，在`/etc/profile`末尾添加

```{text}
export JAVA_HOME=/opt/jdk1.7
export JRE_HOME=/opt/jdk1.7/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

现在用命令行查看java版本。对了。好吧这看上去成功了。教程到此结束。

小白的我一开始以为这真的就对了。后来部署flash的安全沙箱监听843端口，需要sudo，然后就找不到主函数了。

怎么检查都是正常的，后来突发奇想，`sudo java -version`尼玛gij1.5这也太坑了。

查了一下英文才知道ubuntu是用update-alternatives管理软件版本的。

```{bash}
update-alternatives --display java
```

查看现在的java的链接指向，并查看优先级的值。建议顺便把这个降低。然后设置新装的这个jdk。

```{bash}
update-alternatives --install /usr/bin/java java /opt/jdk1.7/bin/java 300
```

现在装sun-jdk终于不用改`profile`，原来世界是这么美好的。

## 2014年更新

如果你找到了这篇文章，请允许我推荐你使用 [jevn](http://jenv.io)去安装 JDK，适用于所有的 Linux 发行版。版本切换很方便，其他 Java 的工具安装很方便。

