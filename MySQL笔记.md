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

初始化完成后，再次查看表：

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

查看具体表的信息（表的结构）：

```mysql
mysql> desc dept;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| DEPTNO | int         | NO   | PRI | NULL    |       |
| DNAME  | varchar(14) | YES  |     | NULL    |       |
| LOC    | varchar(13) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.02 sec)
```

查看表内部信息：select属于SQL语句。

```mysql
mysql> select * from dept;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)
```

查看创建表的语句：

```mysql
mysql> show create table emp;
```

简单的查询语句（DQL）

​	语法格式： select 字段名1，字段名1... from 表名;

​	！任何SQL语句都是以“;”结尾；SQL语句不区分大小写。

​	例子：查询员工的工资：（字段可以参与运算！）

```mysql
mysql> select ename,sal * 12 from emp;
+--------+----------+
| ename  | sal * 12 |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 19200.00 |
| WARD   | 15000.00 |
| JONES  | 35700.00 |
| MARTIN | 15000.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
14 rows in set (0.01 sec)
```

​	给查询结果的列重命名：用 as 关键字，且可以省略。重命的名字可以为中文，用 单引号 引用。

```mysql
mysql> select ename,sal * 12 as salyear from emp;
+--------+----------+
| ename  | salyear  |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 19200.00 |
| WARD   | 15000.00 |
| JONES  | 35700.00 |
| MARTIN | 15000.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
14 rows in set (0.00 sec)
```

​	条件查询

​		语法格式：

​			select 字段名1,字段名2... from 表名 where 条件;

​		举例

```mysql
mysql> select ename,sal,deptno from emp where sal > 1000 and (deptno = 20 or deptno = 30);
+--------+---------+--------+
| ename  | sal     | deptno |
+--------+---------+--------+
| ALLEN  | 1600.00 |     30 |
| WARD   | 1250.00 |     30 |
| JONES  | 2975.00 |     20 |
| MARTIN | 1250.00 |     30 |
| BLAKE  | 2850.00 |     30 |
| SCOTT  | 3000.00 |     20 |
| TURNER | 1500.00 |     30 |
| ADAMS  | 1100.00 |     20 |
| FORD   | 3000.00 |     20 |
+--------+---------+--------+
9 rows in set (0.00 sec)
```

​		！当运算符的优先级不确定的时候建议加小括号。

​	in等同于or。注意：in后面括号里面的值不是区间，而是具体的值。

​	模糊查询like。

​		在模糊查询中，必须掌握两个特殊的符号，%和_。%代表任意多个字符； _代表任意1个字符。

​	排序。用关键字order by。默认是升序。asc升序；desc降序。

​		举例：按照工资降序排列，当工资相同时，按照用户名的升序排列。

```mysql
mysql> select ename,sal from emp order by sal desc, ename asc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

​		！越靠前的字段越能起到主导作用。 

​	分组函数()：属于 。

​		count、sum、avg、max、min。所有的分组函数都是对一组数据进行排序的。

​		！分组函数自动忽略_NULL_。

​		只要有_NULL_参与的运算，结果都是_NULL_。

​		<font color=#FF0000>！where后面不能直接用分组函数。因为分组函数一般是在group by或having语句执行结束才能执行，而where语句的执行顺序要优于group by和having语句。</font>可以用子查询语句处理相关的问题。

​	ifnull()空处理函数：属于单行处理函数。

​	group by 和 having

​		group by：按照某个字段或者某些字段进行分组。

​		having：是对分组之后的数据进行再次过滤。

​		案例1：找出每个工作岗位的最高薪资。

```mysql
mysql> select max(sal),job from emp group by job;
+----------+-----------+
| max(sal) | job       |
+----------+-----------+
|  1300.00 | CLERK     |
|  1600.00 | SALESMAN  |
|  2975.00 | MANAGER   |
|  3000.00 | ANALYST   |
|  5000.00 | PRESIDENT |
+----------+-----------+
5 rows in set (0.00 sec)
```

​		！分组函数一般都会和group by联合使用。并且任何一个分组函数都是在group by语句执行结束之后才会执行。当一条语句中没有group by 语句的时候，整张表的数据会自成一组。

​		DQL语句的执行顺序：

```mysql
select			5
	...
from			1
	...
where			2
	...
group by		3
	...
having			4
	...
order by		6
	...
