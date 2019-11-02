---
title: Hexo-yilia测试
tags:
  - hexo
  - yilia
categories:
  - 专业
  - 前端
  - hexo
keywords: ''
abbrlink: 6a82640f
date: 2019-07-31 12:03:00
---
# 前言

主要测试音频视频插入
需要安装插件`npm inatall hexo-tag-owl --save` `npm inatall hexo-tag-video --save`
不知道什么原因抖音视频本地测试成功，上线就失败了。
## 插入音频视频
<!--more-->

### 音乐

```
<iframe width="100%" height="100%" frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=29793430&auto=0&height=32"></iframe>
```


<iframe width="100%" height="100%" frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=29793430&auto=0&height=32"></iframe>

### 抖音视频

```
<iframe width="350" height="620" frameborder="1" border="1" marginwidth="1" src="https://aweme.snssdk.com/aweme/v1/playwm/?s_vid=93f1b41336a8b7a442dbf1c29c6bbc5646bd13461529bc94f6724c2ca8dcc40839f088dba6360cf46bb6656bb3299240a79e4c9199871cad8e2f2aac26d7f12d&line=0" ></iframe>
```

<iframe width="350" height="620" frameborder="1" border="1" marginwidth="1" src="https://aweme.snssdk.com/aweme/v1/playwm/?s_vid=93f1b41336a8b7a442dbf1c29c6bbc5646bd13461529bc94f6724c2ca8dcc40839f088dba6360cf46bb6656bb3299240a79e4c9199871cad8e2f2aac26d7f12d&line=0" ></iframe>

### bilibili视频

```
<iframe width="350" height="620" frameborder="1" border="1" marginwidth="1" src="https://aweme.snssdk.com/aweme/v1/playwm/?s_vid=93f1b41336a8b7a442dbf1c29c6bbc5646bd13461529bc94f6724c2ca8dcc40839f088dba6360cf46bb6656bb3299240a79e4c9199871cad8e2f2aac26d7f12d&line=0" ></iframe>
```

<iframe width="1080" height="680" src="//player.bilibili.com/player.html?aid=60571656&cid=105437527&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

