---
layout: post
title: "json 在 Python 爬虫的应用"
date: 2017-03-14 00:21:12
categories:
- Python
- Python基础
tags: 
- Python
- Python基础
- json
description: "本文主要讨论 Python 中如何读写 json 文件"
---



本文是 xchaoinfo 在写 Python 爬虫，使用 json 的一个总结。

### 数据存储

json 这种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成，同时可以存储丰富的数据结构。
xchaoinfo 比较喜欢这样存储数据,
每行一个 json 数据，这样在大文件读写的时候比较方便，我存储和读取过几个 G 的十几个参数的微博用户信息的数据。写入 json 的时候分多次写入，读取的时候边读取边解析 json，基本不会出现内存不足的问题

```Python
d1 = {"id": 1, "name": 'xchaoinfo'}
d2 = {"id": 2, "name": 'Python'}
ls = [d1, d2]

# 写入

with open("data.json", 'w') as fw:
    fw.write("\n".join([json.dumps(l) for l in ls]))
```

```Python
# 读取

with open("data.json") as fr:
    for f in fr:
        js = json.loads(f)
        print(js)

```



### 网络请求

在爬虫开发过程中，构造 GET 或者 POST 请求获取数据是比较常用的技巧。
这里介绍三个特例

```Python
# post json 数据
params = {
    "page": "1",
    "user": "xchaoinfo"
}
html = requests.post(url, data=json.dumps(params))

# 需要对中文编码两次
from urllib.parse import quote_plus
import requests
params = {
    "page": "1",
    "code": quote_plus("中国")
}
# 请求的时候 requests 自动 URL 编码一次
html = requests.get(url, params=params)

# 需要对中文进行 encode 为 GBK
# 一般出现在建站时间比较长的网站
params = {
    "page": "1",
    "code": "中国".encode("gbk")
}
# 请求的时候 requests 自动 URL 编码一次
html = requests.get(url, params=params)

```
xchaoinfo 通过读取 javascript 源码才恍然大悟的，从此走上了在 Python 开发中阅读 javascript 源码的不归路。

注：例子 2 和 3, 有些乱入了，其实 GET POST 的Python 的字典数据结构

### 数据解析   

1. 不知道是否遇到这种类型的类似 json 的数据格式
`jsonp_1492356362(json_format_data)`
可以使用字符串的 `replace` 方法替换后解析，如果你觉得解析变动的的时间戳比较麻烦，可以用 `re.sub` 处理后解析

2. 如果你最近爬取过知乎的数据，就会发现数据被放到 一个 div 的 data=xxx 中了。下面提供一个解析知乎用户页的方法

```Python
soup = BeautifulSoup(html, 'lxml')
data = soup.find("div", id="data")['data-state']
js = json.loads(data)

```
同时这样有一个关于微博解析的方式，和知乎的解析方式差不多
https://www.zhihu.com/question/49695115/answer/118352288

这是 xchaoinfo 一些简单的总结，欢迎提出更多关于 Python 爬虫开发中 json 的应用技巧


首发于微信公众号 xchaoinfo
