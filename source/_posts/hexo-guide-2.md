---
title: GitHub博客搭建(二) Hexo搭建与使用
date: 2018-03-29 22:28:12
tags:
 - blog
 - hexo
 - github
categories: Blog
---

**PS:** 在我学习安装过程中，遇到了两条不同的思路：

（一）首先建立仓库，然后clone到本地，再在里面创建hexo博客。

（二）先在本地创建hexo博客，然后将它与GitHub上面的仓库连接起来。

<!--more-->

# 1.初始化配置

我们假设要将博客放在/GitHub 目录下，

在Git Shell 下，切换到/GitHub路径，执行以下指令，初始化本地博客：

```shell
hexo init <folder> #folder 中将创建需要的文件
cd <folder>
npm install
```

假设所执行的命令为 `hexo init hexo`  ， 生成的文件夹为：`GitHub/hexo`

执行之后，就在本地创建好了hexo博客

### 安装Hexo插件
```shell
npm install hexo-generator-index --save
npm install hexo-generator-archive --save  ##wenti
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save ##
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```

# 2. Hexo主要命令

- 创建新的博客

```shell
hexo new [layout] <title>
# 等价于：
hexo n [layout] <title>
```

layout不同则产生的博客类型和默认路径也不同


| layout | path |userage|
| :----: | :--: | :--: |
| post | source/_post |一篇博客|
| page | source |一个页面|
| draft | source/_draft |草稿|

默认类型为`post` ， 此命令会在对应的文件夹生成title.md文件，然后就可以编辑此文件，用下面的命令生成静态页面。

- 生成静态页面

```shell
hexo generate
#等价于
hexo g
```

执行此命令后，就会产生`public/` 文件夹，所有生成的内容都会保存在这里。

- 在本地查看运行结果

执行以下命令，然后在浏览器打开`localhost:4000` 查看效果。

同时安装插件之后，还可以不用关闭服务，就可以随时查看修改下效果。

```shell
hexo server
#等价于
hexo s
```

|    选项     |              描述              |
| :---------: | :----------------------------: |
| -p , -port  |            重设端口            |
| -s, -static |         只使用静态文件         |
|  -l, -log   | 启动日记记录，使用覆盖格式记录 |

- 发布

```shell
hexo deploy
#等价于
hexo d
```

- 清除内容

```
hexo clean
```

用于清除缓存文件和已经生成的静态文件(`public/`)，如果发现对站点的更改没有生效，可能需要执行该命令。

##### 继续阅读：

##### [Hexo配置与发布](../hexo-guide-3/index.html)

---

# 参考

- [指令|Hexo](https://hexo.io/zh-cn/docs/commands.html)
- [使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
