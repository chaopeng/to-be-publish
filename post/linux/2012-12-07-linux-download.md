{
layout: "pages",
title: "linux的批量下载工具",
category: "linux",
description: "linux下载工具批量下载用法",
tags: ["linux","wget","multiget"]
}

最近快手看片下东西经常会卡爆内存，无奈之下还是电脑下好存手机好了。
下面是我测试的几个linux的下载工具：

wget
---
老家伙了，连我都知道的东西。下载也是很方便的。弄个文件把要下的链接全放进去，然后`wget -i filename`就可以了。

multiget
---
wget不支持多线程一直是个大问题，所以我一般只是把wget作为比浏览器下载好用一点点的下载。一般我用multiget，批量下载更是方便,特别是某个网站的下载链是”xxx/集数.rmvb”的。 直接xxx/[开始-结束].rmvb。太方便了。 
