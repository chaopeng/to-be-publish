{
layout: "pages",
title: "akka试用——矩阵乘法",
category: "java",
description: "用java、scala的库akka编写矩阵乘法的心得",
tags: ["java","akka"]
}

akka
---
<a href="http://akka.io/" title="akka" target="_blank">akka</a>是一个Java的actor模型的库，可以很好的进行并行计算。并且可以比较简单的扩展成一个集群利于代码的重用。需要中文的文档的可以访问<a href="http://www.gtan.com/akka_doc/index.html" title="akka中文文档" target="_blank">这里</a>，感谢广谈公司的贡献。

akka适用范围
---
akka比较适合适用在一些无状态的场景，其实actor模型对游戏这种有状态的场景的优化并不多，但是也是可以简化并法的模型的。当然从游戏设计的角度出发，大部分的逻辑都应该是无状态的。

akka可以很简单的搭建一个简单的mapreduce的计算模型，也可以搭建一个很简单的流计算模型，关键是用akka来搭建可以更自由的控制底层。

矩阵的乘法
---
学数学的人都对矩阵的乘法有一种痴迷的态度。如果你不知道什么是矩阵的乘法可以去看一下<a href="zh.wikipedia.org/zh-hk/矩陣乘法" title="维基百科-矩阵乘法" target="_blank">维基百科</a>。

矩阵的乘法有很多很经典的优化，这些不是本文的介绍范围我就不多说了。当需要并行的时候，矩阵乘法更重要的是并行，而不是使得耦合度变高的优化，之前曾经在hadoop上做过矩阵的乘法，但是效果不是很好。可以说在1000阶之下，17个节点的hadoop还是要比单机跑lapack要慢很多的，但是总会有单机不能完成的大小的任务的（途乐的万博士说的）。是否在更高阶之上hadoop会比mpi做得更好我不知道。但是这是一个方案。但是hadoop做矩阵的乘法并不是一个很好的选择，发布任务的主函数需要去监听所有任务的完成以及最后收集结果，但是这个api从1.0到2.0的变化太大了。hadoop是一个很不自由的东西。

现在我们尝试一下用akka来实现矩阵的乘法，当然一下程序的完善需要一个过程，我们先看看这个简易的矩阵乘法的包结构。
<pre>
├─actor
│  └─matrix
│          ChengActor.java
│          ChengMasterActor.java
│
├─model
│  ├─matrix
│  │      Matrix.java
│  │
│  └─vector
│          Vector.java
│          VectorException.java
│
└─msg
    └─matrix
            ChengMsg.java
            ChengResultMsg.java
</pre>

首先是model包：matrix和vector。分别实现了矩阵、向量的存储，还有向量的点乘法。具体实现的代码我就不贴在这里了。`VectorException`定义了向量点乘时候的异常。

msg包定义了两种消息，乘法的消息和结果的消息。

actor定义了两个：

`ChengActor`监听`ChengMsg`的消息，做向量的乘法，然后发出`ChengResultMsg`。
```java
public class ChengActor extends UntypedActor {

	@Override
	public void onReceive(Object msg) throws Exception {
		if(msg instanceof ChengMsg){
			ChengMsg chengMsg = (ChengMsg) msg;
			double res = Vector.cheng(chengMsg.v1, chengMsg.v2);
			getSender().tell(new ChengResultMsg(chengMsg.hang,chengMsg.lie,res), getSelf());
		}
		else{
			unhandled(msg);
		}
	}

}
```
`ChengMasterActor`创建的时候定义若干个`ChengActor`，并将需要相乘矩阵分发给`ChengActor`，这里用的路由是akka提供`RoundRobinRouter`。`ChengMasterActor`监听`ChengResultMsg`，并最后组装结果矩阵。

感受
---
整体来说akka的api设计还是不错的当然tell那里确实有点猥琐，不过可以支持路径，真正项目不会使用引用来actor引用来tell的吧。这样不是很好解耦。很多东西都很自由是akka的好处，支持自定义异常处理，支持自定义序列化。可惜集群功能要等到2.1版。等下个版本再折腾吧。

源代码
---
本文所有源代码已托管到<a href="https://gitcafe.com/chao/akka-matrix-multiplication/tree/master/" title="akka的矩阵乘法的源代码" target="_blank">GitCafe</a>。

