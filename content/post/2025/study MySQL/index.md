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
## src 部分解释

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
