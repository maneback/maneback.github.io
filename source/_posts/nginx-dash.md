---
title: 使用Nginx搭建DASH服务
date: 2019-03-28 17:49:05
tags:
  - Nginx
  - Linux
categories: 
  knowledge
description: 教你如何借用nginx服务器搭建dash服务
---

# 写在前面 

最近在做毕设的过程中，需要自己搭建一个dash服务来进行实验，所以把搭建过程以及自己在此过程中踩到的坑记录下来。

在网上搜索到的教程都很迷，仿佛都假设你对这个东西有所了解。（话说有所了解还用看你的教程干嘛）所以本文章写得尽量对0基础的读者尽量友好一点。说是尽量因为实在是水平有限，自己也不深入搞这些，只需要了解皮毛能够完成实验就行了。

# 环境

- ubuntu 16.04
- Nginx 1.10.3
- others：
  - ffmpeg
  - Bento4（主要用到mp4fragment 和mp4dash)
- VLC 视频播放器



## 安装环境

## 1、 虚拟机安装ubuntu

在VMWare上安装ubuntu虚拟机，此部分略过。注意如果想要在其他主机如宿主机上面查看网页，需要配制一块桥接网卡

## 2、安装Nginx

可以参考我的另一篇文章。【TODO】

## 3、安装ffmpeg和Bento4

ffmpeg网上找到有人说需要用源码安装，但是好像很麻烦，之后我又找到了用命令行安装的方法。

```shell
sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
sudo apt-get update
sudo apt-get install ffmpeg
```

