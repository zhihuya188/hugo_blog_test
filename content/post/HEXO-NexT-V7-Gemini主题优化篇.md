---
title: HEXO-NexT-V7-Gemini主题优化篇
mathjax: true
copyright: true
comment: true
top: false
tags: [hexo, next, blog]
categories: [专业,前端,hexo]
abbrlink: 3575fa23
date: 2019-10-31 19:26:57
---

{% note default %}
本文主要介绍了hexo个人博客next主题优化, 启用数据目录的方法,转移自定义 CSS 等配置文件的方法等
{% endnote %}

<!-- more -->

> 本文使用的版本：Node.js: v12.6.0，npm: 6.9.0， hexo: 4.0.0，NexT: v7.4.2

### 启用数据目录

{% note info no-icon %}
参考：[NexT 7.3 数据目录及自定义 CSS 的启用方式](https://www.imczw.com/post/tech/next_data_file.html)
{% endnote %}

在 根目录`/source/_data` 目录下新建 `next.yml` 文件，把 `Next` 的主题配置文件 `next/_config.yml` 内容全部复制到 `next.yml`，然后修改以下位置

```
# If false, merge configs from `_data/next.yml` into default configuration (rewrite).
# If true, will fully override default configuration by options from `_data/next.yml` (override). Only for NexT settings.
# And if true, all config from default NexT `_config.yml` must be copied into `next.yml`. Use if you know what you are doing.
# Useful if you want to comment some options from NexT `_config.yml` by `next.yml` without editing default config.override: false
override: true
```

> 定义 CSS 路径

配置 `/source/_data/next.yml`

```
custom_file_path:
  ... 
  style: source/_data/styles.styl
```

接着在 `/source/_data` 目录下新建 `styles.styl` 文件，把以前 `custom.styl` 文件的内容复制过来即可

### 数据统计

{% note info no-icon %}
参考：[Hexo 搭建个人博客系列：进阶设置篇](http://yearito.cn/posts/hexo-advanced-settings.html)
{% endnote %}

#### 站点访问量统计

{% note info no-icon %}
该功能由 [不蒜子](http://ibruce.info/2015/04/04/busuanzi/) 提供 包括：独立访客数 UV，浏览量 PV
{% endnote %}
效果如下：
![](https://tvax1.sinaimg.cn/large/d818c3ccly1g8hnctz76rj20dx02saa3.jpg)

主题配置文件：`/source/_data/next.yml`

```
busuanzi_count:
  enable: true
  total_visitors: true   # 访客数
  total_visitors_icon: user
  total_views: true   # 访问量
  total_views_icon: eye
  post_views: false
  post_views_icon: eye
```

> 高阶用法：通过修改代码来自定义统计文案

编辑`themes/next/layout/_partials/analytics/busuanzi-counter.swig`

```
{%- if theme.busuanzi_count.enable %}
<div class="busuanzi-count">
  <script{{ pjax }} async src="https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

  {%- if theme.busuanzi_count.total_visitors %}
      <span class="site-uv">
        {{ __('footer.total_visitors', '<span class="busuanzi-value" id="busuanzi_value_site_uv"></span>') }}
      </span>

  {%- endif %}

  {%- if theme.busuanzi_count.total_visitors and theme.busuanzi_count.total_views %}
    <span class="post-meta-divider">|</span>
  {%- endif %}

  {%- if theme.busuanzi_count.total_views %}
    <span class="site-pv">
     {{ __('footer.total_views', '<span class="busuanzi-value" id="busuanzi_value_site_pv"></span>') }}
    </span>
  {% endif %}
</div>
{%- endif %}
```

在自定义样式文件中添加如下样式：

编辑：`/source/_data/styles.styl`

```
//修改不蒜子数据颜色
.busuanzi-value {
  color: #1890ff;
}
```

编辑：`themes\next\languages\zh-CN.yml`

```
footer:
  total_views: "历经 %s 次回眸才与你相遇"
  total_visitors: "我的第 %s 位朋友，"
```

#### 站点运行时间统计

创建文件`source/_data/footer.swig`添加以下代码:

```
{# 页脚站点运行时间统计 #}
{% if theme.footer.ages.enable %}
  <script src="https://cdn.jsdelivr.net/npm/moment@2.22.2/moment.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/moment-precise-range-plugin@1.3.0/moment-precise-range.min.js"></script>
  <script>
    function timer() {
      var ages = moment.preciseDiff(moment(),moment({{ theme.footer.ages.birthday }},"YYYYMMDD"));
      ages = ages.replace(/years?/, "年");
      ages = ages.replace(/months?/, "月");
      ages = ages.replace(/days?/, "天");
      ages = ages.replace(/hours?/, "小时");
      ages = ages.replace(/minutes?/, "分");
      ages = ages.replace(/seconds?/, "秒");
      ages = ages.replace(/\d+/g, '<span style="color:{{ theme.footer.ages.color }}">$&</span>');
      div.innerHTML = `{{ __('footer.age')}} ${ages}`;
    }
    var div = document.createElement("div");
    //插入到copyright之后
    var copyright = document.querySelector(".copyright");
    document.querySelector(".footer-inner").insertBefore(div, copyright.nextSibling);
    timer();
    setInterval("timer()",1000)
  </script>
{% endif %}
```

主题配置文件：`/source/_data/next.yml`

```
custom_file_path:
  ... 
  style: footer: source/_data/footer.swig

footer:
  ...
+ ages:
+     # site running time
+     enable: true
+     # birthday of your site
+     birthday: 20191031
+     # color of number
+     color: "#1890ff"

```
然后补全对应文案：

`themes\next\languages\zh-CN.yml`
```
  footer:
    powered: "由 %s 强力驱动"
    theme: 主题
+   age: 我已在此等候你
    total_views: "历经 %s 次回眸才与你相遇"
    total_visitors: "我的第 %s 位朋友，"
```
刷新浏览器即可生效。

#### 文章访问量统计

{% note info no-icon %}
该功能基于 [LeanCloud](https://leancloud.cn/) 提供后端数据服务
{% endnote %}

效果如下：
![文章访问量](https://tva2.sinaimg.cn/large/d818c3ccly1g8ho8u0ywxj20ts032dfz.jpg)

在 LeanCloud 上注册账号并创建应用，新建一个名为 Counter 的 Class，ACL 权限设置为 无限制：

{% note warning %}
在 LeanCloud 中的 Class 可以理解为数据库中的数据表。Counter 用于存储记录文章访问量，记录是以 url 作为唯一依据的，所以根据默认的 permalink 组成结构，如果你更改了文章的发布日期和标题中的任意一个，都会造成文章阅读数值的清零重计。
{% endnote %}

在控制台的 设置 -> 应用 Key 中获取 App ID 和 App Key 填入到主题配置文件中：

`/source/_data/next.yml`

```
leancloud_visitors:
  enable: true
  app_id: ***<app_id***
  app_key: ***<app_key>***
  security: false
  betterPerformance: false
```

刷新浏览器即可生效。

{% note warning %}
如果遇到如下报错，可能是你配置了 security: true 但又没有做好安全策略配置。

阅读次数： Counter not initialized! See more at console err msg.
{% endnote %}

#### 站点及文章字数统计

{% note info no-icon %}
该功能由 [hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time) 提供
{% endnote %}

在根目录下执行如下命令安装相关依赖
`npm install hexo-symbols-count-time --save`

将如下配置项添加到站点配置文件中，这些配置项主要用于控制每项统计信息是否显示。

`_config.yml`

```
symbols_count_time:
  symbols: true   # 统计单篇文章字数
  time: false   # 取消估算单篇文章阅读时间
  total_symbols: true  # 统计站点总字数
  total_time: false  # 取消估算站点总阅读时间
```

在主题配置文件中做如下修改，这些配置项主要用于控制统计信息的显示样式。

`/source/_data/next.yml`
```
symbols_count_time:
  separated_meta: false  # 统计信息不换行显示
  item_text_post: true  # 文章统计信息中是否显示“本文字数/阅读时长”等描述文字
  item_text_total: true   # 站点统计信息中是否显示“本文字数/阅读时长”等描述文字
  awl: 4  # Average Word Length：平均字符长度
  wpm: 275  # Words Per Minute：阅读速度
```

### 站点个性化设置

#### 热门文章排行榜

效果如下：
![](//tvax1.sinaimg.cn/large/d818c3ccly1g8i8hbqs2nj20ry0hdq3m.jpg)

{% note info no-icon %}
该功能同样是基于 LeanCloud 提供的后端服务支持。具体实现方案如下：
{% endnote %}

在站点目录下执行以下命令新建页面
`hexo new page top`

`/source/_data/next.yml`
```
menu:
    home: / || home
+   top: /top/ || signal
    tags: /tags/ || tags
    categories: /categories/ || th
    archives: /archives/ || archive
    about: /about/ || user
```

在语言包中新增菜单中文：

`themes\next\languages\zh-CN.yml`
```
  menu:
    home: 首页
    archives: 归档
    categories: 分类
    tags: 标签
    about: 关于
+   top: 排行榜
```

然后在新增的排行榜页面内添加以下内容：

`source\top\index.md`
```
---
title: 文章阅读量排行榜
comments: false
keywords: top,文章阅读量排行榜
description: 博客文章阅读量排行榜
---

<style>.post-description { display: none; }</style>

<div id="top"></div>
<script src="https://cdn1.lncld.net/static/js/av-core-mini-0.6.4.js"></script>
<script>AV.initialize("[APP_ID]", "[APP_KEY]");</script> //输入个人LeanCloud账号AppID 输入个人LeanCloud账号AppKey
<script type="text/javascript">
  var time=0
  var title=""
  var url=""
  var query = new AV.Query('Counter');
  query.notEqualTo('id',0);
  query.descending('time'); //结果按阅读次数降序排序
  query.limit(10); //最终只返回10条结果
  query.find().then(function (todo) {
    for (var i=0;i<1000;i++){
      var result=todo[i].attributes;
      time=result.time;
      title=result.title;
      url=result.url;
      var content="<a href='"+"https://zhihuya188.github.io"+url+"'>"+title+"</a>"+"<br />"+"<font color='#555'>"+"阅读次数："+time+"</font>"+"<br /><br />";
      document.getElementById("top").innerHTML+=content
    }
  }, function (error) {
    console.log("error");
  });
</script>
```

#### 站内搜索

{% note info no-icon %}
该功能由 [hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb) 提供
{% endnote %}

在根目录下执行以下命令安装相关依赖：
`npm install hexo-generator-searchdb --save`

主题配置文件`/source/_data/next.yml`

```
local_search:
  enable: true
  trigger: auto  # 每次输入改变都执行搜索
  top_n_per_article: 3  # 每篇文章显示的搜索结果数量
  unescape: false
```

在站点配置文件中添加以下字段：

`_config.yml`
```
search:
  path: search.xml
  field: post  # 指定搜索范围，可选 post | page | all
  format: html  # 指定页面内容形式，可选 html | raw (Markdown) | excerpt | more
  limit: 10000
```
#### 文末版权声明

主题配置文件`/source/_data/next.yml`

```
# 添加文章版权信息
creative_commons:
  license: by-nc-sa
  sidebar: true
  post: true
  language: zh-CN
```

#### 添加打赏功能

启用主题配置文件中的打赏相关字段，并将个人收款码图片置于 themes\next\source\images\ 目录下，注意保持图片命名与配置文件中一致：

主题配置文件`/source/_data/next.yml`

```
# 配置微信，支付宝打赏
# Reward (Donate)
reward_settings:
  # If true, reward would be displayed in every article by default.
  # You can show or hide reward in a specific article throuth `reward: true | false` in Front-matter.
  enable: true
  animation: true
  comment: 谢谢您请我吃糖果 #打赏描述

reward:
  wechatpay: /images/alipay.jpg #微信支付的二维码图片地址
  #alipay: /images/alipay.png
  #bitcoin: /images/bitcoin.png
```

#### 添加图片灯箱

{% note info no-icon %}
添加灯箱功能，实现点击图片后放大聚焦图片，并支持幻灯片播放、全屏播放、缩略图、快速分享到社交媒体等，该功能由 [fancyBox](https://github.com/fancyapps/fancybox) 提供
{% endnote %}

在根目录下执行以下命令安装相关依赖：

`git clone https://github.com/theme-next/theme-next-fancybox3 themes/next/source/lib/fancybox`

在主题配置文件中设置 `fancybox: true`：

`/source/_data/next.yml`
```
fancybox: true
```

刷新浏览器即可生效。

#### 相关文章推荐

{% note info no-icon %}
该功能由 [hexo-related-popular-posts](https://github.com/tea3/hexo-related-popular-posts) 插件提供
{% endnote %}

在站点根目录中执行以下命令安装依赖：

`npm install hexo-related-popular-posts --save`

在主题配置文件中开启相关文章推荐功能：

`/source/_data/next.yml`
```
related_posts:
  enable: true
  title:  # custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
```

此时会在每篇文章结尾根据标签相关性和内容相关性来推荐相关文章。

#### 点击侧边栏头像回到博客首页

编辑`themes/next/layout/_partials/sidebar/site-overview.swig`

```
+    <a href="/">
      <img class="site-author-image" itemprop="image" alt="{{ author }}"
        src="{{ url_for(theme.avatar.url or theme.images + '/avatar.gif') }}">
+    </a>
```
#### 设置 hexo 永久链接

在根目录下执行以下命令安装相关插件：

`npm install hexo-abbrlink --save`

在站点配置文件`_config.yml`中修改`permalink`为如下：

```
permalink: archives/:abbrlink/
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
permalink_defaults:
```

#### nofollow 标签的使用

减少出站链接能够有效防止权重分散，hexo 有很方便的自动为出站链接添加 nofollow 的插件。

在根目录下执行以下命令安装相关插件：
`npm install hexo-autonofollow --save`

在站点配置文件`_config.yml`中修改`nofollow`为如下：

```
# 外部链接优化
nofollow:
    enable: true
    exclude:     # 例外的链接，可将友情链接放置此处
    - 'yousite'
```

#### 自定义友链页面

{% note info no-icon %}
参考：[Hexo NexT主题自定义友链页面](https://www.jianshu.com/p/ef110f36650b)
{% endnote %}

效果如下：
![](//tva4.sinaimg.cn/large/d818c3ccly1g8i9uefm8aj20tt09ijrt.jpg)

> 新增links页面

在根目录下执行
`hexo new page links`

> 配置menu

主题配置文件`next.yml`中`menu:`下添加：

```
  links: /links/ || link
```

`/themes/next/languages/zh-Hans.yml`文件中`menu:`下增加中文描述：

```
links: 友链
```

> 新增`links.swig`页

在`/themes/next/layout/`新建`links.swig`，内容如下：

<details><summary>点击显/隐内容</summary>
<p>

```
{% block content %}
  {######################}
  {### LINKS BLOCK ###}
  {######################}

    <div id="links">
        <style>

            #links{
               margin-top: 5rem;
            }

            .links-content{
                margin-top:1rem;
            }

            .link-navigation::after {
                content: " ";
                display: block;
                clear: both;
            }

            .card {
                width: 300px;
                font-size: 1rem;
                padding: 10px 20px;
                border-radius: 4px;
                transition-duration: 0.15s;
                margin-bottom: 1rem;
                display:flex;
            }
            .card:nth-child(odd) {
                float: left;
            }
            .card:nth-child(even) {
                float: right;
            }
            .card:hover {
                transform: scale(1.1);
                box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
            }
            .card a {
                border:none;
            }
            .card .ava {
                width: 3rem!important;
                height: 3rem!important;
                margin:0!important;
                margin-right: 1em!important;
                border-radius:4px;

            }
            .card .card-header {
                font-style: italic;
                overflow: hidden;
                width: 236px;
            }
            .card .card-header a {
                font-style: normal;
                color: #2bbc8a;
                font-weight: bold;
                text-decoration: none;
            }
            .card .card-header a:hover {
                color: #d480aa;
                text-decoration: none;
            }
            .card .card-header .info {
                font-style:normal;
                color:#a3a3a3;
                font-size:14px;
                min-width: 0;
                text-overflow: ellipsis;
                overflow: hidden;
                white-space: nowrap;
            }
        </style>
        <div class="links-content">
            <div class="link-navigation">

                {% for link in theme.mylinks %}

                    <div class="card">
                        <img class="ava" src="{{ link.avatar }}"/>
                        <div class="card-header">
                        <div><a href="{{ link.site }}" target="_blank">@ {{ link.nickname }}</a></div>
                        <div class="info">{{ link.info }}</div>
                        </div>
                    </div>

                {% endfor %}

            </div>
            {{ page.content }}
            </div>
        </div>

  {##########################}
  {### END LINKS BLOCK ###}
  {##########################}
{% endblock %}

```

</p>
</details>

<br>

> 修改`page.swig`

```
{% block title %}
    {%- set page_title_suffix = ' | ' + title %}

    {%- if page.type === 'categories' and not page.title %}
      {{- __('title.category') + page_title_suffix }}
    {%- elif page.type === 'tags' and not page.title %}
      {{- __('title.tag') + page_title_suffix }}
    <!-- 友情链接-->
    {% elif page.type === 'links' and not page.title %}
      {{ __('title.links') + page_title_suffix }}
    <!-- 友情链接结束-->
    {%- elif page.type === 'schedule' and not page.title %}
      {{- __('title.schedule') + page_title_suffix }}
    {%- else %}
      {{- page.title + page_title_suffix }}
    {%- endif %}
  {% endblock %}
```

> 引入links.swig

接着在`/themes/next/layout/page.swig`中，引入刚才新建的`links.swig`:

```
{% elif page.type === 'categories' %}
          <div class="category-all-page">
            <div class="category-all-title">
              {{ _p('counter.categories', site.categories.length) }}
            </div>
            <div class="category-all">
              {{ list_categories() }}
            </div>
          </div>
        <!-- 友情链接-->
        {% elif page.type === 'links' %}
          {% include 'links.swig' %}
        <!-- 友情链接-->
```

> 配置友链


在主题配置文件中配置友链末尾处新增mylinks,如下：

`/source/_data/next.yml`
```
mylinks:

- nickname: Wales Mei  #友链名称
  avatar: https://walesexcitedmei.github.io/images/avatar.jpg  #友链头像
  site: https://walesexcitedmei.github.io  #友链地址
  info: 一个正在奋斗的 OIer  #友链说明
```

#### 文章置顶

> 在站点目录下安装为如下插件：

```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

> 修改文章模板

修改站点目录下`scaffolds/post.md`

```
top : false
```
这样以来以后生成的 `post` 文件都自己带有 `top` 属性。需要置顶的文章把 `false` 改为 `true` 就行了.

> 设置置顶标志

打开：主目录`/themes/next/layout/_macro` 目录下面的 `post.swig` 文件，定位到 `<div class="post-meta">` 插入以下代码：
    
```
<div class="post-meta">
        {% if post.top %}
            <i class="fa fa-thumb-tack"></i>
            <font color=7D26CD>置顶</font>
            <span class="post-meta-divider">|</span>
          {% endif %}
```
<hr>

