---
title: 核心脚本
date: 2017-09-05 02:46:53
tags: 内核
categories: 工具
copyright: true
password: showmethemoney    
abstract: 流式计算-lua计算实现.
message: 请输入密码.
---

## redis版本·配置化计算内核

### lua实现

```
redis.call("SADD", KEYS[1], ARGV[1])
redis.call("incrBy", KEYS[2], 1)
local ot=ARGV[3] + 0
local m10=ARGV[5] + 0
if ot < m10 or ARGV[10]~="9" then
    return {'ot', ot, 'ARGV10', ARGV[10]}
end
redis.call("hincrby",  KEYS[3], ARGV[1], 1)
local count_month = redis.call("hincrby",  KEYS[4], ARGV[1], 1) or 0
local count_month2 = redis.call("hget",  KEYS[5], ARGV[1]) or 0
local count_month3 = redis.call("hget",  KEYS[6], ARGV[1]) or 0
count_month = count_month + count_month2 + count_month3
local max = ARGV[8] + 0
local act_day=0
if count_month >= max then
    redis.call("SADD", KEYS[7], ARGV[1])
    act_day=1
    redis.call("SADD", KEYS[9], ARGV[1])
    redis.call("SADD", KEYS[11], ARGV[1])
end
local alw_day=0
max = ARGV[9] + 0
if count_month >= max then
    redis.call("SADD", KEYS[8], ARGV[1])
    alw_day=1
    redis.call("SADD", KEYS[10], ARGV[1])
    redis.call("SADD", KEYS[12], ARGV[1])
end
return {"act", act_day, "alw", alw_day}
```

### 脚本工具
测试脚本工具。
```
redis-cli -a 123456 --eval redis.lua cell_tolu_2017090100_1 cell_tolt_2017090100_1 cell_oln_2017090100_1 cell_oln_201709_1 cell_oln_201708_1 cell_oln_201707_1 cell_act_20170901_1 cell_alw_20170901_1 cell_act_2017090100_1 cell_alw_2017090100_1 cell_act_201709_1 cell_alw_201709_1 , 24:df:6a:85:d1:c1 1504281600 2000 1504281600 600 1504281600 1512057600 5 2 9
```

### 样例数据

```
line对应原始数据
{"count":"5",
"data":[
{"MAC":"24:df:6a:85:d1:c1","signal":"-65","hit":"00004","find":"2016-06-20/14:00:04","refresh":"2016-06-20/14:00:04","state":"new","residence":"33"},
{"MAC":"76:a6:82:b0:91:7b","signal":"-77","hit":"00003","find":"2016-06-20/14:00:07","refresh":"2016-06-20/14:00:07","state":"new","residence":"67"},
{"MAC":"90:18:7c:6e:36:51","signal":"-80","hit":"00002","find":"2016-06-20/14:00:12","refresh":"2016-06-20/14:00:12","state":"new","residence":"123"},
{"MAC":"dc:37:14:20:32:5a","signal":"-76","hit":"00002","find":"2016-06-20/14:00:10","refresh":"2016-06-20/14:00:10","state":"new","residence":"423"},
{"MAC":"f4:29:81:a0:f9:17","signal":"-66","hit":"00004","find":"2016-06-20/14:00:05","refresh":"2016-06-20/14:00:05","state":"new","residence":"32"}
]}

base64加密: `eyJjb3...In0KXX0=`

最终日志格式：
222.128.177.26	13/May/2016:14:00:13 +0800	/discount/deviceAsys.do	line=eyJjb3VudCI6IjUiLAoiZGF0YSI6Wwp7Ik1BQyI6IjI0OmRmOjZhOjg1OmQxOmMxIiwic2lnbmFsIjoiLTY1IiwiaGl0IjoiMDAwMDQiLCJmaW5kIjoiMjAxNi0wNi0yMC8xNDowMDowNCIsInJlZnJlc2giOiIyMDE2LTA2LTIwLzE0OjAwOjA0Iiwic3RhdGUiOiJuZXciLCJyZXNpZGVuY2UiOiIzMyJ9LAp7Ik1BQyI6Ijc2OmE2OjgyOmIwOjkxOjdiIiwic2lnbmFsIjoiLTc3IiwiaGl0IjoiMDAwMDMiLCJmaW5kIjoiMjAxNi0wNi0yMC8xNDowMDowNyIsInJlZnJlc2giOiIyMDE2LTA2LTIwLzE0OjAwOjA3Iiwic3RhdGUiOiJuZXciLCJyZXNpZGVuY2UiOiI2NyJ9LAp7Ik1BQyI6IjkwOjE4OjdjOjZlOjM2OjUxIiwic2lnbmFsIjoiLTgwIiwiaGl0IjoiMDAwMDIiLCJmaW5kIjoiMjAxNi0wNi0yMC8xNDowMDoxMiIsInJlZnJlc2giOiIyMDE2LTA2LTIwLzE0OjAwOjEyIiwic3RhdGUiOiJuZXciLCJyZXNpZGVuY2UiOiIxMjMifSwKeyJNQUMiOiJkYzozNzoxNDoyMDozMjo1YSIsInNpZ25hbCI6Ii03NiIsImhpdCI6IjAwMDAyIiwiZmluZCI6IjIwMTYtMDYtMjAvMTQ6MDA6MTAiLCJyZWZyZXNoIjoiMjAxNi0wNi0yMC8xNDowMDoxMCIsInN0YXRlIjoibmV3IiwicmVzaWRlbmNlIjoiNDIzIn0sCnsiTUFDIjoiZjQ6Mjk6ODE6YTA6Zjk6MTciLCJzaWduYWwiOiItNjYiLCJoaXQiOiIwMDAwNCIsImZpbmQiOiIyMDE2LTA2LTIwLzE0OjAwOjA1IiwicmVmcmVzaCI6IjIwMTYtMDYtMjAvMTQ6MDA6MDUiLCJzdGF0ZSI6Im5ldyIsInJlc2lkZW5jZSI6IjMyIn0KXX0=&bid=E4956E403E05&date=2016-06-20/14:00:13\x0A	-	-
```

