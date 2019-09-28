# 1、时间

## now()

**现在**

`mysql> select now() from dual;`
+---------------------+
| now()               |
+---------------------+
| 2019-09-21 22:12:57 |
+---------------------+
1 row in set (0.00 sec)



**有轻微错误：现在的前一秒、现在、现在的后一秒**

`mysql> select now()-1 ,now(),now()+1 from dual;`
+----------------+---------------------+----------------+
| now()-1        | now()               | now()+1        |
+----------------+---------------------+----------------+
| 20190921221430 | 2019-09-21 22:14:31 | 20190921221432 |
+----------------+---------------------+----------------+
1 row in set (0.00 sec)



## date_add(now(),interval 偏移量 年月日)

**现在的前一天、现在、现在的后一天**

`mysql> select date_add(now(),interval -1 day),now(),date_add(now(),interval 1 day) from dual;`
+---------------------------------+---------------------+--------------------------------+
| date_add(now(),interval -1 day) | now()               | date_add(now(),interval 1 day) |
+---------------------------------+---------------------+--------------------------------+
| 2019-09-20 22:19:40             | 2019-09-21 22:19:40 | 2019-09-22 22:19:40            |
+---------------------------------+---------------------+--------------------------------+
1 row in set (0.00 sec)



**一年后的现在**

`mysql> select date_add(now(),interval 1 year) from dual;`
+---------------------------------+
| date_add(now(),interval 1 year) |
+---------------------------------+
| 2020-09-21 22:28:20             |
+---------------------------------+
1 row in set (0.00 sec)



## addtime(now(),秒数)

**11秒后的时间、现在** 

`mysql> select addtime(now(),11),now() from dual;`
+---------------------+---------------------+
| addtime(now(),11)   | now()               |
+---------------------+---------------------+
| 2019-09-21 22:23:51 | 2019-09-21 22:23:40 |
+---------------------+---------------------+
1 row in set (0.00 sec)

**11秒后的时间、现在** 

`mysql> select addtime(now(),'0:1:1'),now() from dual;`
+------------------------+---------------------+
| addtime(now(),'0:1:1') | now()               |
+------------------------+---------------------+
| 2019-09-21 22:35:19    | 2019-09-21 22:34:18 |
+------------------------+---------------------+
1 row in set (0.00 sec)





# 2、字符串

#### 字符串拼接：

`select concat('hello ', 'mysql ', 'haha ', 'hehe ') from dual;`   

> Oracle默认只能拼两个，如需多个拼接可以使用嵌套。
>
> select 'hello ' || 'mysql ' from dual;	‘||’ 在 MySQL不可以使用。



