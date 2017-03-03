{
layout: "pages",
title: "Chrome开发笔记",
category: "Chromium",
description: "Chrome的开发笔记整理",
tags: ["chrome","chromium"]
}

[TOC]

## 引子

关注我[微博](http://weibo.com/chaojianpeng)的朋友都知道我最近去了Google的Chrome组。
具体我所在的组是在做Chromium的输入相关的工作，比如鼠标事件，键盘事件，手势事件之类的东西，
所以大家有相关的bug也可以报给我。
因为工作不涉及闭源部分，基本上我算是一个开源软件工作者。
本文也会只涉及Chromium的开发，下面我统一叫Chrome。

## 环境部署

我部署的环境在Linux上的，基本上我们组的同事也是主要在Linux工作站（24核48线程，编译一遍只需要10分钟）上开发，
需要远程开发的时候就用笔记本的Chrome Remote Desktop，完全用SSH会导致Layout Test无法运行。

部署步骤可以看这篇[文章](https://www.chromium.org/developers/how-tos/get-the-code)，
Android的话可以看这篇[文章](https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md)。
Chrome的构建系统以前用的是GYP，现在用的是ninja，都是为Chrome写的，所以基本也就只有Chrome在用-_-|||。

由于构建系统不是主流的玩意，基本上不需要考虑有很好的IDE支持。对于IDE支持我暂时尝试过Clion和Eclipse，
结果是Clion基本不需要考虑了，读入项目等待了1个小时都还没有反应；Eclipse的话相对好一些，大概2分钟就开始有反应，
然后后台index，index大概花了2个半小时，写代码的时候的提示还可以，只是不可以关，Eclipse没有对index的结果缓存。

IDE行不通之后我试了VIM，VIM的配置文档在[这里](https://chromium.googlesource.com/chromium/src.git/+/master/tools/vim/chromium.ycm_extra_conf.py)。
注意要先配好Clang，而去将Build改成用Clang，文档在[这里](https://chromium.googlesource.com/chromium/src/+/master/docs/clang.md)。
我配完玩了有点鸡肋，补全找到的东西不是很好，跳转会显示找不到文件，会报一堆错。我本来以为只有我自己是这样，问了一下其他同事，他们也是这样，我可受不了这样的渣渣。

暂时我找到最好的编辑器是Sublime Text，文档在[这里](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_sublime_dev.md)，
我昨天配了半个小时，暂时只有Lint工作得不正常（每行都报错）。代码补全还不错，不过只能在Linux下工作，而且件作者已弃坑-_-|||。

## 链接

- [Code Search](https://cs.chromium.org/)
- [Issue](https://bugs.chromium.org/p/chromium/issues/list)
- [文档](https://www.chromium.org/developers)
- [比较新一点的文档](https://chromium.googlesource.com/chromium/src/+/master/docs)
- [Testharness.js](http://testthewebforward.org/docs/testharness-tutorial.html)

吐槽一下吧，很多东西都是没有文档的，特别有些Feature是其他公司写的，连设计文档都没有-_-|||。

## 代码片段

### 怎样获得call stack

```c++
#include "base/debug/stack_trace.h"

base::debug::StackTrace st; st.Print();
```



## 测试

我做的这块有两种测试一个叫[Layout Test](https://www.chromium.org/developers/testing/webkit-layout-tests)，
一块叫Blind Unit Test。其实二者没有很严格分开使用范围，很多不是Unit Test的也写在Blind Unit Test。
Layout Test是用HTML写的，原理是build一个开放了浏览器内部API的浏览器。这样什么东西都能测。
Blind Unit Test是Unit Test，Mock了很多东西，所以有时候逻辑不能完整跑，好处是可以拿到很多浏览器内部状态去检查。

Blind Unit Test没有文档，代码在`third_party/WebKit/Source/web/tests`，然后执行`ninja -C out/Default webkit_unit_tests`运行。
