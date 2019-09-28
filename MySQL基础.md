# 1、Ubuntu18.04 安装 MySQL

### 安装MySQL：

`sudo apt-get update`

`sudo apt-get install mysql-server`

`sudo apt-get install mysql-client`

`sudo apt-get install libmysqlclient-dev`



### 测试MySQL：

在XShell中，输入语句`sudo mysql -u root -p`回车，输入用于密码和数据库密码，登录数据库；



###MySQL基本操作（在XShell中操作，实际很少会在服务器上直接操作）

- 以root用户身份登录服务器中的MySQL：`sudo mysql -u root –p`

- 服务启动和关闭：`service mysql start` 	和`service mysql stop`

- 查看mysql用户（安装MySQL时，系统自动创建该用户）的进程`ps -u mysql`

  ​				  PID TTY          TIME CMD

   				  7586 ?       		00:00:50 mysqld		守护进程

- 如果登录期间发生异常，无法登录：使用root杀死mysql的后台进程`kill -9 PID`

- 修改密码：`mysql> set password=password('123456'); 	将密码设置为：123456`

- 退出登录： `quit/exit`





# 2、对数据库的增删改查

登录数据库中（`sudo mysql -uboot -p`）后:

- 创建数据库：`create database dbdj;`默认字符集不带中文；

- 删除数据库：`drop database lajidb;`

- 可用语句`alter database dbdj character set utf8;`改为包含中文

- 查看数据库：`show databases`;

- 查看某个数据库的创建方式：`show create database dbdj;`

  



# 3、对表的增删改查

对表本身进行操作：创建，查看，修改，删除

### 3.1 **创建表**

- 选择数据库：`use mydb2;`
- 创建表：`create table t1 (id int, name varchar(30))；`  
- 创建一个员工表：`create table employee(empno int, ename varchar(20), sal int);`

> mysql中的数据类型：
>
> ![img](file:///C:\Users\84746\AppData\Local\Temp\ksohtml13120\wps2.jpg) 
>
> bit     1位　	可以指定位数，如：bit(3)
>
> int     2字节 	可以指定最大位数，如：int<4>　最大为4位的整数
>
> float   2个字节　可以指定最大的位数和最大的小数位数，如：float<5,2> 最大为一个5位的数，小数位最多2位 
>
> double　4个字节　可以指定最大的位数和最大的小数位数，如：float<6,4> 最大为一个6位的数，小数位最多4位
>
> char　  必须指定字符数,如char(5) 为不可变字符　即使存储的内容为'ab',也是用5个字符的空间存储这个数据
>
> varchar　必须指定字符数,如varchar(5) 为可变字符　如果存储的内容为'ab',占用2个字符的空间；如果为'abc',则占用3个字符的空间
>
> text: 大文本(大字符串)
>
> blob：二进制大数据　如图片，音频文件，视频文件
>
> date: 日期　如：'1921-01-02'
>
> datetime: 日期+时间　如：'1921-01-02 12:23:43'
>
> timeStamp: 时间戳，自动赋值为当前日期时间

### 3.2 **查看表**

- 查看所有的表：`show tables;`
- 查看指定表的创建语句：`show create table employee;`
- 显示指定表的结构：`desc employee;`

### 3.3 **修改表**

- 更改表名：    `rename table employee to worker;`
- 增加一个字段：`alter table employee add column height double;` （column关键字在Oracle中，添加则语法错误）
- 修改一个字段：`alter table employee modify column height float;`
- 修改字段名: `alter table employee change column height height1 float;`
- 删除一个字段：`alter table employee drop column height1;`
- 修改表的字符集:`alter table employee character set gbk;`

### 3.4 **删除表**

- 删除employee表：`drop table employee;`(MySQL中会直接删除，不能使用purge，添加会出现语法错误)



> 注意：字段不区分大小写，库名大小写敏感



# 4、对数据的增删改查

### 4.1 **create数据**

创建一个员工表，新建employee表并向表中添加一些记录：

```
create table employee(

	id int,

	name varchar(20),

	sex int,

	birthday date,

	salary double,

	entry_date date,

	resume text

);
```

- `insert into employee values(1,'张三',1,'1983-04-27',15000,'2012-06-24','一个大牛');`
- `insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(2,'李四',1,'1984-02-22',10000,'2012-07-24','一个中牛');`

- `insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(3,'王五',0,'1985-08-28',7000,'2012-08-24','一个小虾');`

### 4.2 **update数据**

- 将所有员工薪水都增加500元：`update employee set salary=salary+500;`
- 将王五的员工薪水修改为10000元，resume改为也是一个中牛：

`update employee set salary=10000, resume='也是一个中牛' where name='王五';`

### 4.3 **delete数据**

- 删除表中姓名为王五的记录：`delete from employee where name='王五';`	【注意from不能省略】
- 删除表中所有记录：`delete from employee;` 
- 使用truncate删除表中记录：`truncate employee;`--无条件 效率高

### 4.4 **retrieve数据**

- 查看表中全部内容：`select * from employee;`
- 按需查找：`select id, name as "名字", salary "月薪", salary*12 年薪  from employee where id >=2;`



