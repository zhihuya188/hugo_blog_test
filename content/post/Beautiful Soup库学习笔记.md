---
title: Beautiful Soup库学习笔记
tags:
  - Beautiful Soup
categories:
  - 专业
  - python
keywords: Beautiful Soup库学习笔记
abbrlink: 246bc7f2
date: 2019-08-05 22:50:23
---
# 说明
Beautiful Soup库学习笔记
<!--more-->
## Beautiful Soup库

安装：`pip install biautifulsoup4`

演示HTML页面地址：http://python123.io/ws/demo.html


```
#!/usr/bin/env python
# coding=utf-8
"""
AUthor: yangyang
Blog: yangyang188.coding.me


date: 2019/8/4 21:14

"""
import requests
from bs4 import BeautifulSoup
r = requests.get('https://python123.io/ws/demo.html')
# print(r.text)
demo = r.text
# html 解析
soup = BeautifulSoup(demo, 'html.parser')
# 修饰html
print(soup.prettify())

```

基本元素

Attributes(/'ætrə,bjʊt/)属性

### Beautiful Soup库解析器

解释器|使用方法|条件
---|---|---
bs4的HTML解析器 | BeautifulSoup(mk,'html.parser') |安装bs4库
lxml的HTML解析器    | BeautifulSoup(mk,'lxml') |pip install lxml
lxml的XML解析器     |BeautifulSoup(mk,'xml') |pip install lxml
html5lib的解析器    |BeautifulSoup(mk,'html5lib')| pip install html5lib

BeautifulSoup类的基本元素

基本元素 | 说明
---|---
Tag |标签，最基本的信息组织单元，分别用<>和</>标明开头和结尾
Name |标签的名字，\<p>…\</p>的名字是'p'，格式：<tag>.name
Attributes| 标签的属性，字典形式组织，格式：<tag>.attrs
NavigableString| 标签内非属性字符串，<>…</>中字符串，格式：<tag>.string
Comment |标签内字符串的注释部分，一种特殊的Comment类型

### 基于bs4库的HTML库的内容遍历方法

标签树的下行遍历

BeautifulSoup类型是标签树的根节点

属性|说明
---|---
.contents |子节点的列表，将<tag>所有儿子节点存入列表
.children |子节点的迭代类型，与.contents类似，用于循环遍历儿子节点
.descendants| 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历

标签树的上行遍历

属性|说明
---|---
.parent| 节点的父亲标签
.parents| 节点先辈标签的迭代类型，用于循环遍历先辈节点

标签树的平行遍历

属性|说明
---|---
.next_sibling |返回按照HTML文本顺序的下一个平行节点标签
.previous_sibling| 返回按照HTML文本顺序的上一个平行节点标签
.next_siblings |迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
.previous_siblings| 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签

平行遍历发生在同一个父节点下的各节点间

![遍历](https://wx1.sinaimg.cn/large/d818c3ccly1g5p5xhb0bej21960lf0wt.jpg)

### 基于bs4库的HTML 格式输出

bs4库的prettify()方法
```python3


#!/usr/bin/env python
# coding=utf-8
"""
AUthor: yangyang
Blog: yangyang188.coding.me


date: 2019/8/4 21:14

"""
import requests
from bs4 import BeautifulSoup
r = requests.get('https://python123.io/ws/demo.html')
# print(r.text)
demo = r.text
# html 解析
soup = BeautifulSoup(demo, 'html.parser')
# 修饰html
print(soup.prettify())
```
### bs4库的编码

bs4库将任何HTML输入都变成utf‐8编码
Python 3.x默认支持编码是utf‐8,解析无障碍

### 小结

Beautiful Soup库入门

```
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p>data</p>', 'html.parser')
```

bs4库的基本元素

Tag\
Name\
Attributes\
NavigableString\
Comment 

bs4库的遍历功能

.contents \
.children \
.descendants\

.parent\
.parents\

.next_sibling \
.previous_sibling\
.next_siblings\
.previous_siblings\