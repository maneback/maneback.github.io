---
title: GitHub博客搭建(三) Hexo配置与发布
date: 2018-03-31 11:31:02
tags:
  - blog
  - hexo
  - github
categories:
  - Blog
description: 配置Hexo，Hexo文件夹介绍
---

# 一、Hexo文件夹

在`GitHub/hexo` 路径下安装完本地博客后，文件夹中的内容如下：

```
|-- _config.yml
|-- package.json
|-- scaffolds
|-- scripts
|-- source
    |-- _drafts
    |-- _posts
|-- themes
```

### 网站配置文件

`_config.yml` 是博客的配置文件，主要修改的部分有：

1.  **site部分**

```yml
#site
title: Hexo
subtitle:
description:
keywords:
author: John Doe
language:
timezone:
```

`title` 与`subtitle` 是网站的标题和副标题，这个是肯定要修改的。其他的`language` 和`timezone` 是语言和时区，按需更改，保持默认也可以。`language` 可以设置成`zh-CN` 。

2.  **在URL部分，有以下一项设置：**

```yml
parmalink: /:year/:month/:day/:title/
```

可以修改为：

```yml
parmalink: /:year:month:day/:title/
```

这样，就可以避免生成的文件产生多层嵌套，或者也可以以月为单位来组织文件夹。

3. **Deployment 部分**

这部分是用于发布的，只若发布到GitHub上面，则按照以下的格式设置即可

```
deploy:
  type: git
  repo: #<name.github.io>仓库的ssh地址
  branch: master
```

这样，就可以直接使用`hexo d` 命令发布到GitHub上面了。

4. theme部分
5. theme是设置网站的主题，Hexo有很多精美的主题，可以在[Themes|Hexo](https://hexo.io/themes/) 上面查看。首先要将你想要使用的Theme下载下来，然后将这个地方改成那个主题的名字即可。

**！注意：** 在每个设置项的冒号后面都**有一个空格**， 若忘记加空格则会在后面的过程中报错。截止目前为止，我所遇到的报错都是因为空格问题。(PS: 回头来看，原来是因为YAML文件的语法要求空格)

### Theme

在`theme/` 下面存放的是主题，可以在[Themes|Hexo](https://hexo.io/themes/) 上面查看，选择自己喜欢的主题，然后用git下载下来。需要注意的是一定要保持配置文件中theme选项的名字要与theme中主题文件夹的命名保持一致。

我所使用的主题是[Manpassant](https://github.com/tufu9441/maupassant-hexo.git)，以它为例，在Git Bash中切换到`GitHub/Hexo/` 路径下，输入以下命令

```shell
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant

##使用此主题需要的插件
npm install hexo-renderer-pug --save
npm install hexo-renderer-sass --save
```

有时候，有些主题还需要下载其他的插件，按照GitHub网页上面的提示安装就好。

安装成功之后，再把配置文件里面的主题项修改了就可以了。

好多人都推荐的主题：[NexT](https://github.com/iissnan/hexo-theme-next)， 可是我没有用这个2333。自己查资料解决就好。
**PS:真香警告：** 我还是换回了最多人使用人见人爱的[NEXT](https://github.com/theme-next/hexo-theme-next)主题。


### source/

此文件夹里面保存的是创建的博客的原始Markdown文件，创建之后在这里编辑修改就好。或者也可以在这里直接新建.md文件。不同类型的文件保存在不同的文件夹中。

### public/

当执行过`hexo g` 命令后，就会新生成一个`publib/` 文件夹，这里保存的是由Markdown文件生成的静态页面，分为不同的文件夹和页面。

# 二、发布

在`public/` 文件夹中建立git仓库，然后与远端的`<yourname>.github.io.git`仓库连到一起，就可以同访问`<yourname>.githubio` 成功访问网站了。这样，每次更新博客之后，都可以推送到远端来查看了。



##### 继续阅读：

[Hexo写作](https://hexo.io/zh-cn/docs/writing.html)

---

# 参考

- [Themes|Hexo](https://hexo.io/themes/)
