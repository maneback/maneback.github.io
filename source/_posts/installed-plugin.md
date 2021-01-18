---
title: Hexo 必要的插件
date: 2021-01-05 17:53:18
tags: 
  - blog
  - hexo
categories: Blog
description: 配置环境要用到的插件。
---

#### 基础环境及必要插件

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





####  在hexo 博客中插入图片。

先安装插件 `hexo-asset-image`


```shell
npm install -g hexo-cli --save
npm install https://github.com/CodeFalling/hexo-asset-image --save
```



第一种最简单的方法是使用图床，但是之前折腾过一段时间使用 PicGO +github 图床，但是稳定，后放弃。改用 hexo 自带的资源文件夹。

第二种方法是启用 assert 资源文件夹，但是这个东西很不智能。当你在 markdown 文档引用图片（不管是相对路径还是绝对路径）都能在文档中显示，但是渲染成 HTML 文件时，就会路径出错了。要用这个新的插件  `hexo-asset-image` 。

#### 在 hexo 博客中正常显示 latex 公式

hexo 竟然对 latex 公式的支持这么不智能。唉，绝了。

更换 renderer。

```
npm uninstall hexo-renderer-marked
npm install hexo-renderer-pandoc
```

安装完 `hexo-render-pandoc` ，它在运行 `hexo s` 命令时会报错：

```shell
[ERROR][hexo-renderer-pandoc] pandoc exited with code 9: pandoc: Unknown extension: smart
```

这时需要把 `node_modules\hexo-renderer-pandoc\index.js` 文件中的这个东西

```javascript
var args = [ '-f', 'markdown-smart'+extensions, '-t', 'html-smart', math]
```

改成：

```javascript
var args = [ '-f', 'markdown'+extensions, '-t', 'html', math]
```

参考来源  [hexo-renderer-pandoc issues36](https://github.com/wzpan/hexo-renderer-pandoc/issues/36#issuecomment-555134526)