# Python操作Mysql数据库
title: Python操作Mysql数据库总结
date: 2018-01-10
tags: Python

---

> 最近工作上，需要使用Python合并多个结构不一的mysql数据库，许久没有使用，操作起来略显生疏，所以还是想要总结一下，方便以后使用。

<!--more-->

## 连接Mysql数据库
```
import MYSQLdb as mdb
con = mdb.connect('localhost', 'name', 'password', 'databasename', charset='utf8')
```
## 创建数据库表
```
cursor = con.cursor()

sql = """
    create table if not exists users(
    id int(11) not null auto_increment primary key,
    name varchar(128),
    teamid int(11),
    constraint foreign key(teamid) references Teams(id)
    )
"""
cursor.execute(sql)
```
## Mysql常用命令

 1. 将某个.sql文件导入mysql数据库

```
create database 数据库名
use 数据库名
source /path/.sql
```

 2.  将mysql中某个数据库导出为.sql文件
```
mysqldump -u root -p 数据库名 > 导出的文件名
```

 3. 将数据库中的文件导出为.excel文件
首先查询本机mysql导出的安全目录，使用命令
```
show global variables like '%secure%';
```
然后，切换到要导出的mysql数据库中，使用命令将其导出为.txt文件：
``` 
select name,email from users into outfile '/var/lib/mysql-files/test.txt';  
```
但是存入的mysql-fils一般是只有管理员才能打开的文件夹，这里需要切换权限。
 4. 更新数据库中的某个字段
``` 
update table set name=' ' where id = ' ' 
```
 5. 向数据库中的某个表中添加某个字段
```
insert into user (userid,score) values (16,0);
```

 6. 向已有的数据库表中，向固定的某个字段后添加一个字段：
``` 
alter table tablename add id_name 类型 after 已有的字段
```
比如：
``` 
alter table team add userid varchar(11) after teamid
```
 7. 向数据库中已有的某个字段设置默认值
``` 
update tablename set 字段=0
```

 8.  更改mysql数据库中字段的排列位置
``` 
alter table 表名 modify 字段一 数据类型  after 字段二
```
表示将字段一放置在字段二之后

## 遇到的问题总结
关于数据库中文乱码的问题解决方案，将数据库中数据导入数据库中出现中文乱码的话，那很可能存在的一个问题就是编码不统一。
若编码不正确的话，那一定是编码的格式不够统一，对于中文，可以统一设置成utf-8编码。
需要注意的是，这里需同时将mysql配置文件处对应的客户端和服务端的编码格式进行修改。