```

​		案例2：找出不同部门不同工作岗位的最高薪资，按部门排序。

```mysql
mysql> select deptno,job,max(sal) from emp group by deptno,job order by deptno;
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     10 | CLERK     |  1300.00 |
|     10 | MANAGER   |  2450.00 |
|     10 | PRESIDENT |  5000.00 |
|     20 | ANALYST   |  3000.00 |
|     20 | CLERK     |  1100.00 |
|     20 | MANAGER   |  2975.00 |
|     30 | CLERK     |   950.00 |
|     30 | MANAGER   |  2850.00 |
|     30 | SALESMAN  |  1600.00 |
+--------+-----------+----------+
9 rows in set (0.00 sec)
```

​		案例3：找出不同部门的最高薪资，要求工资大于2900。

```mysql
mysql> select max(sal),deptno from emp group by deptno having max(sal) > 2900;  这个方式不建议使用，因为会增加分组的数量，效率更低。
mysql> select max(sal),deptno from emp where sal > 2900 group by deptno;	建议使用where先进行一次条件筛选，这样的话，进行分组的数量就会减少，使效率更高。
+----------+--------+
| max(sal) | deptno |
+----------+--------+
|  3000.00 |     20 |
|  5000.00 |     10 |
+----------+--------+
2 rows in set (0.00 sec)
```

​		案例3：找出不同部门的平均薪资，要求平均薪资大于2000。

​		这种情况就不能用where，只能用having进行过滤。

```mysql
mysql> select avg(sal),deptno from emp group by deptno having avg(sal) > 2000;
+-------------+--------+
| avg(sal)    | deptno |
+-------------+--------+
| 2175.000000 |     20 |
| 2916.666667 |     10 |
+-------------+--------+
2 rows in set (0.01 sec)
```

distinct关键字的使用：对查询结果集进行数据去重。distinct只能出现在字段的最前面，如果有多个关键字，则是对其的联合去重。

​	案例：统计岗位的数量

```mysql
mysql> select count(distinct job) from emp;
+---------------------+
| count(distinct job) |
+---------------------+
|                   5 |
+---------------------+
1 row in set (0.02 sec)
```

连接查询：从多张表中进行数据查询。

​	连接查询的分类：
​		按照表的连接方式来划分：

​			内连接：

​				等值连接

​				非等值连接

​				自连接

​			外连接：

​				左外连接

​				右外连接

​			全连接（用得少。了解）

​	关于表的别名（as关键字，可省略）的使用

​		好处：执行效率高；可读性好。

```mysql
笛卡尔乘积现象：两张表连接查询的时候，没有条件就会出现这个现象。
mysql> select e.ename,d.dname from emp e,dept d; 
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | OPERATIONS |
| SMITH  | SALES      |
| SMITH  | RESEARCH   |
| SMITH  | ACCOUNTING |
| ALLEN  | OPERATIONS |
| ALLEN  | SALES      |
| ALLEN  | RESEARCH   |
| ALLEN  | ACCOUNTING |
| ...    | ...        |
+--------+------------+
56 rows in set (0.00 sec)
```

​	在连接查询的时候，加条件进行查询可以避免笛卡尔乘积现象的出现。

​		注意：以上面结果为例，匹配结果还是56次，但显示的只是有效记录。

```mysql
SQL92：
    select
        ...
    from
        ...
    where
        ...
 SQL99:
 	select
 		...
 	from
 		...
 	join
 		...
 	on
 		...
	where
		...
