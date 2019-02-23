---
title: GitHub博客搭建(一) Hexo介绍
date: 2018-03-30 22:28:12
tags:
 -blog
 -hexo
 -github
categories: Blog
---

### 0. 废话

作为一个计算机专业的学生，最近突然萌生了自己搭建博客网站的想法，遂开始到处查找资料，最终利用GitHub Page和Hexo，历时两天，经历无数次摸索和失败之后，终于成功了。于是将自己的学习过程整理了出来。

# 1. 什么是Hexo？

Hexo是一个快速、简洁且高效的博客框架，可以用Markdown来解析文章，利用精美的主题来生成静态网页。也就是说，你可以用Markdown来写作，然后由它来帮你生成网页。

在我看来，由它生成的博客页面要比其他大多数现有的博客网站要精美得多，而且再搭配[GitHub Page](https://pages.github.com/) ，非常适合用来建个人博客（当然，也可以放到自己的域名的网站上），而且这过程中也锻炼了对Git和GitHub的理解。



# 2. 准备工作

- 安装Git

作为一个想要自建博客的人，理应知道Git和[GitHub](https://github.com)是什么。

下载Git并安装，并注册GitHub账号。(详细过程略过，不在我们的讨论范围内。)

- 安装Node.js

[Node.JS](http://nodejs.org/)

**注意：** 新版已经会自动npm配置Path环境变量，否则需要自己配置。

```
C:\Program Files\nodejs\node_modules\npm
```

将其加入到Path 环境变量中，路径是在你电脑中的安装路径。

- 创建GitHub仓库：

首先申请一个GitHub账号，然后新建一个仓库，名称格式为：

`<yourname>.github.io`

必须符合命名格式，才能生成页面，当一切都配置成功之后，就可以通过以下地址访问自己的博客

`https://<yourname>.github.io`

  ​

# 3. 安装Hexo

首先，创建一个目录，如我将它起名为`GitHub` ，然后在`Git Shell` 中切换到该目录下，输入以下命令；

```shell
npm install hexo-cli -g
npm install hexo --save
```

如果命令无法继续运行，可以切换npm源：

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

命令运行结束，Hexo就安装成功了。

##### 继续阅读：

##### [Hexo搭建和使用](../hexo-guide-2/index.html)

# 延伸

##### Git 教程

[Git教程-廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

[Git documentation](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

##### Markdown教程

[献给写作者的 Markdown 新手指南 - 简书](https://www.jianshu.com/p/q81RER)

[我的Markdown教程](.)

##### 我使用的markdown编辑器：**typora** ，** Atom

下载链接：
https://typora.io/
https://atom.io/

---

# 参考

- [Hexo文档](https://hexo.io/zh-cn/docs/index.html)

- [使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
