{
layout: "pages",
title: "一个价值2.5亿的教程：如何开发一款自主知识产权的浏览器",
category: "Chromium",
description: "自主知识产权的浏览器开发教程",
tags: ["chrome","chromium"]
}

最近有公司通过开发“自主知识产权”浏览器拿到了2.5亿的投资，相信大家都希望能学习如何开发“自主知识产权”浏览器。本教程价值过亿，希望大家看完以后都给我发红包。

第一步你需要准备若干台配置好一点的电脑，Mac，Win，Linux各一台。内存起码要16G，否则会在build的时候OOM，硬盘最好是512以上的SSD，CPU则是越多越好（买按摩店的CPU就对了）。

第二步你需要部署编译环境，这里有各个平台的[教程](https://www.chromium.org/developers/how-tos/get-the-code)，跟着做就可以了。

第三步我们需要改名字，这一步其实改的文件相当多，作为一个“自主知识产权”的浏览器只需要跟着[这里](https://github.com/Eloston/ungoogled-chromium/issues/159)做就可以了。

第四步我们需要改图标改皮肤，去到[src/chrome/app/theme/](https://cs.chromium.org/chromium/src/chrome/app/theme/)，把你想改的图标都改一下就行了。

现在我们可以开始打包了。

```
# Linux
gn args out/R

is_component_build = false
is_debug = false
enable_nacl = false
remove_webcore_debug_symbols = true
enable_linux_installer = true

ninja -C out/R chrome/installer/linux

# Mac
gn args out/R

is_component_build = false
is_debug = false
enable_nacl = false
remove_webcore_debug_symbols = true

ninja -C out/R chrome/installer/linux

# Win
gn args out/R

is_component_build = false
is_debug = false
enable_nacl = false
remove_webcore_debug_symbols = true

ninja -C out/R mini_installer

# Android
ninja -C out/Android chrome_public_apk
```

好了，至此大家都学会了如何开发“自主知识产权”浏览器。希望大家都能赚到大钱。最后大家别忘了给我发红包哦。

最后开源协议等同于合同，请大家赚大钱的同时遵守开源协议，尊重开源作者与开源社区的贡献。