```

​	SQL99比SQL92语法结构更清晰，表的连接条件和后面的where条件分离了。	

​	内连接之等值连接：最大的特点：条件是等量关系。inner join... on...。inner可以省略。

​		案例：查询每个员工的部门编号，要求显示员工名和部门名。

```mysql
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
```

​	内连接之非等值连接：最大的特点：条件不是等量关系。

​		案例：找出每个员工的工资等级，要求显示员工名、工资、工资等级。

```mysql
mysql> select e.ename,e.sal,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+
14 rows in set (0.00 sec)
```

​	内连接之自连接：最大的特点：一张表看成两张表。

​		案例：找出每个员工的上级领导，要求显示员工名和对应的领导名字。

```mysql
mysql> select e.ename as '员工名',b.ename as '领导名' from emp a join emp b on a.mgr=b.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
13 rows in set (0.04 sec)
```

外连接 outer join。outer 可以省略。

​	和内连接的区别：

​		内连接：假设A和B表进行连接，使用内连接的话，凡是A表和B表能够匹配上的记录查询出来，这就是内连接。A、B两张表没有主副之分，平等的。

​		外连接：假设A和B表进行连接，使用内连接的话，A、B两张表中有一张表是主表，一张表是副表，查询以主表中的数据为主。当副表中的数据没有和主表中的数据匹配上，副表自动模拟出NULL与之匹配。

​	左外连接（左连接）：左边的表为主表。left join

​		案例1：找出所有员工的领导。

```mysql
mysql> select a.ename as '员工名',b.ename as '领导名' from emp a left join emp b on a.mgr=b.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| KING   | NULL   |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
14 rows in set (0.00 sec)
```

​	右外连接（右连接）：右边的表为主表。right join

```mysql
mysql> select a.ename as '员工名',b.ename as '领导名' from emp b right join emp a on a.mgr=b.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| KING   | NULL   |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
14 rows in set (0.00 sec)
```

​	案例2：找出哪个部门没有员工。

```mysql
mysql> select d.* from emp e right join dept d on e.deptno=d.deptno where e.empn
o is null;
+--------+------------+--------+
| DEPTNO | DNAME      | LOC    |
+--------+------------+--------+
|     40 | OPERATIONS | BOSTON |
+--------+------------+--------+
1 row in set (0.00 sec)
```

​	案例3：展出每一个员工的部门名称、工资等级、以及上级领导。

```mysql
mysql> select e.ename as '员工',d.dname as '部门名称',s.grade as '工资等级',a.ename as '上级领导' from emp e left join emp a on e.mgr=a.empno join dept d on e.deptno=d.deptno join salgrade s on e.sal between s.losal and s.hisal;
+--------+------------+----------+----------+
| 员工   | 部门名称   | 工资等级 | 上级领导	 |
+--------+------------+----------+----------+
| SMITH  | RESEARCH   |        1 | FORD     |
| ALLEN  | SALES      |        3 | BLAKE    |
| WARD   | SALES      |        2 | BLAKE    |
| JONES  | RESEARCH   |        4 | KING     |
| MARTIN | SALES      |        2 | BLAKE    |
| BLAKE  | SALES      |        4 | KING     |
| CLARK  | ACCOUNTING |        4 | KING     |
| SCOTT  | RESEARCH   |        4 | JONES    |
| KING   | ACCOUNTING |        5 | NULL     |
| TURNER | SALES      |        3 | BLAKE    |
| ADAMS  | RESEARCH   |        1 | SCOTT    |
| JAMES  | SALES      |        1 | BLAKE    |
| FORD   | RESEARCH   |        4 | JONES    |
| MILLER | ACCOUNTING |        2 | CLARK    |
+--------+------------+----------+----------+
14 rows in set (0.01 sec)
```

嵌套子句

​	在from后面嵌套子查询

​		案例：找出每个部门平均薪资的薪资等级。

```mysql
第一步：找出每个部门平均薪资（按照部门编号分组，求平均值）
mysql> select deptno,avg(sal) from emp group by deptno;
+--------+-------------+
| deptno | avg(sal)    |
+--------+-------------+
|     20 | 2175.000000 |
|     30 | 1566.666667 |
|     10 | 2916.666667 |
+--------+-------------+
3 rows in set (0.00 sec)
第二步：按部门的平均薪资划分等级。（将第一步查询结果看成一张新表a，将a表与salgrade表进行连接查询）
SQL92写法：
mysql> select a.deptno as '部门编号',a.avg as '平均薪资',s.grade as '薪资等级' from (select deptno,avg(sal) as avg from emp group by deptno) a,salgrade s where a.avg between s.losal and s.hisal;
SQL99写法：
mysql> select a.deptno as '部门编号',a.avg as '平均薪资',s.grade as '薪资等级' from (select deptno,avg(sal) as avg from emp group by deptno) a join salgrade s on a.avg between s.losal and s.hisal;
+----------+-------------+----------+
| 部门编号 | 平均薪资    | 薪资等级    |
+----------+-------------+----------+
|       20 | 2175.000000 |        4 |
|       30 | 1566.666667 |        3 |
|       10 | 2916.666667 |        4 |
+----------+-------------+----------+
3 rows in set (0.00 sec)

