---
title: Music
date: 2020-03-03 21:22:16  
categories: Amazing
description: 在 hexo 博客中插入音乐
---



终于找到了在博客中插入音乐的方法了。之前在网易云生成外链的方法不知道怎么就失效了呢。

新的方法使用了 [hexo-tag-aplayer](https://github.com/MoePlayer/hexo-tag-aplayer) 和 [MetingJS](https://github.com/metowolf/MetingJS) 。

先来一首最近非常非常喜欢的一个翻唱歌手唱的一首歌：

{% meting "567935536"  "netease"  "song" %}

#### 1. 安装

```
npm install  --save hexo-tag-aplayer
```

#### 2.使用

aplayer可以引用本地的音乐文件，但是有时候显然会比较麻烦

```html
{% aplayer title author url [picture_url, narrow, autoplay, width:xxx, lrc:xxx] %}
```

- `title` : 曲目标题
- `author`: 曲目作者
- `url`: 音乐文件 URL 地址
- `picture_url`: (可选) 音乐对应的图片地址
- `narrow`: （可选）播放器袖珍风格
- `autoplay`: (可选) 自动播放，移动端浏览器暂时不支持此功能
- `width:xxx`: (可选) 播放器宽度 (默认: 100%)
- `lrc:xxx`: （可选）歌词文件 URL 地址

需要开启hexo的文章资源文件夹功能，将图片、音乐文件、歌词文件都放在与文章相对应的资源文件夹中，然后直接引用

#### 3. MetingJS

现在hexo的配置文件`_config.yml`中设置：

```yaml
aplayer:
  meting:true
```

接着就可以用`\{% meting %}`在文章中使用播放器了。

```html
<!-- 简单示例 (id, server, type)  -->
{% meting "60198" "netease" "playlist" "mini:true" %}
```

| 选项          | 默认值     | 描述                                                        |
| ------------- | ---------- | ----------------------------------------------------------- |
| id            | **必须值** | 歌曲 id / 播放列表 id / 相册 id / 搜索关键字                |
| server        | **必须值** | 音乐平台: `netease`, `tencent`, `kugou`, `xiami`, `baidu`   |
| type          | **必须值** | `song`, `playlist`, `album`, `search`, `artist`             |
| fixed         | `false`    | 开启固定模式                                                |
| mini          | `false`    | 开启迷你模式                                                |
| loop          | `all`      | 列表循环模式：`all`, `one`,`none`                           |
| order         | `list`     | 列表播放模式： `list`, `random`                             |
| volume        | 0.7        | 播放器音量                                                  |
| lrctype       | 0          | 歌词格式类型                                                |
| listfolded    | `false`    | 指定音乐播放列表是否折叠                                    |
| storagename   | `metingjs` | LocalStorage 中存储播放器设定的键名                         |
| autoplay      | `true`     | 自动播放，移动端浏览器暂时不支持此功能                      |
| mutex         | `true`     | 该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停 |
| listmaxheight | `340px`    | 播放列表的最大长度                                          |
| preload       | `auto`     | 音乐文件预载入模式，可选项： `none`, `metadata`, `auto`     |
| theme         | `#ad7a86`  | 播放器风格色彩设置                                          |