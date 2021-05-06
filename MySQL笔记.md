## MySQL基础知识

### 表Table

什么是表？

​	table是数据库的基本组成单元，所有的数据都以表格的形式组织，目的是可读性强。

一个table包括行和列：

​	行：被称为数据/记录（data）

​	列：被称为字段（column）

| 学号(int) | 姓名（varchar） | 年龄(int) |
| --------- | --------------- | --------- |
| 110       | zs              | 20        |
| 111       | ls              | 22        |

每个字段应该包含的属性有：

​	字段名、数据类型、相关的约束。

SQL语句分类：

​	DQL（数据查询语言）：查询语句，凡是select语句都是DQL；

​	DML（数据操作语言）：insert delete update，对表当中的数据进行增删改；

​	DDL（数据定义语言）：create drop alter，对表结构的增删改；

​	TCL（事物控制语言）：commit提交事务，rollback回滚事务；

​	DCL（数据控制语言）：grant授权、revoke撤销权限等。

导入数据：

​	第一步：登录mysql数据库管理系统

​		dos命令窗口： mysql -uroot -p1233

​	第二步：查看有哪些数据库

​		show databases;（这个不是SQL语句，属于MySQL的命令。）

```mysql
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sakila             |
    | sys                |
    | world              |
    +--------------------+
    6 rows in set (0.02 sec)
```

​	第三步：创建新的数据库

​		create database testsql;

​	第四步：使用创建的数据库

​		use testsql;

​	第五步：查看当前数据库中有哪些表

​		show tables;

​	第六步：初始化数据

​		mysql> source E:\研究生\2021\MySQLNotes\SQL文件\testsql\testsql.sql

​	初始化完成后，再次查看表：

```
mysql> show tables;
+-------------------+
| Tables_in_testsql |
+-------------------+
| dept              |
| emp               |
| salgrade          |
+-------------------+
3 rows in set (0.00 sec)
```

