---
title: 'HEXO-NexT-V7-Gemini写作技巧篇'
mathjax: true
copyright: true
comment: true
tags:
  - hexo
  - blog
  - next
categories: [专业, 前端, hexo]
abbrlink: e198fb5
top : true
date: 2019-10-28 10:54:26
---
![hexo-themes-NexT](https://tvax1.sinaimg.cn/large/d818c3ccly1g8ghdak6tfj20k80a4wei.jpg)
{% note default %}
NexT 主题优化
本文使用的版本：Node.js: v12.6.0，npm: 6.9.0， hexo: 4.0.0，NexT: v7.4.2
{% endnote %}

<!-- more -->


{% note default %}
参考：
[NexT 官方使用文档](http://theme-next.iissnan.com/)
[HEXO-NexT-主题提高博客颜值](http://walesexcitedmei.github.io/2018/08/30/HEXO-NexT-主题提高博客颜值/)

{% endnote %}

## 文章的模板文件

文章的模板文件修改`blog/scaffolds/post.md`

<details><summary>点击显/隐内容</summary>
<p>

```  
---
title: {{ title }}
date: {{ date }}
tags:
categories:
mathjax: true
copyright: true
comment: true
---

{% note default %}
{% endnote %}

<!-- more -->

---
```

</p>
</details>

<br>
{% note default %}
请参阅 [Hexo 的分类与标签文档](https://hexo.io/zh-cn/docs/front-matter.html#%E5%88%86%E7%B1%BB%E5%92%8C%E6%A0%87%E7%AD%BE)，了解如何为文章添加标签或者分类。
{% endnote %}

{% tabs 选项卡 %}
<!-- tab 新建页面-->
在终端窗口下，定位到 Hexo 站点目录下。使用 hexo new page 新建一个页面，命名为 categories ：

```
$ cd your-hexo-site
$ hexo new page categories
```
<!-- endtab -->
 <!-- tab 设置页面类型-->
编辑刚新建的页面，将页面的 type 设置为 categories ，主题将自动为这个页面显示分类。页面内容如下：

```
---
title: 分类
date: 2014-12-22 12:39:04
type: "categories"
---
```
<!-- endtab -->
<!-- tab 修改菜单-->
在菜单中添加链接。编辑 主题配置文件 ， 添加 categories 到 menu 中，如下:

```
menu:
  home: /
  archives: /archives
  categories: /categories
```
<!-- endtab -->
{% endtabs %}


## Markdown 技巧与内置样式
### 分隔线和空行
```
这是文字
<hr />
上面是分隔线
<br />
上面是空行
```

### 居中和右对齐
```
<!-- 居中 -->
<center>内容</center>
<!-- 右对齐 -->
<div style="text-align:right">内容</div>
```

### 字体大小和颜色
```
<font color="#187892" size="number">内容</font>
<!-- 详细请查看 http://www.w3school.com.cn/tags/tag_font.asp -->
```

### 文本居中的引用
```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="blockquote-center" 是必须的 -->
<blockquote class="blockquote-center">blah blah blah</blockquote>

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% centerquote %}
blah blah blah
{% endcenterquote %}

<!-- 标签别名 -->
{% cq %} blah blah blah {% endcq %}
```

示例：
```
<!-- 标签别名 -->
{% cq %} hexo博客 {% endcq %}

{% note default %}
<p><br /></p>
{% cq %} hexo博客 <p>积跬步以致千里，积怠情以致深渊</p>
<br />
{% endcq %}
<p>只比你努力一点的人、其实已经甩你很远。</p>
{% endnote %}
```

效果如下：

<details><summary>点击显/隐内容</summary>
<p>

<!-- 标签别名 -->
{% cq %} hexo博客 {% endcq %}

{% note default %}
<p><br /></p>
{% cq %} hexo博客 
<p>积跬步以致千里，积怠情以致深渊</p>
{% endcq %}
<p>只比你努力一点的人、其实已经甩你很远。</p>
{% endnote %}

</p>
</details>

### Todo list
```
<ul>
<li><i class="fa fa-check-square"></i> 已完成</li>
<li><i class="fa fa-square"></i> 未完成</li>
</ul>
```

### Note 嵌套 Todo list
```
<!-- 一共有两种写法，效果看下面 -->
<div class="note primary"><br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-square"></i> 未完成<br>  
    <i class="fa fa-square"></i> 未完成<br>  
    <i class="fa fa-square"></i> 未完成<br>
</div>
<div class="note primary">
  <p>
    <i class="fa fa-check-square"></i> 已完成
    <i class="fa fa-check-square"></i> 已完成
    <i class="fa fa-check-square"></i> 已完成
    <i class="fa fa-check-square"></i> 已完成
    <i class="fa fa-check-square"></i> 已完成
    <i class="fa fa-square"></i> 未完成
    <i class="fa fa-square"></i> 未完成
    <i class="fa fa-square"></i> 未完成
  </p>
</div>
```
效果如下：

<div class="note primary"><br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-check-square"></i> 已完成<br>  
    <i class="fa fa-square"></i> 未完成<br>  
    <i class="fa fa-square"></i> 未完成<br>  
    <i class="fa fa-square"></i> 未完成<br>
</div>

### Note 标签

在主题配置文件_config.yml里有一个关于这个的配置:

```
# Note tag (bs-callout).
note:
  # 风格
  style: flat
  # 要不要图标
  icons: true
  # 圆角矩形
  border_radius: 3
  light_bg_offset: 0
```

用 HTML 写就是这个样子

```
<div class="note default"><p>default</p></div>
<div class="note primary"><p>primary</p></div>
<div class="note success"><p>success</p></div>
<div class="note info"><p>info</p></div>
<div class="note warning"><p>warning</p></div>
<div class="note danger"><p>danger</p></div>
<div class="note danger no-icon"><p>danger no-icon</p></div>
```

用 swig 语法写就是这样

```
{% note [class] %}
Any content (support inline tags too).
{% endnote %}
[class] : default | primary | success | info | warning | danger.
          May be not defined.
```
里面的三种风格长啥样？开启图标长啥样？可以查看[这个页面](https://github.com/iissnan/hexo-theme-next/pull/1697)，更多的介绍也在这个页面，请自行查看

最后的几种效果：
{% tabs 选项卡b %}
<!-- tab default-->
```
{% note default %}
default
{% endnote %}
```
<div class="note default"><p>default</p></div>
<!-- endtab -->
<!-- tab primary-->
```
{% note primary %}
primary
{% endnote %}
```
<div class="note primary"><p>primary</p></div>
<!-- endtab -->
<!-- tab success-->
```
{% note success %}
success
{% endnote %}
```
<div class="note success"><p>success</p></div>
<!-- endtab -->
<!-- tab info-->
```
{% note info %}
info
{% endnote %}
```
<div class="note info"><p>info</p></div>
<!-- endtab -->
<!-- tab warning-->
```
{% note warning %}
warning
{% endnote %}
```
<div class="note warning"><p>warning</p></div>
<!-- endtab -->
<!-- tab danger-->
```
{% note danger %}
danger
{% endnote %}
```
<div class="note danger"><p>danger</p></div>
<!-- endtab -->
<!-- tab danger-no-icon-->
```
{% note danger no-icon %}
danger no-icon
{% endnote %}
```
<div class="note danger no-icon"><p>danger no-icon</p></div>
<!-- endtab -->
{% endtabs %}


### Label 标签

`label` 标签不建议加在段首, 首先可以在主题配置文件中有配置：
```
# Label tag.
label: true
```

然后效果如下（@前面的是label的名字，后面的是要显示的文字）
{% tabs 选项卡c %}
<!-- tab default-->
{% label default@default %}
<!-- endtab -->
<!-- tab primary-->
{% label primary@primary %}
<!-- endtab -->
<!-- tab success-->
{% label success@success %}
<!-- endtab -->
<!-- tab info-->
{% label info@info %}
<!-- endtab -->
<!-- tab warning-->
{% label warning@warning %}
<!-- endtab -->
<!-- tab danger-->
{% label danger@danger %}
<!-- endtab -->
{% endtabs %}


### Tab 选项卡

当然也是要先配置一下：
```
# Tabs tag.
tabs:
  enable: true
  transition:
    tabs: true
    labels: true
  border_radius: 3
```

代码如下：
```
{% tabs 选项卡, 2 %}
<!-- tab -->
**这是选项卡 1** 呵呵哈哈哈哈哈哈
<!-- endtab -->
<!-- tab -->
**这是选项卡 2** 额。。。
<!-- endtab -->
<!-- tab -->
**这是选项卡 3** 哇，你找到我了！
<!-- endtab -->
{% endtabs %}

{% tabs 选项卡 %}
<!-- tab [name1]-->
**这是选项卡 1** 呵呵哈哈哈哈哈哈
<!-- endtab -->
<!-- tab [name2]-->
**这是选项卡 2** 额。。。
<!-- endtab -->
<!-- tab [name3]-->
**这是选项卡 3** 哇，你找到我了！
<!-- endtab -->
{% endtabs %}
```

效果如下：
{% tabs 选项卡a %}
<!-- tab name1-->
**这是选项卡 1** 呵呵哈哈哈哈哈哈
<!-- endtab -->
<!-- tab name2-->
**这是选项卡 2** 额。。。
<!-- endtab -->
<!-- tab name3-->
**这是选项卡 3** 哇，你找到我了！
<!-- endtab -->
{% endtabs %}

### 增加代码块折叠功能

markdown 本身便支持显示/隐藏代码块功能，语法如下：

```
<details><summary>点击显/隐内容</summary>
<p>

此处为要显示/隐的内容，亦可在此添加代码块，注意上下各空一行，

</p>
</details>
```

效果如下：
<details><summary>点击显/隐内容</summary>
<p>

此处为要显示/隐的内容，亦可在此添加代码块，注意上下各空一行，

</p>
</details>

<hr>
{% note default %}
参考文章: [@手把手教你如何搭建个人博客（四）](http://seyvoue.com/manual/ea65db2f.html)
{% endnote %}



