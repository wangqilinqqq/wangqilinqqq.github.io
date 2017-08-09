---
title: MySQL学习笔记
date: 2017-07-28 11:11:11
categories: MySQL
tags: MySQL
---

**MySQL简介**

MySQL 是一个 DBMS（数据库管理系统），由瑞典 MySQLAB 公司开发，目前属于 Oracle 公司，MySQL 是最流行的关系型数据库管理系统（关系数据库，是建立在关系数据库模型基础上的数据库，借助于集合代数等概念和方法来处理数据库中的数据）。由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发者都选择 MySQL 作为网站数据库。MySQL 使用 SQL 语言进行操作。

<!--more-->

**笔记开始**

## MySQL基本指令

>启动MySQL
```bash
# 启动 MySQL 服务
sudo service mysql start  
```

***

>登录MySql
```bash
# 使用 root 用户登录，实验楼环境的密码为空，直接回车就可以登录
mysql -u root 
```

***

>查看数据库
```bash
# 查看数据库
show databases;
```

***

>创建数据库
```bash
# CREATE DATABASE <数据库名字>;
CREATE DATABASE mysql_shiyan;
```

***

>连接数据库
```bash
# 选择连接其中一个数据库，语句格式为 use <数据库名>
use information_schema
```

***

***

>查看表
```bash
# 使用命令 show tables; 查看数据库中有哪些表（**注意不要漏掉";"**）：
show tables;
```

***

>新建数据表
```bash
CREATE TABLE 表的名字
(
列名a 数据类型(数据长度),
列名b 数据类型(数据长度)，
列名c 数据类型(数据长度)
);

# 我们尝试在 mysql_shiyan 中新建一张表 employee，包含姓名，ID 和电话信息，所以语句为：
CREATE TABLE employee (id int(10),name char(20),phone int(12));
```

***

>MySQL的数据类型
<table><tr><th>数据类型</th><th>大小(字节)</th><th>用途</th><th>格式</th></tr><tr><td>INT</td><td>4</td><td>整数</td><td></td></tr><tr><td>FLOAT</td><td>4</td><td>单精度浮点数</td><td></td></tr><tr><td>DOUBLE</td><td>8</td><td>双精度浮点数</td><td></td></tr><tr><td>ENUM</td><td></td><td>单选,比如性别</td><td>ENUM('a','b','c')</td></tr><tr><td>SET</td><td></td><td>多选</td><td>SET('1','2','3')</td></tr><tr><td>DATE</td><td>3</td><td>日期</td><td>YYYY-MM-DD</td></tr><tr><td>TIME</td><td>3</td><td>时间点或持续时间</td><td>HH:MM:SS</td></tr><tr><td>YEAR</td><td>1</td><td>年份值</td><td>YYYY</td></tr><tr><td>CHAR</td><td>0~255</td><td>定长字符串</td><td></td></tr><tr><td>VARCHAR</td><td>0~255</td><td>变长字符串</td><td></td></tr><tr><td>TEXT</td><td>0~65535</td><td>长文本数据</td><td></td></tr></table>

**CHAR 和 VARCHAR** 的区别: CHAR 的长度是固定的，而 VARCHAR 的长度是可以变化的，比如，存储字符串 “abc"，对于 CHAR(10)，表示存储的字符将占 10 个字节(包括 7 个空字符)，而同样的 VARCHAR(12) 则只占用4个字节的长度，增加一个额外字节来存储字符串本身的长度，12 只是最大值，当你存储的字符小于 12 时，按实际长度存储。

**ENUM和SET的区别**: ENUM 类型的数据的值，必须是定义时枚举的值的其中之一，即单选，而 SET 类型的值则可以多选。

***

>插入数据
```bash
INSERT INTO 表的名字(列名a,列名b,列名c) VALUES(值1,值2,值3);

# 我们尝试向 employee 中加入 Tom、Jack 和 Rose：
INSERT INTO employee(id,name,phone) VALUES(01,'Tom',110110110);

INSERT INTO employee VALUES(02,'Jack',119119119);

INSERT INTO employee(id,name) VALUES(03,'Rose');
```

