---
layout: post
title: Git 常用资源
category: 资源
tags: git
keywords: git
description:
---

Git简介

- git branch 查看所有分支
- git branch -a 查看远程分支

- Git rm – 如何使文件脱离git的版本管理，但不是会删除它.  
>Git rm命令将允许你取消git对文件的版本控制. 这个 –cached选项允许你把文件保留在你的硬盘上.  
每隔一段时间都会有一些不应该被git管理的文件被误加入到git中. 常见的例子是配置文件, 由包含你的个人设置的IDE生成的工程文件，甚至有人决定要签入的临时文件. 这些文件是必要的, 所以往往不能完全删除它们。然而将它们复制到其他地方, 或者从git中删除然后替换它们，这一过程是非常痛苦的, 更别提容易出现的错误.  
通过添加 –git rm 命令的缓存的选项, 你是能到远程文件文件从 git 控制同时保持工作树中的文件. 他们命令的语法是:  
`git rm --cached file`  
Git 将不再跟踪此文件，尽管它仍然是在您的硬盘上.  
运行上述命令后, 一定要添加一个条目到您 .gitignore 文件以便 ' 文件’ 没有显示在 ' git 状态’ 和，它不小心以后将无法重新添加.

- 远程仓库地址变更  
    1.删除后添加
    ```
    git remote rm origin
    git remote add origin [url]
    ```
    2.修改命令  
    ```
    git remote origin set-url [url]
    ```

- http 访问git 记住密码输入  
```
git config --global credential.helper store
```

- 拉取远程分支并创建本地分支
```
git checkout -b 本地分支名 origin/远程分支名
```






参考资源
----

[http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
