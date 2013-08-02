{
layout: "pages",
title: "mysqldump常用语句",
category: "运维心得",
description: "mysqldump常用语句笔记",
tags: ["mysqldump","mysql"]
}


## 导出全库/表

```
mysqldump -h{host} -u{用户名} -p{密码} -P{端口} --default-character-set=utf8 --opt -e --triggers -R --hex-blob --single-transaction {库名} {表名} > {文件名}
```

## 导出表结构

```
mysqldump --opt -d -h{host} -u{用户名} -p{密码} -P{端口} {库名} {表名} > {文件名}
```

## 按条件导出表数据(只有数据)

```
mysqldump -e -t -c -h{host} -u{用户名} -p{密码} -P{端口} {库名} {表名} --where="{字段1}='' and {字段2}='' and {字段3}=''" > {文件名}
```

## 参数说明

使用--help看。

个人喜欢加的参数：

```
--compact 减少注释
-c 完整insert 
-e 使用批量插入
```

## 数据库表结构同步工具

```
schemasync mysql://{用户名}:{密码}@{ip}/{库名} mysql://{用户名}:{密码}@{ip}/{库名} # 前为源 后为目标
```
