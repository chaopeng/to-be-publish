{
layout: "pages",
title: "Awesome Java",
category: "java",
description: "记录我常用的java工具、框架、库",
tags: ["java"]
}

Github 上其实有个类似的[文章](https://github.com/akullpp/awesome-java)，我也整理一下我自己的列表吧，大部分都是我在用的或者曾经用过的。

## JVM环境

- [jenv](http://jenv.io/) 很方便安装 Java，JVM 语言，版本切换以及各类 Java 工具，无论开发环境还是生产环境都适合。[开发者的微博](http://weibo.com/u/1786665787)。

## JVM语言

- [Groovy](http://groovy.codehaus.org/) 语法最接近 Java 的 JVM 语言，但是是动态语言，做 DSL 非常方便，GroovyClassLoader 也可以直接做 Java 的 HotSwapClassLoader。
- [Scala](http://www.scala-lang.org/) 可能是语法最复杂的语言，有 FP 有 OO，语法糖非常多，应该算现在最流行的 JVM 语言，也为 JVM 语言提供了很多很高质量的库。
- [Clojure](http://clojure.org/) JVM 上的 Lisp，老实说我没啥了解。


## 打包工具

- [Apache Maven](http://maven.apache.org/) 很成熟的打包、项目管理、依赖管理工具。
- [Gradle](https://www.gradle.org/) 基于Groovy的新一代的Maven，语法更简洁易懂，多项目管理，插件开发更加简单。

## IDE

- [IntelliJ IDEA](https://www.jetbrains.com/idea/) 毫无疑问当前最好的 Java IDE。

## 库

### ByteCode

- [ASM](http://asm.ow2.org/) Java ByteCode 底层库。
- [Javassist](http://www.csg.ci.i.u-tokyo.ac.jp/~chiba/javassist/) ASM 的封装，写起来跟写 Java 一样简单。
- [Byte Buddy](https://github.com/raphw/byte-buddy) ASM 的封装，看起来跟 Javassist 差不多。

### 序列化与反序列化

- [Jackson](http://wiki.fasterxml.com/JacksonHome) 曾经性能最好的 json 序列化/反序列化库，现在还支持 Avro, CBOR, CSV, Smile, XML 和 YAML。
- [fastjson](https://github.com/alibaba/fastjson) 在去年我测试过是性能最好的 json 序列化/反序列化库。
- [Protocol Buffer](https://github.com/google/protobuf) Google 的跨平台二进制序列化协议，在游戏界广泛使用多年。
- [protostuff](https://github.com/protostuff/protostuff) 支持多种协议的序列化/反序列化，支持 runtime protobuf。
- [FlatBuffers](http://google.github.io/flatbuffers/) Google 新一代跨平台二进制序列化协议，时间效率上比 Protocol Buffer 要好。
- [Avro](http://avro.apache.org/) 一种二进制序列化协议，Hadoop 的子项目。


### 集合

- [Highly Scalable Java](http://sourceforge.net/projects/high-scale-lib/) 据说 Dr. Cliff Click 是首个 Lock-Free Wait-Free HashTable 的大神。2007年的[Presentattion](http://web.stanford.edu/class/ee380/Abstracts/070221_LockFreeHash.pdf)

### 工具包

- [Guava](https://github.com/google/guava) 简化集合操作，各种工具类。这个小项目几乎可顶一整个 Apache Commons。
- [Apache Commons](http://commons.apache.org/) Apache Commons 啥都有。

### 日志

- [SLF4J](https://github.com/qos-ch/slf4j) Logging Facade，Format 简直不能更爽。你可能还需要 `jcl-over-slf4j` 来让 Spring 依赖于 SLF4J。
- [logback](https://github.com/qos-ch/logback) 你还在用被作者 Ceki Gülcü 抛弃 `log4j`？

### 网络IO

- [Netty](http://netty.io) Java 首选 NIO 服务端框架，用来做游戏服务器多年，用来做 WebServer 有奇效。
- [spray](http://spray.io/) Scala Http 框架，跟 Akka 完美整合。
- [OkHttp](http://square.github.io/okhttp/) 简单易用的 http client。

### 分布式应用框架

- [vert.x](http://vertx.io/) 分布式 JVM 语言应用框架。
- [akka](http://akka.io/) Scala 默认 Actor 实现，Actor 框架，支持分布式。我之前的游戏服务器就用这个。
- [Apache Storm](http://storm.apache.org/) 分布式流式计算框架。很适合做日志分析。
- [Hadoop](http://hadoop.apache.org/) 不解释。
- [Spark](https://spark.apache.org/) 并行计算框架。

### 机器学习

- [Spark MLlib](https://spark.apache.org/mllib/)
- [datumbox-framework](https://github.com/datumbox/datumbox-framework)

### NLP

- [Apache UIMA](https://uima.apache.org/) 非结构化数据分析框架，我们现在的产品有一个 DEMO 就用他来做 NLP，好重好重。
- [Stanford CoreNLP](http://nlp.stanford.edu/software/corenlp.shtml) 啥，你不知道 CoreNLP。
- [FNLP](https://github.com/xpqiu/fnlp/) 中文 NLP 库，据说是现在中文 NLP 最好的开源库。复旦[邱锡鹏](http://weibo.com/u/1891924883)老师开发。

### 搜索引擎

- [lucene](http://lucene.apache.org/) Hadoop, Solr, Nutch 之前都是 lucene 的子项目。
- [elasticsearch](http://www.elasticsearch.org/) ES 是你另外一个选择。

### 测试

- [JUnit](http://junit.org/) 不解释。
- [mockito](https://github.com/mockito/mockito) Mock 的最佳选择。

### 数据库

- [druid](https://github.com/alibaba/druid) 为监控而生的数据库连接池！温少加油。
- [Commons DbUtils](http://commons.apache.org/proper/commons-dbutils/) 我就喜欢用这种轻量级的东西，错误不会爆得乱七八糟。
- [H2](http://h2database.com/html/main.html) Java 的嵌入式数据库。

### 新一代 J2EE

- [Grails](https://grails.org/) 看名字就知道是山寨谁的。
- [Play Framework](https://www.playframework.com/) Play1 出来的时候相当惊艳，Play2的步子迈得有点大，扯到蛋了。 
- [Lift](http://liftweb.net/) 没用过，不评价。
- [Ninja](http://www.ninjaframework.org/) 没用过，不评价。
- [JFinal](http://www.jfinal.com/) 试用过一下，不错。Route 那里的设计依旧不是很理想，可能是作者为了避免使用 Annotation。JFinal 的插件作者 Dreampie 的另外一个项目[resty](https://github.com/Dreampie/resty)的 Route 的设计就好多了，不过 resty 把整个项目都搞复杂了。

### 模板引擎

- [Beetl](http://www.oschina.net/p/beetl) 国人出品的模板引擎，几年前我写 ChaosBlog 的时候就用了。
- [FreeMarker](http://freemarker.org/) 
- [Velocity](http://velocity.apache.org/) 

### 同人逼死官方

- [Joda Time](http://www.joda.org/joda-time/) Joda Time 还需多介绍吗？ Java8 之前的唯一选择。
- [lyra](https://github.com/jhalterman/lyra) 一个 recovery 做得更好的 RabbitMQ Client。
- [redisson](https://github.com/mrniko/redisson) 一个新的 Java Redis 客户端，值得 Star 观望。

### 其他

- [jersey](https://jersey.java.net/) RESTful Web Services 老实说我只用了他的 Annotation 定义。
- [Spring](http://spring.io/) 我不喜欢Spring。
- [Guice](https://github.com/google/guice) Google 的依赖注入。
- [Spring Loaded](https://github.com/spring-projects/spring-loaded) Spring 的 HotSwap Class Loader。
- [Apache POI](http://poi.apache.org/) 读写 MS Office 库。恩，我喜欢他的命名规则，Horrible XX Format。
- [Jetty](http://eclipse.org/jetty/) Web容器，可以做嵌入式 HttpServer。


