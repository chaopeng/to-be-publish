{
layout: "pages",
title: "ChaosBlog重构计划",
category: "ChaosBlog",
description: "经过3年时间的使用，是时候重构ChaosBlog了",
tags: ["ChaosBlog"]
}

## 缘起

ChaosBlog 自从 2013-01-06 完成以后就一直没有什么大的变动。做得最多就是换换 CDN。当然这3年了 ChaosBlog 也一直用的好好的。早一段时间，我看到一个新的库 [clipboardjs](http://clipboardjs.com/) 的出现，很适合用于替换我以前用 flash 实现的剪贴板功能，所以上个月有空，我就给 [ChaosCodebox](https://github.com/chaopeng/chaoscodebox) 换上了，换上的过程也是十分轻松。但是想了想经过了3年是时候去以前的代码，鄙视一下以前的自己。所以就有了这个重构的计划。

## 计划

-   修改剪贴板插件，移除 flash 依赖

    flash 几乎是上一个时代的代名词了，有些浏览器还开始了默认选项不打开 flash 支持，这个3年前呼风唤雨的平台终于算是走到了尽头，是时候换上新东西了。由于公司内部系统也在用 ChaosCodeBox 的 flash 剪贴板，所以我就上班时间去改到clipboardjs了。

- ChaosBlog 代码重构，打包支持

    3年前的我不知道打包为何物，不知道依赖管理为何物，现在知道了，自然要换到我熟悉的 Gradle。同时也要整理下这3年前的代码。

- markdown 引擎更换（考虑）

    3年前我选择了 Python-Markdown 作为引擎，不知道现在是否有更好的选择。

- markdown 格式更换（考虑）

    3年前我选择了使用一个json放在文章最前面来做 meta 信息，然而这个东西会使得我的文章直接在 Github 上显示得很奇怪，我考虑将此换掉。

- ChaosBlog 分页支持

    ChaosBlog 是没有实现文章分页的，现在确实有必要去做做这个事情了。

- 前端响应式修改

    现在啥都说移动端了，不说别的，我自己在手机上看都不舒服，确实要改了。

## 进度

- ChaosCodebox 修改

    完成

- ChaosBlog 代码重构，打包支持

    Gradle打包，项目目录修改都好了，但是以前的代码确实有点看不下去，待我一步步重构/写。
    
- markdown 引擎更换

    考察了一波 Java 的 Markdown 引擎确实还是没有像 Python-Markdown 那么完整的，但是现在的我应该有能力自己改一个了。昨天试了一下，花了3个小时给 [pegdown](https://github.com/sirthias/pegdown) 加了 TOC 的支持，这样就跟我在用 Python-Markdown 的特性一致了，而且我还多加了在 `TOC` 标签做标题等级限制。已经 PR 了，现在等 pegdown 那边 merge 就可以用了。我没想到我也能这么轻松改 Parser，挺惊喜的，毕竟这块东西我以前完全没有接触过。
    
- markdown 格式更换
- ChaosBlog 分页支持
- 前端响应式修改

## 反思

其实，这次重构 ChaosBlog 更多是用来反思以前自己写的代码。后续我应该会写一些反思 ChaosBlog 文章，待续吧。

