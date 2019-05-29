---
layout: post
title:  控制台管理mysql
date:   2019-05-29 10:08:00 +0800
categories: document
tag: 教程
---
## 用户与权限类
1、 用root登录
```
cd ~/mysql&&./bin/mysql --defaults-file=etc/user.root.cnf
```
2、 查看mysql中所有的账号

```
SELECT DISTINCT CONCAT('''',user,'''@''',host,''';') AS query FROM mysql.user;

select user,host from mysql.user;
```
3、 新建用户

```
// 光新建用户
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
// 新建用户并授权
grant all privileges on *.*  to 'username'@'%' identified by 'password';
```
4、 修改密码
```
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```
4、删除用户
```
DROP USER 'username'@'host';
```

## 通用
1、 查询一个库中表的个数（用于大量表）

```
SELECT count(TABLE_NAME) FROM information_schema.TABLES WHERE TABLE_SCHEMA='dbname';  
```
2、 修改主键

```
alter table sbtest1 modify column id int(10) unsigned;
ALTER TABLE sbtest1 DROP PRIMARY KEY;
ALTER TABLE sbtest1 ADD PRIMARY KEY ( `k`,`id` )
```

3、统计mysql-bin000117二进制中的UPDATE|INSERT|SELECT中的执行次数。
```
./mysqlbinlog --no-defaults --base64-output=decode-rows -v -v mysql-bin.000117 mysql-bin.000118 mysql-bin.000119 | awk '/###/ {if($0~/UPDATE|INSERT|SELECT/)count[$2" "$NF]++}END{for(i in count) print i,"\t",count[i]}' | column -t | sort -k3nr
```

4、 修改表名

```
rename table tblName to tblName1;
```
5、 查看权限
```
show grants for user@'IP';
```
6、把某db的所有权限授予给某user
```
GRANT ALL PRIVILEGES ON a1.* TO 'liuya'@'%' IDENTIFIED BY 'liuya123' WITH GRANT OPTION;   
```
7、剥夺权限
```
revoke ALL PRIVILEGES ON a1.* from 'liuya'@'%';
flush privileges;
```
8、 删除数据
```
truncate table tablename //删除表中数据
```

## 数据导入导出
1、导出指定db
```
mysqldump -uUserName -pPassWord [database name] > dump.sql
```
2、只导出结构
```
mysqldump --no-data --databases mydatabase1 mydatabase2 mydatabase3 > test.dump
```

3、导入(执行sql文件）
```
mysql [database name] < [backup file name]
//或者进入mysql命令行，然后
source [backup file name]
```