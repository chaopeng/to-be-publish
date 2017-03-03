{
layout: "pages",
title: "翻译：用vscode开发Chromium",
category: "Chromium",
description: "用vscode开发Chromium，插件与配置",
tags: ["chrome","chromium"]
}

[TOC]

这篇文档是我写的，原文位置在
[这里](https://chromium.googlesource.com/chromium/src/+/master/docs/vscode.md)。
部分内容可能跟官方文档不同。

[Visual Studio Code](http://code.visualstudio.com/)
([Wikipedia](https://en.wikipedia.org/wiki/Visual_Studio_Code)) 是微软基于
Electron（一个基于Chromium的GUI开发框架）开发的开源跨平台文本编辑器。Visual Studio Code
社区现在比较活跃，而且也有很多优秀的插件。而且使用vscode开发Chromium并不需要太多的配置，而且
对电脑性能要求也不高。

## 安装插件

`ctrl+p` 输入 `ext install cpptools you-complete-me clang-format`。 
更多插件: https://marketplace.visualstudio.com/search?target=vscode

建议你安装你习惯的快捷键，比如安装Eclipse的快捷键：
`ext install vscode-eclipse-keybindings`。
更多的快捷键插件：https://marketplace.visualstudio.com/search?target=vscode&category=Keymaps

## 配置

打开配置 `File/Code - Preferences - Settings`，粘贴下面的：

```json
{
  "editor.tabSize": 2,
  "editor.rulers": [80],
  // Exclude
  "files.exclude": {
    "**/.git": true,
    "**/.svn": true,
    "**/.hg": true,
    "**/.DS_Store": true,
    "**/out": true
  },
  // YCM
  "ycmd.path": "<your_ycmd_path>",
  "ycmd.global_extra_config": 
      "<your_chromium_path>/src/tools/vim/chromium.ycm_extra_conf.py",
  "ycmd.confirm_extra_conf": false,
  "ycmd.use_imprecise_get_type": true,
  // clang-format
  "clang-format.style": "Chromium",
  "editor.formatOnSave": true
}
```

### 安装自动补全引擎(ycmd)

```
$ git clone https://github.com/Valloric/ycmd.git ~/.ycmd
$ cd ~/.ycmd
$ ./build.py --clang-completer
```

## 工作流程

1. `ctrl+p` 打开文件
2. `ctrl+o` 跳转到symbol，`ctrl+l`跳转到行
3. <code>ctrl+`</code> 打开terminal

## Tips

### 笔记本上使用

因为我们使用ycmd来做自动补全和简单的代码跳转，所以可以关闭掉CPP插件的自动补全和index的。
不关的话会很耗电，而且插件并不能帮你找到什么。

```
"C_Cpp.autocomplete": "Disabled",
"C_Cpp.addWorkspaceRootToIncludePath": false
```

### 启动Sublime-like代码地图

```
"editor.minimap.enabled": true,
"editor.minimap.renderCharacters": false
```

### 更多

https://github.com/Microsoft/vscode-tips-and-tricks/blob/master/README.md