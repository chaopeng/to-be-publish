{
layout: "pages",
title: "log4j怎样把配置文件放在包外",
category: "java",
description: "把log4j的配置文件放在包外的方法",
tags: ["java","log4j"]
}

在主函数加上这个static块就可以了。
```bash
static {
	PropertyConfigurator.configure(
		System.getProperty("user.dir") 
		+ File.separator + "conf"
		+ File.separator + "log4j.properties");
}
```
