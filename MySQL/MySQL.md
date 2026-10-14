# MySQL

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

> 以下章节待补充。

### 2.1 存储引擎

---

### 2.2 索引

---

### 2.3 SQL 优化

---

### 2.4 视图、存储过程、触发器

---

### 2.5 锁

---

### 2.6 InnoDB 引擎

---

### 2.7 MySQL 管理

