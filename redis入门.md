---
title: redis入门
date: 2017-07-02 
tags: NoSql
categories: java
---

#### NOSQL概述

NoSql = not only sql 泛指非关系型数据库

为什么需要NoSql
- High performance - 高并发读写
- Huge Storage - 海量数据的高效率存储和访问
- High Scalability && High Availability - 高可扩展性和高可用性

NoSql数据库的四大分类
- 键值存储
- 列存储
- 文档数据库
- 图形数据库

四类Nosql数据库的比较
![Nosql数据库](http://ok803i6ds.bkt.clouddn.com/2017/03/5958a5f50001014f12800720.jpg "四类Nosql数据库的比较")

<!--more-->

#### Redis概述
 
高性能键值对数据库，支持的键值数据类型：
1.字符串类型
2.列表类型
3.有序集合类型
4.散列类型
5.集合类型

Redis的应用场景
1.缓存
2.任务队列
3.网站访问统计
4.数据过期处理
5.应用排行榜
6.分布式集群架构中的session分离

#### Redis安装

我使用的windows版的redis-2.6 http://pan.baidu.com/s/1boV3V8F

解压之后，进入到bin目录下的release目录，解压redisbin64.zip
![redis入门](http://ok803i6ds.bkt.clouddn.com/2017/04/20170702162251.png)
拷贝redisbin64文件下的所有程序到根目录redis-2.6下
![redis入门](http://ok803i6ds.bkt.clouddn.com/2017/05/20170702162621.png)
redis的配置文件为同目录下的redis.conf

打开cmd，输入redis-service.exe即可启动默认配置的redis
出现
![redis入门](http://ok803i6ds.bkt.clouddn.com/2017/06/20170702162937.png)
代表启动成功

重新打开一个cmd，输入redis-cli.exe即可启动客户端，对redis进行访问。
![redis入门](http://ok803i6ds.bkt.clouddn.com/2017/07/20170702163242.png)











