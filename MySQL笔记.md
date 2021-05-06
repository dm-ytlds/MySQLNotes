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

