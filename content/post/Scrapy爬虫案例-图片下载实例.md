---
title: Scrapy爬虫案例-“图片下载”实例
tags:
  - scrapy
  - spider
categories:
  - 专业
  - python
  - scrapy
keywords: Scrapy爬虫案例-“图片下载”实例
abbrlink: 9a596e53
date: 2019-08-11 15:50:23
---
# 说明
Scrapy爬虫案例-Scrapy爬虫案例-“图片下载”实例
<!--more-->

> 参考：静觅博客 \
> 作者：崔庆才 \
> 网站：https://cuiqingcai.com/ \
> 博客：https://cuiqingcai.com/4421.html

## “图片下载”实例

### 功能描述

目标：获取页面的图片信息，提取其中的图片名称和链接

技术路线：Scrapy-CrawlSpider-xpath-re

目标网站：https://www.mm131.net/

### 定向爬虫的可行性

```
# https://www.mm131.net/robots.txt
User-agent: * 
Disallow: /plus/
Disallow: /data/
Disallow: /img/
Disallow: /images/
Disallow: /include/
Disallow: /templets/
Disallow: /search/
Disallow: /index/
Disallow: /404.html
Disallow: /500.html
Disallow: /index2.html
Disallow: /index3.html
Sitemap:https://www.mm131.net/sitemap.xml
```

> 请注意：这个例子仅探讨技术实现，请不要不加限制的爬取该网站

### 程序的结构设计

步骤1：寻找网址规则编写匹配规则，循环获取页面

步骤2：对于每个页面，提取图片名称和图片地址

步骤3：下载图片

### 实例编写

#### 创建项目
```python3
# 创建项目mm131scrapy
scrapy startproject mm131scrapy
# cd mm131scrapy 创建爬虫spider
scrapy genspider spider mm131.net
```
####  创建启动程序`run.py`
```python3
# run.py
from scrapy.cmdline import execute
execute(['scrapy', 'crawl', 'mm131'])
```
#### 安装三方库(随机headers)：

`scrapy-fake-useragent`

官方网址: https://pypi.org/project/scrapy-fake-useragent/

安装 `pip install scrapy-fake-useragent`

```python3
# 在setting.py开启
DOWNLOADER_MIDDLEWARES = {
    'scrapy_fake_useragent.middleware.RandomUserAgentMiddleware': 400, # 开启
}
```
#### 目录结构
```
# 目录结构
.
└── mm131scrapy
    ├── mm131scrapy
    │   ├── __init__.py
    │   ├── items.py
    │   ├── middlewares.py
    │   ├── pipelines.py
    │   ├── __pycache__
    │   │   ├── __init__.cpython-36.pyc
    │   │   └── settings.cpython-36.pyc
    │   ├── settings.py
    │   └── spiders
    │       ├── __init__.py
    │       ├── __pycache__
    │       │   └── __init__.cpython-36.pyc
    │       └── spider.py
    ├── run.py
    └── scrapy.cfg

```
#### 编写`itesms.py`

```python3
<!--# 编写itesms.py增加三个字段-->

import scrapy


class MzituScrapyItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    name = scrapy.Field()
    image_urls = scrapy.Field()
    url = scrapy.Field()
    pass
```
#### 编写:`spider.py`
```python3
<!--#编写:spider.py-->


#!/usr/bin/env python
# coding=utf-8
"""
项目描述：爬取mm131.net上的图片并下载
运行环境：win7 64 + python3.7 +scrapy1.7.3
运行方式：进入mm131目录（scrapy.cfg所在目录)输入：

python run.py

代码详解：https://yangyang188.coding.me/
文件: spider.py
创建时间：2019/8/11 8:56
创建者：yangyang
Blog: yangyang188.coding.me
"""

from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule
from ..items import Mm131ScrapyItem


class SpiderSpider(CrawlSpider):
    name = 'mm131'
    allowed_domains = ['mm131.net']
    start_urls = ['http://mm131.net/']

    rules = (
        Rule(
            LinkExtractor(
                allow=r'(http|https)://www.mm131.net/qingchun/\d+.html'),
            callback='parse_item',
            follow=True),
        Rule(
            LinkExtractor(
                allow=r'(http|https)://www.mm131.net/qingchun/\d+_\d+.html'),
            callback='parse_item',
            follow=True),
    )

    def parse_item(self, response):

        item = Mm131ScrapyItem()
        item['name'] = response.xpath(
            "//div[@class='content']/h5/text()").extract_first()
        item['url'] = response.url
        item['image_urls'] = response.xpath(
            "//div[@class='content-pic']/a/img/@src").extract()

        yield item

<!--编写结束-->
```
---

