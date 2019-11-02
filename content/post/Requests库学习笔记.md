---
title: Requests库学习笔记
tags:
  - Requests
categories:
  - 专业
  - python
keywords: Requests库学习笔记
abbrlink: 4d89c206
date: 2019-08-011 0:21:23
---
# 说明
Requests库学习笔记
<!--more-->

> 学习教程：Python网络爬虫与信息提取 
> 授课老师：嵩天 
> 官方网站：https://python123.io 
> 教程链接：https://python123.io/index/courses/804


## Requests库
中文文档：https://2.python-requests.org//zh_CN/latest/user/quickstart.html


- 安装：`pip install requests`
- 验证安装：
```python3
>>> import requests
>>> r =requests.get("http://www.baidu.com")
>>> print(r.status_code)
200
>>> r.text
<!DOCTYPE html>\r\n<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8>...
```
- Requests库的7个主要方法

方法	|	说明
---|---
requests.request()	|	构造一个请求，支撑以下各方法的基础方法
requests.get() 	|	获取HTML网页的主要方法，对应于HTTP的GET
requests.head()	| 	获取HTML网页头信息的方法，对应于HTTP的HEAD
requests.post() 	|	向HTML网页提交POST请求的方法，对应于HTTP的POST
requests.put() 	|	向HTML网页提交PUT请求的方法，对应于HTTP的PUT
requests.patch() 	|	向HTML网页提交局部修改请求，对应于HTTP的PATCH
requests.delete() 	|	向HTML页面提交删除请求，对应于HTTP的DELETE


### Requests库的request()方法

`requests.request(method, url, **kwargs)`
- method : 请求方式，对应get/put/post等7种
- url : 拟获取页面的url链接
- **kwargs: 控制访问的参数，共13个

method : 请求方式

```
r = requests.request('GET', url, **kwargs)
r = requests.request('HEAD', url, **kwargs)
r = requests.request('POST', url, **kwargs)
r = requests.request('PUT', url, **kwargs)
r = requests.request('PATCH', url, **kwargs)
r = requests.request('delete', url, **kwargs)
r = requests.request('OPTIONS', url, **kwargs)
```

**kwargs: 控制访问的参数，均为可选项

参数| 说明
---|--
params :| 字典或字节序列，作为参数增加到url中
data    : |字典、字节序列或文件对象，作为Request的内容	
json : |JSON格式的数据，作为Request的内容
headers :| 字典，HTTP定制头
cookies : |字典或CookieJar，Request中的cookie
auth : |元组，支持HTTP认证功能
files   : |字典类型，传输文件
timeout :| 设定超时时间，秒为单位
proxies : |字典类型，设定访问代理服务器，可以增加登录认证
allow_redirects :| True/False，默认为True，重定向开关
stream  : |True/False，默认为True，获取内容立即下载开关
verify  : |True/False，默认为True，认证SSL证书开关
cert    : |本地SSL证书路径


### Requests库的get()方法

```python3
# 构造一个向服务器请求资源的Request对象。
# 返回一个包含服务器资源的Response对象
r = requests.get(url)
```

`requests.get(url,params=None, **kwargs)`
- url : 拟获取页面的url链接
- params : url中的额外参数，字典或字节流格式，可选
- **kwargs: 12个控制访问的参数

Response对象的属性

属性 | 说明
---|---
r.status_code  | HTTP请求的返回状态，200表示连接成功，404表示失败
r.text  | HTTP响应内容的字符串形式，即，url对应的页面内容
r.encoding |  从HTTP header中猜测的响应内容编码方式
r.apparent_encoding |  从内容中分析出的响应内容编码方式（备选编码方式）
r.content | HTTP响应内容的二进制形式

### Requests库的异常

异常|说明
---|---
requests.ConnectionError| 网络连接错误异常，如DNS查询失败、拒绝连接等
requests.HTTPError HTTP|错误异常
requests.URLRequired URL|缺失异常
requests.TooManyRedirects |超过最大重定向次数，产生重定向异常
requests.ConnectTimeout |连接远程服务器超时异常
requests.Timeout |请求URL超时，产生超时异常

Response的异常

异常|说明
---|---
r.raise_for_status()|如果不是200，产生异常 requests.HTTPError

### 爬取网页的通用代码框架

```python3
import requests
from pprint import pprint


def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()  # 如果状态码不是200，引发HTTPError异常
        r.encoding = r.apparent_encoding
        return r.text
    except BaseException:
        return "产生异常"


if __name__ == "__main__":
    url = 'http://www.baidu.com'
    pprint(getHTMLText(url))

```
实例1：京东商品页面的爬取
```
import requests


def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()  # 如果状态码不是200，引发HTTPError异常
        r.encoding = r.apparent_encoding
        print(r.text[:1000])
    except:
        print("产生异常")


if __name__ == '__main__':
    url = 'https://item.jd.com/2967929.html'
    getHTMLText(url)

```

实例2：亚马逊商品页面的爬取

```
import requests


def getHTMLText(url):
    try:
        kv = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'}
        r = requests.get(url, headers=kv, timeout=30)
        r.raise_for_status()  # 如果状态码不是200，引发HTTPError异常
        r.encoding = r.apparent_encoding
        print(r.text[1000:2000])
    except:
        print("产生异常")


if __name__ == '__main__':
    url = 'https://www.amazon.cn/gp/product/B01M8L5Z3Y'
    getHTMLText(url)

```

实例3：百度/360搜索关键字提交

```
import requests


def getHTMLText(url):
    try:
        kv = {'wd': keyword}
        r = requests.get(url, params=kv)
        print(r.request.url)
        r.raise_for_status()
        print(len(r.text))
    except:
        print('爬取失败')


if __name__ == '__main__':
    keyword = 'Python'
    url = 'http://www.baidu.com/s'
    getHTMLText(url)

```

```
import requests


def getHTMLText(url):
    try:
        kv = {'q': keyword}
        r = requests.get(url, params=kv)
        print(r.request.url)
        r.raise_for_status()
        print(len(r.text))
    except:
        print('爬取失败')


if __name__ == '__main__':
    keyword = 'Python'
    url = 'http://www.so.com/s'
    getHTMLText(url)

```

实例4：网络图片的爬取和存储

```
import requests
import os


def getDownloadImages(url):
    try:
        if not os.path.exists(root):
            os.mkdir(root)
        if not os.path.exists(path):
            r = requests.get(url)
            with open(path, 'wb') as f:
                f.write(r.content)
                f.close()
                print('文件保存成功')
        else:
            print('文件已经存在')
    except:
        print('爬取失败')


if __name__ == '__main__':
    url = 'http://image.nationalgeographic.com.cn/2017/0211/20170211061910157.jpg'
    root = ".//images//"
    path = root + url.split('/')[-1]
    getDownloadImages(url)

```

实例5：IP地址归属地的自动查询

```
import requests

url = 'http://m.ip138.com/ip.asp?ip='


def getHTMLText(url):
    try:
        r = requests.get(url + '182.32.20.67')
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.text[-500:])
    except BaseException:
        print("爬取失败")


if __name__ == '__main__':
    url = 'http://m.ip138.com/ip.asp?ip='
    getHTMLText(url)

```





