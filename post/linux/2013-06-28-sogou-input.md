{
layout: "pages",
title: "ubuntu安装搜狗输入法",
category: "linux",
description: "ubuntu安装sogou for linux",
tags: ["linux","ubuntu", "ppa", "搜狗", "输入法"]
}

相信用linux的兄弟心中一直都一根去不掉的刺，没有一个好的拼音输入法。至于有什么问题相信用linux做桌面系统的都知道。

等了那么多年还是deepin给力跟搜狗合作搞了sogou for linux，好吧这玩意还是基于fcitx的，fcitx原来会遇到的问题还是会遇到。但是搜狗输入法原来的一些人性化设计还是保留了。我用了1个月了，感觉不错。这里推荐一下，这里是安装方法：

```{shell}
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install fcitx-sogoupinyin
```

这里是没有皮肤的，如果你实在想要搜狗的皮肤的话，可以去[这里](http://packages.linuxdeepin.com/deepin/pool/main/f/fcitx-skins/) 下载，确实看上去也不错。

