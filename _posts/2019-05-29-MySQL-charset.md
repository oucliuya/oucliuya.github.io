---
title: MySQL的字符集
layout: post
date:   2019-05-29 10:08:00 +0800
categories: document
tag: 笔记
---

## 1 utf8字符集问题
### 1.1 简介
mysql 5.5.3增加了utf8mb4（utf8 more byte 4），专门用来支持四字节的unicode（utf8字符集每个字符最多使用三个字节）。 utf8mb4 编码是 utf8编码的超集，所以把utf8改成utf8mb4不需要做其他转换。

开发时，为了获取更好的兼容性，应该总是使用 utf8mb4 而非 utf8.  对于 CHAR 类型数据，utf8mb4 会多消耗一些空间，根据 Mysql 官方建议，使用 VARCHAR  替代 CHAR。

### 1.2 修改MySQL的编码为utf8mb4
#### 1.2.1 MySQL版本为 5.5.3+

#### 1.2.2 MySQL驱动 5.1.13+

#### 1.2.3 修改MySQL配置文件my.cnf
```
修改mysql配置文件my.cnf（windows为my.ini）
my.cnf一般在etc/mysql/my.cnf位置。找到后请在以下三部分里添加如下内容：
[client]
default-character-set = utf8mb4
[mysql]
default-character-set = utf8mb4
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect=’SET NAMES utf8mb4’
```
#### 1.2.4 重启数据库，检查变量
```
SHOW VARIABLES WHERE Variable_name LIKE 'character\_set\_%' OR Variable_name LIKE 'collation%';
```
要保证以下5个变量为utf8mb4：
![](media/15571962426038/15571964982291.jpg)

附：各种与字符集相关的变量

| Variable_name            | Value              |define |
| --- | --- | --- |
| character_set_client     | utf8mb4            | 客户端来源数据使用的字符集，也就是客户端发过来的查询语句使用的什么字符集 |
| character_set_connection | utf8mb4            | MySQL接受到用户查询后，按照character_set_client将其转化为character_set_connection设定的字符集。|
| character_set_database   | utf8               | 当前选中数据库的默认字符集 |
| character_set_filesystem | binary             | 进行文件读写时转换文件名， 默认binary为不转换 | 
| character_set_results    | utf8mb4            | 查询结果编码的字符集 | 
| character_set_server     | utf8mb4            | 默认的内部操作字符集 | 
| character_set_system     | utf8               | 元数据的字符集，如库表名、用户名等 | 
| collation_connection     | utf8mb4_unicode_ci | 执行字符比较时采用的编码规则 | 
| collation_database       | utf8_general_ci    | | 
| collation_server         | utf8mb4_unicode_ci | |


#### 1.2.5 数据库连接的配置
数据库连接参数中:
characterEncoding=utf8会被自动识别为utf8mb4，也可以不加这个参数，会自动检测。
而autoReconnect=true是必须加上的。

### 1.2.6 将db和table的编码转为utf8mb4
db:
```
ALTER DATABASE DB_NAME CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

table:
```
ALTER TABLE TABLE_NAME CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci; 
```

还可以更改列的编码
```
 ALTER TABLE table_name CHANGE column_name column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;  
```

#### 1.2.7 插入一条emoji，测试


## 2 latin1字符集包含gbk汉字，使用utf8读取，会失败
原因是latin1字符集可以根据输入的字符集自动转换。 步骤是
1. 设置shell的字符集为你要存入地字符集。如gbk
2. 执行```set names latin1```
3. 插入数据

读取地时候也应该使用```set names latin1```，如果使用gbk或者utf8来读，会丢数据
## 3 gbk字符集表使用utf8读取，会失败
插入gbk字符集的步骤是
1. 设置shell的字符集为gbk
2. 执行```set names gbk```
3. 插入数据
读取时自然也要```set names gbk```, 如果使用utf8读，有可能丢数据（对于在gbk和utf8中编码相同的字符不会出错。反之出错）

## 附录1 常用字符集

| char set | length | 特点 |
| --- | --- | --- |
| utf8 | 变长，1~3字节 | 适合处理各种不同语言的文字 |
| Utf8mb4 | 变长，1~4字节 | 比utf8增加了一些生僻字和emoji |
| Gbk | 定长，双字节 | 适用于只支持一般中文，且数据量大，性能要求高的场景 |
| Latin1 | MySQL默认字符集 | 实际可以根据存入数据的类型转换字符集 |

