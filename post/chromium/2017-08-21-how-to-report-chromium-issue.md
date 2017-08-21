{
layout: "pages",
title: "如何提交Chrome的bug",
category: "Chromium",
description: "如何提交Chrome的bug",
tags: ["chrome","chromium"]
}

## 引子

近一年，我在微博，知乎，微博，还有各种微信前端群收了不少的关于chromium的bug。有些真的是我工作范围，我自己去验证去提去修，有些不是我范围的，我就只能帮忙提交或者找已经提交的issue。由于前端群太多了，我加不来，加了也看不完，所以开了个专门收bug的群，一是方便各位群里的大大如果发现有前端群在聊chromium的bug可以帮忙引到这里来，让他们的bug能够最终去到正确的地方，二是有个地方给各位大大讨论。

同时经过这一年的收bug，我也发现很多朋友并不知道如何给chromium报bug，所以这里就提供一下方法。

注：我的工作范围是滚动，滚动条，触摸板，pinch-zoom，鼠标事件，hover，hit test这些。这一些范围的bug，我非常乐意去修或者告诉你为什么修不了。有相关的bug，欢迎email：chaopeng#chromium.org (# -> @)。

## 如何提交一个Chromium的bug

### 什么时候我应该觉得这是一个Chrome的bug

对于使用者：

1. 用Chrome访问一个网页崩溃了，用别的浏览器不会
2. 用Chrome访问一个网页行为跟别的浏览器不一致，比如某个元素的位置不一样
3. 用Chrome访问一个网页，我某个行为不能达到我预想的效果，比如滚动条拉了不滚动

对于开发者：

1. 某个HTML, CSS或者JS崩溃了
1. 某个HTML, CSS或者JS没有达到预想的效果 （W3C标准未覆盖的）
2. 某个HTML, CSS或者JS效果跟其他浏览器不一样 （W3C标准未覆盖的）
3. 某个HTML, CSS或者JS效果跟W3C标准不一样
4. 某个HTML, CSS或者JS的效率在新版本变差了

注：有些时候是某个插件导致的，这时候可以开右上角Guest排除插件的影响。

### 找复现的规律

对于一个不能复现的bug，我们一般是束手无策的，一般最后只能不了了之。所以复现规律很重要，对于非开发者而言，复现一般是访问某个网页，经过某几步特定的操作可以复现。对于开发者而言，最好提供一个简单复现的页面，当然这也不一定，chromium会对复杂网页做些优化，某些bug可能只在复杂的网页上出现。同时，系统版本，chromium版本也是非常重要的。

### 提交

至此，我们已经有足够的信息去提交一个bug了。用Chrome访问crbug.com（可能需要番茄），注册一个帐号，点左上角`New issue`，Template选开发者（developer）或者使用者（user）。然后就可以开始填了。

```
Chrome Version: (copy from chrome://version) Chrome版本
OS: (e.g. Win7, OSX 10.9.5, etc...) 系统版本

What steps will reproduce the problem? 复现步骤
(1)
(2)
(3)

What is the expected result? 预期结果

What happens instead? 实际结果

Please use labels and text to provide additional information. 请使用标签


For graphics-related bugs, please copy/paste the contents of the about:gpu
page at the end of this report. 如果是图像相关的请贴about:gpu的内容
```

好的至此我们已经提交了一个bug。

## 为chromium开发者提供更多的线索

### 对于英文不好/会的朋友怎么办

不好的你可以用Google翻译机翻，一般来说效果还可以，如果你不确定行不行，你可以把你的中文描述也放上去，chromium那么多中国人总能帮你翻译好。其实其他国家用撇脚英文报bug的多了去了，很多时候我一看上去看不懂，再去问提issue的人。

第二你可以拍个视频，现在拍视频那么简单，而且视频比文字有时候准确多了。

### 新旧版本不一致

比如你发现了个问题，旧版本是好（快）的，新版本坏（慢）了，这在提交的时候可以加上：

This works at xxx, but not at xxx. 然后在labels那里加Needs-Bisect，这样就会有专门的QA同事去做二分，查坏掉的commit。

### 对于Crash

https://www.chromium.org/for-testers/bug-reporting-guidelines/reporting-crash-bug 暂时不翻译了，对于国内的用户因为qiang的问题，往往不能自动report，只能按Collecting Crash Data那段的方法去做了。

### 对于编译上的问题

一般很少发到crbug.com，你可以选择发到Google Groups的chromium-dev。 https://groups.google.com/a/chromium.org/forum/#!forum/chromium-dev

### 更多问题

你可以在https://www.chromium.org/for-testers/bug-reporting-guidelines找到答案。如果还有不清晰的地方也欢迎各种PM我，你总能在某个社交平台找到我。
