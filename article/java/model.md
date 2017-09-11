---
title: 领域模型
date: 2017-08-31 22:53:53
tags: 模型
categories: java
copyright: true
password: bigdata
photos:
 - http://bruce.u.qiniudn.com/2013/11/27/reading/photos-0.jpg
 - http://bruce.u.qiniudn.com/2013/11/27/reading/photos-1.jpg
---

## 关于树形机构的model设计
假设有一个树形结构，每层代表一个mode，有没有办法设计一个灵活的模型，支持查询当前mode的上下游mode。更甚可能支持lazy load。
示例:
```
|-a
|-- b1 -----|- b2
|-- c1 - c2 |- c3 - c4
```
通过a能get到b1和b2， 通过b1能get到a、c1、c2.
伪代码
```
a.getBList...
b1.getA...
b1.getCList...

```


### 好文
#### chrome 自动重试http请求，会导致超时后任务重复执行。
https://www.bennadel.com/blog/3257-google-chrome-will-automatically-retry-requests-on-certain-error-responses.htm

https://segmentfault.com/a/1190000005894812
