---
title: 账号系统
date: 2017-09-03 02:46:53
tags: 环境
categories: 教育
copyright: true
password: showmethemoney         
abstract: 环境部署文档.
message: 请输入密码.
---

系统|账号|密码
---|---|---
[bi](http://115.182.107.206/bi/#/app/passengerOverview) | admin | admin
[ambari](http://115.182.107.206:8080/#/login) | ambari | ambari)P:? )P:?

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
```
su - hive
hive -e "alter table suanpan add partition (dt='2017090922') location '/suanpan/log-v1-storm/20170909/22'"
exit

create EXTERNAL table IF NOT EXISTS
suanpan (id0 string, id1 string, id2 string, id3 string, id4 string, pdate string, time bigint, index1 bigint, index2 bigint, index3 bigint, index4 bigint)
partitioned by (dt string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';

alter table suanpan add partition (dt='2017090921') location '/suanpan/log-v1-storm/20170909/21';

select id3,count(1) from suanpan where dt=2017090921 group by id3;

```

hive-hbase
```
CREATE TABLE suanpan_hbase  
(id0 string, id1 string, id2 string, id3 string, id4 string, pdate string, time bigint, index1 bigint, index2 bigint, index3 bigint, index4 bigint)
partitioned by (dt string)
 STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'   
WITH SERDEPROPERTIES  
("hbase.columns.mapping" = ":key,info:id1,info:id2,info:id3,info:id4,info:pdate,info:time,info:index1,info:index2,info:index3,info:index4")
TBLPROPERTIES ("hbase.table.name"="suanpan_hbase");  

-- id0 as key
insert into table suanpan_hbase PARTITION(dt=2017090921) select id0 as key,id1 , id2 , id3 , id4 , pdate , time , index1 , index2 , index3 , index4  from suanpan where dt=2017090921;

hive:
select * from suanpan_hbase limit 10;

hbase:
scan 'suanpan_hbase', {STARTROW=>'9364:a6:82:b0:91:7b', ENDROW => '9364:a6:82:b0:91:7b'}

```
