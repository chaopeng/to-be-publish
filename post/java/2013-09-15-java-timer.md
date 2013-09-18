{
layout: "pages",
title: "java timer是个坑",
category: "java",
description: "how to solve java timer timer already cancel",
tags: ["java","timer"]
}


上周我要开发一个ios的push，push这坑很深就暂且不表了。这里说说这段开发差点触发的一次服务器的故障。

我们游戏是山寨coc的，所以需要延时推送。所以就需要一个定时器了。Java的Timer很简单，只要实现一个TimerTask然后new一个Timer再在里面执行schedule就可以了，而且还可以对TimerTask取消。看上去世界很美好。但是用了这个的后果就是我当天要等到晚上11点做临时维护。

当天测试的时候，以前都很顺利，没有出现什么状况，于是在测试服上测试了3个小时就决定将其作为明天更新的版本。第二天回来，服务器更新一切都很顺利，到了下午的时候QA同事突然发现测试服很多功能都不能用了，上去看日志。发现一堆`timer already cancel`。马上查代码完全没有对timer进行过cancel()，只对timer调用过schedule()，只有TimerTask才有cancel。当时就蒙，赶紧google，stackoverflow。发现timer有2个坑：

1. TimerTask的run()默认不会处理任何的异常。需要用try-catch保护。这个我事先已经想到了。
2. timer会因系统时间修改而崩溃，测试服上确实部署了时间同步的脚本，timer也是这样死掉的。真心庆幸这个测试服的timer死得快。

同时还推荐了使用ScheduledExecutorService来替代Timer。终于解决了这个坑了，然后就是等到11点更新。

写下这篇文章，让遇到`timer already cancel`然后觉得不明觉厉的朋友可以快速解决问题。
