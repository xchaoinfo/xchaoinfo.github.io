---
layout: post
title: "xlwings 让你的 Excel 飞起来 "
date: 2017-03-16 16:39:12
categories:
- Python
- Excel
tags: 
- Python
- Excel
- xlwings
description: "本文主要利用 xlwings 驱动 Excel 来自动化处理一些工作, 可以方便的将Python丰富的库和 Excel 的强大的功能结合，是让 Python 来替代一部分 VBA 的功能."
---

## xlwings 让你的 Excel 飞起来 

### 简介
众所周知，VBA 可以很高效的操作 Excel，提高办公效率。在 Python 中，我们可以通过 pywin32 来调用 windows 系统的 API 来实现 VBA 的很多功能，但是写起来比较复杂。

开始之前, 回忆一下使用 Excel 的场景。
我们会同时打开多个 Excel 文件, 多个 workbook, 每个 workbook 又可以用多个 sheet。
同时，我们还会在多个 sheet workbook Excel 窗口之间切换。
对一个或多个单元格进行增删改查，设置格式，合并分拆等操作。
利用 xlwings 这些操作都可以轻松搞定，一次编写彻底解决"手抽筋的问题" 

xlwings 是基于 BSD-licensed 的一个 Python 第三方的模块，对 pywin32 进行了封装，可以很方便的和 Excel 进行交互，它有以下优点：

1. 语法接近 VBA
2. 可以用 Python 代码取代 VBA 编写宏
3. 在 windows 可以用 Python 编写 Excel 用户自定义函数
4. 全功能支持 Numpy Pandas matplotlib 等科学计算库
5. 支持 Windows 和 MacOS
6. 支持 Py2.7 Py3.3+

xlwings 可以让你的 Excel 飞起来，正如 Selenium 可以让你的浏览器飞起来一样。

### 安装

推荐使用 Anaconda https://www.continuum.io/downloads 来安装，可以省去很多麻烦，
```conda install xlwings```
使用 pip 安装，需要先手动安装 pywin32 下载地址
https://sourceforge.net/projects/pywin32/files/pywin32/ 

安装 pywin32 后，使用 pip 安装即可

```pip install xlwings```

官方安装文档
http://docs.xlwings.org/en/stable/installation.html

### 快速入门

#### 打开工作表

```Python3
import xlwings as xw
# 打开一个新的 workbook 
wb = xw.Book()
# 打开当前目录已经存在的一个 workbook 
wb = xw.Book('FileName.xlsx')

# 输入完整的路径打开一个 workbook 
FileName = "C:\\python\\to\\file.xlsx"
FileName = r"C:\python\to\file.xlsx"
# 注意 windows 字符 "\" 逃逸的问题
wb = xw.Book(fn)

```

#### 打开 sheet 的三种方式

```python3

# 打开第一个 sheet
sheet = wb.sheets[0]
# 打开名字为 "xchaoinfo" sheet
sheet = wb.sheets["xchaoinfo"]
# 打开当前活动的 sheet
sheet = wb.sheets.active

```

#### 读写数据到 sheet 中

```pyhon3

# 当前活动的 sheet 中读写一个单元格的数据
>>> import xlwings as xw
>>> wb = xw.Book()
>>> sht = wb.sheets.active
>>> sht.range('A1').value = "xchaoinfo"
>>> sht.range('A1').value
'xchaoinfo'
# 当前活动的 sheet 中读写一行单元格的数据
# 将列表储存在A1：C1中
>>> sht.range('A1').value=["name","age","gender"]
>>> sht.range('A1:C1').value
['name', 'age', 'gender']
# 当前活动的 sheet 中读写一列单元格的数据
# 将列表储存在A1:A3中
>>> sht.range('A1').options(transpose=True).value=["xchaoinfo",18,1]
>>> sht.range("A1:A3").value
['xchaoinfo', 18.0, 1.0]
>>> sht.range('A1:A3').value = ["Robot", 20, 2]
>>> sht.range("A1:A3").value
['Robot', 18.0, 1.0]
# 当前活动的 sheet 中读写多行多列单元格的数据
# 将2x2表格，即二维数组，储存在A1:B2中，如第一行1，2，第二行3，4
>>> wb = xw.Book()
>>> sht = wb.sheets.active
>>> sht.range('A1').options(expand='table')=[[1,2],[3,4]]
>>> sht.range("A1").expand().value
[[1.0, 2.0], [3.0, 4.0]]
# expand 的详细用法请参考文档
>>> sht.range('A1:B2').value = [[1,2],[3,4]]
>>> sht.range('A1:B2').value
[[1.0, 2.0], [3.0, 4.0]]
```

### 应用实例
删除 Excel 文件中，满足条件的单元格所在的一整行

```python3
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# @Date    : 2017-03-16 14:14:03
# @Author  : xchaoinfo (xchaoinfo)
# @github  : https://github.com/xchaoinfo

import xlwings as xw

fn = "data.xlsx"


class DeleteTools(object):
    """删除满足某些条件的行
    data.xlsx 中有很多重复的数据
    需要删除那些重复的
    """

    def __init__(self, fn):
        super(DeleteTools, self).__init__()
        self.ExistSet = set()
        self.ToDelList = list()
        self.fn = fn

    def rule(self, value):
        # 可以自定义规则来操作
        pass

    def Delete(self):
        # visible 控制 Excel 打开是否显示界面
        # add_book 控制是否添加新的 workbook
        app = xw.App(visible=True, add_book=False)
        # app.display_alerts = False

        # 打开 data.xlsx 文件到 wookbook 中
        wb = app.books.open(fn)
        # 切换到当前活动的 sheet 中
        sheet = wb.sheets.active

        # 选择 A1 所在的一列
        # 当 Excel 格式复杂的时候,不建议使用 expand
        # 可以这样选择
        ARange = sheet.range("A1:A100")
        # ARange = sheet.range("A1").expand("down")
        for A in ARange:
            if str(A.value).strip() not in self.ExistSet:
                self.ExistSet.add(str(A.value).strip())
            else:
                # address = A.address
                # 获取 A 所在的位置坐标
                self.ToDelList.append(A.address)
                # print(A.value)

        while self.ToDelList:
            td = self.ToDelList.pop()
            # 删除 A 所在的一行
            sheet.range(td).api.EntireRow.Delete()
        # 保存 wookbook
        # 相当于Excel 的 Ctrl+S 快捷键
        sheet.autofit()
        wb.save()
        app.quit()


if __name__ == '__main__':
    d = DeleteTools(fn)
    d.Delete()

```

### 参考
插上翅膀，让Excel飞起来——xlwings（一）
http://www.jianshu.com/p/e21894fc5501
xlwings 官方文档
http://docs.xlwings.org/en/stable/

更多内容详见官方文档


文中的代码示例可以在 github 上找到 https://github.com/xchaoinfo/Py-example-by-xchaoinfo
本文同步发布在 知乎专栏: 可爱的 Python

首发于微信公众号：xchaoinfo

