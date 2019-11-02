---
title: HEXO-NexT-V7-Gemini基础建站篇
mathjax: true
copyright: true
comment: true
top: false
tags:
  - hexo
  - next
  - blog
categories:
  - 专业
  - 前端
  - hexo
abbrlink: 9ecd29f3
date: 2019-10-30 22:19:07
---
{% note default no-icon %}
{% cq %}
<p>
本文主要介绍了如何利用hexo创建个人博客，选择主题，基本配置。内容包括：安装环境，git配置，配置站点信息，修改主题，博客自定义图标、显示分类/标签/归档页的内容量，头像效果等 
</p>
{% endcq %}
<p>本文使用的版本：Node.js: v12.6.0，npm: 6.9.0， hexo: 4.0.0，NexT: v7.4.2</p>
{% endnote %}

<!-- more -->
{% note primary no-icon %}
next文档:https://theme-next.iissnan.com/
Hexo官网文档：https://hexo.io/docs/
Node.js官方下载地址：https://nodejs.org/
git官方下载地址：http://git-scm.com/download/
{% endnote %}

### 安装环境
- 安装 git
- 安装 Node.js
- 安装 Hexo

```
# 安装hexo
npm install -g hexo-cli

# 创建项目
hexo init mysite

# 清除缓存
hexo clean
# 生成静态文件
hexo g
# 本地测试
hexo server
# 部署
hexo d

```


### git配置

```
# 配置用户
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
# 生成密钥
ssh-keygen -t rsa -C "youremail"

# id_rsa.pub复制到github
# 测试是否连接成功
ssh -T git@github.com
```

### 配置站点信息

```
# /_config.yml
# Site
title: 洋洋的小站
subtitle: 洋洋个人博客
description: 人生苦短我用python。
keywords: "Python, 爬虫, django"
author: 洋洋
language: zh-CN

# 主题
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:zhihuya188/zhihuya188.github.io.git
  branch: master
```

### 必要插件

```
# git
npm install hexo-deployer-git --save
# rss
npm install hexo-generator-feed --save

# mathjax
npm un hexo-renderer-marked --save
npm i hexo-renderer-kramed --save

# 搜索
npm install hexo-generator-searchdb --save
```

### 修改主题

下载主题
```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

修改`/_config.yml`
```
# /_config.yml
theme: next
```
### 主题配置

> 样式

修改`themes/next/_config.yml`
```
# themes/next/_config.yml
# 样式
scheme: Gemini
```

> avatar(个人头像)

将其放置到 themes/next/source/images/avatar.png 路径，然后在主题 `_config.yml` 文件下编辑 `avatar` 的配置，修改为正确的路径即可。

```
# themes/next/source/images/avatar.png 个人头像
# 配置 themes/next/_config.yml
# Sidebar Avatar
avatar:
  # In theme directory (source/images): /images/avatar.gif
  # In site directory (source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /images/avatar.png
  # If true, the avatar would be dispalyed in circle.
  # 选项是是否显示圆形
  rounded: true
  # If true, the avatar would be rotated with the cursor.
  # 是否带有旋转效果
  rotated: true
```

> code

```
codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: night bright
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style: mac
```

> top(返回到网站的上端)

```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  # 阅读百分比
  scrollpercent: true

# reading_process
# 阅读进度
reading_progress:
  enable: true
  # Available values: top | bottom
  position: top
  color: "#222"
  height: 2px
```


> github_banner

```
# github_banner
# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: true
  permalink: https://github.com/zhihuya188/zhihuya188.github.io
  title: zhihuya188
```

> gitalk(评论)

```
# gitalk
# Multiple Comment System Support
comments:
  # Available values: tabs | buttons
  style: tabs
  # Choose a comment system to be displayed by default.
  # Available values: changyan | disqus | disqusjs | facebook_comments_plugin | gitalk | livere | valine | vkontakte
  active: gitalk

# 配置
# Gitalk
# Demo: https://gitalk.github.io
# For more information: https://github.com/gitalk/gitalk
gitalk:
  enable: true
  github_id: {your_id}
  repo: nightteam.github.io # Repository name to store issues
  client_id: {your client id} # GitHub Application Client ID
  client_secret: {your client secret} # GitHub Application Client Secret
  admin_user: {your_id} # GitHub repo owner and collaborators, only these guys can initialize gitHub issues
  distraction_free_mode: true # Facebook-like distraction free mode
  # Gitalk's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Available values: en | es-ES | fr | ru | zh-CN | zh-TW
  language: zh-CN
```

> pangu(中文和英文间距问题)

```
# 中文和英文间距
pangu: true
```

> math

```
# 公式
math:
  enable: true

  # Default (true) will load mathjax / katex script on demand.
  # That is it only render those page which has `mathjax: true` in Front-matter.
  # If you set it to false, it will load mathjax / katex srcipt EVERY PAGE.
  per_page: true

  # hexo-renderer-pandoc (or hexo-renderer-kramed) required for full MathJax support.
  mathjax:
    enable: true
    # See: https://mhchem.github.io/MathJax-mhchem/
    mhchem: true
```

> pjax

```
# 类似于ajax 根据情况开
pjax: false
```

```
# 依赖的库
$ cd themes/next
$ git clone https://github.com/theme-next/theme-next-pjax source/lib/pjax
```

### 标签页 分类页

> 创建

```
# 根目录执行
# 标签页
hexo new page tags
# 分类页
hexo new page categories
```

> 配置

```
---
title: tags
type: "tags"
comments: false
date: 2019-10-25 12:05:47
---

---
title: categories
type: "categories"
comments: false
date: 2019-10-25 12:10:44
---
```

### 搜索页

> 安装插件

```
npm install hexo-generator-searchdb --save
```
> 配置

```
# /_config.yml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

### 404 页面

> 创建

```
hexo new page 404
```
> 配置

```
# /source/404/404.html

---
title: 404 Not Found：该页无法显示
date: 2019-09-22 10:41:27
toc: false
comments: false
permalink: /404
---
<!DOCTYPE html>
<html>
    <head>
         <meta charset="UTF-8" />
         <title>404</title>                                                                                                                                        
    </head>
    <body>
         <script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" homePageName="返回首页" homePageUrl="https://zhihuya188.github.io"></script>
    </body>
</html>
```

> 修改 menu

```
# 主菜单
menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat

```

### 部署脚本

> 创建文件

```
# /
touch deploy.sh
```

> 编辑

```
hexo clean
hexo generate
hexo deploy
```

> 部署执行

```
sh deploy.sh
```






