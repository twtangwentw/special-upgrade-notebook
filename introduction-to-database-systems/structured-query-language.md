---
description: 结构化查询语言(Structured Query Language，SQL)是关系数据库的标准语言，也是一个通用的、功能极强的关系数据库语言
---

# 第三章\~关系数据库标准SQl

## 3.1 SQL 概述 <a href="#3.1-overview" id="3.1-overview"></a>

### 3.1.1 SQL 的产生与发展 <a href="#3.1.1" id="3.1.1"></a>

<mark style="color:orange;">SQL</mark> 在 <mark style="color:purple;">1974</mark> 年由 Boyce 和 Chamberlin 提出，最初叫 <mark style="color:green;">Sequel</mark>

目前<mark style="background-color:blue;">没有一个</mark>数据库系统能够支持 SQL 标准的<mark style="color:red;">所有概念和特性</mark>，同时许多软件厂商对 SQL 基本命令集还进行了<mark style="background-color:red;">不同程度的扩充和修改</mark>，由可以支持标准之外的一些功能特性

### 3.1.2 SQL 的特点 <a href="#3.1.2" id="3.1.2"></a>

{% hint style="info" %}
SQL 集<mark style="color:blue;">数据查询</mark>、<mark style="color:purple;">数据操纵</mark>、<mark style="color:red;">数据定义</mark>和<mark style="color:green;">数据控制</mark>功能于一体，下面是SQL的主要特点：
{% endhint %}

1. 综合统一
2. 高度非过程化
3. 面向集合的操作方式
4. 以同一种语法结构提供多种使用方式
5. 语言简单，易学易用

### 3.1.3 SQL 的基本概念 <a href="#3.1.3" id="3.1.3"></a>

<img src="../.gitbook/assets/file.drawing (4).svg" alt="SQL 对关系数据库模式的支持" class="gitbook-drawing">

## 3.2 学生-课程数据库 <a href="#3.2-student-course-database" id="3.2-student-course-database"></a>

这一章所使用到数据库模式 S-T

学生表：$$Student(\underline{Sno}, Sname, Ssex, Sage, Sdept)$$

课程表：$$Course(\underline{Cno}, Cname, Cpno, Ccredit)$$

学生选课表：$$SC(\underline{Sno, Cno}, Grade)$$

各表的数据如下：

{% tabs %}
{% tab title="Student" %}
| 学号(Sno)   | 姓名(Sname) | 性别(Ssex) | 年龄(Sage) | 所在系(Sdept) |
| --------- | --------- | -------- | -------- | ---------- |
| 201215121 | 李勇        | 男        | 20       | CS         |
| 201215122 | 刘晨        | 女        | 19       | CS         |
| 201215123 | 王敏        | 女        | 18       | MA         |
| 201215125 | 张立        | 男        | 19       | IS         |
{% endtab %}

{% tab title="Course" %}
| 课程号(Cno) | 课程名(Cname) | 先行课(Cpno) | 学分(Ccredit) |
| -------- | ---------- | --------- | ----------- |
| 1        | 数据库        | 5         | 4           |
| 2        | 数学         |           | 2           |
| 3        | 信息系统       | 1         | 4           |
| 4        | 操作系统       | 6         | 3           |
| 5        | 数据结构       | 7         | 4           |
| 6        | 数据处理       |           | 2           |
| 7        | PASCAL 语言  | 6         | 4           |


{% endtab %}

{% tab title="CS" %}
| 学号(Sno)   | 课程号(Cno) | 成绩(Grade) |
| --------- | -------- | --------- |
| 201215121 | 1        | 92        |
| 201215121 | 2        | 85        |
| 201215121 | 3        | 88        |
| 201215122 | 2        | 90        |
| 201215122 | 3        | 80        |
{% endtab %}
{% endtabs %}

## 3.3 数据定义 <a href="#3.3-data-definition" id="3.3-data-definition"></a>

{% hint style="info" %}
SQL 的数据定义功能包括<mark style="color:blue;">模式定义</mark>、<mark style="color:purple;">表定义</mark>、<mark style="color:orange;">视图定义</mark>和<mark style="color:red;">索引的定义</mark>
{% endhint %}

SQL 的数据定义语句：

| 操作对象 | 创建            | 删除          | 修改          |
| ---- | ------------- | ----------- | ----------- |
| 模式   | CREATE SCHEMA | DROP SCHEMA |             |
| 表    | CREATE TABLE  | DROP TABLE  | ALTER TABLE |
| 视图   | CREATE VIEW   | DROP VIEW   |             |
| 索引   | CREATE INDEX  | DROP INDEX  | ALTER INDEX |

> <mark style="background-color:red;">这里的模式(Schema) 对应 MySQL 数据库的 Database</mark>

