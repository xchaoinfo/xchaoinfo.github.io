---
title: Hello World
excerpt: 主要讨论 Linux 的相关知识和经验
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)


<!-- more -->

## 起步

1 学会使用命令帮助
2 常见的特殊符号

### 学会使用命令帮助
Linux系统大多数的操作都是通过命令来完成的, 几千个命令， 加上第三方开发的命令。数量会更多 对于普通开发用户来,常用的命令也有几十甚至上百个, 在加上不同命令和参数的组合,想要完全记住, 几乎是费力不讨好的事情, 好在linux提供了完善的命令帮助文档, 能够让我们只需要记住命令的名称或者模糊的关键词,就能够流畅的使用命令.

同时在搜索引擎的帮助下, 也可以得到很多命令的使用方法，而后在通过阅读命令的文档，确认命令的可靠性. 测试，应用

从应用常见出发

了解如下几个情况
1. 知道命令名称, 忘记了估计的用法, 可以使用 man command 来查看用法,
man command
bash 内部的命令的 help command 来查看
例如 bash 

第三方安装的, 例如
command -h
python -h 可以
command -help
java -help

command --help
command --help 


2. 不知道名称 man -k "keywords"
3. which 查看安装路径
4. whatis 简介
    `whatis -w "parall*" 支持模糊匹配`

5. info 详细信息
    info man | more
h 
jk
/pattern 向前搜索 
?pattern 向后搜索


### 常见的特殊符号

| 管道符号
* 通配符
> 重定向覆盖 (python中的 open w)
>> 重定向追加 (python中的 open a)



思路：

1 应用场景 —> 实用demo -> 命令全面解读




在linux终端，面对命令不知道怎么用，或不记得命令的拼写及参数时，我们需要求助于系统的帮助文档； linux系统内置的帮助文档很详细，通常能解决我们的问题，我们需要掌握如何正确的去使用它们；

在只记得部分命令关键字的场合，我们可通过man -k来搜索；
需要知道某个命令的简要说明，可以使用whatis；而更详细的介绍，则可用info命令；
查看命令在哪个位置，我们需要使用which；
而对于命令的具体参数及使用方法，我们需要用到强大的man；
下面介绍这些命令；

命令使用
查看命令的简要说明
简要说明命令的作用（显示命令所处的man分类页面）:

$whatis command
正则匹配:

$whatis -w "loca*"
更加详细的说明文档:

$info command
使用man
查询命令command的说明文档:

$man command
eg：man date
使用page up和page down来上下翻页

在man的帮助手册中，将帮助文档分为了9个类别，对于有的关键字可能存在多个类别中， 我们就需要指定特定的类别来查看；（一般我们查询bash命令，归类在1类中）；


eg:
$whatis printf
printf               (1)  - format and print data
printf               (1p)  - write formatted output
printf               (3)  - formatted output conversion
printf               (3p)  - print formatted output
printf [builtins]    (1)  - bash built-in commands, see bash(1)
我们看到printf在分类1和分类3中都有；分类1中的页面是命令操作及可执行文件的帮助；而3是常用函数库说明；如果我们想看的是C语言中printf的用法，可以指定查看分类3的帮助：

$man 3 printf

$man -k keyword
查询关键字 根据命令中部分关键字来查询命令，适用于只记住部分命令的场合；

eg：查找GNOME的config配置工具命令:

$man -k GNOME config| grep 1
对于某个单词搜索，可直接使用/word来使用: /-a; 多关注下SEE ALSO 可看到更多精彩内容

查看路径
查看程序的binary文件所在路径:

$which command
eg:查找make程序安装路径:

$which make
/opt/app/openav/soft/bin/make install
查看程序的搜索路径:

$whereis command
当系统中安装了同一软件的多个版本时，不确定使用的是哪个版本时，这个命令就能派上用场；

总结
whatis info man which whereis


pip install cheat 可以提供使用实例
tab 自动补全



Ctrl+R 快捷键快速
history
!history 的编号 快速执行命令
`cat ~/.bash_history`


