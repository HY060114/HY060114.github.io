---
title: "洛克学习MySQL"
date: 2025-03-24
lastmod: 2025-03-27
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
## student 表
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
ALTER TABLE student DROP COLUMN phone;
#6.修改列名：ALTER TABLE 表名 CHANGE COLUMN 旧列名 新列名 类型，例如：将name修改为stu_name；
ALTER TABLE student CHANGE COLUMN name stu_name VARCHAR(50);
#7.修改表名：ALTER TABLE 旧表名 RENAME TO 新表名，例如：将student修改为students;
ALTER TABLE student RENAME TO students;
#8.删除表：DROP TABLE [ IF EXISTS ] 表名;
DROP TABLE IF EXISTS student;

```
## src 解释

**针对 SELECT DATABASES();**  
在 MySQL 里面 `SELECT` 指向查询，`DATABASES` 为数据库的复数，用于查看当前 MySQL 中所有数据库

**CREATE TABLE student**  
表示创建对应名称的表，这里表指向 `student`

**stu_no VARCHAR(15) COMMENT '学号',**  
`VARCHAR` 前面填写要表达的数据最大字节长度，`COMMENT '学号'` 为该字段添加注释  
***请不要忽略逗号，最后不要忘记添加分号；所有符号必须是英文输入状态***

**VARCHAR 和 CHAR 的不同用法**  
- `CHAR` 为固定长度，例如性别只有男和女，则长度固定，可用 `CHAR(1)`  
- `VARCHAR` 为非固定长度，例如国际电话区号，不同国家前缀不同，如 +86、+1、+81，因此长度不固定，使用 `VARCHAR(n)`，括号内填写最大可能字节

**DESC 解析**  
`DESC` 的全称为 `DESCRIBE` 直译为 描述 在 MySQL 中用于查询表结构信息  
示例：  
```mysql
DESC student;

```

**ALTER TABLE student ADD COLUMN idcard CHAR(18) COMMENT '身份证号';**  
`ALTER TABLE` 用于修改已经存在的表结构  
`ADD COLUMN` 表示在 `student` 表中新增字段：  

**ALTER TABLE student DROP COLUMN 'phone'**  
`ALTER TABLE` 用于修改已经存在的表结构  
`DROP COLUMN` 表示删除 `student` 表结构中某字段  → 在这段src中指向 `phone` 字段  



## book 表

```mysql
-- 创建图书表
CREATE TABLE book(
    book_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '图书ID（主键，自增）',
    isbn VARCHAR(20) UNIQUE NOT NULL COMMENT '国际标准书号（唯一，非空）',
    title VARCHAR(200) NOT NULL COMMENT '书名（非空）',
    author VARCHAR(100) NOT NULL COMMENT '作者（非空）',
    publisher VARCHAR(100) COMMENT '出版社（无约束，允许为空）',
    publish_year YEAR COMMENT '出版年份',
    price DECIMAL(8,2) COMMENT '价格（无CHECK约束）',
    stock INT NOT NULL COMMENT '库存数量（整数，不能为空）',
    category VARCHAR(50) COMMENT '分类（如：技术、小说）',
    language VARCHAR(20) COMMENT '语言（如：中文、英文）'
) COMMENT '图书表';

-- 插入第1条数据，包含isbn, title, author, publisher, publish_year, price, stock, category, language
INSERT INTO book(isbn, title, author, publisher, publish_year, price, stock, category, language)
VALUES('978-7-111-12345-6', 'MySQL从入门到精通', '张三', '机械工业出版社', 2022, 89.00, 10, '技术', '中文');

-- 插入第2条数据，包含isbn, title, author, publish_year, price, stock, category, language（缺少publisher字段）
INSERT INTO book(isbn, title, author, publish_year, price, stock, category, language)
VALUES('978-7-123-12345-1', 'Java程序设计', '翠花', 2025, 109.00, 6, '语言', '中文');

-- 插入第3条数据，包含isbn, title, author, stock（缺少publisher, publish_year, price, category, language字段）
INSERT INTO book(isbn, title, author, stock)
VALUES('978-7-302-12345-3', 'Python编程入门', '李四', 20);

-- 插入第4条数据，包含isbn, title, author, publish_year, price, stock（缺少publisher, category, language字段）
INSERT INTO book(isbn, title, author, publish_year, price, stock)
VALUES('978-7-115-12345-5', '数据结构与算法', '王五', 2023, 59.00, 15);

