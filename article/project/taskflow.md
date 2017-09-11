---
title: 部署文档
date: 2017-09-03 02:46:53
tags: 环境
categories: 教育
copyright: true
password: datang         
abstract: 环境部署文档.
message: 请输入密码.
---

## 节点介绍
节点|动作|脚本
---|---|---
kafka | mock数据 | producer.sh
storm | 计算 | lesson-hdfs.sh
redis |  |   |
hdfs | 回流数据 |  |
mysql |  | lesson-pipeling.sh
hive |  |  |
hbase |  | |
bi | 展示 | |

## 节点流转
```
kafka -> storm -> redis -> bi
kafka -> storm -> redis -> mysql -> bi
kafka -> storm -> hdfs
kafka -> storm -> hdfs -> hive
kafka -> storm -> hdfs -> hive -> hbase
```
