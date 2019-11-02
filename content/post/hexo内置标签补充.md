---
title: hexo内置标签补充
mathjax: true
copyright: true
comment: true
top: false
tags:
  - hexo
categories:
  - 专业
  - 前端
  - hexo
abbrlink: 445ba317
date: 2019-11-01 09:53:46
---

{% note default %}
本文主要介绍hexo内部标签使用
{% endnote %}

<!-- more -->

## hexo内置标签

### button 按钮

通过 button 标签可以快速添加带有主题样式的按钮，语法如下：

```
{% button /path/to/url/, text, icon [class], title %}
```

也可简写为：

```
{% btn /path/to/url/, text, icon [class], title %}
```

{% note info no-icon %}
其中， 图标 ID 来源于 [FontAwesome](https://fontawesome.com/v4.7.0/icons/) 。
{% endnote %}

使用示例如下：

```
{% btn #, 文本 %}
{% btn #, 文本 & 标题,, 标题 %}
{% btn #, 文本 & 图标, home %}
{% btn #, 文本 & 大图标 (固定宽度), home fa-fw fa-lg %}

```

{% btn #, 文本 %}
{% btn #, 文本 & 标题,, 标题 %}
{% btn #, 文本 & 图标, home %}
{% btn #, 文本 & 大图标 (固定宽度), home fa-fw fa-lg %}

{% btn /, 首页 & 图标, home %}

### 插入 Swig 代码

如果需要在页面内插入 Swig 代码，包括原生 HTML 代码，JavaScript 脚本等，可以通过 raw 标签来禁止 Markdown 引擎渲染标签内的内容。语法如下：

```
{% raw %}
content
{% endraw %}
```
该标签通常用于在页面内引入三方脚本实现特殊功能，尤其是当该三方脚本尚无相关 hexo 插件支持的时候，可以通过写原生 Web 页面的形式引入脚本并编写实现逻辑。

### 网易云音乐

在网页版云音乐中找到歌曲，点击生成外链播放器

根据个人喜好选择播放器尺寸和播放模式

复制代码

```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1337926081&auto=0&height=66"></iframe>
```

效果如下：

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1337926081&auto=1&height=66"></iframe>

### b站视频

在b站找到喜欢的视频，点击分享，生成外链播放器

根据个人喜好选择播放器尺寸和播放模式

复制代码

```
<iframe src="//player.bilibili.com/player.html?aid=35314148&cid=61896359&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
```
效果如下：
<iframe src="//player.bilibili.com/player.html?aid=35314148&cid=61896359&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

<hr>