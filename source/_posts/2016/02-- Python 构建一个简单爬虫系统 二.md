---
title: Python 构建一个简单爬虫系统 (二)
date: 2016-10-01 21:43:37
categories:
- Python
- Python爬虫
tags:
- Python
- Python爬虫
description: "本文是用 Python 构建一个简单爬虫系统的第二篇，上一篇介绍了通过 requests 和 Beautifulsoup 来做一个网页的抓取和解析。本篇介绍通过 queue 和 threading 模块，使用队列和多线程来进行大规模数据的抓取。"
---



## 目录
 - 0x00·[背景简介]
 - 0x01·[queue、threading 基础]
 - 0x02·[多线程爬虫实例]

## 背景简介

Q1: 据说由于 GIL(全局锁) 的存在，Python 多线程很鸡肋，多线程 Python 爬虫能提高速度吗？


A1: 要很好的回答这个问题，首先要搞清楚两个概念：进程与线程，I/O 密集型与计算密集型任务。
#### 进程与线程

操作系统的设计，可以归结为三点：

1. 以多进程形式，允许多个任务同时运行；
2. 以多线程形式，允许单个任务分成不同的部分运行；
3. 提供协调机制，一方面防止进程之间和线程之间产生冲突，另一方面允许进程之间和线程之间共享资源。




#### I/O 密集型与计算密集型任务

简单的来说是频繁的进行 CPU 的计算，还是文件的读写操作。

#### 总结

对于计算密集型的任务，由于 GIL 的存在使用多进程，能提高计算效率
对于I/O 密集型的任务，例如网络爬虫，大多数时间都是等待从网上下载文件写入到本地，使用多线程能显著提高爬虫的爬取速度。


## queue、threading 基础

### queue 

queue 模块提供了一个适用于多线程编程的数据结构，可以用来在线程间安全地传递消息或者其他数据，它会为提供者处理锁定，使多个线程可以安全地处理同一个 queue 实例。

下面介绍一些简单的用法，更复杂的用法参考 queue 的文档

```python

# 创建一个 queue 实例
q = queue.Queue()

# 向 queue 添加一个元素
item = "xchaoinfo"
q.put(item)

# 从 queue 取得一个元素
data = q.get()

# 获取 queue 的元素的个数
q.qsize()

# 判断 queue 是否为空
q.empty()
```

### threading

threading 在 `_thread` 模块的基础的提供多线程处理的更高级别的接口。


下面介绍一些 threading 的简单用法

不带参数的线程创建
```python
import threading

def do_work():
    print("start working")

t = threading.Thread(target=do_work)
t.start()

```
带参数的线程创建
```python
import threading

def do_work(num):
    print("start working of %s" % num)

# args 是 tuple 类型的参数，
t = threading.Thread(target=do_work, args=(1,))
t.start()

```
其实，更常用的方法是继承 threading.Thread 类，然后重写 run 方法


## 多线程爬虫实例

假设我们已经把要爬取的 url 存放到 url.txt 文件里，并且每行存放一个 url, 每个 url 都是一张图片的地址，我们要把所有图片下载到本地，把 url 的后面部分来作为图片的文件名
```python
import requests
import threading
import queue
import os


# 从 txt 文件中获取 url, 并且处理下载后的文件名
def get_url_fn_from_txt():
    path = "img/"
    if not os.path.exists(path):
        os.mkdir(path)
    with open("url.txt") as fr:
        url_list = [f.strip() for f in fr if f.strip()]

    url_fn_list = [tuple(u, path + u.split("/")[-1]) for u in url_list]

    return url_fn_list


# 下载 img 的函数，下载成功返回 1 否则返回 0
def download_img(url, fn):
    try:
        if os.path.exists(fn):
            return 1
        img = requests.get(url, timeout=3)
        with open(fn, 'wb') as fw:
            fw.write(img.content)
            fw.close()
        return 1
    except:
        return 0


class DownloadThread(threading.Thread):
    """重写 run 函数"""

    def __init__(self, que):
        self.que = que

    def run(self):
        while not self.que.empty():
            url, fn = self.que.get()
            # for u in uid:
            if download_img(url, fn):
                pass
            else:
                self.que.put(tuple(url, fn))


def main():
    url_fn_list = get_url_fn_from_txt()
    que = queue.Queue()
    for url_fn in url_fn_list:
        que.put(url_fn)

    thread_num = 20  # 线程数
    for i in range(thread_num):
        download = DownloadThread(que)
        download.start()


if __name__ == '__main__':
    main()


```


这是比较简陋的一个实例，可以在此基础上加入更多功能。如有错误，烦请指出，不胜感激。



参考：

1. http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html (进程与线程)
2. http://blog.chinaunix.net/uid-116213-id-3086203.html (I/O 密集型与计算密集型任务)
3. queue 和 threading 的用法参考 Python 的文档