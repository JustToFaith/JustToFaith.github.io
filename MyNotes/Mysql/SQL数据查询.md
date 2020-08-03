# SQL基本的数据查询操作（单表查询）

> SQL语句的查询操作是数据库的核心操作，本篇博客讲述用SELECT语句进行单表查询操作

1. 其`SELECT`查询的一般格式为：

   ``` sql
   SELECT [ALL | DISTINCT] <目标列表达式> [,<目标列表达式>] ...
   FROM <表名或视图名> [,<表名或视图名>] | (<SELECT 语句>) [AS] <别名>
   [WHERE <条件表达式>]
   [GROUP BY <列名1> [HAVING <条件表达式>]]
   [ORDER BY <列名2>  [ASC | DESC]]
   ```

   方括号`[]`中的为可选语句，`WHERE`是条件查询,`GROUP BY`对查询的结果按某一列进行分组,`ORDER BY`对查询的结果按某一列进行排序。当然，具体使用哪种方式进行查询需要我们根据实际情况来选择。

2. 

```sql
create database stu_course charset=utf8;


create table Student
(Sno CHAR(9) primary key,
Sname char(20) unique,
Ssex char(2),
Sage smallint,
 Sdept char(20));


 create table Course
(Cno char(4) primary key,
Cname char(40) not null,
Cpno char(4),
Ccredit smallint,
foreign key(Cpno) references Course(Cno));

create table SC
(Sno char(9),
 Cno char(9),
Grade smallint,
primary key(Sno,Cno),
foreign key(Sno)references Student(Sno),
foreign key(Cno)referecces Course(Cno));


insert into Student 
(Sno,Sname,Ssex,Sage,Sdept)
values
('201215121','李勇','男',20,'CS'),
('201215122','刘晨','女',19,'CS'), 
('201215123','王敏','女',18,'MA'), 
('201215125','张立','男',19,'IS');


insert into Course
(Cno,Cname,Cpno,Ccredit)
values
('1','数据库','5',4),
('2','数学','',2),
('3','信息系统','1',4),
('4','操作系统','6',3),
('5','数据结构','7',4),
('6','数据处理','',2),
('7','PASCAL'语言,'6',4);
```

