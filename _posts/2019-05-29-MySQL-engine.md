---
title: MySQL的存储引擎
layout: post
date:   2019-05-29 10:08:00 +0800
categories: document
tag: 总结
---
[参考](https://blog.csdn.net/wjtlht928/article/details/46641865)
## 操作
### 展示存储引擎
1. ```show engines```
2. ```show variables like '%storage_engine%'```
3. ```show create table t1```


### 建表指定存储引擎
```CREATE TABLE t (i INT) ENGINE = INNODB```

### 转换存储引擎


## 对比
### Innodb
支持事务安全的引擎，支持外键、行锁、事务是它的最大特点。如果有大量的update和insert，建议使用InnoDB，特别是针对多个并发和QPS较高的情况。
1. count()慢，因为innodb未存储表的总行数。得执行统计，占用大量资源。

### Myisam 
基于传统的ISAM类型，ISAM是Indexed Sequential Access Method (有索引的顺序访问方法) 的缩写. Myisam不是事务安全，不支持外键。 适合用于执行大量的select和insert的场景

1. count() 快，因为myisam存储了表的总行数

