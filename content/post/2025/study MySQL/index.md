---
title: "洛克学习MySQL"
date: 2025-03-24
lastmod: 2025-03-24
description: 垃圾学校教MySQL 因为每节课时间跨度过长我只有记下来
tags:
  - MySqlx
  - School
  - study
categories:
  -记录学习MySQL
image: 
slug: study-MySQL

---
## 学习过的MySQL指令
```mysql
#1.查看当前正在使用的数据库
SELECT DATABASES();
#2.创建一个学生表：student，字段包含：学号，姓名，性别，年龄，专业，手机号码，入学时间
CREATE TABLE student(
	stu_no VARCHAR(15) COMMENT '学号',
	name VARCHAR(20) COMMENT '姓名',
	gender CHAR(1) COMMENT ' 性别',
	age TINYINT UNSIGNED COMMENT '年龄',
	zhuanye VARCHAR(20) COMMENT '专业',
	phone VARCHAR(20) COMMENT '电话号码',
	entry_date date COMMENT '入学时间'
)COMMENT '学生表';
#3.查看当前表结构  desc 表名；
DESC student;
#4.添加字段：ALTER TABLE 表名 ADD COLUMN 列名 类型 (长度) COMMENT ' ' ，例如：添加身份证号字段 ;
ALTER TABLE student add COLUMN idcard CHAR(18) COMMENT '身份证号';

DESC student;
#5.删除字段：ALTER TABLE 表名 DROP COLUMN 字段名;
ALTER TABLE 表名 DROP COLUMN phone;
#6.修改列名：ALTER TABLE 表名 CHANGE COLUMN 旧列名 新列名 类型，例如：将name修改为stu_name；
#7.修改表名：ALTER TABLE 旧表名 RENAME TO 新表名，例如：将student修改为students;
#8.删除表：DROP TABLE [ IF EXISTS ] 表名;

```
## src部分解释


**针对SELECT DATABASES();**: 在MySQL里面`select`指向查询 `databases` 为数据库的复数

**CREATE TABLE student**中表示创建对应`name`的表这里指向 `student`

**stu_no VARCHAR(15) COMMENT '学号',**  `varchar`前面则输入要表达的数据id 在comment单引号中汉字对应前面数据id的注释解析 ***这里请不要忽略逗号，*** 最后不要忘记添加; ***所有符号都要检查是否都是english状态下***

**varchar和char的不同用法** `char为固定长度` eg:性别只有男和女 则是固定字节用char `narchar非固定长度` 例如国际电话 有不同的前缀 例如cn则是+86别的地区不同就会使得长度非固定则用varchar在括号里填写数据所对应的最大字节

**DESC解析** `DESC`的全称为`DESCRIBE` 直译为描述 在MySQL作用查询表结构数据信息 
```mysql
DESC student
```

这里则是查询并返回 student 表结构的数据

**ALTER TABLE student add COLUMN idcard CHAR(18) COMMENT '身份证号';** 这里的`ALTER TABLE`则是用来修改已经存在的表结构 在这段里面则指向 修改student表结构的数据 后面的`add CILUMN`表明在`student`表中添加一项数据 `idcard CHAR(18) COMMENT '身份证号';` 

**ALTER TABLE 表名 DROP COLUMN phone;** 




