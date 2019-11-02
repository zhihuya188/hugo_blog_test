---
title: hexo-next-blog
comments: true
tags:
  - hexo
  - next
categories:
  - blog
abbrlink: bdb4dc99
date: 2019-10-25 23:03:00
---

## 说明
记录个人博客hexo-next配置过程
<!-- more -->

> next文档:https://theme-next.iissnan.com/
> 
> Hexo官网文档：https://hexo.io/docs/
> 
> Node.js官方下载地址：https://nodejs.org/
> 
> git官方下载地址：http://git-scm.com/download/

### 安装环境

- 安装 Node.js
- 安装 Hexo

```
# 安装hexo
npm install -g hexo-cli

# 创建项目
hexo init hexo_next_blog

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
### 项目根配置文件

```
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
```
git clone https://github.com/theme-next/hexo-theme-next themes/next

# 根配置
theme: next

# 主题配置文件
# 样式
scheme: Pisces

# 图标
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

# code
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

# top
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

# bookmark
bookmark:
  enable: false
  # Customize the color of the bookmark.
  color: "#222"
  # If auto, save the reading progress when closing the page or clicking the bookmark-icon.
  # If manual, only save it by clicking the bookmark-icon.
  save: auto

# github_banner
# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: true
  permalink: https://github.com/NightTeam/nightteam.github.io
  title: NightTeam GitHub


# gitalk
# Multiple Comment System Support
comments:
  # Available values: tabs | buttons
  style: tabs
  # Choose a comment system to be displayed by default.
  # Available values: changyan | disqus | disqusjs | facebook_comments_plugin | gitalk | livere | valine | vkontakte
  active: gitalk

配置
# Gitalk
# Demo: https://gitalk.github.io
# For more information: https://github.com/gitalk/gitalk
gitalk:
  enable: true
  github_id: NightTeam
  repo: nightteam.github.io # Repository name to store issues
  client_id: {your client id} # GitHub Application Client ID
  client_secret: {your client secret} # GitHub Application Client Secret
  admin_user: germey # GitHub repo owner and collaborators, only these guys can initialize gitHub issues
  distraction_free_mode: true # Facebook-like distraction free mode
  # Gitalk's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Available values: en | es-ES | fr | ru | zh-CN | zh-TW
  language: zh-CN

# pangu
# 中文和英文间距
pangu: true

# math
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

# pjax
# 类似于ajax
pjax: true
# 依赖的库
$ cd themes/next
$ git clone https://github.com/theme-next/theme-next-pjax source/lib/pjax

# 根目录执行
# 标签页
hexo new page tags
# 分类页
hexo new page categories

配置
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

# 搜索
npm install hexo-generator-searchdb --save
# 项目目录配置
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
# 主题配置
# Local search
# Dependencies: https://github.com/wzpan/hexo-generator-search
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 5
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
```
### 404

```

---
title: 404 Not Found：该页无法显示
date: 2019-07-28 12:47:20
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
### 部署脚本
```
touch deploy.sh
vim deploy.sh

hexo clean
hexo generate
hexo deploy

sh deploy.sh
```


---
### 文章阴影设置未解决

```
# ~\themes\next-reloaded\source\css\main.styl
//My Layer
//--------------------------------------------------
@import "_custom/custom.styl";


# _custom/custom.styl



```

### 评论补充
```
Register Application
在GitHub上注册新应用，链接：https://github.com/settings/applications/new
参数说明：
Application name： # 应用名称，随意
Homepage URL： # 网站URL，如https://asdfv1929.github.io
Application description # 描述，随意
Authorization callback URL：# 网站URL，https://asdfv1929.github.io
```
