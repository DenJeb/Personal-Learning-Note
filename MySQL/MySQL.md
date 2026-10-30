# MySQL

> 🏠 [返回笔记首页](<../README.md>)

## 目录

- [1. 基础篇](#1-基础篇)
  - [1.1 概述](#11-概述)
  - [1.2 SQL 分类](#12-sql-分类)
  - [1.3 DDL](#13-ddl)
  - [1.4 DML](#14-dml)
  - [1.5 DQL](#15-dql)
  - [1.6 DCL](#16-dcl)
  - [1.7 函数](#17-函数)
  - [1.8 约束](#18-约束)
  - [1.9 多表查询](#19-多表查询)
  - [1.10 事务](#110-事务)
- [2. 进阶篇](#2-进阶篇)
  - [2.1 存储引擎](#21-存储引擎)
  - [2.2 索引](#22-索引)
  - [2.3 SQL 优化](#23-sql-优化)
  - [2.4 视图、存储过程、触发器](#24-视图存储过程触发器)
  - [2.5 锁](#25-锁)
  - [2.6 InnoDB 引擎](#26-innodb-引擎)
  - [2.7 MySQL 管理](#27-mysql-管理)

---

## 1. 基础篇

### 1.1 概述

#### 1.1.1 数据库相关概念

| 概念                        | 说明                                             |
| --------------------------- | ------------------------------------------------ |
| DB（数据库）                | 存储数据的仓库，数据按一定格式组织存放           |
| DBMS（数据库管理系统）      | 操纵和管理数据库的大型软件，如 MySQL、Oracle     |
| SQL（Structured Query Language） | 操作关系型数据库的编程语言，是各 DBMS 通用的语言 |

#### 1.1.2 MySQL 简介

MySQL 是一款开源免费的关系型数据库管理系统（RDBMS），由瑞典 MySQL AB 公司开发，后被 Sun 收购，最终归属 Oracle。

- 体积小、速度快、成本低，是目前最流行的开源数据库之一
- 社区版免费，企业版收费

**下载**：Windows 安装器官网下载地址：[downloads.mysql.com/archives/installer](https://downloads.mysql.com/archives/installer/)

Windows 下 MySQL 以服务方式运行：

```bash
net start mysql80    # 启动
net stop mysql80     # 停止
```

客户端连接：

```bash
mysql -u root -p
```

#### 1.1.3 数据模型

MySQL 属于关系型数据库，数据以**二维表**的形式存储：

| id  | name | age  |
| --- | ---- | ---- |
| 1   | Den  | 20   |
| 2   | Jeb  | 18   |

- 一行 = 一条记录，一列 = 一个字段
- 表与表之间通过外键建立关联（见 1.8 约束）

---

### 1.2 SQL 分类

SQL 根据功能主要分为以下几类：

| 分类      | 全称                         | 作用        |
| ------- | -------------------------- | --------- |
| **DDL** | Data Definition Language   | 定义数据库、表结构 |
| **DML** | Data Manipulation Language | 增、删、改数据   |
| **DQL** | Data Query Language        | 查询数据      |
| **DCL** | Data Control Language      | 用户与权限管理   |

---

### 1.3 DDL

> **DDL（Data Definition Language）**：数据定义语言，用于操作数据库和表结构。

#### 1.3.1 数据库操作

**查看数据库**

```sql
SHOW DATABASES;
```

**创建数据库**

```sql
CREATE DATABASE test;
```

指定字符集：

```sql
CREATE DATABASE test
DEFAULT CHARACTER SET utf8mb4;
```

**删除数据库**

```sql
DROP DATABASE test;
```

**使用数据库**

```sql
USE test;
```

**查看当前使用的数据库**

```sql
SELECT DATABASE();
```


#### 1.3.2 表操作

**查看当前数据库中的所有表**

```sql
SHOW TABLES;
```

**查看表结构**

```sql
DESC user;
```

**查看完整建表语句**

```sql
SHOW CREATE TABLE user;
```

**创建表**

```sql
CREATE TABLE user (
    id INT,
    name VARCHAR(20),
    age INT
);
```

**添加字段**

```sql
ALTER TABLE user
ADD email VARCHAR(50);
```

**修改字段类型**

```sql
ALTER TABLE user
MODIFY age TINYINT;
```

**修改字段名和类型**

```sql
ALTER TABLE user
CHANGE name username VARCHAR(20);
```

**删除字段**

```sql
ALTER TABLE user
DROP email;
```

**修改表名**

```sql
ALTER TABLE user
RENAME TO users;
```

**删除表**

```sql
DROP TABLE users;
```

如果不确定表是否存在：

```sql
DROP TABLE IF EXISTS users;
```

> ⚠️ `DROP TABLE` 会删除表结构以及表中的所有数据。

---

### 1.4 DML

> **DML（Data Manipulation Language）**：数据操作语言，用于操作表中的数据。

#### 1.4.1 INSERT

**插入一条数据**

```sql
INSERT INTO users (id, username, age)
VALUES (1, 'Tom', 18);
```

**插入多条数据**

```sql
INSERT INTO users (id, username, age)
VALUES
    (2, 'Jack', 20),
    (3, 'Alice', 19);
```

如果按照表中所有字段的顺序插入：

```sql
INSERT INTO users
VALUES (4, 'Bob', 21);
```


#### 1.4.2 UPDATE

修改数据：

```sql
UPDATE users
SET age = 22
WHERE id = 4;
```

同时修改多个字段：

```sql
UPDATE users
SET username = 'Robert',
    age = 23
WHERE id = 4;
```

> ⚠️ **注意**
>
> `UPDATE` 不写 `WHERE`，会修改所有数据。

```sql
UPDATE users
SET age = 20;
```


#### 1.4.3 DELETE

删除指定数据：

```sql
DELETE FROM users
WHERE id = 4;
```

> ⚠️ **注意**
>
> `DELETE` 不写 `WHERE`，会删除表中的所有数据。

```sql
DELETE FROM users;
```

---

### 1.5 DQL

> **DQL（Data Query Language）**：数据查询语言，核心语句是 `SELECT`。

#### 1.5.1 基本查询

查询所有字段：

```sql
SELECT *
FROM users;
```

查询指定字段：

```sql
SELECT username, age
FROM users;
```


#### 1.5.2 字段别名

使用 `AS`：

```sql
SELECT username AS name
FROM users;
```

`AS` 可以省略：

```sql
SELECT username name
FROM users;
```


#### 1.5.3 条件查询

使用 `WHERE`：

```sql
SELECT *
FROM users
WHERE age > 18;
```

常见比较运算符：

| 运算符         | 含义   |
| ----------- | ---- |
| `=`         | 等于   |
| `<>` / `!=` | 不等于  |
| `>`         | 大于   |
| `<`         | 小于   |
| `>=`        | 大于等于 |
| `<=`        | 小于等于 |


#### 1.5.4 逻辑运算符

**AND**

多个条件同时满足：

```sql
SELECT *
FROM users
WHERE age >= 18
  AND age <= 30;
```

**OR**

满足其中一个条件：

```sql
SELECT *
FROM users
WHERE age < 18
   OR age > 60;
```

**NOT**

对条件取反：

```sql
SELECT *
FROM users
WHERE NOT age = 18;
```


#### 1.5.5 BETWEEN

查询指定范围：

```sql
SELECT *
FROM users
WHERE age BETWEEN 18 AND 30;
```

等价于：

```sql
SELECT *
FROM users
WHERE age >= 18
  AND age <= 30;
```

> 💡 `BETWEEN ... AND ...` 包含两端的值。


#### 1.5.6 IN

判断是否属于指定集合：

```sql
SELECT *
FROM users
WHERE age IN (18, 20, 22);
```

等价于：

```sql
SELECT *
FROM users
WHERE age = 18
   OR age = 20
   OR age = 22;
```


#### 1.5.7 LIKE

用于模糊查询。

`%` 表示任意数量的字符：

```sql
SELECT *
FROM users
WHERE username LIKE 'Tom%';
```

`_` 表示一个字符：

```sql
SELECT *
FROM users
WHERE username LIKE 'Tom_';
```


#### 1.5.8 NULL

不能使用 `=` 判断 `NULL`：

```sql
SELECT *
FROM users
WHERE age = NULL;
```

应该使用 `IS NULL`：

```sql
SELECT *
FROM users
WHERE age IS NULL;
```

判断非空：

```sql
SELECT *
FROM users
WHERE age IS NOT NULL;
```

> ⚠️ `NULL` 表示“没有值”，不是普通的数据值。


#### 1.5.9 DISTINCT

去除查询结果中的重复数据：

```sql
SELECT DISTINCT age
FROM users;
```


#### 1.5.10 聚合函数

> **聚合函数**：对一列数据做整体计算，返回单个结果。
>
> 💡 本小节及 1.5.11~1.5.13 的示例基于 1.8.3 定义的 `dept`/`emp` 表与 1.9.2 的示例数据，首次阅读可先跳到 1.9.2 查看。

| 函数      | 作用             |
| --------- | ---------------- |
| COUNT     | 统计数量         |
| MAX / MIN | 最大值 / 最小值  |
| SUM       | 求和             |
| AVG       | 平均值           |

```sql
-- 查询员工总数、最高薪资、平均薪资
SELECT COUNT(*), MAX(salary), AVG(salary)
FROM emp;
```

> ⚠️ 聚合函数不计算 `NULL` 值（`COUNT(*)` 除外，它统计的是行数）。
>
> 💡 `COUNT(字段)` 会忽略该字段为 `NULL` 的行，`COUNT(*)` 统计所有行。

#### 1.5.11 分组查询

> **分组查询**：按某列分组（`GROUP BY`），对每组分别做聚合计算。

```sql
SELECT 分组字段, 聚合函数(...)
FROM 表
[WHERE 条件]
GROUP BY 分组字段
[HAVING 分组后过滤条件];
```

```sql
-- 查询每个部门的平均薪资
SELECT dept_id, AVG(salary) AS avg_salary
FROM emp
WHERE dept_id IS NOT NULL
GROUP BY dept_id;
```

`WHERE` 与 `HAVING` 的区别：

| 关键字 | 过滤时机 | 能否使用聚合函数 |
| ------ | -------- | ---------------- |
| WHERE  | 分组之前 | 不能             |
| HAVING | 分组之后 | 能               |

```sql
-- 查询平均薪资高于 6500 的部门
SELECT dept_id, AVG(salary) AS avg_salary
FROM emp
WHERE dept_id IS NOT NULL
GROUP BY dept_id
HAVING AVG(salary) > 6500;
```

#### 1.5.12 排序查询

> **排序查询**：`ORDER BY` 对查询结果排序。

```sql
-- 按薪资升序（ASC 为默认值，可省略）
SELECT name, salary
FROM emp
ORDER BY salary;

-- 按薪资降序
SELECT name, salary
FROM emp
ORDER BY salary DESC;

-- 多字段排序：先按部门升序，同部门内按薪资降序
SELECT name, dept_id, salary
FROM emp
ORDER BY dept_id, salary DESC;
```

#### 1.5.13 分页查询

> **分页查询**：`LIMIT` 限制返回的记录数，常配合起始索引实现分页。

```sql
-- 第 1 页：从索引 0 开始取 2 条
SELECT *
FROM emp
LIMIT 0, 2;

-- 第 2 页：跳过前 2 条，取 2 条
SELECT *
FROM emp
LIMIT 2, 2;
```

> 💡 语法为 `LIMIT 起始索引, 查询条数`，起始索引从 0 开始。第 n 页的起始索引 = (n - 1) × 每页条数。

#### 1.5.14 执行顺序

DQL 的书写顺序与执行顺序不同：

```sql
SELECT ...          -- 5
FROM ...            -- 1
WHERE ...           -- 2
GROUP BY ...        -- 3
HAVING ...          -- 4
ORDER BY ...        -- 6
LIMIT ...;          -- 7
```

> 💡 由此可知：`WHERE` 中不能使用 `SELECT` 里定义的别名（`WHERE` 先于 `SELECT` 执行），而 `ORDER BY` 中可以。

---

### 1.6 DCL

> **DCL（Data Control Language）**：数据控制语言，用于管理数据库用户和权限。

#### 1.6.1 用户管理

**查询用户**

```sql
SELECT *
FROM mysql.user;
```

**创建用户**

```sql
CREATE USER 'dev'@'localhost'
IDENTIFIED BY '123456';
```

**修改用户密码**

```sql
ALTER USER 'dev'@'localhost'
IDENTIFIED BY '654321';
```

**删除用户**

```sql
DROP USER 'dev'@'localhost';
```


#### 1.6.2 权限管理

**查询用户权限**

```sql
SHOW GRANTS FOR 'dev'@'localhost';
```

**授予查询权限**

```sql
GRANT SELECT
ON test.users
TO 'dev'@'localhost';
```

**授予所有权限**

```sql
GRANT ALL
ON test.*
TO 'dev'@'localhost';
```

**撤销查询权限**

```sql
REVOKE SELECT
ON test.users
FROM 'dev'@'localhost';
```

---

### 1.7 函数

> 函数是一段可以直接调用的代码。MySQL 中常用函数按功能分为四类：字符串函数、数值函数、日期函数、流程函数。

| 分类       | 常用函数                                                                       |
| ---------- | ------------------------------------------------------------------------------ |
| 字符串函数 | CONCAT、LOWER、UPPER、LPAD、RPAD、TRIM、SUBSTRING、REPLACE、LENGTH、CHAR_LENGTH |
| 数值函数   | CEIL、FLOOR、MOD、RAND、ROUND、ABS                                              |
| 日期函数   | CURDATE、CURTIME、NOW、YEAR、MONTH、DAY、DATE_ADD、DATEDIFF、DATE_FORMAT       |
| 流程函数   | IF、IFNULL、CASE                                                                |

#### 1.7.1 字符串函数

**CONCAT**：拼接多个字符串

```sql
SELECT CONCAT('Hello', ' ', 'World');
-- 结果：Hello World
```

**LOWER / UPPER**：字符串转小写 / 转大写

```sql
SELECT LOWER('MySQL'), UPPER('mysql');
-- 结果：mysql  MYSQL
```

**LPAD / RPAD**：左 / 右填充到指定长度

```sql
SELECT LPAD('1', 5, '0');
-- 结果：00001
```

> 💡 常用于给编号补零，如员工编号 `00001`。

**TRIM**：去除字符串两端的空格

```sql
SELECT TRIM('  hello  ');
-- 结果：hello
```

**SUBSTRING**：截取字符串，从第 start 个字符开始截取 len 个

```sql
SELECT SUBSTRING('Hello World', 1, 5);
-- 结果：Hello
```

**REPLACE**：替换字符串中的指定内容

```sql
SELECT REPLACE('Hello World', 'World', 'MySQL');
-- 结果：Hello MySQL
```

**LENGTH / CHAR_LENGTH**：返回字节数 / 字符数

```sql
SELECT LENGTH('你好'), CHAR_LENGTH('你好');
-- 结果：6  2
```

> ⚠️ `LENGTH` 统计的是字节数。utf8mb4 下一个汉字占 3 个字节，统计字符个数要用 `CHAR_LENGTH`。


#### 1.7.2 数值函数

**CEIL**：向上取整

```sql
SELECT CEIL(1.1);
-- 结果：2
```

**FLOOR**：向下取整

```sql
SELECT FLOOR(1.9);
-- 结果：1
```

**MOD**：取模（求余数）

```sql
SELECT MOD(7, 3);
-- 结果：1
```

**RAND**：生成 0 ~ 1 之间的随机小数

```sql
SELECT RAND();
-- 结果：0.7924302761102893（每次执行都不同）
```

**ROUND**：四舍五入，第二个参数指定保留的小数位数

```sql
SELECT ROUND(3.14159, 2);
-- 结果：3.14
```

**ABS**：取绝对值

```sql
SELECT ABS(-10);
-- 结果：10
```

> 💡 `RAND()` 常用于生成随机验证码或随机抽取数据。


#### 1.7.3 日期函数

**CURDATE / CURTIME / NOW**：当前日期 / 当前时间 / 当前日期时间

```sql
SELECT CURDATE(), CURTIME(), NOW();
-- 结果：2026-10-10  14:30:00  2026-10-10 14:30:00
```

**YEAR / MONTH / DAY**：提取日期中的年 / 月 / 日

```sql
SELECT YEAR('2026-10-10'), MONTH('2026-10-10'), DAY('2026-10-10');
-- 结果：2026  10  10
```

**DATE_ADD**：在日期上加上一个时间间隔

```sql
SELECT DATE_ADD('2026-10-10', INTERVAL 1 DAY);
-- 结果：2026-10-11
```

**DATEDIFF**：计算两个日期相差的天数（第一个日期减去第二个日期）

```sql
SELECT DATEDIFF('2026-10-10', '2026-10-01');
-- 结果：9
```

**DATE_FORMAT**：按指定格式格式化日期

```sql
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
-- 结果：2026-10-10 14:30:00
```

> 💡 常用格式符：`%Y` 四位年份、`%m` 月份、`%d` 日期、`%H` 24 小时制小时、`%i` 分钟、`%s` 秒。


#### 1.7.4 流程函数

**IF**：条件成立返回第一个值，否则返回第二个值

```sql
SELECT username,
       IF(age >= 18, '成年', '未成年') AS status
FROM users;
```

**IFNULL**：第一个值为 `NULL` 时返回第二个值

```sql
SELECT IFNULL(NULL, '默认值');
-- 结果：默认值
```

> 💡 常用于给查询结果中的空值兜底，如把 `NULL` 显示为「暂无」。

**CASE**：多条件判断，类似编程语言中的 if-else if-else

```sql
SELECT username,
       CASE
           WHEN age < 18 THEN '未成年'
           WHEN age < 30 THEN '青年'
           ELSE '中老年'
       END AS age_group
FROM users;
```

> 💡 `CASE` 必须以 `END` 结尾，`ELSE` 可以省略（省略时默认返回 `NULL`）。

---

### 1.8 约束

> **约束**：作用于表中字段上的规则，用于限制存储在表中的数据，保证数据的正确性、有效性和完整性。

约束可以在创建表时添加，也可以在修改表时（`ALTER TABLE`）添加。

#### 1.8.1 约束分类

| 约束     | 描述                                     | 关键字        |
| -------- | ---------------------------------------- | ------------- |
| 非空约束 | 该字段的值不能为 `NULL`                  | NOT NULL      |
| 唯一约束 | 该字段的所有值不能重复                   | UNIQUE        |
| 主键约束 | 一行数据的唯一标识，非空且唯一           | PRIMARY KEY   |
| 默认约束 | 未指定该字段的值时，使用默认值           | DEFAULT       |
| 检查约束 | 保证字段值满足指定条件                   | CHECK         |
| 外键约束 | 让两张表的数据建立连接，保证数据一致性   | FOREIGN KEY   |

> ⚠️ `CHECK` 从 MySQL 8.0.16 开始才真正校验，之前的版本语法上支持但不生效。
>
> 💡 主键 vs 唯一约束：主键非空且唯一，一张表只能有一个；唯一约束允许任意多个 `NULL`（`NULL` 之间不相等，不视为重复），一张表可以有多个唯一约束。`AUTO_INCREMENT` 一般配合主键使用，插入时不指定值会自动递增。

#### 1.8.2 约束演示

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(20) NOT NULL UNIQUE,
    age TINYINT CHECK (age > 0 AND age <= 120),
    status CHAR(1) DEFAULT '1'
);
```

- `id`：主键 + 自增
- `username`：非空 + 唯一
- `age`：检查约束，只能为 1 ~ 120
- `status`：默认值 `'1'`

#### 1.8.3 外键约束

外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。外键所在的表称为**子表**（从表），被引用的表称为**父表**（主表）。

**创建表时添加外键**

```sql
-- 父表
CREATE TABLE dept (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL
);

-- 子表
CREATE TABLE emp (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    dept_id INT,
    CONSTRAINT fk_emp_dept FOREIGN KEY (dept_id) REFERENCES dept (id)
);
```

**已有表添加 / 删除外键**

```sql
ALTER TABLE emp
ADD CONSTRAINT fk_emp_dept
FOREIGN KEY (dept_id) REFERENCES dept (id);

ALTER TABLE emp
DROP FOREIGN KEY fk_emp_dept;
```

> ⚠️ 添加外键后，子表外键字段的值必须能在父表中找到；父表记录被子表引用时不能直接删除。
>
> 💡 外键名称通常命名为 `fk_子表名_父表名`。

#### 1.8.4 外键删除更新行为

当父表记录被删除或更新时，可以指定子表的联动行为：

| 行为                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| NO ACTION / RESTRICT  | 默认行为：父表被引用时禁止删除 / 更新（报错）                |
| CASCADE               | 父表删除 / 更新时，子表中对应记录也删除 / 更新               |
| SET NULL              | 父表删除 / 更新时，子表外键字段置为 `NULL`（要求该字段允许为空） |
| SET DEFAULT           | 父表删除 / 更新时，子表外键字段设为默认值（InnoDB 不支持）                    |

```sql
ALTER TABLE emp
ADD CONSTRAINT fk_emp_dept
FOREIGN KEY (dept_id) REFERENCES dept (id)
ON UPDATE CASCADE
ON DELETE CASCADE;
```

> `ON UPDATE` 指定父表主键更新时的行为，`ON DELETE` 指定父表记录删除时的行为。

---

### 1.9 多表查询

#### 1.9.1 多表关系

实际项目中多张表之间存在关联，常见关系有三种：

**一对多（多对一）**：如部门与员工，一个部门有多个员工。在多的一方（`emp`）建立外键指向一的一方：

```sql
CREATE TABLE emp (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    dept_id INT,
    CONSTRAINT fk_emp_dept FOREIGN KEY (dept_id) REFERENCES dept (id)
);
```

**多对多**：如学生与课程，一个学生选多门课，一门课有多个学生。需要建立**中间表**，拆成两个一对多：

```sql
-- student（id, name）、course（id, name）表结构略
CREATE TABLE student_course (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    CONSTRAINT fk_sc_student FOREIGN KEY (student_id) REFERENCES student (id),
    CONSTRAINT fk_sc_course FOREIGN KEY (course_id) REFERENCES course (id)
);
```

**一对一**：如用户与用户详情。在任意一方建立外键，并给该外键添加 `UNIQUE` 约束。

#### 1.9.2 多表查询概述

直接同时查询多张表会产生**笛卡尔积**：结果为两张表所有行的两两组合，大量组合没有实际意义。

```sql
SELECT *
FROM emp, dept;
-- 行数 = emp 行数 × dept 行数
```

通过**连接条件**消除笛卡尔积：

```sql
SELECT e.name, d.name
FROM emp e, dept d
WHERE e.dept_id = d.id;
```

> 💡 多表查询时通常给表起别名（`AS` 可省略），用 `表别名.字段名` 指代字段。

本节示例数据（`emp` 在 1.8.3 基础上增加 `salary` 薪资、`manager_id` 领导 id 两列，`Bob` 的 `dept_id` 为 `NULL` 供外连接演示）：

```sql
INSERT INTO dept VALUES
    (1, '研发部'), (2, '市场部'), (3, '财务部'), (4, '销售部');

INSERT INTO emp VALUES
    (1, 'Tom', 1, 8000, NULL),
    (2, 'Jack', 1, 6000, 1),
    (3, 'Alice', 2, 7000, 1),
    (4, 'Bob', NULL, 5000, 2);
```

#### 1.9.3 内连接

> **内连接**：只返回两张表之间能匹配上的数据。

隐式写法（`WHERE` 连接条件，即 1.9.2 的写法）：

```sql
SELECT e.name, d.name
FROM emp e, dept d
WHERE e.dept_id = d.id;
```

显式写法（`INNER JOIN ... ON`，推荐）：

```sql
SELECT e.name, d.name
FROM emp e
INNER JOIN dept d ON e.dept_id = d.id;
```

> ⚠️ 没有部门的 `Bob`（`dept_id` 为 `NULL`）不会出现在内连接结果中。

#### 1.9.4 外连接

> **外连接**：返回一方表的全部数据，另一表无匹配时补 `NULL`。

左外连接（包含左表全部）：

```sql
SELECT e.name, d.name
FROM emp e
LEFT JOIN dept d ON e.dept_id = d.id;
-- 包含所有员工，Bob 的部门名显示为 NULL
```

右外连接（包含右表全部）：

```sql
SELECT e.name, d.name
FROM emp e
RIGHT JOIN dept d ON e.dept_id = d.id;
-- 包含所有部门，没有员工的部门员工名显示为 NULL
```

> 💡 左外连接最常用。右外连接都可以换成左外连接（两张表互换位置），掌握一种即可。

#### 1.9.5 自连接

> **自连接**：表与自身进行连接。必须给表起**不同的别名**来区分角色。

```sql
-- 查询员工及其直属领导（manager_id 指向本表的 id）
SELECT e.name AS 员工, m.name AS 领导
FROM emp e
LEFT JOIN emp m ON e.manager_id = m.id;
```

> 💡 使用左外连接是为了保留没有领导的员工（如 `Tom`）。

#### 1.9.6 联合查询

> **联合查询**：用 `UNION` 把多个查询的结果合并成一个结果集。

```sql
SELECT name FROM emp WHERE salary > 7000
UNION
SELECT name FROM emp WHERE dept_id = 2;
```

- `UNION`：合并并**去重**
- `UNION ALL`：合并但**不去重**

> ⚠️ 各查询的列数必须一致，字段类型也要兼容。

#### 1.9.7 子查询

> **子查询**：SQL 语句中嵌套的 `SELECT` 语句（写在括号内），外层的为主查询。

按返回结果分类：

| 类型       | 返回结果       | 常用操作符              |
| ---------- | -------------- | ----------------------- |
| 标量子查询 | 单个值         | `=  <>  >  <`           |
| 列子查询   | 一列（多行）   | `IN  NOT IN  ANY  ALL`  |
| 行子查询   | 一行多列       | `=  <>  IN`             |
| 表子查询   | 多行多列       | 作为临时表（`FROM` 后） |

#### 1.9.8 标量子查询

返回单个值，常用 `=` 等比较运算符：

```sql
-- 查询在研发部工作的员工
SELECT *
FROM emp
WHERE dept_id = (SELECT id FROM dept WHERE name = '研发部');
```

#### 1.9.9 列子查询

返回一列多行，常配合 `IN`：

```sql
-- 查询有员工的部门
SELECT *
FROM dept
WHERE id IN (SELECT dept_id FROM emp WHERE dept_id IS NOT NULL);
```

- `ANY` / `SOME`：满足子查询结果中的**任意一个**即可

```sql
-- 查询薪资比研发部任意一人高的员工
SELECT *
FROM emp
WHERE salary > ANY (SELECT salary FROM emp WHERE dept_id = 1);
```

- `ALL`：必须满足子查询结果中的**全部**

```sql
-- 查询薪资比市场部所有人都高的员工
SELECT *
FROM emp
WHERE salary > ALL (SELECT salary FROM emp WHERE dept_id = 2);
```

#### 1.9.10 行子查询

返回一行多列，用括号包裹多列进行比较：

```sql
-- 查询与 Tom 薪资、部门都相同的员工
SELECT *
FROM emp
WHERE (salary, dept_id) = (SELECT salary, dept_id FROM emp WHERE name = 'Tom');
```

#### 1.9.11 表子查询

返回多行多列，作为临时表放在 `FROM` 后（通常需要起别名；MySQL 8.0.14 起可省略）：

```sql
-- 查询薪资高于本部门平均薪资的员工
SELECT e.name, e.salary
FROM emp e
JOIN (SELECT dept_id, AVG(salary) AS avg_salary
      FROM emp
      WHERE dept_id IS NOT NULL
      GROUP BY dept_id) t
  ON e.dept_id = t.dept_id
WHERE e.salary > t.avg_salary;
```

---

### 1.10 事务

#### 1.10.1 事务简介

> **事务**：一组操作的集合，是不可分割的工作单位——这些操作**要么同时成功，要么同时失败**。

典型场景：转账（A 账户 -500，B 账户 +500），两步必须同时成功或同时失败，否则会出现数据错误。

默认情况下，MySQL 中每条 DML 语句都是自动提交的，无法保证一组操作的整体性，因此需要事务来控制。

#### 1.10.2 事务操作

示例数据：

```sql
CREATE TABLE account (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    money DECIMAL(10, 2)
);

INSERT INTO account VALUES
    (1, '张三', 1000),
    (2, '李四', 1000);
```

**方式一：修改提交方式**

```sql
SELECT @@autocommit;    -- 1：自动提交，0：手动提交
SET @@autocommit = 0;   -- 改为手动提交

UPDATE account SET money = money - 500 WHERE name = '张三';
UPDATE account SET money = money + 500 WHERE name = '李四';

COMMIT;                 -- 全部成功 → 提交
ROLLBACK;               -- 出错 → 回滚，撤销事务内所有操作
```

**方式二：显式开启事务（推荐）**

```sql
START TRANSACTION;      -- 或 BEGIN

UPDATE account SET money = money - 500 WHERE name = '张三';
UPDATE account SET money = money + 500 WHERE name = '李四';

COMMIT;                 -- 全部成功 → 提交
ROLLBACK;               -- 任一步失败 → 回滚
```

#### 1.10.3 四大特性 ACID

| 特性                     | 说明                                         |
| ------------------------ | -------------------------------------------- |
| 原子性 Atomicity         | 事务是最小的操作单元，要么全部成功，要么全部失败 |
| 一致性 Consistency       | 事务完成后，所有数据必须保持一致的合法状态   |
| 隔离性 Isolation         | 多个事务并发执行时互不干扰（由隔离级别保证） |
| 持久性 Durability        | 事务一旦提交，对数据的改变就是永久的         |

#### 1.10.4 并发事务问题

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事务读到了另一个事务**未提交**的数据                     |
| 不可重复读 | 同一事务两次读取同一条记录，结果不同（期间被其他事务修改）   |
| 幻读       | 同一事务按条件查询到的行数前后不一致（期间被其他事务插入/删除） |

#### 1.10.5 事务隔离级别

| 隔离级别           | 脏读 | 不可重复读 | 幻读 | 说明                       |
| ------------------ | ---- | ---------- | ---- | -------------------------- |
| READ UNCOMMITTED   | 发生 | 发生       | 发生 | 性能最高，问题最多         |
| READ COMMITTED     | 解决 | 发生       | 发生 |                            |
| REPEATABLE READ    | 解决 | 解决       | 发生 | MySQL 默认级别             |
| SERIALIZABLE       | 解决 | 解决       | 解决 | 串行执行，性能最低         |

```sql
-- 查看隔离级别
SELECT @@TRANSACTION_ISOLATION;

-- 设置隔离级别（SESSION 当前会话 / GLOBAL 全局，二选一）
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

> 💡 隔离级别越高，数据越安全，但性能越低。
>
> ⚠️ MySQL 默认级别 REPEATABLE READ 在实际中（InnoDB 的 MVCC 机制下）基本不会出现幻读；5.7 及更早版本查看隔离级别的变量名为 `tx_isolation`。

---

## 2. 进阶篇

> 2.4 及以下章节待补充。

### 2.1 存储引擎

#### 2.1.1 MySQL 体系结构

MySQL 整体分为四层：

![MySQL 体系结构](<图片和附件/MySQL 体系结构.png>)

#### 2.1.2 存储引擎简介

> **存储引擎**：存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎基于**表**（同一库中不同表可以指定不同引擎），是插件式的。

```sql
-- 查看支持的引擎
SHOW ENGINES;

-- 建表时指定引擎
CREATE TABLE t1 (
    id INT PRIMARY KEY
) ENGINE = INNODB;

-- 查看表使用的引擎
SHOW CREATE TABLE t1;
```

![三种常见存储引擎的特点](<图片和附件/存储引擎特点.jpg>)

> ⚠️ MySQL 5.5 之后默认引擎是 InnoDB。

#### 2.1.3 InnoDB

> **InnoDB**：兼顾高可靠性和高性能的通用存储引擎，MySQL 默认引擎。

特点：

- 支持事务，DML 遵循 ACID
- 行级锁，并发访问性能高
- 支持外键约束
- 每张表对应一个表空间文件 `xxx.ibd`（8.0 起，表结构、数据、索引都存在其中）

#### 2.1.4 MyISAM

> **MyISAM**：MySQL 早期的默认引擎。

特点：

- 不支持事务、不支持外键
- 表级锁，并发能力弱
- 访问速度快
- 对应三个文件：`xxx.sdi`（表结构）、`xxx.MYD`（数据）、`xxx.MYI`（索引）

> 💡 适合读多写少、不需要事务的场景，如日志表、报表数据。

#### 2.1.5 Memory

> **Memory**：数据全部存放在内存中的引擎。

特点：

- 数据存内存，服务重启后**数据丢失**
- 默认使用 hash 索引，访问速度极快
- 表结构文件 `xxx.sdi`

> 💡 适合临时表、缓存表等对持久性无要求、对速度要求高的场景。

#### 2.1.6 存储引擎选择

![存储引擎对比](<图片和附件/存储引擎对比.png>)

> 💡 没有特殊要求时一律使用 InnoDB。

---

### 2.2 索引

#### 2.2.1 索引概述

> **索引（index）**：帮助 MySQL 高效获取数据的**有序**数据结构。

优点：

- 提高数据检索效率，降低磁盘 IO 成本
- 通过索引列对数据排序，降低排序成本

缺点：

- 索引列占用磁盘空间
- 降低增删改（INSERT / UPDATE / DELETE）的效率（每次修改都需要维护索引）

```sql
-- 无索引：全表扫描，逐行比较
SELECT *
FROM users
WHERE age = 45;
```

#### 2.2.2 索引结构

> MySQL 常见的索引结构：B+Tree、Hash、R-tree（空间索引）、Full-text（全文索引）。InnoDB 默认使用 **B+Tree**。

结构演变（为什么不直接用二叉树）：

![索引结构对比](<图片和附件/索引结构对比.png>)

> 💡 索引存储在磁盘上，每访问一个节点就是一次磁盘 IO。树越"矮胖"，IO 次数越少。

B+Tree 与 B-Tree 的区别：

1. 所有数据都存储在**叶子节点**，非叶子节点只存键值（索引值）——每个节点能存更多键，树更矮
2. 叶子节点之间用[双向链表](<../数据结构/数据结构（C语言）.md#2-3>)连接——范围查询和排序效率高

> 💡 二叉树、红黑树、B-Tree / B+Tree 的理论知识见 [二叉树](<../数据结构/数据结构（C语言）.md#5-2>)、[红黑树](<../数据结构/数据结构（C语言）.md#7-7>)、[B 树](<../数据结构/数据结构（C语言）.md#7-8>)与[B+ 树](<../数据结构/数据结构（C语言）.md#7-9>)。

#### 2.2.3 Hash 索引

> **Hash 索引**：基于哈希表实现，Memory 引擎默认使用。

- 等值查询效率极高，O(1)
- 不支持范围查询（如 `>`、`BETWEEN`），无法利用索引排序
- 存在 hash 冲突问题

> 💡 哈希表的实现细节见 [散列表（哈希表）](<../数据结构/数据结构（C语言）.md#7-10>)。

#### 2.2.4 为什么 InnoDB 选择 B+Tree

1. 相比二叉树 / 红黑树：B+Tree 是多路平衡树，层级低，磁盘 IO 次数少
2. 相比 B-Tree：数据都在叶子节点且成链表，范围查询、排序更高效
3. 相比 Hash：支持范围查询和排序（Hash 只适合等值查询）

> 💡 详细对比与推导见 [B 树](<../数据结构/数据结构（C语言）.md#7-8>)与[B+ 树](<../数据结构/数据结构（C语言）.md#7-9>)。

#### 2.2.5 索引分类

按存储形式：

| 类型     | 说明                                                                 |
| -------- | -------------------------------------------------------------------- |
| 聚簇索引 | 索引与行数据存储在一起，叶子节点保存的是**行数据**；每张表有且仅有一个 |
| 二级索引 | 索引与数据分开存储，叶子节点保存的是对应的**主键值**；一张表可以有多个 |

> **回表查询**：先通过二级索引查到主键值，再回到聚簇索引查完整的行数据。

按功能：

| 类型     | 关键字                         |
| -------- | ------------------------------ |
| 主键索引 | PRIMARY KEY（主键自动创建）    |
| 唯一索引 | UNIQUE                         |
| 普通索引 | INDEX                          |
| 全文索引 | FULLTEXT（用于全文检索）       |

按字段个数：单列索引、联合索引（组合索引，遵循[最左前缀法则](#228-索引使用)）。

> 💡 没有主键时，InnoDB 使用第一个非空唯一索引作为聚簇索引；两者都没有时自动生成隐藏的 `row_id`。

#### 2.2.6 索引语法

```sql
-- 创建普通索引
CREATE INDEX idx_emp_name ON emp (name);

-- 创建唯一索引
CREATE UNIQUE INDEX idx_users_username ON users (username);

-- 创建联合索引
CREATE INDEX idx_emp_dept_salary ON emp (dept_id, salary);

-- 查看表的索引
SHOW INDEX FROM emp;

-- 删除索引
DROP INDEX idx_emp_name ON emp;
```

#### 2.2.7 性能分析

**查看执行频次**

```sql
SHOW GLOBAL STATUS LIKE 'Com_______';
```

通过 Select / Insert / Update / Delete 的计数，判断当前数据库以查询为主还是以增删改为主。

**慢查询日志**

记录执行时间超过指定时长的 SQL（默认 10 秒），默认关闭：

```sql
-- 查看慢查询日志配置
SHOW VARIABLES LIKE 'slow_query_log';

-- 动态开启（重启失效，永久生效需修改配置文件）
SET GLOBAL slow_query_log = 1;
SET GLOBAL long_query_time = 2;
```

**SHOW PROFILES**

查看每条 SQL 的具体耗时（先开启 profiling）：

```sql
SET profiling = 1;

SHOW PROFILES;             -- 查看各 SQL 的耗时
SHOW PROFILE FOR QUERY 5;  -- 查看指定 SQL 各阶段的耗时
```

> ⚠️ MySQL 8.0 中 profiling 已过时，官方推荐使用 performance_schema 分析。

**EXPLAIN 执行计划**

```sql
EXPLAIN SELECT * FROM emp WHERE name = 'Tom';
```

重点字段：

| 字段         | 说明                                                        |
| ------------ | ----------------------------------------------------------- |
| id           | 查询序号（多表查询时越大越先执行，相同则从上到下）          |
| type         | 访问类型，性能由好到差：system > const > eq_ref > ref > range > index > ALL |
| possible_keys| 可能用到的索引                                              |
| key          | 实际使用的索引（NULL 表示没用上索引）                       |
| key_len      | 索引使用的字节数                                            |
| rows         | 预估扫描的行数                                              |
| Extra        | 附加信息（Using where / Using index 等）                    |

> 💡 性能优化最关注 `type`（避免 ALL 全表扫描）和 `key`（是否用上索引）。

#### 2.2.8 索引使用

**最左前缀法则**

> 联合索引要遵守**最左前缀法则**：查询条件从索引的最左列开始，且不跳过中间的列，索引才能完整生效。

以联合索引 `idx(a, b, c)` 为例：

| 查询条件                      | 索引使用情况          |
| ----------------------------- | --------------------- |
| `WHERE a = 1`                 | 使用 a                |
| `WHERE a = 1 AND b = 2`       | 使用 a、b             |
| `WHERE a = 1 AND b = 2 AND c = 3` | 全部使用          |
| `WHERE b = 2`（缺少最左列 a） | 索引失效              |
| `WHERE a = 1 AND c = 3`（跳过 b） | 只使用 a，c 用不上 |

**范围查询**

联合索引中，范围查询（`>`、`<`）右侧的列索引失效；`>=`、`<=` 不会失效。

```sql
-- 建立联合索引 (age, status)
CREATE INDEX idx_users_age_status ON users (age, status);

-- status 用不到索引：age 是范围查询（>），右侧列失效
SELECT * FROM users WHERE age > 30 AND status = '0';

-- status 可以用到索引：>= 不导致右侧列失效
SELECT * FROM users WHERE age >= 30 AND status = '0';
```

**索引失效的情况**

1. 在索引列上进行运算或函数操作：`WHERE SUBSTRING(name, 1, 3) = 'Tom'`、`WHERE id + 1 = 10`
2. 字符串字段与数字比较时不加引号（发生隐式类型转换）：`WHERE sn = 100000003145001`
3. 头部模糊匹配：`WHERE name LIKE '%张'`（`'张%'` 可以使用索引，`'%张%'` 不行）
4. 用 `OR` 连接的条件：两侧字段都有索引才生效，任一侧没有索引则整体失效
5. 数据分布影响：MySQL 评估走索引比全表扫描还慢时（如条件几乎命中全表），会放弃索引

> 💡 是否用上索引看 `EXPLAIN`：`key` 为 NULL 表示未使用，`type` 为 ALL 表示全表扫描。

**SQL 提示**

优化器有时会选错索引，可以手动提示：

```sql
-- 建议使用指定索引
SELECT * FROM users USE INDEX (idx_users_age_status) WHERE age = 30;

-- 忽略指定索引
SELECT * FROM users IGNORE INDEX (idx_users_age_status) WHERE age = 30;

-- 强制使用指定索引
SELECT * FROM users FORCE INDEX (idx_users_age_status) WHERE age = 30;
```

**覆盖索引**

> **覆盖索引**：查询需要返回的列在索引中能全部找到，无需回表查询。

- 尽量使用覆盖索引，少用 `SELECT *`
- `EXPLAIN` 的 `Extra` 列：
  - `Using index`：使用了覆盖索引，不需要回表
  - `Using index condition`：用到了索引，但需要回表
  - `Using where; Using index`：查找用了索引，所需数据也在索引中，无需回表

**前缀索引**

> 字符串字段较长时，可以只对字段的前缀建立索引，减小索引占用的空间。

```sql
-- 只对 username 的前 5 个字符建索引
CREATE INDEX idx_users_username_prefix ON users (username(5));

-- 计算前缀的区分度，选择合适的前缀长度（越接近 1 越好）
SELECT COUNT(DISTINCT SUBSTRING(username, 1, 5)) / COUNT(*) FROM users;
```

**单列索引与联合索引**

查询条件涉及多个字段时，优先建立联合索引：多个单列索引往往只能用上其中一个，且容易回表；联合索引更容易做到覆盖索引。

#### 2.2.9 索引设计原则

1. 数据量较大且查询频繁的表才建立索引（小表直接全表扫描更快）
2. 常作为 `WHERE`、`ORDER BY`、`GROUP BY` 条件的字段建立索引
3. 选择**区分度**高的列建索引（区分度 = `COUNT(DISTINCT 列) / COUNT(*)`，越接近 1 越好）；能建唯一索引就建唯一索引
4. 字符串字段较长时，根据字段特点建立前缀索引
5. 尽量使用联合索引、减少单列索引——更容易覆盖索引，节省空间、避免回表
6. 控制索引数量：索引越多，增删改时维护索引的代价越大
7. 索引列尽量加 `NOT NULL` 约束——优化器知道列不含 NULL 值，能更好地选择索引

> 💡 索引不是越多越好：索引提升查询效率，但占用磁盘空间并拖慢增删改，需要在两者之间权衡。

---

### 2.3 SQL 优化

#### 2.3.1 插入数据优化

**批量插入**：一次插入多条代替多次单条插入，减少事务提交和网络往返开销。

```sql
-- 推荐：一次插入多条（单次建议 500~1000 条，不要过大）
INSERT INTO users (username, age)
VALUES ('Tom', 18), ('Jack', 20), ('Alice', 19);
```

**手动控制事务**：

```sql
START TRANSACTION;
INSERT INTO users (id, username, age) VALUES (1, 'Tom', 18), (2, 'Jack', 20);
INSERT INTO users (id, username, age) VALUES (3, 'Alice', 19), (4, 'Bob', 21);
COMMIT;
```

> 💡 大批量插入时，逐条自动提交会让每条 INSERT 都触发一次事务和日志刷盘，手动包在一个事务里可以大幅减少开销。

**主键顺序插入**：按主键递增顺序插入，比乱序插入快——顺序插入只需追加到 B+Tree 尾部，乱序插入可能触发页分裂。

**大批量插入使用 LOAD 指令**：

```sql
LOAD DATA LOCAL INFILE '/path/users.csv'
INTO TABLE users
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n';
```

> 💡 百万级数据导入时，`LOAD DATA` 比逐条 INSERT 快一个数量级；导入前可临时关闭唯一性校验（`SET UNIQUE_CHECKS = 0`），导入后再开启。

#### 2.3.2 主键优化

InnoDB 的数据按主键顺序组织存储（聚簇索引），因此主键设计直接影响性能。

**页分裂与页合并**：

- 页分裂：乱序插入（如主键用 UUID 随机值）时，新数据可能插入已满的页中间，导致页拆分成两页，浪费空间且降低性能
- 页合并：删除大量记录后，相邻页的利用率过低时，InnoDB 会把它们合并

**主键设计原则**：

1. 尽量使用**自增主键**（`AUTO_INCREMENT`），保证顺序插入，避免页分裂
2. 主键尽量短：二级索引叶子节点存的是主键值，主键越长，所有二级索引都越大
3. 避免使用 UUID、身份证号等作为主键（太长且无序）

> ⚠️ 不要用有业务含义的字段（如手机号）做主键，业务变更时改主键代价极大。

#### 2.3.3 ORDER BY 优化

排序有两种方式：

| 方式         | 说明                                                    |
| ------------ | ------------------------------------------------------- |
| Using filesort | 通过索引或全表扫描读出数据，再在内存/磁盘中排序，效率低 |
| Using index    | 直接按索引的有序性返回结果，无需额外排序，效率高        |

```sql
-- 联合索引 (age, phone) 下（只查索引列，才能 Using index）
SELECT age, phone FROM users ORDER BY age;          -- Using index（age 有序）
SELECT age, phone FROM users ORDER BY age, phone;   -- Using index（联合索引有序）
SELECT age, phone FROM users ORDER BY phone;        -- Using filesort（不满足最左前缀）
```

> 💡 优化目标：尽量让 `ORDER BY` 走索引顺序，`EXPLAIN` 的 Extra 中出现 `Using index`。

**多字段排序方向**：联合索引要求排序方向一致，`ORDER BY age ASC, phone DESC` 无法完全利用索引（MySQL 8.0 支持降序索引，可显式创建 `INDEX (age ASC, phone DESC)`）。

#### 2.3.4 GROUP BY 优化

`GROUP BY` 本质是排序后分组，因此同样可以走索引：

- 分组字段满足最左前缀法则时，可利用索引有序性减少排序开销
- 分组字段顺序与联合索引顺序一致时效率最高
- 若分组后还需要 `HAVING` 过滤，尽量让过滤条件也能用上索引

```sql
-- 联合索引 (dept_id, salary)
SELECT dept_id, AVG(salary) FROM emp GROUP BY dept_id;        -- 可用索引
SELECT dept_id, AVG(salary) FROM emp GROUP BY dept_id, salary; -- 可用索引
```

#### 2.3.5 LIMIT 优化

深分页是常见性能问题：`LIMIT 1000000, 10` 需要先扫描并丢弃前 100 万条记录。

```sql
-- 慢：扫描 100 万 + 10 行
SELECT * FROM users ORDER BY id LIMIT 1000000, 10;
```

优化方案：

1. **子查询 + 覆盖索引**：先在索引上定位起始主键，再回表取数据

```sql
SELECT u.*
FROM users u
JOIN (SELECT id FROM users ORDER BY id LIMIT 1000000, 10) t ON u.id = t.id;
```

2. **基于上一页的最后一条记录**（游标分页，适合连续翻页）：`WHERE id > 上一页最后一条的 id LIMIT 10`

3. **业务约束**：限制最大可翻页数，或改为"加载更多"式分页

#### 2.3.6 COUNT 优化

`COUNT(*)`、`COUNT(1)`、`COUNT(列名)` 的区别：

| 写法         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| COUNT(*)     | 统计所有行，InnoDB 会选最小的二级索引扫描（不读数据，速度快）|
| COUNT(1)     | 统计所有行，效果与 COUNT(*) 接近                             |
| COUNT(列名)  | 只统计该列非 NULL 的行                                       |
| COUNT(主键)  | 统计所有行（主键非空）                                       |

> 💡 InnoDB 不像 MyISAM 那样在元数据中保存总行数（因为有事务和 MVCC），`COUNT(*)` 需要扫描索引计数。数据量大时，可考虑用计数表维护总数。

#### 2.3.7 UPDATE 优化

`UPDATE` 尽量按**索引字段**定位记录：使用索引时加的是行锁，不走索引时会退化为表锁，并发能力大幅下降。

```sql
-- 有 id 索引：只锁对应行，其他行可并发修改
UPDATE users SET age = 20 WHERE id = 1;

-- 无索引字段：扫描全表（相当于锁表），并发性能差
UPDATE users SET age = 20 WHERE username = 'Tom';
```

> ⚠️ 与索引无关的条件在事务中执行 UPDATE，容易造成并发阻塞甚至死锁。

---

### 2.4 视图、存储过程、触发器

---

### 2.5 锁

---

### 2.6 InnoDB 引擎

---

### 2.7 MySQL 管理

