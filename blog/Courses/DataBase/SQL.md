---
layout: default
author: Zefeng Zhu
date: 2020-04-21
---

# Structured Query Language (SQL)

* 一种介于关系代数与关系演算之间的语言
* 四大功能于一体
  * 定义 `DDL`
  * 查询 `DML`
  * 操纵 `DML`
  * 控制 `DCL`
* 关系数据库的标准语言

## SQL支持的三级逻辑结构

* 外模式
* 模式
* 存储模式

## SQL支持的基本概念

* 基本表 Basic Table
* 视图 View
* 存储文件

## 数据定义功能 `DDL`

* `CREATE`
* `DROP`
* `ALTER`

操作对象|创建|删除|修改
-|-|-|-
基本表 (模式)|`CREATE TABLE`|`DROP TABLE`|`ALTER TABLE`
视图 (外模式)|`CREATE VIEW`|`DROP VIEW`|
索引 (内模式)|`CREATE INDEX`|`DROP INDEX`|
数据库|`CREATE DATABASE`||

### 定义基本表

```SQL
CREATE TABLE Student2
(
    Sno CHAR(10) NOT NULL UNIQUE,
    Sn CHAR(10),
    Sex CHAR(2) DEFAULT 'M',
    Age NUMBER(4),
    Dept CHAR(10)
);
```

* 表名
  * 字母开头，大小写无关
  * 最多30个字符
  * 不能与已有的表或视图重名
  * 不能使用保留关键字
* 列名
  * 同表名
  * 且同一表中不能有相同列名
  * 一表中最多有254个列名 (取决于RDBMS)
* 数据类型
* 约束 (Constraint)
  * NOT NULL
  * Primary Key
  * UNIQUE
  * Foreign Key
  * Check

```SQL
CREATE TABLE Student2
(
    Sno CHAR(10) NOT NULL UNIQUE,
    Primary Key(Sno)
);

ALTER TABLE Subject
    ADD CONSTRAINT Sub_FK Foreign Key(Sub_ID)
    REFERENCES STUDENT(Sub_ID);

CONSTRAINT CHK_EMP_ZIP CHECK(ZIP IN ('46274', '46227', '46576'));
```

### 修改基本表

* 增删新列
* 增删完整性约束条件
* 修改原有列定义

```SQL
ALTER TABLE <表名>
[ADD <新列名> <数据类型> [完整性约束]]
[DROP <完整性约束名>]
[MODIFY <列名> <数据类型>];
```

```SQL
ALTER TABLE Student2 ADD Scome DATE;

ALTER TABLE Student2 DROP Scome;

ALTER TABLE Student2 MODIFY Sno CHAR(5);

ALTER TABLE Student2 DROP UNIQUE(Sno);
```

### 重命名基本表

```SQL
RENAME TABLE Student2 TO Stud2
```

### 删除基本表

```SQL
DROP TABLE Stud2
```

### 定义索引

使用索引的原因

* 优化查询的执行速度
* 四基于索引字段内容的数据排序更方便
* 通过`UNIQUE` 或 `PRIMARY KEY`，增强引用完整性约束

```SQL
CREATE [UNIQUE][CLUSTER] INDEX <索引名>
    ON <表名> (<列名>[<次序>][,<列名>[次序]]);

DROP INDEX <索引名>;
```

```SQL
CREATE UNIQUE INDEX SCI ON Sc(Sno, Cno);

DROP INDEX SCI;
```

注:

* 次序包括`ASC`、`DESC`，缺省为`ASC`
* `UNIQUE` 表示此索引的每一个索引值值对应唯一的数据记录
* `CLUSTER` 表示建立聚簇索引，可以提高查询效率，对不经常更新的列建立

说明:

1. 一个表可以有多个索引
2. 索引会影响系统的开销 (空间、速度)
3. 索引由系统自动使用与维护
4. 先录数据后建索引
5. 对索引列进来定义为`NOT NULL`
6. 使用唯一索引保证列值的唯一性

## 数据查询功能 `DML`

* 不改变数据本身，对现成的基本表和视图进行数据查询
* 单表查询
* 连表查询
* 子查询块嵌套查询
* 并交差集合查询

### 单表查询

#### 投影查询

```SQL
/*查询指定列*/
SELECT Sn, Age, Sex FROM Student;
/*查询全部列*/
SELECT * FROM Student;
/*查询经过计算的值*/
SELECT Sn,2001-Age,ISLOWER(Dept) FROM Student;
/*列标题使用别名*/
SELECT Sn Name,2001-Age Birthday FROM Student;
```

#### 选取查询

```SQL
/*比较大小*/
SELECT * FROM Sc WHERE Cno='C2';
/*多重条件查询*/
SELECT * FROM Sc WHERE Cno='C2' AND G>85;
/*确定范围*/
SELECT * FROM Sc WHERE G>=60 AND G<=75;
SELECT * FROM Sc WHERE G BETWEEN 60 AND 75;
/*消除重复行*/
SELECT DISTINCT Sno FROM Sc
    WHERE Cno='C1' OR Cno='C2';
/*确定集合*/
SELECT DISTINCT Sno FROM Sc
    WHERE Cno IN ('C1', 'C2');

SELECT DISTINCT Sno FROM Sc
    WHERE Dept NOT IN ('CS', 'MA');
/*设计空值的查询*/
SELECT * FROM Course WHERE Cn IS NULL;
SELECT * FROM Course WHERE LEN(TRIM(Cn))=0;
/*字符匹配*/
SELECT * FROM Student WHERE Dept LIKE 'CS';
SELECT * FROM Course WHERE Cn NOT LIKE 'P%';
SELECT * FROM Course WHERE Cn LIKE 'o_';

```