### 3.3.1 模式定义与删除 <a href="#3.3.1" id="3.3.1"></a>

**1. 定义模式**

```sql
// 模式的定义语句
CREATE SCHEMA [<模式名>] AUTHORIZATION <用户名>;

// eg:
// 为用户 TANG 定义一个学生-课程数据库 S-T
CREATE SCHEMA "S-T" AUTHORIZATION TANG;
// 为用户 TANG 定义一个 TANG 数据库
CREATE SCHEMA AUTHORIZATION TANG;
```

**2. 删除模式**

```sql
// 模式的删除语句
DROP SCHEMA <模式名> <CASCADE | RESTRICT>;

// eg:
// 删除 "S-T" 模式，同时删除模式下创建的全部对象
DROP SCHEMA "S-T" CASCADE;
// 删除 "S-T" 模式，如果模式下还有对象则拒绝删除
DROP SCHEMA "S-T" RESTRICT;
```

### 3.3.2 基本表的定义、删除与修改 <a href="#3.3.2" id="3.3.2"></a>

**1. 定义基本表**

{% hint style="info" %}
创建了一个<mark style="color:orange;">模式</mark>就建立了一个<mark style="color:purple;">命名空间</mark>，在这个<mark style="color:purple;">命名空间</mark>中首先要定义的就是<mark style="color:blue;">基本表</mark>
{% endhint %}

```sql
// 基本表的定义格式如下
CREATE TABLE [<模式名>.]<表名> (
    <列名><数据类型> [列级完整性约束条件]
    [,<列名><数据类型> [列级完整性约束条件]
    ···
    [,<表级完整性约束条件>]
);

// eg:
// 建立一个学生表
CREATE TABLE Student (
    Sno CHAR(9) PRIMARY KEY,                        // 列级完整性约束条件，Sno 是主码
    Sname CHAR(20) UNIQUE,                          // Sname 取唯一值
    Ssex CHAR(2),
    Sage SMALLINT,
    Sdept CHAR(20)
);
// 建立一个课程表
CREATE TABLE Course (
    Cno CHAR(4) PRIMARY KEY,                        // 列级完整性约束条件，Cno 是主码
    Cname CHAR(40) NOT NULL,                        // 列级完整性约束条件，Cname 不能取空值
    Cpno CHAR(4),
    Ccredit SMALLINT,
    FOREIGN KEY (Cpno) REFERENCES Course(Cno)       // 表级完整性约束条件，Cpno 是外码，被参照表是 Course，被参照列是 Cno
);
// 建立一个学生选课表
CREATE TABLE SC (
    Sno CHAR(9),
    Cno CHAR(4),
    Grade SMALLINT,
    FOREIGN KEY (Sno) REFERENCES Student(Sno),      // 表级完整性约束条件，Sno 是外码，被参照表是 Student，被参照列是 Sno
    FOREIGN KEY (Cno) REFERENCES Course(Cno)        // 表级完整性约束条件，Cno 是外码，被参照表是 Course，被参照列是 Cno
);
```

**2. 数据类型**

| 数据类型                       | 含义                       |
| -------------------------- | ------------------------ |
| CHAR(n),CHARACTER(n)       | 长度为n的定长字符串               |
| VARCHAR(n),VARCHARACTER(n) | 最大长度为n的变长字符串             |
| CLOB                       | 字符串大对象                   |
| BLOB                       | 二进制大对象                   |
| INT,INTEGER                | 长整型（4字节）                 |
| SMALLINT                   | 短整型（2字节）                 |
| BIGINT                     | 大整型（8字节）                 |
| NUMERIC(p, d)              | 定点数，小数点前 p 位，小数点后 d 位    |
| DECIMAL(p, d),DEC(p,d)     | 同 NUMERIC                |
| REAL                       | 取决于机器精度的单精度浮点数           |
| DOUBLE PRECISION           | 取决于机器精度的双精度浮点数           |
| FLOAT(n)                   | 可选精度浮点数，精度至少为n位          |
| BOOLEAN                    | 逻辑布尔量                    |
| DATE                       | 日期，包含年、月、日，格式位YYYY-MM-DD |
| TIME                       | 时间，包含时、分、秒，格式为 HH:MM:SS  |
| TIMESTAMP                  | 时间戳类型                    |
| INTERVAL                   | 时间间隔类型                   |

**3. 模式与表**

> 每个基本表都属于一个模式，一个模式包含多个基本表

定义基本表时又三种方法指定它所属的模式：

1. 在创建表时现实指定模式名: `CREATE TABLE "S-T".Student (···);`
2. 创建模式时同时创建表
3. 所在当前工作默认所属模式

```sql
// 查看当前的搜索路径
SHOW search_path;                    // $user, PUBLIC
// 修改搜索路径
SET search_par TO "S-T", PUBLIC;
```

