---
title: 在github提交中插入emoji表情
date: 2019-06-07 21:02:24
tags: 
  - git
  - github
  - emoji
categories: 
  - Amazing
description: 给你的github添加一些emoji表情
---

### 给你的github提交中添加一些emoji表情

`git commit`只有文字描述太无趣了，考虑来点有趣的，在github提交中加入一些emoji表情。💢

后来才知道，这个玩意，早就有人做出了规范说明，什么情况下，哪个表情用于什么用途，都是有规定的。方法很简单，遵循以下格式

`:emoji1: :emoji2: information`

额外要求：首字母大写，祈使语气，句末不要加句号

emoji使用指南

| emoji            | emoji代码                    | 使用说明              |
| ---------------- | ---------------------------- | --------------------- |
| 🎉 庆祝           | `:tada:`                     | 初次提交              |
| ✨火花            | `:sparkles:`                 | 引入新功能            |
| 🔖书签            | `:bookmark:`                 | 发行/版本标签         |
| 🐛bug             | `:bug:`                      | 修复bug               |
| 🚑急救车          | `:ambulance:`                | 重要补丁              |
| 🌐地球            | `:globe_with_meridians:`     | 国际化和本地化        |
| 💄口红            | `:lipstick:`                 | 更新UI和样式文件      |
| 🚨警灯            | `:rotating_light:`           | 移除linter警告        |
| 🔧扳手            | `:wrench:`                   | 修改配置文件          |
| ➕加号            | `:heavy_plus_sign:`          | 增加一个依赖          |
| ➖减号            | `:heavy_minus_sign:`         | 减少一个依赖          |
| ⬆上升箭头        | `:arrow_up:`                 | 升级依赖              |
| ⬇下降箭头        | `:arrow_down:`               | 降级依赖              |
| ⚡闪电<br />🐎赛马 | `:zap:`<br />`:racehorses:`  | 性能提升              |
| 📈上升趋势图      | `:chart_with_upwards_trend:` | 添加分析或跟踪代码    |
| 🚀火箭            | `:rocket:`                   | 部署功能              |
| 📝备忘录          | `:memo:`                     | 撰写文档              |
| ✅白色复选框      | `:white_check_mark:`         | 增加测试              |
| 🔨锤子            | `:hammer:`                   | 重大重构              |
| 🎨调色板          | `:art:`                      | 改进代码结构/代码格式 |
| 🔥火焰            | `:fire:`                     | 移除代码或文件        |
| ✏铅笔            | `:pencil2:`                  | 修复typo              |
| 🚧施工            | `:comstruction:`             | 工作进行中            |
| 👷工人            | `:construction_worker:`      | 添加CI构建系统        |
| 💚绿心            | `:green_heart:`              | 修复CI构建问题        |
| 🔒锁              | `:lock:`                     | 修复安全问题          |
| 🐋鲸鱼            | `:whales:`                   | Docker相关工作        |
| 🍎苹果            | `:apple:`                    | 修复macOS下的问题     |
| 🐧企鹅            | `:penguin:`                  | 修复Linux下的问题     |
| 🏁旗帜            | `:checked_flag:`             | 修复windows下的问题   |



PS：我还找到了如何在markdown文档中添加的emoji的方法。

使用搜狗输入法自带的emoji太大的，而且本地图片无法在网页中加载。

我也不知道在网页上会显示多大，会不会调整，但是我找到了一个网站：

[emoji之家](<http://emojihomepage.com/>)

可以在上面的链接中搜索想要的emoji，然后直接点击复制，在markdown文件中粘贴就好了。😎