```

​		案例：找出每个部门薪资等级的平均值。这里就不需要将

```mysql
第一步：找出每个部门员工的薪资等级
mysql> select e.ename,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+--------+-------+
| ename  | grade |
+--------+-------+
| SMITH  |     1 |
| ALLEN  |     3 |
| WARD   |     2 |
| JONES  |     4 |
| MARTIN |     2 |
| BLAKE  |     4 |
| CLARK  |     4 |
| SCOTT  |     4 |
| KING   |     5 |
| TURNER |     3 |
| ADAMS  |     1 |
| JAMES  |     1 |
| FORD   |     4 |
| MILLER |     2 |
+--------+-------+
14 rows in set (0.00 sec)
第二步：在第一步的基础上，按部门分组，计算平均等级。
mysql> select e.deptno,avg(s.grade) from emp e join salgrade s on e.sal between s.losal and s.hisal group by e.deptno;
+--------+--------------+
| deptno | avg(s.grade) |
+--------+--------------+
|     20 |       2.8000 |
|     30 |       2.5000 |
|     10 |       3.6667 |
+--------+--------------+
3 rows in set (0.00 sec)
```

​	在select后面嵌套子查询

​		案例：找出每个员工所在的部门名称，要求显示员工名和部门名。

```mysql
第一种方式：
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
第二种方式：
mysql> select e.ename,(select d.dname from dept d where e.deptno=d.deptno) as dname from emp e;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
```

union（可以将查询结果集相拼接）

​	案例：找出工作岗位是SALESMAN和MANAGER的员工。

```mysql
方式1：
mysql> select ename,job from emp where job='SALESMAN' or job='MANAGER';
方式2：
mysql> select ename,job from emp where job in ('SALESMAN','MANAGER');
方式3：
mysql> select ename,job from emp where job='SALESMAN' union select ename,job from emp where job='MANAGER';
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| MARTIN | SALESMAN |
| TURNER | SALESMAN |
| JONES  | MANAGER  |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
+--------+----------+
7 rows in set (0.00 sec)
```

limit（主要用在分页查询中。重点\*****）

​	limit是MySQL特有的，其他数据库中没有，非通用。（oracle中有一个相同的机制，叫做rownum）

​	limit的作用：取结果集中的部分数据。

​	limit语法机制：

​	limit startIndex, length

​		startIndex表示起始位置；

​		length表示取几个。

​	案例：找出工资排名在第4到底9名的员工。（降序取前6个）

```mysql
mysql> select ename,sal from emp order by sal desc limit 3,6;
+--------+---------+
| ename  | sal     |
+--------+---------+
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
+--------+---------+
6 rows in set (0.00 sec)
```

创建表

​	建表的语法格式：

```mysql
create table 表名(
	字段名1 数据类型,
    字段名2 数据类型,
    ...
);
```

​	--->关于MySQL当中字段的数据类型：

| 数据类型 |   类型名称   |                             描述                             |
| :------: | :----------: | :----------------------------------------------------------: |
|   int    |    整数型    |                         java中的int                          |
|  bigint  |   长整数型   |                         java中的long                         |
|  float   |    浮点型    |                     java中的float double                     |
|   char   |  定长字符串  |                        java中的String                        |
| varchar  | 可变长字符串 |    最多255个字符。对应java中的StringBuffer或StringBuilder    |
|   date   |   日期类型   |                                                              |
|   BLOB   | 二进制大对象 | Binary Large Object。存储图片、视频等流媒体信息。java中的Object |
|   CLOB   |  字符大对象  |   Character Large Object。存储较大文本信息。java中的Object   |

​	--->char和vachar如何选择？

​		在实际开发中，当某个字段中的数据长度不发生改变的时候，是定长的，例如：性别，生日等都是采用char。当一个字段的数据长度不确定，例如：简介，姓名等都是采用varchar。

insert语句向表中插入数据

​	语法格式：

```mysql
insert into 表名(字段名1,字段名2,...) values (值1,值2,...);
values后面可以跟多条语句，用逗号隔开。
```

​		要求：字段的数量和值的数量相同，并且数据类型也要对应相同。

表的复制

​	语法格式：

```mysql
create table 表名 as select语句;   将查询结果当做表创建出来。
```

修改表中数据 update

​	语法格式：

```mysql
update 表名 set 字段名1=值1,字段名2=值2,... where 条件;
如果没有条件，则更新整张表的数据。
```

​	案例：将部门10的LOC修改为SHANGHAI，将部门名称修改为RENSHIBU。

```mysql
mysql> update dept set loc='SHANGHAI', dname='RENSHIBU' where deptno=10;
Query OK, 1 row affected (0.12 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from dept;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | RENSHIBU   | SHANGHAI |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)
```

删除数据 delete 删除大表用truncate

​	语法格式：

```mysql
delete from 表名 where 条件;
注意：没有加条件，则全部删除。
truncate table 表名;
表被截断，不可回滚。
```

增删改查有一个术语：CRUD操作。

​	Create（添加） Retrieve（检索） Update（修改） Delete（删除）

！！！约束 constraint

​	约束的作用：保证表中数据的合法性、有效性、完整性。

​	常见的约束有哪些？

​		非空约束：not null。约束的字段不能为空。

​		唯一约束：unique	约束的字段不能重复。

​		主键约束：primary key 约束的字段既不能为空，也不能重复，简称PK。

​		外键约束：foreign key	简称FK。

​		检查约束：check	目前MySQL不支持该约束。

```mysql
建表
create table t_user(
    id int,
    username varchar(255) not null,
    password varchar(255)
);
插入数据
mysql> insert into t_user(id,password) values(1,'123');
出现错误：username 没有默认值。
ERROR 1364 (HY000): Field 'username' doesn't have a default value
mysql> insert into t_user(id,username,password) values(1,'zs','123');
Query OK, 1 row affected (0.01 sec)
```