**4. 修改基本表**

```sql
// 修改基本表的语法格式
ALTER TABLE <表名>
[ADD [COLUMN] <新列名><数据类型> [列级完整性约束]]
[ADD <表级完整性约束>]
[DROP [COLUMN] <列名> [CASCADE | RESTRICT]]
[DROP CONSTRAINT <完整性约束名> [RESTRICT | CASCADE]]
[ALTER COLUMN <列名><数据类型>]

eg:
// 在 Student 表中添加入学时间
ALTER TABLE Student ADD Sentrance DATE;
// 修改年龄的数据类型为整型
ALTER TABLE Student ALTER COLUMN Sage INT;
```

**5. 删除基本表**

```sql
// 删除基本表语法
DROP TABLE <表名> [RESTRICT | CASCADE];

// eg:
// 删除 Student 表, 和所有关联它的对象
DROP TABLE Student CASCADE;
```

### 3.3.3 索引的建立与删除 <a href="#3.3.3" id="3.3.3"></a>

{% hint style="info" %}
数据库的索引类似图书前面的的目录，能快速定位到需要查询的内容
{% endhint %}

**1. 建立索引**

```sql
// 建立索引的语法
CREATE [UNIQUE | CLUSTER] INDEX <索引名>
ON <表名>(<列名>[<次序>][,<列名>[<次序>]] ···);
// 次序：ASC(升序) DESC(降序)

// eg: 
CREATE UNIQUE INDEX Student_Sno ON Student(Sno);
CREATE UNIQUE INDEX Couse_Cno ON COURSE(Cno);
CREATE UNIQUE INDEX SC_Sno_Cno ON SC(Sno ASC, Cno DESC);
```

**2. 修改索引**

```sql
// 修改索引的语法
ALTER INDEX <索引名> RENAME TO <新索引名>;

// eg:
// 修改索引名由 Student_Sno 到 Student_Unique_Sno
ALTER INDEX Student_Sno RENAME TO Student_Unique_Sno;
```

**3. 删除索引**

```sql
// 删除索引语法
DROP INDEX <索引名>;

// eg:
// 删除 Student_Sno 索引
DROP INDEX Student_Sno;
```

### 3.3.4 数据字典 <a href="#3.3.4" id="3.3.4"></a>

{% hint style="info" %}
<mark style="color:red;">数据字典</mark>是关系数据库管理系统内部的一组<mark style="color:purple;">系统表</mark>，它记录了数据库中<mark style="background-color:red;">所有的定义信息</mark>，在进行查询优化和查询处理时，数据字典中的信息是其中重要依据
{% endhint %}

## 3.4 数据查询 <a href="#3.4-data-query" id="3.4-data-query"></a>

{% hint style="info" %}
<mark style="color:red;">数据查询</mark>是数据库的核心功能。SQL 提供了 SELECT 语句进行数据查询
{% endhint %}

{% code title="数据查询的一般格式" %}
```sql
SELECT [ALL | DISTINCT] <目标列表达式>[, <目标列表达式>] ···
FROM <表名 | 视图名 > | (SELECT 语句) [AS] <别名>[, <表名 | 视图名> | (SELECT 语句) [AS] <别名>] ···
[WHERE <条件表达式>]
[GROUP BY <列名> [HAVING <条件表达式>]]
[ORDER BY <列名> [ASC | DESC]];
```
{% endcode %}

条件表达式的常用查询条件：

| 查询条件       | 谓词                                                                            |
| ---------- | ----------------------------------------------------------------------------- |
| 比较         | **`=`**,**`>`**,**`<`**,**`>=`**,**`<=`**,**`!=`**,**`<>`**,**`!>`**,**`!<`** |
| 确定范围       | BETWEEN AND，NOT BETWEEN AND                                                   |
| 确定集合       | IN，NOT IN                                                                     |
| 字符匹配       | LIKE，NOT LIKE                                                                 |
| 空值         | **`IS NULL`**，**`IS NOT NULL`**                                               |
| 多重条件(逻辑运算) | AND,OR,NOT                                                                    |

### 3.4.1 单表查询 <a href="#3.4.1" id="3.4.1"></a>

示例：