![img](file:///C:\Users\84746\AppData\Local\Temp\ksohtml13120\wps3.jpg) 



#### 日期转字符串：

在MySQL中没有`to_date`函数，进行日期转换需使用`date_format()`来代替。

`select date_format('2013-5-11', 'yyyy-mm-dd') from dual;`  在Oracle中的‘yyyy-mm-dd’MySQL下不支持。

`select date_format(now(), '%Y-%m-%d') from dual;` 			 y和Y不一样。

+--------------------------------+
| date_format(now(), '%Y-%m-%d') |
+--------------------------------+
| 2019-09-21                     |
+--------------------------------+
1 row in set (0.00 sec)

​	

`select date_format(now(), '%Y-%c-%d %h:%i:%s') from dual;` 	 c和m、M不一样，所以yyyy-mm-dd hh24:mi:ss格式在MySQL中对应'%Y-%c-%d %h:%i:%s'

+-----------------------------------------+
| date_format(now(), '%Y-%c-%d %h:%i:%s') |
+-----------------------------------------+
| 2019-9-21 10:41:53                      |
+-----------------------------------------+
1 row in set (0.00 sec)



#### 字符串转日期：

`select str_to_date('2013-6-04 05:14:15' , '%Y-%c-%d %h:%i:%s') from dual;`

+---------------------------------------------------------+
| str_to_date('2013-6-04 05:14:15' , '%Y-%c-%d %h:%i:%s') |
+---------------------------------------------------------+
| 2013-06-04 05:14:15                                     |
+---------------------------------------------------------+
1 row in set (0.00 sec)



# 3、数学相关函数

![img](file:///C:\Users\84746\AppData\Local\Temp\ksohtml13120\wps4.jpg) 

进制转换：

`mysql> select conv(10,10,2),conv(10,10,16) from dual;`
+---------------+----------------+
| conv(10,10,2) | conv(10,10,16) |
+---------------+----------------+
| 1010          | A              |
+---------------+----------------+
1 row in set (0.00 sec)



1.2 **多表查询**

创建多表查询案例——MySQL建表_仿照oracle建表脚本.sql  【mysql> source 绝对路径/脚本名】

Oracle中连接方法：

​		等值连接

​		不等值连接	

​		外连接

​		自连接

MySQL 使用SQL99标准的连接查询（JOIN..ON..）

### 1.2.1 **交叉连接：**

​	叉集，即笛卡尔集

​		select e.*, d.*

​		from emp e cross join dept d

​	无连接条件

### 1.2.2 **满外联接**

​	任一边有值就会显示。

​		select e.*, d.*

​		from emp e full outer join dept d		

​		on e.deptno=d.deptno

​	也可以省略outer关键字

### 1.2.3 **内连接**

​	只返回满足连接条件的数据（两边都有的才显示）。 对应等值连接

​		select e.*, d.*

​		from emp e inner join dept d

​		on e.deptno=d.deptno

​	也可以省略inner关键字。

​	对应Oracle写法：

select e.*, d.*

from emp e , dept d

where e.deptno=d.deptno

### 1.2.4 **左外连接**

​	左边有值才显示。

​		select e.*, d.*

​		from emp e left outer join dept d

​		on e.deptno=d.deptno

​	也可以省略outer关键字

### 1.2.5 **右外连接**

​	右边边有值才显示。

​		select e.*, d.*

​		from emp e right outer join dept d

​		on e.deptno=d.deptno

​	也可以省略outer关键字

​	【注意】SQL99中，外链接取值与关系表达式=号左右位置无关。取值跟from后表的书写顺序有关。 

“xxx left outer join yyy” 则为取出xxx的内容。

​			“xxx right outer join yyy”则为取出yyy的内容

### 1.2.6 **对比练习**

#### **题目****1：**

查询员工信息,员工号,姓名,月薪,部门名称

select ...

from emp e, dept d

where e.deptno = d.deptno;

Oracle实现：

​		select e.deptno, e.ename, e.sal, d.dname

​		from emp e, dept d

​		where e.deptno = d.deptno

SQL99实现：

​		select e.deptno, e.ename, e.sal, d.dname

​		from emp e inner join dept d

​		on e.deptno = d.deptno

​	对比记忆规律：

​		“,” → [inner] join 

​		where → on	

​	对比结论：mysql能识别Oracle中使用 = 连接的书写方法。

#### **题目2：**

统计各个部门员工总人数

分析：部门包括10/20/30 → 分组

各部门人数 → 多表 

select ...

from emp e, dept d

where d.deptno = e.deptno

group by ...			

(注意：group by后面出现的子集应在select下进行检索)

实现为：

select d.deptno, d.dname, count(e.deptno)

from dept d, emp e

where d.deptno = e.deptno

group by d.deptno, d.dname 

​	查询发现没有40号部门。此时应使用外链接保存一侧结果。

oracle实现：

select d.deptno, d.dname , count(e.deptno)

from dept d, emp e

where d.deptno = e.deptno (+)

group by d.deptno, d.dname 

SQL99实现：

select d.deptno, d.dname , count(e.deptno)

from dept d left outer join emp e

on d.deptno = e.deptno

group by d.deptno, d.dname 

对比记忆规律：

​		“,”→ left**/**right outer join 

​		where → on	

​	结论：oracle的语法(+) mysql不支持

### 1.2.7 **自连接**

查询员工、老板信息，显示: xxx的老板是xxx

分析：将一张emp表当成两张表看待：员工表、老板表（员工表的老板 是 老板表的员工）

1. 先按照oracle语法写	

select e.ename, b.ename

from emp e, emp b

where e.mgr = b.empno

2. 完善显示格式concat

select concat( e.ename, ' 的老板是 ',  b.ename )

from emp e, emp b

where e.mgr = b.empno

3. 显示king的老板

select concat( e.ename, ' 的老板是 ',  b.ename )

from emp e, emp b

where e.mgr = b.empno (+)

4. 改用MySQL支持的SQL99语法

select concat( e.ename, ' 的老板是 ',  b.ename )

from emp e left outer join emp b

on e.mgr = b.empno ;

5. 滤空修正nvl

select concat( e.ename, ' 的老板是 ',  nvl(b.ename, '他自己' ) )

from emp e left outer join emp b

on e.mgr = b.empno ;

结论 nvl 在mysql下不能使用： ERROR 1305 (42000): FUNCTION mydb61.nvl does not exist

6. 滤空修正 ifnull

select concat( e.ename, ' 的老板是 ',  ifnull(b.ename, '他自己' ) )    

from emp e left outer join emp b

on e.mgr = b.empno ;

注意：

​	Oracle中有一个通用函数，与MYSQL中的ifnull函数名字相近：

​	nullif：如nullif(a, b) 当 a = b 时返回null, 不相等的时候返回a值。nullif('L9,999.99', 'L9,999.99')

​	mysql中nullif（）函数也存在。

## 1.3 **表的约束**

​	*定义主键约束　primary key:	不允许为空，不允许重复

​	*定义主键自动增长　auto_increment

​	*定义唯一约束　unique

​	*定义非空约束　not null

​	*定义外键约束　constraint ordersid_FK foreign key(ordersid) references orders(id)

​	*删除主键：alter table tablename drop primary key ;

MySQL中约束举例：

​	create table myclass (

​		id INT(11) primary key auto_increment,

​		name varchar(20) unique

​	);

​	create table student (

​		id INT(11) primary key auto_increment, 

​		name varchar(20) unique,

​		passwd varchar(15) not null,

​		classid INT(11),  							

​		constraint stu_classid_FK foreign key(classid) references myclass(id)

​	);

​	check约束在MySQL中语法保留，但没有效果。

​    可以通过SELECT * FROM information_schema.`TABLE_CONSTRAINTS`;查看表的约束。

# 2 **mysql中文乱码问题**

三层因素：

因素1： MySQL自身的设计

【实验步骤1】：	

mysql> show variables like 'character%';	 查看所有应用的字符集

【实验步骤2】：	

$ mysql -uroot -p123456 --default_character_set=gbk 指定字符集登录数据库

​	mysql> show variables like 'character%';

​	影响了与客户端相关联的 3处 (最外层)

在这种状态下执行use mydb2;

​	mysql> select * from employee;			

​	查看输出，会出现乱码。

​	原来的三条数据，是以utf8的形式存储到数据库中，当使用gbk连接以后，数据库仍按照utf8的形式将数据返回，出错。

【实验步骤3】：

在该环境下插入带有中文的一行数据。

​	mysql> insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(10,'张三疯',1,'1983-09-21',15000,'2012-06-24','一个老牛');

​	ERROR 1366 (HY000): Incorrect string value: '\x80\xE4\xB8\xAA\xE8\x80...' for column 'resume' at row 1

因素2：操作系统的语言集

​	linux操作系统 是一个 多用户的操作

​	[root@localhost ~]# cat /etc/sysconfig/i18n

​	LANG="zh_CN.UTF-8"	

​	操作系统的菜单按照zh_CN显示,  文件存储按照utf8

​	linux操作系统语言环境 和 用户的配置的语言环境LANG 相互影响

​	[mysql01@localhost ~]$ echo $LANG

​	zh_CN.UTF-8

【实验步骤4】： 

修改用户下的.bash_profile 中的LANG，屏蔽操作系统的LANG设置。再查数据库

​		mysql> select * from employee;

结论： 用户的LANG设置，影响了应用程序的语言环境,导致myql的语言环境发生了改变：

​	mysql> show variables like 'character%';

​	在此环境下，检索中文会出现乱码。

【实验步骤5】：在上述环境之下，向数据库中插入中文。

insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(5,'张三疯',1,'1987-05-21',15000,'2014-06-24','一个老牛');

数据能插入到数据库中，没 有 报 任 何 错 误！但显示不正确。

因素3：文件存储格式 

 