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

系统|账号|密码
---|---|---
[bi](http://115.182.107.206/bi/#/app/passengerOverview) | admin | admin
[ambari](http://115.182.107.206:8080/#/login) | ambari | ambari)P:? )P:?

## 节点流转
[节点任务流程](/blog/2017/09/03/blog/article/project/taskflow/)

## hdfs
http://115.182.107.206:81/explorer.html#/suanpan/log-v1-storm/

## pipeline
`/opt/shopping/tmp/.tmp/lesson-pipeling.sh`

```
#/bin/bash

# year=`date +%Y`
# month=`date +%m`
# day=`date +%d`
# hour=`date -d "-1 hour" "+%H" `
date=`date +%Y%m%d`
ydate=`date -d "-1 day" +%Y%m%d`
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $ydate 23 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 00 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 01 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 02 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 03 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 04 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 05 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 06 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 07 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 08 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 09 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 10 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 11 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 12 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 13 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 14 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 15 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 16 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 17 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 18 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 19 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 20 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 21 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 22 1
java -cp smart-pipeline-0.0.1.jar com.nel.pipeline.Starter com.nel.db.impl.MysqlDataPipeline $date 23 1


```

## hive
`suanpan`
```


alter table suanpan add partition (dt='2017090922') location '/suanpan/log-v1-storm/20170909/22';

select id3,count(1) from suanpan where dt=2017090921 group by id3;

```
## hbase
`hive-hbase`
```
insert into table suanpan_hbase PARTITION(dt=2017090921) select id0 as key,id1 , id2 , id3 , id4 , pdate , time , index1 , index2 , index3 , index4  from suanpan where dt=2017090921;

hive:
select * from suanpan_hbase limit 10;

hbase:
scan 'suanpan_hbase', {STARTROW=>'9364:a6:82:b0:91:7b', ENDROW => '9364:a6:82:b0:91:7b'}

```
