{
layout: "pages",
title: "如果我发现了Chromium的性能问题",
category: "Chromium",
description: "如果我发现了Chromium的性能问题",
tags: ["chrome","chromium"]
}

## 引子

在知乎里，我最常收到的评论是：Chrome卡了。大概每天都能收到的那种。为了给大家解决或者正确的去report这个问题，经过会花费我不少时间，今天我就来说说，当我觉得Chrome卡了，该怎么办。

## 一般流程

0. 不管了，卡就卡吧。（手动滑稽)
1. 看看是电脑在卡还是浏览器在卡。
2. 看Chrome里面的任务管理器，确认有没有页面或者扩展在挖矿。
3. 最近Chrome在DevTools加了Performance Monitor，这里可以看到究竟是什么卡了，JS还是浏览器。 
4. 当然很多同学遇到的卡，不是整个Chrome都卡，这时候你需要看看是什么卡，比如是鼠标移动卡，滚动卡还是什么别的。
5. 当我知道是滚动卡，再来多一些信息，比如滚动是来自滚动条的，鼠标滚轮的，触摸板的还是触摸屏的。
6. 再看看是开所有URL都卡，还是只有某个URL卡。如果所有URL都卡，甚至连Google搜索页都卡，一般而言都不会是我们的锅。
7. 如果是某个URL卡，看看是在这个网页所有地方滚动都卡还是在某个地方滚动才卡。

好了做完以上流程，你能得到这样的一句话：

我用Chrome浏览（某个具体URL）的时候，在某个地方滚动（或者别的操作）时候会卡。后面加上系统，Chrome版本，可能电脑的型号。就已经是一个不错的report了。

## 如果我是比较厉害的用户

一般来说来找到我的，都是比较厉害的用户。这时候你可以做一件事件来帮助Chromium开发者研究这个卡，毕竟我们经常都没有能复现的环境。

打开chrome://tracing，点record，点Edit categories，在Record Categories下面选All，然后点右下角Record。
切换到你的页面，重复你会卡的操作若干次。再点回来tracing这个tab，按stop，点save。
这样就会生成一个Chromium Tracing的report，这个也提交或者发给我。我就可以进去看到是什么东西有问题了。

## 如果我是开发者

如果我是开发者，发现我新搞的东西好像让滚动卡了，该怎么办呢？

除了可以给我看看Chromium Tracing，你还可以去看看DevTools - Render里面的工具，可以高亮滚动慢的元素，高亮重绘的元素，显示FPS，可高端了。
这里放一下[Chrome不能优化滚动的理由](https://cs.chromium.org/chromium/src/cc/input/main_thread_scrolling_reason.h)。

```C++
// Non-transient scrolling reasons.
kNotScrollingOnMain = 0,
kHasBackgroundAttachmentFixedObjects = 1 << 0,
kHasNonLayerViewportConstrainedObjects = 1 << 1,
kThreadedScrollingDisabled = 1 << 2,
kScrollbarScrolling = 1 << 3,
kPageOverlay = 1 << 4,

// This bit is set when any of the other main thread scrolling reasons cause
// an input event to be handled on the main thread, and the main thread
// blink::ScrollAnimator is in the middle of running a scroll offset
// animation. Note that a scroll handled by the main thread can result in an
// animation running on the main thread or on the compositor thread.
kHandlingScrollFromMainThread = 1 << 13,
kCustomScrollbarScrolling = 1 << 15,

// Style-related scrolling on main reasons.
// These *AndLCDText reasons are due to subpixel text rendering which can
// only be applied by blending glyphs with the background at a specific
// screen position; transparency and transforms break this.
kNonCompositedReasonsFirst = 16,
kHasOpacityAndLCDText = 1 << 16,
kHasTransformAndLCDText = 1 << 17,
kBackgroundNotOpaqueInRectAndLCDText = 1 << 18,
kHasBorderRadius = 1 << 19,
kHasClipRelatedProperty = 1 << 20,
kHasBoxShadowFromNonRootLayer = 1 << 21,
kIsNotStackingContextAndLCDText = 1 << 22,
kNonCompositedReasonsLast = 22,

// Transient scrolling reasons. These are computed for each scroll begin.
kNonFastScrollableRegion = 1 << 5,
kFailedHitTest = 1 << 7,
kNoScrollingLayer = 1 << 8,
kNotScrollable = 1 << 9,
kContinuingMainThreadScroll = 1 << 10,
kNonInvertibleTransform = 1 << 11,
kPageBasedScrolling = 1 << 12,

// The maximum number of flags in this struct (excluding itself).
// New flags should increment this number but it should never be decremented
// because the values are used in UMA histograms. It should also be noted
// that it excludes the kNotScrollingOnMain value.
kMainThreadScrollingReasonCount = 23,
```

其中有几个很常见的理由就是：

1. 你没有给ScrollableArea一个纯色的背景，无背景或者图片背景都会被认为滚动可能导致重绘而放弃优化
2. 监听wheel，并且在里面进行大量重绘

## 更多

好了，我知道的基本都说了，希望大家以后找我的时候能够更好的表达自己的问题吧。
关于如何报bug的，你可以参考我前一篇文章，[如何给Chrome报bug](http://chaopeng.me/blog/2017/08/21/how-to-report-chromium-issue.html)。