## MySQL约束
<table><tr><th>约束类型：</th><th>主键</th><th>默认值</th><th>唯一</th><th>外键</th><th>非空</th></tr><tr><td>关键字：</td><td>PRIMARY KEY</td><td>DEFAULT</td><td>UNIQUE</td><td>FOREIGN KEY</td><td>NOT NULL</td></tr></table>

### 主键
>主键就是一张数据表中唯一的值，比如用户信息表的 ID,商品表的 SKU。都是唯一的，不重复的。
```bash
# 直接在后面定义主键-主键名即列名
CREATE TABLE employee
(
id	INT(10)PRIMARY KEY,
name char(20)
);

# 自定义主键名称-主键的名称是自定义的
CREATE TABLE department
(
  dpt_name   CHAR(20),
  people_num INT(10),
  CONSTRAINT dpt_pk PRIMARY KEY (dpt_name)
 );

# 复合主键-由表的二列或多列组成的复合主键，即这2列的值都是唯一的，不重复的。
CREATE TABLE project
(
  proj_num   INT(10),
  proj_name  CHAR(20),
  CONSTRAINT proj_pk PRIMARY KEY (proj_num,proj_name)
 );

```

### 外键
>外键就是以另一个表的主键来约束自己这一列。
一个表可以有多个外键，每个外键必须参考另一个表的主键。
被外键约束的列，取值必须在它参考的列中有取值。
```bash
CREATE TABLE department
(
  dpt_name   CHAR(20) NOT NULL,
  people_num INT(10) DEFAULT '10',
  CONSTRAINT dpt_pk PRIMARY KEY (dpt_name)
 );

CREATE TABLE employee
(
  id      INT(10) PRIMARY KEY,
  in_dpt  CHAR(20) NOT NULL,
  CONSTRAINT emp_fk FOREIGN KEY (in_dpt) REFERENCES department(dpt_name)
  #   自定义外键名称    in_dpt为外键    参考的是department表的dpt_name列
 );

# 如果在employee表中存一行数据
# 如果in_dpt列所对应的值在对应表department的dpt_name列没有对应值
# 那么就会报错，无法成功插入数据。
```

## SELECT语句详解

### 基本的SELECT语句

>SELECT 语句的基本格式为：
```bash
SELECT 要查询的列名 FROM 表名字 WHERE 限制条件;

# * 代表查看所有

SELECT name,age FROM employee;

# 而大多数情况，我们只需要查看某个表的指定的列
# 比如要查看employee 表的 name 和 age：
```

### 数学符号条件

>SELECT 语句常常会有 WHERE 限制条件，用于达到更加精确的查询。
WHERE限制条件可以有数学符号 (=,<,>,>=,<=) ，刚才我们查询了 name 和 age，现在稍作修改：
```bash
# 筛选出 age 大于 25：
SELECT name,age FROM employee WHERE age>25;

# 或者查找一个名字为 Mary 的员工的 name,age 和 phone：
SELECT name,age,phone FROM employee WHERE name='Mary';
```

### “AND”与“OR”
>从这两个单词就能够理解它们的作用。WHERE 后面可以有不止一条限制，
而根据条件之间的逻辑关系，可以用 OR(或) 和 AND(且) 连接：
AND->且 
OR->或
```bash
#筛选出 age 小于 25，或 age 大于 30
SELECT name,age FROM employee WHERE age<25 OR age>30;

#筛选出 age 大于 25，且 age 小于 30
SELECT name,age FROM employee WHERE age>25 AND age<30;
```

### IN 和 NOT IN
>关键词IN和NOT IN的作用和它们的名字一样明显，
用于筛选“在”或“不在”某个范围内的结果
```bash
#比如说我们要查询在dpt3或dpt4的人:
SELECT name,age,phone,in_dpt FROM employee WHERE in_dpt IN ('dpt3','dpt4');

#而NOT IN的效果则是，如下面这条命令，查询出了不在dpt1也不在dpt3的人：
SELECT name,age,phone,in_dpt FROM employee WHERE in_dpt NOT IN ('dpt1','dpt3');
```

