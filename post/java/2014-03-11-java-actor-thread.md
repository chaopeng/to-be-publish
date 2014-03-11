{
layout: "pages",
title: "Java 的Actor模型——线程模型的实现",
category: "java",
description: "分析在Java实现Actor模型中的线程模型",
tags: ["java","actor", "akka", "netty"]
}

本文默认你知道：

* [Actor模型](http://en.wikipedia.org/wiki/Actor_model)
* [akka](http://akka.io/)

最近很长一段时间我都在使用 akka 重构公司原有的框架，一直想写一篇 akka 的文章但是对 akka 认识的还是比较皮毛，所以很长时间没有写博客。在完成了大部分的工作之后，现在开始闲下来阅读 akka，看看这个号称 Scala 第一框架的[源码](https://github.com/akka/akka)。

对我来说最好奇的地方肯定是 actor 模型的底层线程调度。现在我们开始看 akka 的源码。

##akka

熟悉 akka 的朋友的都知道 akka 的默认线程池实现是使用 Doug Lea 的 `ForkJoinPool`，那我们来分析一下 `ActorRef.tell(msg, fromActorRef)`究竟做了什么事情。

```
ActorRef.tell(message, sender)
-> ActorCell # sendMessage(message, sender)
-> Dispatch # sendMessage(msg: Envelope)
-> Dispatcher # dispatch(this, msg)
-> Mailbox # enqueue(receiver, msg), registerForExecution(mbox)
```

看到这里消息已经传递到目标 actor 的 mailbox ，并将加入到 executorService 里面了。这里我们可以留意到 Mailbox 的其实是一个 Runable 来的，当 executorService 调度到这个 mailbox 的时候就会调用 processMailbox()。

可以看见这个 processMailbox() 只有对执行任务数量以及执行时间的限制，没有线程安全的代码。这里是在 mailbox 调用 registerForExecution 的时候就已经保证在 executorService 中只加入了一次。

看到这里，其实 akka 的线程调度的思路已经很清晰了。

其实 actor 模型并没有那么神秘，早在使用 netty 的时候，我们已经见过。netty 把这个叫做 `OrderedMemoryAwareThreadPoolExecutor`。

##netty

我们再看看netty的实现，netty的实现没有那么复杂核心就是 `OrderedMemoryAwareThreadPoolExecutor$ChildExecutor` 。
ChildExecutor 同时实现了 Executor 和 Runnable， ChildExecutor 的 execute() 将实际要运行的任务 offer 到 ConcurrentLinkedQueue，而 run() 将 queue 中的任务完成。调度上不一样的地方在于 ChildExecutor 的线程安全不是在加入任务的时候保证的，而是在 run() 的时候对 isRunning cas。同时 netty 的做法是一次将队列所有的任务全部完成，这样就通过限制队列的长度来确保公平性的，这样的缺点是如果队列达到限制长度的话，会产生阻塞。

##actor模型的线程模型实现要点

将放入线程池的任务，转化为针对拥有这个 actor 的队列，真正的任务是任务队列而不是单个任务。