### mock数据工具
```
#!/bin/bash

#regx
checkMonth() {
    month=$1
    case $month in
        *"01"*) newMonth='Jan';;
        *"02"*) newMonth='Feb';;
        *"03"*) newMonth='Mar';;
        *"04"*) newMonth='Apr';;
        *"05"*) newMonth='May';;
        *"06"*) newMonth='Jun';;
        *"07"*) newMonth='Jul';;
        *"08"*) newMonth='Aug';;
        *"09"*) newMonth='Sep';;
        *"10"*) newMonth='Oct';;
        *"11"*) newMonth='Nov';;
        *"12"*) newMonth='Dec';;
        *) newMonth=$month
    esac
    echo $newMonth
}

# month1=`checkMonth "09"`
# checkMonth $1

year=`date +%Y`
month=`date +%m`
day=`date +%d`
# date -d "-$1 hour" "+%H"
hour=`date +%H`

rand="{'count':'5','data':[{'MAC':'"${RANDOM}":df:6a:85:d1:c1','signal':'-65','hit':'00004','find':'2016-06-20/14:00:04','refresh':'2016-06-20/14:00:04','state':'disapear','residence':'"${RANDOM}"'},{'MAC':'"${RANDOM}":a6:82:b0:91:7b','signal':'-77','hit':'00003','find':'2016-06-20/14:00:07','refresh':'2016-06-20/14:00:07','state':'disapear','residence':'"${RANDOM}"'},{'MAC':'"${RANDOM}":18:7c:6e:36:51','signal':'-80','hit':'00002','find':'2016-06-20/14:00:12','refresh':'2016-06-20/14:00:12','state':'disapear','residence':'"${RANDOM}"'},{'MAC':'"${RANDOM}":37:14:20:32:5a','signal':'-76','hit':'00002','find':'2016-06-20/14:00:10','refresh':'2016-06-20/14:00:10','state':'disapear','residence':'"${RANDOM}"'},{'MAC':'"${RANDOM}":29:81:a0:f9:17','signal':'-66','hit':'00004','find':'2016-06-20/14:00:05','refresh':'2016-06-20/14:00:05','state':'disapear','residence':'"${RANDOM}"'} ]}"

data=`echo -n $rand | base64 | xargs -i echo -n {}`
#data=`echo $data | base64`
# echo -e $rand | base64 | xargs -i echo -n {} > /smart/loggw/test.log

postData1="222.128.177.26	"$day"/"`checkMonth $month`"/"$year":"$hour":00:13 +0800	/discount/deviceAsys.do	line="$data"&bid=E4956E403E05&date="$year"-"$month"-"$dayr"/"$hour":00:13\x0A	-	-"
postData2="222.128.177.26	"$day"/"`checkMonth $month`"/"$year":"$hour":00:13 +0800	/discount/deviceAsys.do	line="$data"&bid=E1954E403E01&date="$year"-"$month"-"$dayr"/"$hour":00:13\x0A	-	-"
echo $postData1
echo $postData2

/usr/hdp/2.4.0.0-169/kafka/bin/kafka-console-producer.sh --broker-list 101.201.209.126:6667 --topic log-v1 <<EOF
$postData1
$postData2
EOF
```

如果需要补数据，修改year、day、hour，使其支持参数化。这里用hour做例子介绍。
修改hour变量 为 ``` hour=`date -d "-$1 hour" "+%H"` ```
脚本名称为producer.sh，随机100次mock。
```
for l in $( seq 1 100 )
  do
  sh producer.sh 1
  sh producer.sh 3
  sh producer.sh 4
done
```

## redis 辅助工具

### 清理key
```
EVAL 'local keys = redis.call("keys", KEYS[1]); for i = 1, #keys do redis.call("del", keys[i]) end ; ' 1 cus_*
```

### shell

checkMonth() {
    month=$1
    case $month in
        *"01"*) newMonth='Jan';;
        *"02"*) newMonth=${month/Feb/02};;
        *"03"*) newMonth=${month/Jan/01};;
        *"04"*) newMonth=${month/Jan/01};;
        *"05"*) newMonth=${month/Jan/01};;
        *"06"*) newMonth=${month/Jan/01};;
        *"07"*) newMonth=${month/Jan/01};;
        *"08"*) newMonth=${month/Jan/01};;
        *"09"*) newMonth=${month/Jan/01};;
        *"10"*) newMonth=${month/Jan/01};;
        *"11"*) newMonth=${month/Jan/01};;
        *"12"*) newMonth=${month/Jan/01};;
        *) newMonth=$month
    esac
    echo $newMonth
}


#mock 前1、3、4小时的数据。
