---
title: Hexo yilia配置
tags:
  - hexo
  - yilia
categories:
  - 专业
  - 前端
  - hexo
keywords: 经过多方收集整理的hexo yilia配置，仅供参考
abbrlink: 4392eea4
date: 2019-07-29 08:59:41
---
## 前言

刚接触搭Hexo是在Crossin的编程教室的[建免费博客](https://lab.crossincode.com/deploy/)
接触的hexo 主题也是yilia。并且很喜欢这个主题。经过多方收集整理的hexo yilia配置，
其中有：修复缺失模块，配置头像，文章摘要，菜单优化，静态图床，统计，置顶404

仅供参考

<!--more-->

## 查看所有文件，提示缺失模块

```
# 缺少模块。
# 1、请确保node版本大于6.2
# 2、在博客根目录（注意不是yilia根目录）执行以下命令:

npm i hexo-generator-json-content --save

# 3、在根目录_config.yml里添加配置：
# 在全局配置文件_config.yml进行配置

jsonContent:
    meta:false
    pages:false
    posts:
      title:true
      date:true
      path:true
      text:false
      raw:false
      content:false
      slug:false
      updated:false
      comments:false
      link:false
      permalink:false
      excerpt:false
      categories:false
      tags:true
```
## 配置图片资源

添加图片资源文件夹。 路径为 `themes/yilia/source/`下，可添加一个 `/assets/images/` 文件夹，里面存放图片资源即可

配置文件中直接引用即可。路径为 themes/yilia/_config.yml，找到如下即可

```
# 头像图片
avatar:  /assets/images/head.jpg
# 网页图标
favicon:  /assets/images/head.jpg
```
## 文章如何显示摘要

在你 MD 格式文章正文插入 `<!-- more -->`

## 增加菜单


```
hexo new page "archives"
hexo new page "about"
hexo new page "categories"
```
修改 themes/yilia/_config.yml

```
menu:
  主页: /
  分类: /categories
  归档: /archives
  关于: /about
```
`编辑hexo/source/about/index.md`

### categories配置

**本节就实现多层分类的显示效果，具体操作如下：**

`编辑hexo/source/categories/index.md`

1、修改categories/index.md为：
```
---
title: 文章分类
date: 2019-07-29 10:13:21
type: "categories"
layout: "categories"
comments: false
---

```
指定layout为categories，渲染时就会使用categories.ejs进行渲染。

2、新建yilia/layout/categories.ejs，内容如下：

```
<article class="article article-type-post show">
  <header class="article-header" style="border-bottom: 1px solid #ccc">
  <h1 class="article-title" itemprop="name">
    <%= page.title %>
  </h1>
  </header>

  <% if (site.categories.length){ %>
  <div class="category-all-page">
    <h2>共计&nbsp;<%= site.categories.length %>&nbsp;个分类</h2>
    <%- list_categories(site.categories, {
      show_count: true,
      class: 'category-list-item',
      style: 'list',
      depth: 3,
      separator: ''
    }) %>
  </div>
  <% } %>
</article>

```

3、修改yilia\source\main.0cf68a.css，将下面的内容添加进去：

```
category-all-page {
    margin: 30px 40px 30px 40px;
    position: relative;
    min-height: 70vh;
  }
  .category-all-page h2 {
    margin: 20px 0;
  }
  .category-all-page .category-all-title {
    text-align: center;
  }
  .category-all-page .category-all {
    margin-top: 20px;
  }
  .category-all-page .category-list {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  .category-all-page .category-list-item-list-item {
    margin: 10px 15px;
  }
  .category-all-page .category-list-item-list-count {
    color: $grey;
  }
  .category-all-page .category-list-item-list-count:before {
    display: inline;
    content: " (";
  }
  .category-all-page .category-list-item-list-count:after {
    display: inline;
    content: ") ";
  }
  .category-all-page .category-list-item {
    margin: 10px 10px;
  }
  .category-all-page .category-list-count {
    color: $grey;
  }
  .category-all-page .category-list-count:before {
    display: inline;
    content: " (";
  }
  .category-all-page .category-list-count:after {
    display: inline;
    content: ") ";
  }
  .category-all-page .category-list-child {
    padding-left: 10px;
  }

```

4、修改自己的文章

```
---
title: Hexo yilia基础配置
date: 2019-07-29 08:59:41
toc: true
top: 4
tags: [hexo, yilia]
categories: [专业, 前端, hexo]
keywords: 
---
```
5、生成html
```
hexo clean 
hexo g
hexo s
```
访问categories，达到了预期效果，如下图：

![分类](http://wx2.sinaimg.cn/mw690/d818c3ccly1g5ghpnz6qaj208109wjrb.jpg)


## 静态图床

可以去chrome网上应用商店下载一个叫[微博图床](https://chrome.google.com/webstore/detail/%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A/pinjkilghdfhnkibhcangnpmcpdpmehk?hl=zh-CN)的chrome插件，下图是插件的界面，操作简单方便，具体使用看说明就可以啦，比较简单，这样图床的问题就解决了。

---

## 统计篇

### 1、添加站点访问量

> 增加不蒜子统计

下面是个人改变完成后的\themes\yilia\layout\\_partial\footer.ejs文件：

```
<footer id="footer">
  <div class="outer">
    <div id="footer-info">
    	<div class="footer-left">
    		&copy; <%= date(new Date(), 'YYYY') %> <%= config.author || config.title %>
    	</div>
      	<div class="footer-right">
      		<a href="http://hexo.io/" target="_blank">Hexo</a>  Theme <a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a> by Litten
      		<!--# PV方式，单个用户连续点击 n 篇，记录 n 次记录值-->
			<span id="busuanzi_container_site_pv">
                本站总访问量：<span id="busuanzi_value_site_pv"></span>次
            </span>
            <span class="post-meta-divider">|</span>
            <!--# UV方式，单个用户连续点击 n 篇，记录 1 次记录值-->
			<span id="busuanzi_container_site_uv">
                本站访客数：<span id="busuanzi_value_site_uv"></span>人
            </span>
			
      	</div>
    </div>
  </div>
</footer>
<!--增加不蒜子统计-->
<script  async  src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>

```
`<a href="http://hexo.io/" target="_blank">Hexo</a>  Theme <a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a> by Litten`（原来是主题的说明，不要可以删除）。

### 2、单篇文章点击量

在`themes/yilia/layout/_partial/article.ejs`中 在

`<%- partial('post/title', {class_name: 'article-title'}) %>`

后面插入如下代码

```
<!--显示阅读次数-->
<% if (!index && post.comments){ %>
  <br/>
  <a class="cloud-tie-join-count" href="javascript:void(0);" style="color:gray;font-size:14px;">
  <span class="icon-sort"></span>
  <span id="busuanzi_container_page_pv" style="color:#ef7522;font-size:14px;">
            阅读数: <span id="busuanzi_value_page_pv"></span>次 &nbsp;&nbsp;
  </span>
  </a>
<% } %>
<!--显示阅读次数完毕-->
```

### 3、字数、阅读时长添加


#### 安装 hexo-wordcount

在博客目录下打开Git Bash Here 输入命令:

`npm install hexo-wordcount -–save`

#### 文件配置
在`theme\yilia\layout\_partial\post`下创建`word.ejs`文件：

```
<div style="margin-top:10px;">
    <span class="post-time">
      <span class="post-meta-item-icon">
        <i class="fa fa-keyboard-o"></i>
        <span class="post-meta-item-text">  字数统计: </span>
        <span class="post-count"><%= wordcount(post.content) %>字</span>
      </span>
    </span>

    <span class="post-time">
      &nbsp; | &nbsp;
      <span class="post-meta-item-icon">
        <i class="fa fa-hourglass-half"></i>
        <span class="post-meta-item-text">  阅读时长: </span>
        <span class="post-count"><%= min2read(post.content) %>分</span>
      </span>
    </span>
</div>
```
然后在 `themes/yilia/layout/_partial/article.ejs`中添加:

```
<div class="article-inner">
    <% if (post.link || post.title){ %>
      <header class="article-header">
        <%- partial('post/title', {class_name: 'article-title'}) %>
        <% if (!post.noDate){ %>
        <%- partial('post/date', {class_name: 'archive-article-date', date_format: null}) %>
        <!-- 需要添加的位置 -->
        <!-- 开始添加字数统计-->
        <% if(theme.word_count && !post.no_word_count){%>
          <%- partial('post/word') %>
          <% } %>
        <!-- 添加完成 -->

        <% } %>
      </header>
```

#### 开启功能

在站点的_config.yml中添加下面代码
```
# 是否开启字数统计
#不需要使用，直接设置值为false，或注释掉
word_count: True
```
效果预览：https://yangyang188.github.io


### 4、Hexo-Yilia网站运行时间

在`hexo/themes/yelee/layout/_partial/footer.ejs`文件中`<div class="footer-left">`内（具体位置可自选）加入如下代码：

```
			<span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span>
			<script>
				var now = new Date(); 
				function createtime() { 
					var grt= new Date("11/23/2018 20:00:00");//此处修改你的建站时间或者网站上线时间 
					now.setTime(now.getTime()+250); 
					days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days); 
					hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours); 
					if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum); 
					mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;} 
					seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum); 
					snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;} 
					document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 "; 
					document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒"; 
				} 
				setInterval("createtime()",250);
			</script>

```
注意：日期格式`11/29/2019 18:00:00`

### 5、在左侧显示总文章数

```
<nav class="header-menu">
    <ul>
    <% for (var i in theme.menu){ %>
        <li><a href="<%- url_for(theme.menu[i]) %>"><%= i %></a></li>
    <%}%>
    </ul>
</nav>
```
后面加上

```
<nav>
    总文章数 <%=site.posts.length%>
</nav>
```



---

## 怎么置顶文章

### 安装插件

```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

### 配置置顶标准

打开：`/themes/*/layout（/_macro）/post.ejs`
直接在最前面加入以下代码即可

```
<% if (page.top) { %>
  <i class="fa fa-thumb-tack"></i>
  <font color=7D26CD>置顶</font>
  <span class="post-meta-divider">|</span>
<% } %>
```

### 配置文章

然后在需要置顶的文章的Front-matter中加上top选项即可
top后面的数字越大，优先级越高

```
---
title: 2019
date: 2019-07-29 16:10:03
top: 5
---
```

### 优先级配置

修改根目录配置文件`/_config.yml`,`top`值`-1`标示根据`top`值倒序（正序设置为1即可），同样date也是根据创建日期倒序。

```
index_generator:
  path: ''
  per_page: 10
  order_by:
    top: -1
    date: -1
```

## hexo添加404公益界面

1. 在博客根目录下终端输入命令:`hexo new page 404`
2. 打开刚新建的页面文件，默认在 Hexo 文件夹根目录下 `/source/404/index.md`；添加以下代码：


```
---
title: 404 Not Found：该页无法显示
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
         <script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" homePageName="返回首页" homePageUrl="https://yangyang188.coding.me"></script>
	</body>
</html>

```
> 改`homePageUrl`:地址为你的主页
>
> `index.md`改为`404.html`

3. 部署:`hexo d -g`


## 参考

[Hexo-Yilia进阶笔记](https://blog.csdn.net/dta0502/article/details/89607895)

[Hexo Yilia 高级配置](http://dongshuyan.com/2019/05/24/hexo%E5%8D%9A%E5%AE%A2%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9/#1%E3%80%81%E5%BE%AE%E4%BF%A1%E5%88%86%E4%BA%AB%E5%BC%82%E5%B8%B8)

[Hexo yilia 主题一揽子使用方案](https://www.liuyun.fun/2018/04/07/Hexo_yilia_%E4%B8%BB%E9%A2%98%E4%B8%80%E6%8F%BD%E5%AD%90%E4%BC%98%E5%8C%96%E6%96%B9%E6%A1%88/#%E6%96%87%E7%AB%A0%E6%98%BE%E7%A4%BA%E7%9B%AE%E5%BD%95)

[超详细Hexo+Github Page搭建技术博客教程【持续更新】](https://segmentfault.com/a/1190000017986794#articleHeader0)

[hexo下yilia主题添加字数统计和阅读时长功能](https://blog.csdn.net/stormdony/article/details/82559179)