### 通配符
>关键字 LIKE 在SQL语句中和通配符一起使用，通配符代表未知字符。
SQL中的通配符是` _ `和 ` % ` 。其中` _ `代表一个未指定字符，` % `代表不定个未指定字符。
```bash
#比如，要只记得电话号码前四位数为1101，
#而后两位忘记了，则可以用两个 _ 通配符代替：
SELECT name,age,phone FROM employee WHERE phone LIKE '1101__';

#另一种情况，比如只记名字的首字母，又不知道名字长度，
#则用 % 通配符代替不定个字符：
SELECT name,age,phone FROM employee WHERE name LIKE 'J%';
```

### 对结果排序
>为了使查询结果看起来更顺眼，我们可能需要对结果按某一列来排序，这就要用到 ORDER BY 排序关键词。
默认情况下，ORDER BY的结果是升序排列，而使用关键词ASC和DESC可指定升序或降序排序。
ASC->升
DESC->降
```bash
#比如，我们按salary降序排列，SQL语句为：
SELECT name,age,salary,phone FROM employee ORDER BY salary DESC;
```

### SQL 内置函数和计算
SQL 允许对表中的数据进行计算。对此，SQL 有 5 个内置函数，这些函数都对 SELECT 的结果做操作：

<table><tr><th>函数名：</th><th>COUNT</th><th>SUM</th><th>AVG</th><th>MAX</th><th>MIN</th></tr><tr><td>作用：</td><td>计数</td><td>求和</td><td>求平均值</td><td>最大值</td><td>最小值</td></tr></table>

>其中 COUNT 函数可用于任何数据类型(因为它只是计数)，而 SUM 、AVG 函数都只能对数字类数据类型做计算，MAX 和 MIN 可用于数值、字符串或是日期时间数据类型。

```bash
#具体举例，比如计算出salary的最大、最小值，用这样的一条语句：
SELECT MAX(salary) AS max_salary,MIN(salary) FROM employee;

#使用AS关键词可以给值重命名，比如最大值被命名为了max_salary：
```

### 子查询
>上面讨论的 SELECT 语句都仅涉及一个表中的数据，然而有时必须处理多个表才能获得所需的信息。例如：想要知道名为 "Tom" 的员工所在部门做了几个工程。员工信息储存在 employee 表中，但工程信息储存在project 表中。
对于这样的情况，我们可以用子查询：
```bash
SELECT of_dpt,COUNT(proj_name) AS count_project FROM project
WHERE of_dpt IN
(SELECT in_dpt FROM employee WHERE name='Tom');

# 这条SQL语句的重点是后面这部分 WHERE of_dpt IN (SELECT in_dpt FROM employee WHERE name='Tom');
# 表示的是只显示 of_dpt列有 dpt4的行
# SELECT in_dpt FROM employee WHERE name='Tom' 这个查询到的值 为 dpt4
# 所以这条SQL语句其实是:
# SELECT of_dpt,COUNT(proj_name) AS count_project FROM project WHERE of_dpt IN('dpt4');
```
>子查询还可以扩展到3层、4层或更多层。

### 连接查询
>在处理多个表时，子查询只有在结果来自一个表时才有用。但如果需要显示两个表或多个表中的数据，这时就必须使用连接 (join) 操作。
连接的基本思想是把两个或多个表当作一个新的表来操作，如下：

```bash
SELECT id,name,people_num
FROM employee,department
WHERE employee.in_dpt = department.dpt_name
ORDER BY id;

#这条语句查询出的是，各员工所在部门的人数，
#其中员工的 id 和 name 来自 employee 表，
#people_num 来自 department 表：

# SQL语句详解:遇到子查询或者连接查询的时候，要习惯拆分。弄懂每一部分，在连接起来理解
# 这条SQL语句的意思是，在employee表和department表两个表中查询 id、name、people_num 三列
# 并只显示 employee表中in_dpt列等于pepartment表中dpt_name列的数据

#另一种
SELECT id,name,people_num
FROM employee JOIN department
ON employee.in_dpt = department.dpt_name
ORDER BY id;
```

## 数据库及表的删除与修改

### 对数据库的修改

#### 删除数据库
>删除数据库
```bash
DROP DATABASE mysql_shiyan;
```
***

