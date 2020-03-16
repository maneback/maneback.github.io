---
title: 一台电脑设置多个github账户
date: 2020-02-10 11:23:55
tags:
  - git
  - github
categories: 技术
description: 在一个电脑上如何通过ssh连接两个远程github账号
---

## 0. 前言

![](https://raw.githubusercontent.com/maneback/TyporaPic/master/img/1572962464812.jpeg)

不知是否有人像我一样，搞了两个GitHub账号，一个公开使用，另一个用来自己留着偷偷搞事情，要做到两个本地账户和远程账号都完全隔离，没有联系，也不会混淆使用

反正我是这么搞的，自从之前那个GitHub账号被公布出去之后，就自己搞了个新的没人知道的账号来搞一些见不得人的事情。（其实只是喜欢保密一些事情。）

但是，一台电脑怎么假装出两个GitHub账户呢？

当初的我以为只要把同一个SSH公钥分别添加到两个GitHub账户就万事大吉了，后来发现我太天真了，并不行。

然后就到处找教程，但是都是错误的，而且错误都是一样的，最后终于七拼八凑整成功了。

----

## 1. 方法

首先，当然是生成两个ssh秘钥，对应两个身份。

假设你当初已经用`ssh-keygen -t rsa -C “youremail@gmail.com”`生成了一对秘钥`id_rsa`和`id_rda`， 保存在了`~/.ssh`文件夹内。

然后，你需要再生成一对个人秘钥：

```
ssh-keygen -t rsa -C “private_email@gmail.com”
```

这时，在执行命令的过程中，要在注意修改文件的命名，因为默认仍然是使用`id_rsa`的文件名，这样就会把之前的秘钥文件覆盖掉。

假设我们生成了一对新的秘钥`private` 和`private.pub`。

然后我们编辑`~/.ssh/config`文件。如果该文件不存在的话，直接创建一个就好。里面的内容如下：

```
# 公共
Host github_public
Hostname ssh.github.com
IdentityFile ~/.ssh/id_rsa
port 22

#个人
Host github_private
Hostname ssh.github.com
IdentityFile ~/.ssh/private
port 22
```

> 网上大部分教程的配置文件中`Hostname`都是`github.com`，配置成这样是不能正常SSH访问的。我也不知道大家为什么都那样写，难道之前的就是这样？

修改完之后，在`git bash `运行下列命令，检查是否正常：

```shell
ssh -T git@github_public
ssh -T git@github_private
```

如果都能返回以下信息，就说明配置正常

```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

同样的方式你就可以配置多个以SSH登录的不同git用户。

当然这种方法也不仅仅限于github的远程仓库。

然后我们还要删除GIT配置的全局用户名和邮件地址，然后在每个项目仓库中按照需求设置不同的`user.name`和 `user.email`：

```shell
git config --global --unset user.name
git config --global --unset user.email
```

然后在添加远程仓库的时候，把`github.com` 修改成`github_public` 或者`github_private`就好了（即上述文件中的HOST选项），如：

```shell
git remote add origin git@github_public:xxx/example.git # 公开内容
git remote add origin git@github_private:xxx/example.git # 个人内容
```

其实，上面的配置信息意思就是，按照不同的Host名称，查找`~/.ssh/config`文件使用不同的SSH文件连接到GitHub。