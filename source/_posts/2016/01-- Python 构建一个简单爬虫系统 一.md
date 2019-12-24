---
layout: post
title: Python 构建一个简单爬虫系统 (一)
date: 2016-09-30 17:37:28
categories:
- Python爬虫
tags:
- Python
- 爬虫
---

本文通过 requests beautifulsoup re 等 Python的模块, 尝试构建一个微型的爬虫系统，本文采用 Python 3 的版本, 本文是第一篇介绍一个网页的简单抓取和解析

<!-- more -->


## 目录
 - 0x00·[简介]
 - 0x01·[简单抓取]
 - 0x02·[解析网页]

## 0x00 简介
1. requests 作为优秀的 Python 第三方的模块，当邂逅了之后，你就会向 urllib say goodbye 了, 本文会介绍一些 requests 的具体的使用方法。
2. beautifulsoup 作为常用的解析网页的第三方的模块，通过 "lxml" 作为解析器，可以达到速度和兼容性的平衡.
当然 xpath 也是一个很好的选择。re 还是无处不在的显示其强大的生命力。

3. 在了解 requests Beautifulsoup4 的用法的基础上，一步步教你怎么构建一个简单的小型爬虫系统，

本文是对 requests Beautifulsoup4 的一个简单的综合应用，关于这两 Python 优秀的第三方模块的使用方法，可以看本文最后我给的参考文档

## 0x01 简单抓取
抓取一个网页并且把内容保存到本地


```python
import requests
def get_html(url):
    html = requests.get(url)
    with open('content.html', 'wb') as fw:
        fw.write(html.content)
        fw.close()
if __name__ == '__main__':
    url = "http://example.com"
    get_html(url)

```



可是你会发现，大多数情况，这个代码是没法工作的，网站会对 request header 做一些限制，

```python
def get_html(url):
    headers = {
        'User-Agent': "Mozilla/5.0 (Windows NT 6.2; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.87 Safari/537.36"
    }
    html = requests.get(url, headers=headers)
    with open('content.html', 'wb') as fw:
        fw.write(html.content)
        fw.close()

if __name__ == '__main__':
    url = "http://example.com"
    get_html(url)
```


主要是后面的方法，我们假装好像浏览器的样子，
还有些网站需要检测 Request headers 里面的 Refer host 等字段，我们会在后面讲到
但是 0x01-1 的代码，还是不够健壮，因为，我们不知道请求是否成功了，如果最后保存到本地的，是一堆 404 的页面，显然是一件很让人沮丧的事情，我们在把代码改下，加上一个请求是否成功的代码。
> http 请求状态有很多种，大多数情况下 200 是我们理想中的状态，

```python
def get_data(url):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 6.2; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.87 Safari/537.36"
    }
    html = requests.get(url, headers=headers)
    if html.status_code == 200:
        print(url, '@ok200', str(time.ctime()))
        with open('content.html', 'wb') as fw:
            fw.write(html.content)
            fw.close()
    else:
        print(url, 'wrong', str(time.ctime()))

if __name__ == '__main__':
    url = "http://example.com"
    get_data(url)

```

我们用 requests 的一个 status_code 方法来判断是否访问成功，然后使用 print 一下，相当于打印日志了，当我们做大规模抓取的时候，print 显然不是一个很好的选择，Python 的 logging 模块能够更好的胜任这项工作，为了方便起见，我们暂时使用简单直接粗暴的 print 来做代替 logging 的工作

由于网络原因，可能会出现访问超时的问题，为了让代码更健壮，我们可以设置 timeout, 同时，如果确定网页不需要重定向， 可以设置 allow_redirects=False

```python
def get_data(url):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 6.2; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.87 Safari/537.36"
    }
    try:
        html = requests.get(url, headers=headers, allow_redirects=Fasle, timeout=3)
        if html.status_code == 200:
            print(url, '@ok200', str(time.ctime()))
            with open('content.html', 'wb') as fw:
                fw.write(html.content)
                fw.close()
        else:
            print(url, 'wrong', str(time.ctime()))
    except Exception as e:
        print(url, e, str(time.ctime()))

if __name__ == '__main__':
    url = "http://example.com"
    get_data(url)

```

至此，我们完成了一个网页的抓取，并且做了相关的异常处理。

## 0x02 解析网页
本节说明下，通过 re 和 beautifulsoup 的组合来解析网页

####1. re (正则表达式) 提取网页
经常用的是贪婪匹配和非贪婪匹配，还有就是单行匹配还是全文匹配
最常用的方法是 re.findall 还有括号的使用
例如

```python
import re
text = """Sxchaoinfo@EgithubE
    xchaoinfo@wechat
    xchaoinfo@zhihuE"""
```

- 模式一

```python
# 加上 re.S 代表是全文匹配
pa = r'S.*?E'
re.findall(pa, text) -> 返回的是["Sxchaoinfo@E",]
re.findall(pa, text, re.S) -> 返回的是["Sxchaoinfo@E",]
```

- 模式二

```python
pa = r'S.*E'
re.findall(pa, text) -> 返回的是['Sxchaoinfo@EgithubE', ]

re.findall(pa, text, re.S) -> 返回的是
['Sxchaoinfo@EgithubE\n    xchaoinfo@wechat\n    xchaoinfo@zhihuE', ]
```


- 模式三

```python
pa = r'S(.*?)E'
re.findall(pa, text) -> 返回的是['xchaoinfo@',]
```

更佳详细的 re(正则表达式) 的使用方法，请参考文档获取其他网站的教程


####2. beautifulsoup 提取网页

Beautifulsoup 解析网页的时候推荐使用 lxml作为解析器，虽然 lxml 的安装可能会花费您一点时间，其他解析器的选择，可以参考官方的文档
最常用的方法是

```python
from bs4 import BeautifulSoup

# 获取页面所有的 url 链接
soup = BeautifulSoup(html_content, 'lxml')
ahref = soup.findAll('a')
for a in ahref:
    print(a['href'])
```



## 参考文档
- [Requests: HTTP for Humans](http://www.python-requests.org/en/master/)
- [requests 中文文档](http://cn.python-requests.org/zh_CN/latest/)
- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [BeautifulSoup 中文文档](http://beautifulsoup.readthedocs.io/zh_CN/latest/)