### 对表的修改

#### 表的重命名

```base

RENAME TABLE <原名> TO <新名>；

```
#### 删除表

```base

DROP TABLE <表名字>;

```
### 对一列的修改

#### 增加一列

```base
ALTER TABLE <表的名字> ADD <列的名字> <数据类型> <约束>

# 如果想把新增的一列放在某列的后面 需要加 after

ALTER TABLE <表的名字> ADD <列的名字> <数据类型> <约束> AFTER <列的名字>

# 如果想把新增的一列放在表的第一列的话 需要加 first参数

ALTER TABLE <表的名字> ADD <列的名字> <数据类型> <约束> AFTER <列的名字>
```

#### 删除一列

```bash
ALTER TABLE <表名字> drop <列名字>
```

#### 重命名

```bash
ALTER TABLE <表名字> CHANGE <原名字> <新名字> <数据类型> <约束>
 ```
 
### 对表内容的修改

#### 修改表中的某个值
```bash
UPDATE <表名字> SET 列1=值1，列2=值2 WHERT <限制条件>

# 比如我想把 employee 表中，id为8的行里面的电话号码改为3838438

UPDATE employee SET phone=3838438 where id=8;
```

#### 修改表的一行

```bash

DELETE FROM <表的名字> WHERE <限制条件>

# 比如我想把 employee 表中，id为8的行删除

DELETE FROM employee WHERE id=8;
```

## 其他基本操作

索引：可以加快查询速度
视图：是一种虚拟存在的表
导入：从文件中导入数据到表
导出：从表中导出到文件中
备份：mysqldump 备份数据库到文件
恢复：从文件恢复数据库

### 索引
>对一张表中的某个列建立索引，有以下两种语句格式：
```bash
ALTER TABLE 表名字 ADD INDEX 索引名 (列名);

CREATE INDEX 索引名 ON 表名字 (列名);

# 我们用这两种语句分别建立索引：

ALTER TABLE employee ADD INDEX idx_id (id);  #在employee表的id列上建立名为idx_id的索引

CREATE INDEX idx_name ON employee (name);   #在employee表的name列上建立名为idx_name的索引
```

### 视图

注意理解视图是虚拟的表：

>创建视图的语句格式为：
```bash
CREATE VIEW 视图名(列a,列b,列c) AS SELECT 列1,列2,列3 FROM 表名字;
```

### 导入
>导入操作，可以把一个文件里的数据保存进一张表。导入语句格式为：
```
LOAD DATA INFILE '文件路径' INTO TABLE 表名字;
```

### 导出
>导出与导入是相反的过程，是把数据库某个表中的数据保存到一个文件之中。导出语句基本格式为：
```
SELECT 列1，列2 INTO OUTFILE '文件路径和文件名' FROM 表名字;

# 现在我们把整个employee表的数据导出到 /tmp 目录下，导出文件命名为 out.txt 具体语句为：

SELECT * INTO OUTFILE '/tmp/out.txt' FROM employee;
```

### 备份

```
mysqldump -u root 数据库名>备份文件名;   #备份整个数据库

mysqldump -u root 数据库名 表名字>备份文件名;  #备份整个表

# 我们尝试备份整个数据库 mysql_shiyan，将备份文件命名为 bak.sql，
# 先 Ctrl+Z 退出 MySQL 控制台，再打开 Xfce 终端，在终端中输入命令：
mysqldump -u root mysql_shiyan > bak.sql;
```

### 恢复

```
# 用备份文件恢复数据库，其实我们早就使用过了。
# 在本次实验的开始，我们使用过这样一条命令
source /tmp/SQL6/MySQL-06.sql
```

还有另一种方式恢复数据库，但是在这之前我们先使用命令新建一个空的数据库 test：

```
mysql -u root          #因为在上一步已经退出了MySQL，现在需要重新登录

CREATE DATABASE test;  #新建一个名为test的数据库
```

再次 Ctrl+Z 退出MySQL，然后输入语句进行恢复，把刚才备份的 bak.sql 恢复到 test 数据库：

```
mysql -u root test < bak.sql
```