```sql
// 查询全体学生的学号和姓名
SELECT Sno, Sname FROM Student;
// 查询全体学生的姓名、学号、所在系
SELECT Sname, Sno, Sdept FROM Student;
// 查询学生的所有信息
SELECT * FROM Student;
// 查询学生的姓名、出生年份、和系的小写
SELECT Sname AS NAME, 2022 - Sage AS BIRTHDAY, LOWER(Sdept) AS DEPT;
// 查询选项课程的学生学号
SELECT DISTINCT Sno FROM SC;
// 查询计算机科学系的全体学生的名单
SELECT Sname FROM Student WHERE Sdept = 'CS';
// 查询年龄在 [20, 23] 岁的学生姓名
SELECT Sname FROM Student WHERE Sage BETWEEN 20 AND 23;
// 查询既不是计算机科学系(CS)、也不是数学系(MA)的学生姓名
SELECT Sname FROM Student WHERE Sdep NOT IN ('CS', 'MA');
// 查询刘性同学的详细信息
SELECT * FROM Student WHERE Sname LIKE '刘%';
// 使用 LIKE 查询 DB_Design 课程的课程号和学分
SELECT Cno, Ccredit FROM Course WHERE Cname LIKE 'DB\_Design' ESCAPE '\';
```

**聚集函数：**

| 聚集函数                           | 说明                  |
| ------------------------------ | ------------------- |
| COUNT(\*)                      | 统计元组个数              |
| COUNT(\[ALL \| DISTINCT] <列名>) | 统计元组中值的个数           |
| SUM(\[ALL \| DISTINCT] <列名>)   | 统计一列值的总和(此列必须是数值型)  |
| AVG(\[ALL \| DISTINCT] <列名>)   | 计算一列值的平均值(此列必须是数值型) |
| MAX(<列名>)                      | 就一列值中的最大值           |
| MIN(<列名>)                      | 求一列值中的最小值           |

{% hint style="info" %}
当<mark style="background-color:red;">聚集函数遇到 NULL 值时</mark>，<mark style="background-color:purple;">除了 COUNT(\*) 外</mark>，<mark style="background-color:red;">都会跳过 NULL 值，而只处理非 NULL 值</mark>，聚集函数不能再 WHERE 语句中使用，但是可以再 HAVING 语句中使用
{% endhint %}

```sql
// 查询平均成绩大于大于 90 分的学生学号和平均成绩
SELECT Sno, AVG(Grade) AS AvgGrade
FROM SC
GROUP BY Sno
HAVING AVG(Grade) >= 90;
```

### 3.4.2 连接查询 <a href="#3.4.2" id="3.4.2"></a>

```sql
// 使用自然连接查询学生选课情况
SELECT S.Sno, Sname, Ssex, Sdept, Cno, Grade
FROM Student AS S, SC
WHERE S.Sno = SC.Sno;
// 查询课程的简介先行课
SELECT F.Cno, S.Cpon
FROM Course AS F, Course AS S
WHERE F.Cpno = S.Cno AND S.Cpno IS NOT NULL;

// 查询学生的学生信息和其选课信息
SELECT S.Sno, Sname, Ssex, Sage, Sdept, Cno, Grade
FROM Student S LEFT OUTER JOIN SC ON (S.Sno = SC.Sno);
```

### 3.4.3 嵌套查询 <a href="#3.4.3" id="3.4.3"></a>



### 3.4.4 集合查询 <a href="#3.4.4" id="3.4.4"></a>

### 3.4.5 基于派生表的查询 <a href="#3.4.5" id="3.4.5"></a>

### 3.4.6 SELECT 语句的一般格式 <a href="#3.4.6" id="3.4.6"></a>

## 3.5 数据更新 <a href="#3.5-data-update" id="3.5-data-update"></a>

### 3.5.1 插入数据 <a href="#3.5.1" id="3.5.1"></a>

### 3.5.2 修改数据 <a href="#3.5.2" id="3.5.2"></a>

### 3.5.3 删除数据 <a href="#3.5.3" id="3.5.3"></a>

## 3.6 空值处理 <a href="#3.6-null-handle" id="3.6-null-handle"></a>

## 3.7 视图 <a href="#3.7-view" id="3.7-view"></a>

### 3.7.1 定义视图 <a href="#3.7.1" id="3.7.1"></a>

### 3.7.2 查询视图 <a href="#3.7.2" id="3.7.2"></a>

### 3.7.3 更新视图 <a href="#3.7.3" id="3.7.3"></a>

### 3.7.4 视图的作用 <a href="#3.7.4" id="3.7.4"></a>

## 3.8 SQL 总结 <a href="#3.8-sql-summary" id="3.8-sql-summary"></a>

{% code title="SQL 语句的关键字：" %}
```sql
ADD
ALTER
ALL
AS
ASC | DESC
AUTHORIZATION
CASCADE
COLUMN
CONSTRAINT
CREATE
DISTINCT
DROP
ESCAPE // 定义换码字符，LIKE '\_' ESCAPT '\'
FOREIGN
FROM
GROUP BY ··· HAVING
INDEX
KEY
ON
ORDER BY
PRIMARY
REFERENCES
RESTRICT
SCHEMA
SET ··· TO ···
SHOW
TABLE
UNIQUE
VIEW
WHERE
```
{% endcode %}
