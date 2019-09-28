# MySQL基础练习

####  创建一个学生表，插入数据

```
create table student(

id int,

name varchar(20),

chinese int,

english int,

math int

);
```

`insert into student(id,name,chinese,english,math) values(1, '范建',80,85,90);`

`insert into student(id,name,chinese,english,math) values(2,'罗况',90,95,95);`

`insert into student(id,name,chinese,english,math) values(3,'杜子腾',80,96,96);`

`insert into student(id,name,chinese,english,math) values(4,'范冰',81,97,85);`

`insert into student(id,name,chinese,english,math) values(5,'申晶冰',85,84,90);`

`insert into student(id,name,chinese,english,math) values(6,'郝丽海',92,85,87);`

`insert into student(id,name,chinese,english,math) values(7,'郭迪辉',75,81,80);`

`insert into student(id,name,chinese,english,math) values(8,'拎壶冲',77,80,79);`

`insert into student(id,name,chinese,english,math) values(9,'任我行',95,85,85);`

`insert into student(id,name,chinese,english,math) values(10,'史泰香',94,85,84);`

#### 基础SQL

查询表中所有学生的信息

`select * from student;`



查询表中所有学生的姓名和对应的英语成绩

`select name,english from student;`



过滤表中重复数据

`select english from student;`

`select DISTINCT english from student;`  DISTINCT可去过滤english中重复的值

`select DISTINCT english,name from student;`DISTINCT可去过滤english和name中同时重复的数据

`select english+chinese+math from student;` 求总分，显示的段名为english+chinese+math

`select english+chinese+math as 总分 from student;`求总分，显示的段名为总分

`select name,english+chinese+math as 总分 from student;`求总分，显示的段名为总分；同时显示姓名列



在所有学生英语分数上加10分特长分。

`select name,english+10 from student;`



统计每个学生的总分

`select english+chinese+math from student;`



使用别名表示学生分数

`select name,english+chinese+math as 总分 from student;`

`select name,english+chinese+math 总分 from student;`



查询姓名为范冰的学生成绩

`select * from student where name='范冰';`



查询英语成绩大于90分的同学

`select * from student where english>90;`



查询总分大于250分的所有同学

`select * from student where english+chinese+math>250;`



查询英语分数在 85－95之间的同学

`select * from student where english>=85 and english<=95;`

`select * from student where english between 85 and 95;`



查询数学分数为84,90,91的同学。

`select * from student where math=84 or math=90 or math=91;`

`select * from student where math in(84,90,91);`



查询所有姓范的学生成绩。

`select * from student where name like '范%';`



查询数学分>85，语文分>90的同学。

`select * from student where math>85 and chinese>90;`



对数学成绩排序后输出。

`select * from student order by math;`



对总分排序后输出，然后再按从高到低的顺序输出

`select * from student order by math+chinese+english desc;`



对姓范的学生成绩排序输出

`select * from student where name like '范%' order by math+chinese+english desc;`

`select name, math+chinese+english from student where name like '范%' order by math+chinese+english desc;`



统计一个班级共有多少学生？

`select count(*) from student;`

`select count(*) as 总人数 from student;`



统计数学成绩大于90的学生有多少个？

`select count(*) from student where math>90;`



统计总分大于250的人数有多少？

`select count(*) from student where math+chinese+english>250;`



统计一个班级数学总成绩？

`select sum(math) from student;`



统计一个班级语文、英语、数学各科的总成绩

`select sum(math), sum(chinese), sum(english) from student;`



统计一个班级语文、英语、数学的成绩总和

`select sum(math+chinese+english)from student;`

`select sum(math)+sum(chinese)+sum(english) from student;`



求一个班级数学平均分？

`select avg(math) from student;`



求一个班级总分平均分

`select avg(math+chinese+english)from student;`

`select avg(math)+avg(chinese)+avg(english) from student;`



求班级最高分和最低分

`select max(math+chinese+english),min(math+chinese+english) from student;`



#### 分组数据

为学生表，增加一个班级列，练习分组查询。		

`alter table student add column class_id int;`		

> 注意语法：Oracle中不能有“column”关键字，MySQL中有没有“column”都可以执行。



更新表：

`update student set class_id=1 where id<=5;`

`update student set class_id=2 where id>5;`

(同`update student set class_id=2 where id between 6 and 10;`)



查出各个班的总分，最高分。

`select class_id,sum(chinese+english+math),max(chinese+english+math) from student group by class_id;`

求各个班级 英语的平均分：

`select class_id, avg(english) from student group by class_id;`



如根据组函数的语法要求，将select后增加name列，而不加至group by 之后：

`select name, class_id, avg(english) from student group by class_id;`

会报错，因为学生的name都是各不相同的，理论应生成学生个数的行，但按班级分组，只能分两个班级。





查询出班级总分大于1300分的班级ID

`select class_id from student group by class_id having sum(math+chinese+english)>1300;`（正常运行）

`select class_id from student where sum(math+chinese+english)>1300 group by class_id ;`（会报错）

对于**组函数**的应用与Oracle类似，可以应用于**Having**中，但不能用于where子句中。