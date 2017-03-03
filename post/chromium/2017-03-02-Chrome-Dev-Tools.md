{
layout: "pages",
title: "开发Chrome的工具-1",
category: "Chromium",
description: "开发Chrome的工具整理",
tags: ["chrome","chromium"]
}

[TOC]

在Chrome也做了半年了，稍微自己做了一些工具，也用了一些别人的、第三方的工具，也做了一些文档。
在这里稍做一下整理。

## IDE/Editor

[这里](https://www.chromium.org/developers/)有好几个IDE/Editor可以选，其中`atom`那个
文档我改过，`qtcreator`和`vscode`的文档是我写的。我从一开始用`sublime`折腾到`atom`再折腾
到`qtcreator`和`vscode`。现在我倾向于在workstation用`qtcreator`，在workstation里面
index整个chromium只需要3分钟，太神了，秒杀VS、Eclipse、Clion、Xcode（这些我都试了）。
然后在笔记本里面就用`vscode`，尽管没有了refactor的功能，但起码代码跳转，补全都是可以的。

这是一篇我
[对`sublime`,`atom`和`vscode`在Chromium下使用的体验文章](https://www.zhihu.com/question/41857899/answer/129138208)。

## Debugger

尽管在大部分情况下打Log都是Chromium最常用的调试手段，但是有些时候Debuggger确实更加实用。
调试上面，可以选择的有Linux下`GDB`，Mac下`LLDB`和Windows下`VS`，Linux下的`LLDB`停不了断
点。图形化前端在Linux下可以选择`qtcreator`，Mac下可以选择`xcode`，我猜`vscode`也可以，不
过现在`vscode`的debug配置有点麻烦，我放弃了。另外在Linux下`GDB`启动很慢，我找到了个
`gn args`可以加速一点，但还是很慢在workstation上比我在rmbp上用`xcode`还慢，所以如果要用
debugger我还是推荐用`xcode`。

## 配合Code Search的工具

Chromium Code Search是非常强大的工具，特别是像Chromium那么庞大的代码，
但是当需要编辑就要复制文件名，然后跳转到对应行号或者函数名，太麻烦了，所以我开发了一个
[chrome扩展](https://chrome.google.com/webstore/detail/ome/ddmghiaepldohkneojcfejekplkakgjg)
，在Chromium Code Search右键就可以在你的Editor上打开相应地文件和位置。不过需要做些
[配置](https://chromium.googlesource.com/chromium/src.git/+/master/tools/chrome_extensions/open_my_editor/README.md)。

1. 安装Chrome Extension
2. 安装依赖`pip install bottle sh`
3. 启动`omed.py`，`python ${CHROMIUM_SRC}/tools/chrome_extensions/open_my_editor/omed.py`
4. 编写你的`myeditor`，参考[`myeditor-example/`](https://chromium.googlesource.com/chromium/src.git/+/master/tools/chrome_extensions/open_my_editor/myeditor-example/)

我另外还做了个小脚本用来从Editor跳转到Chromium Code Search。

```
chrome https://cs.chromium.org/chromium/src/${path}?l=${line}
```

### TODO

有位同事做了一个`sublime`的[插件](https://github.com/karlinjf/ChromiumXRefs)可以使用
Code Search的index来查define，override，调用和跳转。看上去是相当的方便，我准备port到
`vscode`上，当然了`qtcreator`已经有这样的功能了。

## 下载Chromium Archive版本

Chromium的buildbot会将所有build好的二进制放到
[这里](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html)
，所以你可以在这里下载到最新最新的build或者你给定任意版本的build。对于我来说，有时候快速定位一
个bug在新版本能不能复现，很有用，所以我做了一个脚本专门下载最新的或者给定版本的build，代码在
[这里](https://github.com/chaopeng/chromium-downloader)。

## 复制trybot的编译参数

有些时候，trybot会返回一些在本地难以复现的错误。有些时候确实跟trybot的编译参数有关，这时候可以
试试用这个[工具](https://cs.chromium.org/chromium/src/tools/mb/mb.py)。一个例子：

```
${CHROMIUM_SRC}/tools/mb/mb.py gen -m chromium.fyi -b "Site Isolation Android" <output directory>
```

## 给trybot提交perf test

如果你收到一个perf regression该怎么办，很可能这个并不关你事，要证明你可以先创建一个revert 
patch，然后在trybot上跑一次这个test。

```
${CHROMIUM_SRC}/tools/perf/run_benchmark try $trybot $task
```

*暂时就这么多了，等我有新的再开新的文章吧。*