Bento4直接去官网下载可执行文件即可：[Bento4下载](<https://www.bento4.com/downloads/>)

下载之后，将压缩包解压，可以放到任意位置，并将`/Bento/bin`添加到环境变量。Bento里面包含好多工具，在这里我们主要用到`mp4fragment` 和`mp4dash`。

直到把这些工具安装完成并配置好，我们才可以处理我们的视频，并通过nginx推送出去。

#  使用ffmpeg获得不同分辨率的视频

DASH的精髓就是根据网络的状况选择不同的bitrate和resolution的视频，使得在网络不稳定的情况下，尽可能为用户提供最优的服务和最佳质量的视频，提升用户的观看体验。

首先，我们要在这里下载视频 [BigBuckBunny](<https://download.blender.org/peach/bigbuckbunny_movies/>),随便选择一个下载就好，后面我们把视频假设重命名成`video.mp4`。

输入以下指令：

```shell
ffmpeg -i video.mp4 -s 320x180 -c:v libx264 -b:v 500k -g 90 -an video_320x180_500k.mp4
```

各参数含义如下：

```shell
-i # 选择输入文件
-s #选择分辨率
-c;v #选择codec编码器
-b:v # 选择码率
-an # audio no 不要编码audio
-g  # 图像组大小（GOP length）
```

这样我们就得到了分辨率为320x180，码率为500k的一段视频，为了得到更多不同质量的视频，我们只需要将参数修改一下，再次执行以上命令即可。

```shell
ffmpeg -i video.mp4 -s 320x180 -c:v libx264 -b:v 250k -g 90 -an video_320x180_250k.mp4
ffmpeg -i video.mp4 -s 320x180 -c:v libx264 -b:v 750k -g 90 -an video_320x180_750k.mp4
```



可以发现，我们上面的命令是没有处理音频的，因此也还要对音频进行编码

```shell
ffmpeg -i video.mp4 -c:a aac -b:a 500k -vn video_audio_500k.mp4
```

以上命令可能会报错：

```shell
The encoder 'aac' is experimental but experimental codecs are not enabled, add '-strict -2' if you want to use it.
```

按照提示加上` -strict -2 ` 参数即可：

```shell
ffmpeg -i video.mp4 -c:a aac -b:a 500k -vn -strict -2 video_audio_500k.mp4
```

#  生成DASH切片以及mpd文件

在生成切片时我选择的是Bento4工具集中的mp4fragment和mp4dash。

还有另外一个工具mp4box，但是生成的dash文件推送到服务器之后请求会出错，某作者用了mp4box之后因为出错还是转向了Bento4工具集。因此我为了稳（偷）妥（懒），就不再去踩坑mp4box了。两个文件都在

```
/Bento/bin
```

文件夹当中，把Bento安装到任意位置后，将该位置加到环境变量中，就可以更加方便使用了。

mdp文件描述了mpeg dash码流的构成，其实是一个XML文档，通过mpd可以构造出用于HTTP GET下载视频的URL。

处理流程主要是两步，首先使用`mp4fragment`生成视频的`fragment` （我也不知道该怎么理解）再用`mp4box` 来进行下一步的处理就行了。

当然你忘记了`mp4fragment` 命令也没事，要是直接运行`mp4box`的话，它会报错提醒你

```shell
file 1 is not fragmented (use mp4fragment to fragment it)
```

反正就记住有两步，先用`mp4fragment` 再用`mp4dash`。

```shell
mp4fragment video_320x180_250k.mp4 video_320x180_250k_f.mp4
mp4fragment video_320x180_500k.mp4 video_320x180_500k_f.mp4
mp4fragment video_320x180_750k.mp4 video_320x180_750k_f.mp4
 # 当然还有音频的fragment
mp4fragment video_audio_500k.mp4 video_audio500k.f.mp4
```

终于来到了最后一步，进行切片，生成mpd文件以及视频片段:

```shell
mp4dash video_320x180_250k_f.mp4 video_320x180_500k_f.mp4 video_320x180_750k_f.mp4 video_audio500k.f.mp4
```

最后它会生成一个`output` 文件夹，里面包含了一个mpd文件，和一个包含视频片段的文件夹。

当然你也可以指定输出路径，只要加上`--output-dir outputfile` 参数，就可以输出到outputfile文件夹下面了。

# 将视频上传服务器

你只需要将`output` 里面的内容上传至nginx的`/var/www/html/` 路径下，就可以通过VLC播放器请求视频数据了。在打开网络流中输入 `http://ip:80/stream.mpd` 就可以播放视频了。

这时，你在命令行中输入：

```shell
tail -f /var/log/nginx/access.log
```

就可以实时查看播放器正在请求那个bitrete的数据了。



# 踩坑

在我最初的时候，我生成了一个bitrate的文件，测试能否请求。但是当我生成两个不同bitrate文件的fragment，再使用mp4dash命令时候，它会报错：

``` 
MP4dash video tracks are not "aligned" 
```

但是当我写这篇文章的时候，报错又不能复现了，所以我感觉是我当时敲命令时候拉下了什么。

这个错误字面理解就是文件没有对齐，好像跟视频的GOP有关。我的理解就是，比如说，file1 的第一个fragment是0-10s， file2的第一个fragment是0-12s这样的话，每个fragment不是对应相同的视频片段，就会导致出错，反正具体我也不懂。在网上查解决方案时发现这个遇到这个错误的人还不少，而且好像都没有解决方法，连github上面的issue都没有人给出解答。当时都打算放弃Bento改用mp4box试一试了。结果我又看了一遍[`ffmpeg`][1] 和[`Bento`][2]的官网，试了一下设置更多地参数，结果完美解决了问题。

解决方法如下：

- 首先在`ffmpeg` 中限制它的`keyint`。将最大和最小都设置为相同的值，也就成了定值。

```shell
ffmpeg -y -i origin.mp4 -s 160x90 -ac 2 -ab 128k -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 500k -g 90 -an video_160x90_500k.mp4
```

- 在`mp4fragment`中设置强制对齐

```shell
mp4fragment --fragment-duration 2000 --force-i-frame-sync all video_320x180_500k.mp4 video_320x180_500k_f.mp4
```

这样在mp4dash中就没有问题了。

# 总结

DASH是一种流媒体服务协议，本来我的研究方向与这个是没什么关系的，但是本科毕设选了一个比较坑的题目，还要自己搭建DASH服务器环境，才不得不去了解它。

由于在之前都没有了解过这个东西，在网上找的教程，也都没有开篇是给除初学者的教程。方法那些人都默认你了解过这个东西，甚至是用过这个东西，他们并没有把一些潜在的知识给表达出来。虽然当你了解了并且亲手实践过之后，你会觉得没什么，都很简单，那些没有讲明的知识我也自己摸索出来了。可是在这之前，你可能会很不知所措，就像我当初一样。

因为他们并没有把一些很浅显，但是不去实践就无法了解的结果给写出来。所以我当时进行得就很艰难，而且很抗拒，所以一方面我不希望别人再经历像我一样的事情，另一方面，我还是想说，多动手实践，只要这个教程写得还可以，就动手试试。你不知道作者写这篇博客的时候是什么心态，也不知道他是面向谁来写的。你只能去试，去犯错，去解决问题。这样你才能作者隐藏在博客背后没有表达的内容。



# 参考链接

- [Making Your Own Simple MPEG-DASH Server](<https://www.instructables.com/id/Making-Your-Own-Simple-DASH-MPEG-Server-Windows-10/>)
- [MPEG DASH-Bento4](<https://www.bento4.com/developers/dash/>)
- [Nginx 搭建 DASH 服务器](<https://blog.csdn.net/OCTODOG/article/details/79007302>)
- [HOW TO ENCODE MULTI-BITRATE VIDEOS IN MPEG-DASH FOR MSE BASED MEDIA PLAYERS (1/2)](<https://blog.streamroot.io/encode-multi-bitrate-videos-mpeg-dash-mse-based-media-players/>)
- [GPAC DASH](<https://gpac.wp.imt.fr/tag/dash/>)

[1]: https://ffmpeg.org/ffmpeg.html#Complex-filtergraphs-1	"ffmpeg"
[2]: https://www.bento4.com/developers/dash/

