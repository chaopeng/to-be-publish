{
layout: "pages",
title: "linux下模拟网络延迟丢包",
category: "linux",
description: "用tc netem模拟linux下的恶劣网络环境",
tags: ["linux","网络", "延迟", "丢包"]
}

开始开发手游以来，网络环境的恶劣是以前做页游无法想象的。而往往由于设计上考虑不充分，恶劣的网络环境会导致各种莫名其妙的bug。最近调整到公司的研发部工作。第一件事就是想用linux模拟恶劣网络环境让游戏能得到更方便的测试以及方便验证设计。

我的一开始的想法是用一台linux做网关，然后用iptables来模拟恶劣环境。找了一下原来linux上有netem可以方便模拟恶劣环境，而且功能还非常强大。

```{bash}
## 基本命令 ##

# 显示当前有哪些命令
tc qdisc show
# 加入
tc qdisc  add dev eth0 root ...... 
# 修改存在的 qdisc ,记的,加入同一条后只能用 change 来修改
tc qdisc  change  dev eth0 root ...... 
# 删除
tc qdisc del dev eth0 root

## 网络延迟 ##

# 设置固定delay 100ms (所有经过eth0的包都被延时了100ms)：
tc qdisc add dev eth0 root netem delay 100ms
# 设置delay 100ms Jitter 10ms:
tc qdisc change dev eth0 root netem delay 100ms 10ms
# 其实Jitter是有相关性的，如果要设置Jitter的相关性25%：
tc qdisc change dev eth0 root netem delay 100ms 10ms 25%
# 设置Jitter为正态分布。
tc qdisc change dev eth0 root netem delay 100ms 20ms distribution normal

## 网络丢包 ##

# 设置丢包率10%
tc qdisc change dev eth0 root netem loss 10%
# 丢包率也有相关性。 如设置10%的丢包率，但是丢包率之间的相关性为25%
tc qdisc change dev eth0 root netem loss 0.3% 25%

## 包乱序 ##

# 乱序， 每第5个包马上发送，其他的包间隔10ms发送。
tc qdisc change dev eth0 root netem gap 5 delay 10ms
# 乱序， 10%的包（相关性为25%）马上发送，其他的包间隔10ms发送。
tc qdisc change dev eth0 root netem delay 10ms reorder 10% 25%


# 包的duplication。
tc qdisc change dev eth0 root netem duplicate 3%
# 包的corruption。
tc qdisc change dev eth0 root netem corrupt 0.1%
```