#### 编写`pipelines.py`
```python3
<!--编写pipelines.py-->
<!--重写一下ImagesPipeline中的file_path方法！-->


# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html
from scrapy import Request
from scrapy.pipelines.images import ImagesPipeline
from scrapy.exceptions import DropItem
import re


class Mm131ScrapyPipeline(ImagesPipeline):

    def file_path(self, request, response=None, info=None):
        """
        :param request: 每一个图片下载管道请求
        :param response:
        :param info:
        :param strip :清洗Windows系统的文件夹非法字符，避免无法创建目录
        :return: 每套图的分类目录
        """
        item = request.meta['item']
        folder = item['name']
        folder_strip = strip(folder)
        image_guid = request.url.split('/')[-1]
        filename = u'full/{0}/{1}'.format(folder_strip, image_guid)
        return filename

    def get_media_requests(self, item, info):
        """
        :param item: spider.py中返回的item
        :param info:
        :return:
        """
        for img_url in item['image_urls']:
            referer = item['url']
            yield Request(img_url, meta={'item': item,
                                         'referer': referer})

    def item_completed(self, results, item, info):
        image_paths = [x['path'] for ok, x in results if ok]
        if not image_paths:
            raise DropItem("Item contains no images")
        return item


def strip(path):
    """
    :param path: 需要清洗的文件夹名字
    :return: 清洗掉Windows系统非法文件夹名字的字符串
    """
    path = re.sub(r'[？\\*|“<>:/|\(\d{1,2}\)]', '', str(path))
    return path


if __name__ == "__main__":
    a = r'我是一个？\*|“<>:/(10)错误的字符串'
    print(strip(a))

<!--重写结束-->
!--编写pipelines.py结束-->
```
#### 编写：`middlewares.py`
```python3
<!--编写：middlewares.py-->
<!--写一个中间件来处理图片下载的防盗链：-->

class Mm131Images(object):
 
    def process_request(self, request, spider):
        '''设置headers和切换请求头
        :param request: 请求体
        :param spider: spider对象
        :return: None
        '''
        referer = request.meta.get('referer', None)
        if referer:
            request.headers['referer'] = referer
```
#### 编写`settings.py`
```python3
<!--编写settings.py-->
<!--最后一步设置ImagesPipeline的存储目录！-->

IMAGES_STORE = 'F:\mm131scrapy\\'


<!--启用：ITEM_PIPELINES-->

ITEM_PIPELINES = {
    'mm131scrapy.pipelines.Mm131ScrapyPipeline': 300,
}


<!--启用：DOWNLOADER_MIDDLEWARES-->

DOWNLOADER_MIDDLEWARES = {
    # 'mm131scrapy.middlewares.Mm131ScrapyDownloaderMiddleware': 543,
    'mm131scrapy.middlewares.Mm131Images': 544,
    'scrapy_fake_useragent.middleware.RandomUserAgentMiddleware': 400,  # 随机头

}


# 设置图片实效性：

# 图像管道避免下载最近已经下载的图片。使用 FILES_EXPIRES (或 IMAGES_EXPIRES) 设# # 置可以调整失效期限，可以用天数来指定:

在settings.py中写入以下配置。
# 30 days of delay for images expiration
IMAGES_EXPIRES = 30
```
#### 看效果

![效果](http://wx2.sinaimg.cn/mw690/d818c3ccly1g5vs87j6wej21bx0h1ar6.jpg)