-- 插入第5条数据，包含isbn, title, author, publish_year, price, language（缺少publisher, stock, category字段）
-- 注意：stock为NOT NULL字段，必须提供值，这里补充默认值0
INSERT INTO book(isbn, title, author, publish_year, price, stock, language)
VALUES('978-7-121-12345-8', '计算机网络', '赵六', 2024, 79.00, 0, '中文');

UPDATE book SET author = '张凌赫' WHERE book_id = 1;
UPDATE book SET author = '小昭', publish_year = 1990 WHERE book_id = 1;
UPDATE book SET price = price + 10 WHERE book_id = 2;
UPDATE book SET language = '英文' WHERE category = '技术';
UPDATE book SET publisher = '清华大学出版社', price = 45.00 WHERE book_id = 3;
UPDATE book SET stock = stock + 2 WHERE isbn = '978-7-123-12345-6';
UPDATE book SET stock = stock + 5 WHERE stock < 12;
UPDATE book SET author = '黑狗', language = '法文' WHERE author = '翠花';
UPDATE book SET publish_year = 2021, category = '技术' WHERE book_id = 4;
UPDATE book SET language = '中文' WHERE language IS NULL;
UPDATE book SET publisher = '人民出版社', publish_year = '2020' WHERE publisher IS NULL AND publish_year IS NULL;

SELECT * FROM book WHERE book_id = 1;
SELECT * FROM book WHERE category = '技术' AND language = '英文';

