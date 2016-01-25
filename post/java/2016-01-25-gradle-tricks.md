{
layout: "pages",
title: "Gradle小技巧",
category: "java",
description: "记录一些Gradle使用小技巧",
tags: ["java","gradle"]
}

- [gradle-versions-plugin](https://github.com/ben-manes/gradle-versions-plugin)可以很方便检查依赖库的版本更新情况，至于更不更新是你的问题了。定期检查一下总是没有错的。
- [gradle-git](https://github.com/ajoberstar/gradle-git)可以在gradle中调用git，比如release时候自动打tag，build的时候将commit-id打进去，build的时候警告git还没有commit。
- idea插件: idea.module.excludeDirs可以设置不包含的文件夹
- apply from 可以包含其他gradle文件
- settings.gradle 自动包含子项目（子项目需要有build.gradle文件）:

        def path = [] as LinkedList
        rootDir.traverse(
            type: groovy.io.FileType.FILES,
            nameFilter: ~/.+\.gradle/,
            maxDepth: 3,
            preDir: { path << it.name },
            postDir: { path.removeLast() }) { if (path) include path.join(":") }

- build时忽略某些文件:
    
        sourceSets {
          main {
            java {
              exclude 'file...'
            }
          }
        }
