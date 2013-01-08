{
layout: "pages",
title: "为什么会有ChaosBlog",
category: "ChaosBlog",
description: "构思ChaosBlog的心路历程",
tags: ["ChaosBlog", "jekyll"]
}

博客换了很多次了，这是我第二个技术博客，之前一个是比较长时间使用的sina sae的wordpress。
用wordpress不爽的地方在于，我知道这个东西扩展性很好，但是自己做一些扩展总是很麻烦，太臃肿了。
高亮插件用了好多个也始终没有找到一个很舒服的。
后来看了一些用github page写blog大牛，才发现这个才是我想要的写作环境。

* 没有坑爹的富文本编辑器，可以用任意的文本编辑器写作
* 支持版本管理
* 支持用markdown语法写作
* 生成静态页面，免除安全隐患
* github page 免费

这一切看上去都相当美妙。
可惜[Jekyll](http://jekyllrb.com/ "Jekyll")不是很符合我的哲学。

为什么不用Jekyll
----
看过了大牛的用Jekyll写的博客，其实一开始也想用Jekyll的，为了测试一下这个环境我安装了很多东西，试玩了一下发现这个东西不是很适合我。

* markdown的方言很多，Jekyll在用的引擎支持的语法刚好我不是很喜欢
* Jekyll自身太少东西了，必须要配上皮肤，但是换上bootstarp-Jekyll又太复杂了，连disqus都设好了，只需要在config修改账号就可以了。但是换了好多个皮肤都不喜欢排版，也不喜欢
* windows部署相当麻烦
* 本地的Jekyll和github的Jekyll有差异

重写
----

所以我也不将就了，重写吧，我的目标是：

* 文章内容版本管理与生成页面分开管理
* markdown引擎可更换，只需要在命令行调用你喜欢的引擎
* 布局不要太复杂，尽可能把网页不同位置的部件分开

由于我自己是做后端的，前端的东西也是慢慢摸索的，从元旦开始做了一个星期才算是完成了这个chaosblog。
由于前端的东西确实接触不多，所以模板引擎我就挑了一个国人的开源[Beetl](http://www.oschina.net/p/beetl "beetl")。

暂时看上去也就这样子了，先用用看吧。

现在在使用的一些服务
----

* 默认的社交化评论系统用的是多说的
* 百度分享

使用上面的这些服务主要是比较符合我们天朝的国情吧，你懂的。

ChaosBlog的使用说明
----
* 把文章以md或者html后缀存放在`/to-be-publish/post/`下任意子目录，命名规则为`yyyy-mm-dd-题目(英文).后缀`
* 文章开头需要写一段json，这个内容跟Jekyll是一样的
* 修改`/to-be-publish/_config.json`中的`inputpath outputpath`到相应的目录，修改`markdownengine`为你喜欢的markdown引擎，修改其他个性化的项目
* 修改`/to-be-publish/_component/`下面你需要修改的组件，你必须要修改的组件是`includefile`里面的评论系统的账号，当然你完全可以修改成你想要的社交化评论系统
* 在`/to-be-publish/`下执行`buildBlog.bat` 如果是linux则执行`buildBlog.sh`
* 把你的/publish 发布到你的站点吧