SELECT * FROM book;
```



## emp表

```mysql
CREATE TABLE emp (
    id INT COMMENT '编号',
    workno VARCHAR(10) COMMENT '工号',
    name VARCHAR(18) COMMENT '姓名',
    gender CHAR(1) COMMENT '性别',
    age TINYINT UNSIGNED COMMENT '年龄',
    idcard CHAR(18) COMMENT '身份证号',
    workaddress VARCHAR(50) COMMENT '工作地址',
    entrydate DATE COMMENT '入职时间'
) COMMENT='员工表';

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (1, '0001', '柳岩', '女', 28, '123456789012345678', '北京', '2000-01-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (2, '0002', '张无忌', '男', 18, '123456789012345678', '北京', '2005-03-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (3, '0003', '韦一笑', '男', 38, '123456789712345678', '上海', '2005-08-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (4, '0004', '赵敏', '女', 18, '123456757123456780', '北京', '2009-12-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (5, '0005', '小昭', '女', 16, '123456769012345678', '上海', '2007-07-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (6, '0006', '杨逍', '男', 28, '123456789312345678', '北京', '2000-01-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (7, '0007', '范瑶', '男', 40, '123456789212345678', '北京', '2005-08-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (8, '0008', '黛绮丝', '女', 38, '123456157123456780', '天津', '2015-05-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (9, '0009', '范凉凉', '女', 45, '123156789012345678', '北京', '2010-04-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (10, '0010', '陈友谅', '男', 53, '123456789612345678', '上海', '2011-01-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (11, '0011', '张士诚', '男', 55, '123567897123456780', '江苏', '2015-05-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (12, '0012', '常遇春', '男', 32, '123446757152345678', '北京', '2004-02-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (13, '0013', '张三丰', '男', 88, '123656789012345678', '江苏', '2020-11-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (14, '0014', '灭绝', '女', 65, '123456789112345678', '西安', '2013-05-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (15, '0015', '胡青牛', '男', 70, '123456749712345678', '西安', '2018-04-01');

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (16, '0016', '周芷若', '女', 18, NULL, '北京', '2012-06-01');


/*
  44  #1.DQL英文全称是Data Query Language(数据查询语言)，数据查询语言，用来查询数据库中表的记录。
  45  
  46  #2.在一个正常的业务系统中，查询操作的频次要要远高于增删改的，当我们去访问企业官网、电商网站，在这些
  47  #网站中所看到的数据，实际都是需要先从数据库中查询并展示的。
  48  
  49  #一、基本查询：在基本查询的DQL语句中，不带任何的查询条件，查询的语法如下：
  50  #1). 查询多个字段
  51  #   SELECT 字段1, 字段2, 字段3 ... FROM 表名 ;
  52  
  53  #2). 字段设置别名
  54  #   SELECT 字段1 [ AS 别名1 ] , 字段2 [ AS 别名2 ] ... FROM 表名;
  55  #   SELECT 字段1 [ 别名1 ] , 字段2 [ 别名2 ] ... FROM 表名;
  56  
  57  #3). 去除重复记录
  58  #   SELECT DISTINCT 字段列表 FROM 表;
  59  
  60  #案例练习
  61  # A. 查询指定字段 name, workno, age并返回
  62  # B. 查询返回所有字段
  63  # C. 查询所有员工的工作地址,起别名
  64  # D. 查询公司员工的上班地址有哪些(不要重复)
  65  # E.查询公司员工的性别有哪些(不要重复)
*/

SELECT age FROM emp;
SELECT age,gender  FROM emp;
SELECT * FROM emp;

SELECT name AS '姓名' FROM emp;
SELECT gender AS '性别' ,idcard '身份证' from emp;

SELECT DISTINCT gender AS '性别' FROM emp;

#二、条件查询语法：SELECT 字段列表 FROM 表名 WHERE 条件列表 ;
#比较运算符：=, >, <, >=, <=, !=, BETWEEN..AND..., LIKE模糊匹配（配合 % ，_）, IN, IS NULL
#逻辑运算符：
    # 1) AND 或 &&:并且 (多个条件同时成立),
		# 2) OR或 ||:或者 (多个条件任意一个成立)
	  # 3)	NOT 或 !:非 , 不是 
#运算符优先级：NOT > AND > OR，建议使用括号 () 明确逻辑，避免歧义。
#字符串和日期值需用单引号引起来，数值可直接写。

# A. 查询年龄等于 88 的员工
SELECT * FROM emp WHERE age = 88;

# B. 查询年龄小于 20 的员工信息
SELECT * FROM emp WHERE age < 20;

# C. 查询年龄小于等于 20 的员工信息
SELECT * FROM emp WHERE age <= 20;

# D. 查询没有身份证号的员工信息
SELECT * FROM emp WHERE idcard IS NULL;

# E. 查询有身份证号的员工信息
SELECT * FROM emp WHERE idcard IS NOT NULL;

# F. 查询年龄不等于 88 的员工信息
SELECT * FROM emp WHERE age <> 88;

# G. 查询年龄在15岁(包含)到20岁(包含)之间的员工信息
SELECT * FROM emp WHERE age BETWEEN 15 AND

# H. 查询性别为女且年龄小于25岁的员工信息
SELECT * FROM emp WHERE gender = '女' AND age < 25;

# I. 查询年龄等于18或20或40的员工信息
SELECT * FROM emp WHERE age IN (18, 20, 40);

# J. 查询姓名为两个字的员工信息
SELECT * FROM emp WHERE name LIKE '__';

# K. 查询身份证号最后一位是X的员工信息
SELECT * FROM emp WHERE idcard LIKE '%X';

# M. 查询年龄大于30岁的男性员工信息
SELECT * FROM emp WHERE gender = '男' AND age > 30;

# N. 查询姓名中包含“三”字的员工信息
SELECT * FROM emp WHERE name LIKE '%三%';

# O. 查询入职时间在2010年之后的员工信息
SELECT * FROM emp WHERE entry_date > '2010-01-01';

# Q. 查询工号以‘001’开头的员工信息
SELECT * FROM emp WHERE emp_no LIKE '001%';

# R. 查询身份证号倒数第二位是7的员工信息
SELECT * FROM emp WHERE idcard LIKE '%7_';

# T. 查询工作地址不是北京也不是上海的员工信息
SELECT * FROM emp WHERE address NOT IN ('北京', '上海');

# U. 查询姓名为三个字的员工信息
SELECT * FROM emp WHERE name LIKE '___';

# V. 查询年龄在25到35岁之间（包含）的员工信息
SELECT * FROM emp WHERE age BETWEEN 25 AND 35;

# W. 查询身份证号以‘123’开头的员工信息
SELECT * FROM emp WHERE idcard LIKE '123%';

# Y. 查询性别为女且工作地址为北京的员工信息
SELECT * FROM emp WHERE gender = '女' AND address = '北京';
```


## course表
```mysql
-- ==============================
-- 课程信息表 course
-- ==============================

CREATE TABLE course (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '课程ID，主键，自增',
    course_code VARCHAR(20) NOT NULL UNIQUE COMMENT '课程编号，唯一，非空',
    course_name VARCHAR(60) NOT NULL COMMENT '课程名称，非空',
    course_type VARCHAR(30) NULL COMMENT '课程类型（必修/选修），可空',
    credit INT NOT NULL COMMENT '学分，非空',
    class_hour INT NULL COMMENT '总课时，可空',
    teacher_name VARCHAR(30) NOT NULL COMMENT '授课教师，非空',
    department VARCHAR(40) NULL COMMENT '所属院系，可空',
    exam_type VARCHAR(20) NULL COMMENT '考核方式，可空'
) COMMENT='课程信息表';

INSERT INTO course (course_code, course_name, course_type, credit, class_hour, teacher_name, department, exam_type)
VALUES 
('KC001', 'MySQL数据库应用', '必修', 3, NULL, '陈老师', NULL, NULL),
('KC002', 'Java程序设计', NULL, 4, 64, '王老师', '计算机学院', NULL),
('KC003', 'Python数据分析', '选修', 3, NULL, '李老师', '信息学院', NULL),
('KC004', 'Web前端开发', '必修', 2, NULL, '赵老师', NULL, '考试'),
('KC005', '计算机网络', '必修', 3, NULL, '刘老师', NULL, NULL),
('KC006', '软件测试技术', '选修', 2, NULL, '马明哲', NULL, NULL),
('KC007', '车载测试基础', '必修', 2, 32, '王宝丽', NULL, NULL),
('KC008', '大数据技术', '选修', 4, NULL, '吴老师', NULL, '考试'),
('KC009', '物联网应用', '选修', 3, NULL, '周老师', NULL, NULL),
('KC010', '操作系统原理', '必修', 4, 64, '张老师', NULL, NULL),
('KC011', 'C语言程序设计', '选修', 3, NULL, '李老师', NULL, NULL),
('KC012', '软件工程', '必修', 4, 48, '李老师', NULL, '考试'),
('KC013', '数据结构', '必修', 4, NULL, '张老师', '信息学院', NULL),
('KC014', '计算机基础', '必修', 2, 32, '王老师', NULL, '考查'),
('KC015', '网络安全', '选修', 3, NULL, '赵老师', NULL, '考查');

-- ==============================
-- 案例练习 SQL
-- ==============================

-- 1. 查询所有课程信息（基础全表查询）
SELECT * FROM course;

-- 2. 查询课程编号、课程名称、学分、授课教师（指定字段查询）
SELECT course_code, course_name, credit, teacher_name FROM course;

-- 3. 查询学分等于4分的所有课程
SELECT * FROM course WHERE credit = 4;

-- 4. 查询学分大于3分的课程
SELECT * FROM course WHERE credit > 3;

-- 5. 查询课时不为空（NOT NULL）的课程
SELECT * FROM course WHERE class_hour IS NOT NULL;

-- 6. 查询学分等于3分 且 课时大于32的课程（AND）
SELECT * FROM course WHERE credit = 3 AND class_hour > 32;

-- 7. 查询是选修 或 课时等于32的课程（OR）
SELECT * FROM course 
WHERE course_type = '选修' OR class_hour = 32;

-- 8. 查询学分在2~4（包含2和4）之间的课程
SELECT * FROM course 
WHERE credit BETWEEN 2 AND 4;

-- 9. 查询课程名称包含"数据"的课程（模糊匹配）
SELECT * FROM course 
WHERE course_name LIKE '%数据%';

-- 10. 查询授课教师为"陈老师" 且 考核方式是考试的课程
SELECT * FROM course 
WHERE teacher_name = '陈老师' AND exam_type = '考试';

-- 11. 查询课程编号不等于KC001 且 课时为空的课程
SELECT * FROM course 
WHERE course_code <> 'KC001' AND class_hour IS NULL;

-- 12. 查询授课教师姓"王"的课程
SELECT * FROM course 
WHERE teacher_name LIKE '王%';

-- 13. 查询课程名称以"技术"结尾的课程
SELECT * FROM course 
WHERE course_name LIKE '%技术';

-- 14. 查询课程类型包含"选修"的课程
SELECT * FROM course 
WHERE course_type LIKE '%选修%';

-- 15. 查询授课教师为"李老师" 或 "张老师"（去重）
SELECT DISTINCT teacher_name 
FROM course 
WHERE teacher_name IN ('李老师', '张老师');

-- 16. 查询所有不重复的学分
SELECT DISTINCT credit FROM course;

-- 17. 给字段起别名：课程编号、课程名、学分、教师
SELECT 
    course_code AS 课程编号,
    course_name AS 课程名,
    credit AS 学分,
    teacher_name AS 教师
FROM course;

-- 18. 查询所有课程信息（再次练习）
SELECT * FROM course;
```