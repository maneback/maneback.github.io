---
title: git
date: 2018-11-02 21:57:03
tags:
- git
categories: knowledge
---

记录一些常用的Git命令

<!--more-->
- git 设置用户名和邮箱
```shell
git config --global user.name "yourname"
git config --global user.email "youremail"
```

- git 查看用户名和邮箱
```shell
git config user.name
git config user.email
```

- git添加远程仓库
```Shell
git remote add origin git@github.com:xiaosonggao/xiaosogngao.github.io.git
git push -u origin master
```

- 删除远程分支
```shell
git push origin :hexo #: 表示删除分支
```
- commit 报错：
`fatal: LF would be replacesd by CRLF in <some file>`
**解决办法：**
```Shell
git config --global core.autocrlf false
git config --global core.safecrlf false
```
