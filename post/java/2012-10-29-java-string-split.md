{
layout: "pages",
title: "Java字符串分割",
category: "java",
description: "比较各种java字符串分割的坑",
tags: ["java"]
}

java的字符串分割在字符串类里面有一个很方便的函数叫split。调用十分方便，相信一般的初学者都知道。
```java
String s = "1&2&3";
String[] substrs = s.split("&");
```

但是这个常用的函数是有不少坑的。

对特殊串的分割处理
---
比如这样的例子
```java
//--------1---------
String[] substrs = "".split("&"); 
// 这样会substrs 的长度是1, 这个还比较好理解
//--------2---------
String[] substrs = "&".split("&"); 
// 这样会substrs 的长度是0, 这个按照上面的逻辑就不好理解了
//--------3---------
String[] substrs = "&&".split("&"); 
// 这样会substrs 的长度依然是0, 这个按照上面的逻辑就更不好理解了
//--------4---------
String[] substrs = "1&&".split("&"); 
// 这样会substrs 的长度是1, 这个按照上面的逻辑稍微又好理解了一些
```

出现错误不抛出
---
上面的问题通过尝试还是可以有所了解的。问题出现在一次我对uri的处理上面，如这样的串: `http://www.baidu.com/s?wd=av`，ok现在我要分割这个问号了：
```java
String[] substr = "http://www.baidu.com/s?wd=av".split("?");
```
好了，线程直接就断掉了，tmd这什么回事？会想一下才记起来split是支持正则表达式的，这个?确实会引发问题，但是尼玛的给个异常不行吗？这绝对是坑新人啊。

-----------

基于以上问题，我还是建议大家在分割字符串的时候使用StringTokenizer这个类。这个类尽管比较旧，但是能解决上面几个问题。
特殊串处理的结果
---

```java
//--------1---------
StringTokenizer st = new StringTokenizer("", "&");
int n = st.countTokens(); // 结果是0
//--------2---------
StringTokenizer st = new StringTokenizer("&", "&");
int n = st.countTokens(); // 结果是0
//--------3---------
StringTokenizer st = new StringTokenizer("1&", "&");
int n = st.countTokens(); // 结果是1
```
这样的结果比起split好处理多了

不支持正则表达式
---
StringTokenizer不支持正则表达式，但是支持一种很奇怪的方法delim的每一个字符都会视作分割符。当然如果有这样的需求的话，还是用split的正则表达式比较划算。


