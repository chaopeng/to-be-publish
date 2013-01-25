{
layout: "pages",
title: "notepad++上使用markdown",
category: "chaosblog",
description: "在notepad++上的markdown高亮以及生成",
tags: ["markdown", "notepad++"]
}

引言
----
> 自从开始用markdown写博客以后，开始越来越喜欢使用markdown写作，
> 而且最近开始使用markdown写作更多的东西，尝试了好多个markdown的编辑器都不尽人意。
> 或者说是使用起来相当不习惯。所以试着去配置一下notepad++。使得更适合我写作。

markdown的高亮
----
其实有没有高亮其实是没所谓的，毕竟markdown的语法比较简单。
就算没有高亮也不会有太大的问题，但是为了可以看起来更加舒服。
github上面有一个很好的[配置文件] (https://github.com/robwierzbowski/Notepad---Markdown-Highlighting/blob/master/userDefineLang.xml) ，
你也可以使用这个配置文件来配置。当然这个配置文件相当不适合我这个黑夜系的程序猿。
所以我也根据这个修改了[一个] (https://github.com/chaopeng/Notepad---Markdown-Highlighting/blob/master/userDefineLang.xml) 。 

如果你需要使用这个可以运行(即win键+r) 执行`%APPDATA%\Notepad++`，粘贴`userDefineLang.xml`到这个目录。
然后重启一下notepad++就可以看到效果了。

这个是我的notepad++。

![notepad++的markdown的高亮](http://i1303.photobucket.com/albums/ag154/chaopeng/blog/markdown-highlight_zps101099fc.png)

markdown导出html
----
其实markdown导出html，在notepad++是很容易实现的，只需要吧你调用markdown引擎的命令复制到`Run-运行`就可以了。
当然这里说的不会那么简单啦。
我喜欢用python-markdown引擎，大家如果看过我的[chaosblog] (https://github.com/chaopeng/chaosblog) 的话应该都知道。
但是python-markdown只会生成内容，不会带有完整html格式，当然你也可以用插件来生成一个html，但是这样就没有办法去修改markdown的css。
同时也失去了markdown只控制内容而格式无关的特性了。
所以我做了一个[简单的生成html的程序](https://github.com/chaopeng/chaos-markdown) ，
当然这个也是大量重用了chaosblog的代码，所以markdown引擎也是可以更换的，同时你也可以更换css。

如果你想使用我的这个markdown-html生成器，你可以下载源代码编译，也可以使用我已经打包好的。把bin目录的文件复制到你喜欢的位置。
然后你可以使用命令行调用：
```{bat}
java -jar -Dfile.encoding=utf-8 你存放的目录\markdown.jar 输入文件的路径 输出文件的路径 
```

你也可以在notepad++的`Run-运行`保存：
```{npp-cmd}
cmd /c java -jar -Dfile.encoding=utf-8 你存放的目录\markdown.jar "$(FULL_CURRENT_PATH)" "$(CURRENT_DIRECTORY)\$(NAME_PART).html
```
这样你就可以在以后使用notepad++的时候达到一键markdown导出html了。