#### 排序查询

```SQL
SELECT * FROM Sc WHERE Cno='C1'
    ORDER BY G DESC;
```

#### 库函数(集函数)查询

* 按列: `AVG, SUM, COUNT, MAX, MIN`
* 按行: `COUNT(*)`

```SQL
SELECT AVG(Age) FROM Student WHERE Dept='CS';
```

#### 分组查询

* 分组子句 `GROUP BY`
* 分组条件 `HAVING`

```SQL
SELECT Sno, COUNT(Sno) FROM Sc
    GROUP BY Sno;

SELECT Sno, COUNT(Sno) FROM Sc
    GROUP BY Sno HAVING COUNT(Sno)>=4;
```

### 连表查询

* 指涉及多个表的查询

#### 等值与非等值连接

```SQL
SELECT Student.*,Sc.* FROM Student, Sc 
    WHERE Student.Sno=Sc.sno;

SELECT Student.Sno,SN,Sex,Age,Dept,Cno,G FROM Student, Sc 
    WHERE Student.Sno=Sc.sno;
```

#### 复合条件连接

```SQL
SELECT Cno, G FROM Student,Sc
    WHERE Student.Sno=Sc.Sno AND SN='Paul';

SELECT Sn,Cn,G FROM Student,Course,Sc
    WHERE Student.Sno=Sc.Sno AND Course.Cno=Sc.Cno;
```

#### 自身连接

```SQL
SELECT X.Sn, X.Age FROM Student X, Student Y
    WHERE X.Age > Y.Age AND Y.Sn='George';
```

### 子查询块嵌套查询

* 嵌套查询是SQL结构化的体现
* 内部查询 (内层查询、子查询)
* 外部查询 (外层查询、父查询、主查询)
* 子查询不能用ORDER

```SQL
SELECT Sname, Sage FROM Student
    WHERE Sdept <> 'IS' AND Sage < ANY
        (SELECT Sage FROM Student WHERE Sdept='IS')
    ORDER BY Sage DESC;

SELECT Sname FROM Student
    WHERE EXISTS
        (SELECT * FROM Sc
            WHERE Sno=Student.Sno AND Cno='1');
```

### 并交差集合查询

* `UNION`
* `INTERSECT`
* `MINUS`

```SQL
SELECT Sno,Cno FROM Sc WHERE Cno='C2'
    UNION
SELECT Sno,Cno FROM Sc WHERE Cno='C3';

SELECT Sno FROM Sc
    WHERE Cno='C2' AND Sno IN
        (SELECT  Sno FROM Sc WHERE Cno='C3');

SELECT Sno FROM Sc
    WHERE Cno='C2' AND Sno NOT IN
        (SELECT Sno FROM Sc WHERE Cno='C3');
```

## 数据操纵功能 `DML`

### 插入数据

```SQL
/*
    插入单个元组
    未指定列名时，VALUE值顺序应与表结构一致
*/
INSERT INTO Student
    VALUES('S11','Lin','M',18,'CS');
/*
    插入单个元组的部分数据值
    列名顺序不一定与表结构一致
    列表名应与VALUE值一一对应
    空值用NULL表示
*/
INSERT INTO Sc(Sno,Cno)
    VALUES('S11', 'C4');
/*插入子查询结果 (多行记录)*/
INSERT INTO Sc(Sno)
    SELECT Sno FROM Student WHERE Dept='CS';
```

### 删除数据

```SQL
/*删除一个元组的值 (一行记录)*/
DELETE FROM Sc WHERE Sno='S7';
/*删除多个元组的值 (多行记录)*/
DELETE FROM Sc WHERE G<60;
DELETE FROM Sc;
/*带子查询的删除语句*/
DELETE FROM Sc WHERE Sno IN
    (SELECT Sno FROM Student WHERE Sn='Jerry')
```

### 修改数据

```SQL
/*修改一个元组的某些列值*/
UPDATE Student SET Age=20 WHERE Sno='S1'
/*修改多个元组的值 (多行)*/
UPDATE Student SET Age=Age+1;
UPDATE Employee SET Salary=2*Salary
    WHERE Job='PROGRAMMER' AND Salary <=3000;
/*带子查询的删除语句*/
UPDATE Sc SET G=0
    WHERE 'CS'=(SELECT Dept FROM Student
                    WHERE Student.Sno=Sc.Sno);
UPDATE Employee SET Salary=(SELECT 1.5*AVG(Salary) FROM Employee);
/*修改操作与数据库的一致性*/
UPDATE Student SET Sno='99299' WHERE Sno='99101';
UPDATE Sc SET Sno='99299' WHERE Sno='99101';
```