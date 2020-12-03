---
layout: post
title: MySQL 学习笔记
categories:
  - MySQL
tags:
  - SQL
description: 本文主要学习和使用 MySQL 数据库的中问题
date: 2020-03-13 20:26:55
---


环境: centos 7.6 , MySQL 5.7 腾讯云的 VPS

## MySQL 的安装与配置

### 1. 下载安装 Mysql 5.7 
```shell
# 配置镜像源
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
rpm -ivh mysql57-community-release-el7-10.noarch.rpm
# yum 安装
yum install -y  mysql-community-server
```

### 启动

```shell
service mysqld start
systemctl start mysqld.service
```


### 2. 常见问题
1. use mysql 报错
主要是因为开启了用户验证, 但是没有很好的设置账号和密码, 解决方式
首先, `service mysqld stop` 关闭 mysqld 服务
然后, `mysqld --user=root  --skip-grant-tables` 
其中, --user=root 为了解决 MySQL 在 root 用户下启动的安全问题
2. Mysql出现Table 'performance_schema.session_status' doesn't exist 解决办法

更新mysql  `mysql_upgrade -u root -p`
重启mysql `service mysql restart`

3. localhost 登录失败, 可以使用 `mysql -uroot -h127.0.0.1 -p` 登录 
权限配置问题

### 配置



## 基本使用

```sql
CREATE DATABASE hello CHARACTER SET utf8;

CREATE table if not exists


SELECT ListA from xxxx;
SELECT * from xxxx;
select listA, listb from xxxx;

-- 去掉重复的值

select distinct ListA From xxxx;

-- 倒序排列
select * from xxx order by listA desc; 


```


## 进阶使用

## 集群运维
docker k8s
