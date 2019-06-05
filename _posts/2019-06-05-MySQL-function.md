---
title: MySQL的函数、存储过程、触发器、事件、视图
layout: post
date:   2019-06-05 10:38:00 +0800
categories: document
tag: 笔记
---

## 1 定义
* 函数（function）: 如```count()```, ```max()```, ```min()``` 等是系统函数，也可以用自己创建function
* 存储过程（procedure）: 一组为了完成特定功能的SQL 语句集，存储在数据库中，经过第一次编译后再次调用不需要再次编译，比一个个执行sql语句效率高。
* 触发器（trigger）
* 事件（event）事件调度器是MySQL5.1后新增的功能，可以将数据库按自定义的时间周期触发某种操作，可以理解为时间触发器，类似于linux系统下面的任务调度器crontab
* 视图（view）视图是一个虚拟表，其内容由查询定义。 使用它主要出于两种原因：1. 安全原因， 视图可以隐藏一些数据，如：社会保险基金表，可以用视图只显示姓名，地址，而不显示社会保险号和工资数等，2. 另一原因是可使复杂的查询易于理解和使用

## 2 操作

### 2.1 函数
查看函数列表
```
SHOW FUNCTION STATUS where db='指定库';
```
创建
```
CREATE FUNCTION 函数名称(参数列表)
　　RETURNS 返回值类型
　　函数体
```
修改
```
ALTER FUNCTION 函数名称 [characteristic ...]
```
删除
```
DROP FUNCTION [IF EXISTS] 函数名称
```
调用
```
SELECT 函数名称(参数列表)
```

### 2.2 存储过程
查看存储过程列表
```
SHOW PROCEDURE STATUS where db='指定库';
```
创建
```
CREATE PROCEDURE 过程名 (参数列表) [characteristic ...]
    函数体
```
修改
```
ALTER PROCEDURE  过程名 [characteristic ...]
```
删除
```
DROP PROCEDURE [IF EXISTS] 过程名
```
调用
```
CALL 过程名(参数列表)
```
### 2.3 触发器
创建
```
CREATE TRIGGER <触发器名称>  --触发器必须有名字，最多64个字符，可能后面会附有分隔符.它和MySQL中其他对象的命名方式基本相象.
{ BEFORE | AFTER }  --触发器有执行的时间设置：可以设置为事件发生前或后。
{ INSERT | UPDATE | DELETE }  --同样也能设定触发的事件：它们可以在执行insert、update或delete的过程中触发。
ON <表名称>  --触发器是属于某一个表的:当在这个表上执行插入、 更新或删除操作的时候就导致触发器的激活. 我们不能给同一张表的同一个事件安排两个触发器。
FOR EACH ROW  --触发器的执行间隔：FOR EACH ROW子句通知触发器 每隔一行执行一次动作，而不是对整个表执行一次。
<触发器SQL语句>  --触发器包含所要触发的SQL语句：这里的语句可以是任何合法的语句， 包括复合语句，但是这里的语句受的限制和函数的一样。
--创建触发器（CREATE TRIGGER），需要SUPER权限。
```
eg:
{% highlight sql %}
CREATE TRIGGER testref BEFORE INSERT ON test1
  FOR EACH ROW BEGIN
    INSERT INTO test2 SET a2 = NEW.a1;
    DELETE FROM test3 WHERE a3 = NEW.a1;  
    UPDATE test4 SET b4 = b4 + 1 WHERE a4 = NEW.a1;
  END
{% endhighlight %}
删除
```
DORP TRIGGER 方案名称.触发器名称
```

### 2.4 事件
创建
{% highlight sql %}
CREATE

    [DEFINER = { user | CURRENT_USER }]   --定义事件执行的时候检查权限的用户。

    EVENT

    [IF NOT EXISTS]

    event_name

    ON SCHEDULE schedule              --定义执行的时间和时间间隔。

    [ON COMPLETION [NOT] PRESERVE]     --定义事件是一次执行还是永久执行，默认为一次执行，即NOT PRESERVE。

    [ENABLE | DISABLE | DISABLE ON SLAVE]   --定义事件创建以后是开启还是关闭，以及在从上关闭。如果是从服务器自动同步主上的创建事件的语句的话，会自动加上DISABLE ON SLAVE

    [COMMENT 'comment']                 -- 注释

    DO event_body;

 

schedule:

    AT timestamp [+ INTERVAL interval] ...

     | EVERY interval

    [STARTS timestamp [+ INTERVAL interval]...]

    [ENDS timestamp [+ INTERVAL interval] ...]

interval:

  quantity {YEAR | QUARTER | MONTH | DAY | HOUR| MINUTE |

              WEEK | SECOND | YEAR_MONTH |DAY_HOUR |

              DAY_MINUTE |DAY_SECOND| HOUR_MINUTE |

              HOUR_SECOND| MINUTE_SECOND}
{% endhighlight %}

删除：
```
DROP EVENT [IF EXISTS] event_name
```

修改
{% highlight sql %}
ALTER

    [DEFINER = { user | CURRENT_USER }]

    EVENT event_name

    [ON SCHEDULE schedule]

    [ON COMPLETION [NOT] PRESERVE]

    [RENAME TO new_event_name]

    [ENABLE | DISABLE | DISABLE ON SLAVE]

    [COMMENT 'comment']

    [DO event_body]
{% endhighlight %}
查看事件是否开启
```
SHOW VARIABLES LIKE 'event_scheduler';
SELECT @@event_scheduler;
SHOW PROCESSLIST;
//如果看到event_scheduler为on或者PROCESSLIST中显示有event_scheduler的信息说明就已经开启了事件
```
开启事件：
```
SET GLOBAL event_scheduler = ON;    // 立即开启，更改完这个参数就立刻生效了

// 长期开启。在my.ini中的[mysqld]部分添加如下内容，然后重启mysql。
event_scheduler=ON
```

### 2.5 视图
创建：
```
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(列名列表)]
AS 查询语句
[WITH [CASCADED | LOCAL] CHECK OPTION]
```
修改：
```
ALTER [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```
删除：
```
DROP VIEW [IF EXISTS]
view_name [, view_name] ...
[RESTRICT | CASCADE]
```

## 参考
* [MySQL官方文档-创建procedure](https://dev.mysql.com/doc/refman/8.0/en/create-procedure.html)