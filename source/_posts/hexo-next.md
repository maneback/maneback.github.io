---
title: Next主题个性化--hexo
date: 2019-03-02 13:54:05
tags: hexo
categories: Blog
description: 给你的网站添加一些功能。
---

人类的本质就是真香，之前非常地不愿意使用 Next 主题，不过最终现在还是跟随了大潮，换上了这一件热门主题。在网上看到的关于next主题的个性化配置的教程也挺多的，功能也十分强大。所以我选择了这个主题是因为功能真的很强大，很多实用的功能只要修改配置文件就可以使用了，已经有了很多可以选择的现成的工具。

## 1. 评论功能

通过查看next的配置文件，我们发现next支持的可用评论平台有很多，但是我选择的是[Valine](<https://valine.js.org/>)，看了一下，其他的好多都需要注册才能评论，但是我不想要这种，因为需要注册的话，就显得过于繁琐了，我只是发表一下观点，要我注册干啥。所以找了一个不需要注册即可评论的评论系统。既可以匿名，又可以在评论的时候设置昵称等信息。

valine是一款基于leancloud的快速、简洁、高效地无后台的评论系统。

### 配置过程

- 首先，注册[leancloud](<https://leancloud.cn/dashboard/applist.html#/apps>) 获取APP ID 和APP KEY，应用名称自己起一个，选择免费的开发版。

  ![1553782179250](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1553782179250.png)

- 创建好后，进入应用，选择设置->应用key，复制App ID 和App Key。

- 在Next主题的配置文件中搜索valine，将enable 设置为true，将App ID 和App Key复制到相应位置。同时还可以开启其他功能，如邮件通知，评论计数等，按需操作

  ```yml
  valine:
    enable: true # When enable is set to be true, leancloud_visitors is recommended to be closed for the re-initialization problem within different leancloud adk version.
    appid: YOUR-APPID
    appkey: YOUR-APPKEY
    notify: false # mail notifier, See: https://github.com/xCss/Valine/wiki
    verify: false # Verification code
    placeholder: what do you want to say # comment box placeholder
    avatar: mm # gravatar style
    guest_info: nick,mail,link # custom comment header
    pageSize: 10 # pagination size
    visitor: true # leancloud-counter-security is not supported for now. When visitor is set to be true, appid and appkey are recommended to be the same as leancloud_visitors' for counter compatibility. Article reading statistic https://valine.js.org/visitor.html
    comment_count: true # if false, comment count will only be displayed in post page, not in home page
  
  ```

- 然后，在leancloud设置-->安全中心，将你的博客的网址填写到Web安全域名中就可以了。

我不知道为什么，这个评论系统并不能在本地预览，但是部署到github.io 之后就能正常显示了。

- 这样在leancloud的后台，你就可以查看留言了。选择存储-->Comment 查看留言信息，还可以删除留言。

### 配置项

可以看到在Next配置文件中，Valine有很多配置项可用。下面分别讲解一下。



PS：我才发现，原来这个评论系统还可以支持MarkDown语法耶，真的是出乎我的意料。

## 2. 阅读量统计

## 3. 友情链接