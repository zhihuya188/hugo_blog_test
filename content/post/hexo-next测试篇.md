---
title: hexo-next-blog测试篇
tags:
  - hexo
  - next
categories:
  - blog
abbrlink: 8ad88694
date: 2019-10-27 19:21:00
---
{% note default %}
hexo-next-blog美化
{% endnote %}

<!-- more -->


### 添加网站访问量Bug


参考

https://xian6ge.cn/posts/2da0ce2e/
https://chrischen0405.github.io/2018/09/11/post20180911/
http://ibruce.info/2015/04/04/busuanzi/


### 设置 hexo 永久链接

安装插件

`npm install hexo-abbrlink --save`

修改站点配置文件

```
# permalink: :year/:month/:day/:title/
# permalink_defaults:
permalink: posts/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

### seo

参考

http://www.admintony.com/Hexo做SEO优化遇到的坑.html

### 高颜值

http://walesexcitedmei.github.io/2018/08/30/HEXO-NexT-主题提高博客颜值/


### hexo next 怎么添加文章点击图片放大功能

参考
https://www.jianshu.com/p/2d72da2e2f04
![](https://tianma-bucket.oss-cn-shanghai.aliyuncs.com/images/Rem_03.jpg@!w300)

