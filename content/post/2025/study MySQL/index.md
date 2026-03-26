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
## src 部分解释

**针对 SELECT DATABASES();**  
在 MySQL 里面 `SELECT` 指向查询，`DATABASES` 为数据库的复数，用于查看当前 MySQL 中所有数据库。

**CREATE TABLE student**  
表示创建对应名称的表，这里指向 `student`。

**stu_no VARCHAR(15) COMMENT '学号',**  
`VARCHAR` 前面填写要表达的数据最大字节长度，`COMMENT '学号'` 为该字段添加注释。  
***请不要忽略逗号，最后不要忘记添加分号；所有符号必须是英文输入状态。***

**VARCHAR 和 CHAR 的不同用法**  
- `CHAR` 为固定长度，例如性别只有男和女，则长度固定，可用 `CHAR(1)`。  
- `VARCHAR` 为非固定长度，例如国际电话区号，不同国家前缀不同，如 +86、+1、+81，因此长度不固定，使用 `VARCHAR(n)`，括号内填写最大可能字节。

**DESC 解析**  
`DESC` 的全称为 `DESCRIBE`，直译为“描述”。在 MySQL 中用于查询表结构信息。  
示例：  
```mysql
DESC student;

```

**ALTER TABLE student ADD COLUMN idcard CHAR(18) COMMENT '身份证号';**  
`ALTER TABLE` 用于修改已经存在的表结构。  
`ADD COLUMN` 表示在 `student` 表中新增字段：  


测试区域
中文 `cite` 中文<br>
english `cite` english<br>
english `test` english<br>
中文 `test` 中文<br>
