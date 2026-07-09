# mysql 数据库

## 数据库介绍

什么是数据库： 数据库是按照数据结构来组织、存储和管理数据的仓库。

## RDBMS（关系型数据库）专业术语：

- DBMS： 数据库管理系统

- 表： 具有固定的列数，和任意的行数。

- 数据库： 数据库是一些关联表的集合。

- 列： 一个数据项 Field 字段

- 行： 一条记录 row.

- 主键： 主键是唯一的，一个数据表中只能包含一个主键。可以使用主键来查询数据。

- 外键： 外键用于关联两个表。

- 索引： 使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。

- 数据库对象： 存储，管理和使用数据的不同结构形式。如： 表，视图，存储过程，函数，触发器，事件等。

- 数据库： 存储数据库对象的容器。

- MySQL 使用标准的 SQL 数据语言形式

- 事务： 是指作为单个逻辑工作单元执行的一系列操作，要么完全地执行，要么完全地不执行。

## mysql 数据库分为两种：

- 系统数据库（数据库服务器自带的，共有四个）

    + information_schema 存储数据库对象信息、如用户表信息，列信息，根除，字符，区分，里面的内容我们不能动。

    + performance_schema 存储数据库服务器性能参数信息。

    + mysql 存储数据库用户权限信息。

    + sys 通过这个库可以快速的了解系统的元数据信息，这个库是通过视图的形式把 information_schema 和 performance_schema 结合起来，查询出更加令人容易理解的数据。

- 用户数据库（用户自己创建的）

## 存储引擎说明：

- mysql 中的数据用各种不同的技术存储在文件（或者内存）中。

- 每一种技术都使用不同的存储机制，索引技巧，锁定水平并且最终提供广泛的不同的功能和能力。

- 通过选择不同的技术，你能够获得额外的速度或者功能，从而改善你的应用的整体功能。

- 不同的存储引擎性能是不一样的。

### 存储引擎分类：

- MYISAM 不支持事务，也不支持外键，访问速度块，对事务完整性没有要求或者以 select, insert 为主的应用基本都可以使用这个引擎来创建。每个 MYISAM 在磁盘上存储成 3 个文件，其中文件名和表名都相同，但是扩展名为：

    + .frm （存储表定义）结构。

    + .MYD （MYData, 存储数据）数据。

    + .MYI （MYIndex, 存储索引）索引。
- INNODB 该存储引擎提供了具有提交，回滚和崩溃恢复能力的事务安全。但是对比 MYISAM 的存储引擎，INNODB 的处理效率差一些，并会占用更多的磁盘空间以保存数据和索引。mysql 默认使用
- MEMORY: memory 使用存储在内存中的内容来创建表。每个 memory 表实际对应一个磁盘文件，格式是 .frm MEMORY类型的表访问非常快，因为它的数据是放在内存中的，并且默认使用 HASH 索引，但是一旦服务器关闭，表中的数据就会丢失，但是表还会继续存在。



|   特点   | MyISAM | InnoDB | Memory | Archive |
| :------: | :----: | :----: | :----: | :-----: |
| 存储限制 | 256TB  |  64TB  |   有   |   无    |
| 事务安全 |   -    |  支持  |   -    |    -    |
| 支持索引 |  支持  |  支持  |  支持  |         |
|  锁粒度  |  表锁  |  行锁  |  表锁  |  行锁   |
| 数据压缩 |  支持  |   -    |   -    |  支持   |
| 支持外键 |   -    |  支持  |   -    |    -    |



## MySQL 语句规范

- SQL 语句可以单行或多行书写，以分号结尾。
- SQL 语句可以使用 空格/缩进 来增强语句的可读性。
- MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
- 数据库名称、表名称、字段名称全部小写
- 单行注释 -- 注释内容 或 # 注释内容 (MySQL特有)，多行注释 /* 注释内容 */

## 数据库命令

**登录数据库**  输入命令后回车，填写密码。

```sql
mysql -u root -p

-- 或者

mysql [-h 127.0.0.1] [-P 3306] -u root -p
```

退出登录

```mysql
exit;
```

查看数据库  查看都有哪些数据库

```sql
-- SHOW {DATABASES | SCHEMAS} [LIKE 'pattern' | WHERE expr]

SHOW DATABASES;
```

查看数据库的创建详细信息

```sql
-- SHOW CREATE DATABASE 数据库名称;

SHOW CREATE DATABASE itcast;
```

创建数据库

```sql
-- CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name

-- IF NOT EXISTS 忽略警告信息（不会直接把警告信息显示在客户端）
-- SHOW WARNINGS; -- 查看警告信息
create database 数据库的名称;

-- 创建指定编码格式的数据库
create database 数据库名称 character set utf8mb4;
```

修改数据库的字符类型

```sql
alter database 数据库的名称 character set 编码名;
```

删除数据库

```sql
-- DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;
drop database 数据库的名称;
```

使用数据库

```sql
use 数据库的名称;
```

查看当前在使用哪个数据库

```sql
SELECT DATABASE();

-- 或者
select database()\s;
```

查看数据库值都有哪些表

```sql
-- 先 use 数据库名，进入数据库后 再查看有哪些表。

SHOW TABLES;
```

- 没有数据库更改名称的 命令，要想更改名称，可以创建个 新库，然后把表复制过来。删除旧库。

**更改用户的密码（SQL语句）**

```sql
use mysql;          -- 进入数据库
update user set authentication_string = password('123456') where user = 'root' and Host = 'localhost';        -- 更新 root 的密码, host可以省略
flush privileges;   -- 刷新 MySQL 的系统权限相关表
exit;               -- 退出 或者 quit;

-- 或者使用 mysql 命令 （不用进入数据库） 123456 新密码
mysqladmin -u root -p password 123456; -- 输入原始密码。
-- 密码更新完成，可以用新密码连接登录了。
```

## SQL

- SQL(struct query language 结构化查询语言)

- SQL 是专为数据库而建立的操作命令集，是一种功能齐全的数据库语言。

- SQL 语言的功能包括查询、操纵、定义和控制，是一个综合的、通用的关系数据库语言。

### SQL功能划分

- DDL： 数据库定义语言 用来定义数据库对象： 创建、修改或删除 库，表，列等。(create, drop, alter)

- DML： 数据操作语言 用来操作数据库表中的记录（增加、删除、更改） (insert, update, delete)

- DQL： 数据查询语言 用来查询数据（查询） (select)

- DCL： 数据控制语言 用来定义访问权限和安全级别。(grant, commit)

### SQL 中的常用数据类型：

- int: 字段长度不够时，会自动补零。
- double: 浮点型， 例如 double(5, 2) 表示最多5位，其中必须有两位小数，即最大值为 999.99
- varchar: 可变长度字符串类型， varchar(10)
- text: 字符串类型
- enum('value1', 'value2', ...)：枚举值。1个或2和字节，取决于枚举值的个数（最多65535个值）
- blob: 二进制类型 二进制一般是在数据库中存储一个 路径地址链接
- date: 日期类型， 格式为： yyyy-MM-dd;
- time: 时间类型， 格式为： hh:mm:ss;
- datetime: 日期时间类型， 格式为： yyyy-MM-dd hh:mm:ss;
- boolean

在 mysql 中，字符串类型和日期类型都要用单引号括起来。如： 'Myxq' '2019-01-11'

### DDL

DDL 数据库定义语言，用来定义数据库对象(数据库，表, 字段)

##### 数据库操作

###### 查看都有哪些数据库

```sql
SHOW DATABASES;
```

###### 创建数据库

```sql
-- CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];

CREATE DATABASE itcast;

CREATE DATABASE if NOT EXISTS itcast1;

CREATE DATABASE if NOT EXISTS itcast2 DEFAULT CHARSET utf8; -- utf8 字符集已淘汰

CREATE DATABASE IF NOT EXISTS itcast3 DEFAULT CHARSET utf8mb4; -- 更通用

-- 等价

CREATE DATABASE IF NOT EXISTS itcast4 CHARACTER SET utf8mb4; -- 推荐

-- 8.0+ 后 utf8mb4 是默认支持的，所以可以使用简写
CREATE DATABASE if NOT EXISTS itcast5;
```

###### 查看数据库的创建详细信息

```sql
-- SHOW CREATE DATABASE 数据库名称;

SHOW CREATE DATABASE itcast4;
```

###### 修改数据库的字符类型

```sql
-- ALTER DATABASE 数据库的名称 CHARACTER SET 编码名;

ALTER DATABASE itcast CHARACTER SET gbk;
```

###### 删除数据库

会删除当前数据库中所有的数据表以及表中的数据

```sql
-- DROP DATABASE [IF EXISTS] 数据库名;

DROP DATABASE itcast3;

DROP DATABASE IF EXISTS itcast3;
```

###### 使用数据库

```mysql
USE itcast;
```

###### 查看当前操作处于哪个数据库当中

```mysql
SELECT DATABASE();
```

##### 表操作

###### 查看都有哪些表

```sql
-- SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]; 查看表
-- FROM db_name 查看指定数据库的所有表,当前所在的数据库不会被改变，即 不会切换到另外一个数据库。

SHOW TABLES;

SHOW TABLES FROM itcast;
```

###### 创建表

```sql
-- CREATE TABLE IF NOT EXISTS 表名 (
--   字段1 字段1类型 [COMMENT 字段1注释],
--   字段2 字段2类型 [COMMENT 字段2注释],
--   ...
--   字段n 字段n类型 [COMMENT 字段n注释]
-- ) [COMMENT 表注释];

-- 创建用户表
CREATE TABLE tb_user (
  id INT PRIMARY KEY AUTO_INCREMENT COMMENT 'id 编号',
  name VARCHAR(50) COMMENT '姓名',
  age INT COMMENT '年龄',
  gender VARCHAR(1) COMMENT '性别'
) COMMENT '用户表';

-- 创建员工表
CREATE TABLE emp (
  id INT COMMENT '编号',
  workno VARCHAR(10) COMMENT '员工工号',
  name VARCHAR(10) COMMENT '员工姓名',
  gender CHAR(1) COMMENT '性别',
  age TINYINT UNSIGNED COMMENT '年龄',
  idcard CHAR(18) COMMENT '身份证号',
  entrydate DATE COMMENT '入职时间'
) COMMENT '员工信息表';

-- 创建数据表同时将查询结果写入到数据表
CREATE TABLE [IF NOT EXISTS] tbl_name [(create_definition, ...)] select_statement;
```

###### 修改表名

```sql
-- 方式一: RENAME TABLE 旧表名 TO 新表名; (MySQL 独有)

-- 支持一次修改多个表
RENAME TABLE emp TO user_emp, tb_user TO user;

-- RENAME TABLE 支持跨数据库迁移表（直接把表挪到另一个库并改名）
RENAME TABLE itcast.emp TO test.user_emp;

-- 方式二: ALTER TABLE 旧表名 RENAME TO 新表名; (PostgreSQL、MySQL 支持；Oracle、SQL Server 语法不一样。)

-- 一次只能修改一个表
ALTER TABLE emp RENAME TO user_emp;

-- 同样支持跨库改名 (跨数据库迁移表（直接把表挪到另一个库并改名))
ALTER TABLE itcast.emp RENAME TO itcast5.user_emp;
```

注意表名修改之后

	1. 视图、存储过场里写死的旧表名会失效，需要手动同步修改。
 	2. 触发器会跟随表一起迁移，不受影响。
 	3. 如果有外键指向本表，改名不影响外键关联。

###### 修改表的字符集

```sql
-- ALTER TABLE 表名 CHARACTER SET 编码名;

ALTER TABLE emp CHARACTER SET utf8mb4;
```

###### 查看创建表的完整语句

```sql
-- SHOW CREATE TABLE 表名;

SHOW CREATE TABLE emp;
```

###### 查看表结构

```mysql
-- DESC 表名
DESC emp;

-- 或者 SHOW COLUMNS FROM table_name;
SHOW COLUMNS FROM emp;
```

###### 删除表

在删除表时，表中的全部数据都会被删除。

```sql
-- 1. DROP TABLE [IF EXISTS] 表名; (物理删除指定表)

DROP TABLE user_emp;
DROP TABLE IF EXISTS user_emp;

-- 2. TRUNCATE TABLE 表名; (删除指定表，然后在创建该表) (清空表)
TRUNCATE TABLE user_emp;
```

###### 添加字段

```sql
-- 添加 一列字段，并且添加至 指定列的后边。省略指定的列，则会添加到最后一列。若只写 first 不写指定的列名，则添加为 表里的 第一列。
-- 1. ALTER TABLE tab_name ADD [COLUMN] col_name column_definition [FIRST | AFTER col_name];

ALTER TABLE emp ADD nickname2 VARCHAR(30) AFTER age;

-- 添加多列。只能依次添加到表的下方，不能指定位置。
-- ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition, ...);

-- 2. ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];

ALTER TABLE emp ADD nickname VARCHAR(20) COMMENT '昵称';
```

###### 修改字段名和字段类型

```sql
-- ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];

ALTER TABLE emp CHANGE nickname nick_name VARCHAR(20) COMMENT '员工昵称';
```

###### 修改字段类型

```sql
-- ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度) [COMMENT 注释];

ALTER TABLE emp MODIFY nick_name VARCHAR(30); -- 忽略 创建结构中的 COMMENT (SHOW CREATE TABLE emp 查看时 nick_name 列没有 COMMENT)

ALTER TABLE emp MODIFY nick_name VARCHAR(30) COMMENT '昵称2';

ALTER TABLE emp MODIFY nick_name VARCHAR(50) COMMENT '昵称2' COLLATE utf8mb4_bin; -- unf8mb4_bin 会使得当前字段在存储时严格区分大小写，并且后续查询时也区分大小写。
```

###### 删除字段

```sql
-- 删除一列
-- ALTER TABLE 表名 DROP 字段名;

ALTER TABLE emp DROP nick_name;

-- 删除多列
-- ALTER TABLE tbl_name DROP [COLUMN] col_name， DROP [COLUMN] col_name, ...;

ALTER TABLE emp DROP nickname2, DROP age;
```

Mysql 中，表 / 列 可以改名，database 不能改名。**改数据库名没有原生命令**，只能新建库 + 迁移数据间接实现。

方法

1. 完整备份原库

   ```mssql
   -- 导出itcast全量数据、存储过程、触发器
   mysqldump -uroot -p --triggers --routines --events itcast > itcast_back.sql
   ```

2. 创建新库

   ```mysql
   CREATE DATABASE IF NOT EXISTS itcast2 DEFAULT CHARSET utf8mb4;
   ```

3. 导入备份到新库

   ```mysql
   mysql -u root -p itcast2 < itcast_back.sql
   ```

4. 校验数据完全一致后，删除旧库。

### DML

数据操作语言，用来对数据库中表中的数据进行 增删改

##### 插入数据

注意事项

1. 插入数据时，指定的字段顺序需要与值的顺序是 一一对应的。
2. 字符串和日期类型数据应该包含在引号中。
3. 插入的数据大小，应该在字段的规定范围内。

```sql
-- 表：单条添加数据 (建议用方式一，使用明显的方式添加数据)

-- 方式一: 给 指定字段 添加数据
-- INSERT INTO 表名 (字段1, 字段2, ...) VALUES (值1, 值2, ...);

INSERT INTO tb_user (id, name, age, gender) VALUES (1, 'sky', 18, '男');
INSERT INTO tb_user (id, name, age) VALUES (2, '丫丫', 28);

-- 日期格式：'2019/10/12' 或 '2019-10-12'

-- 方式二: 给 全部字段 添加数据
-- INSERT INTO 表名 VALUES (值1, 值2, ...);
-- 没有值的字段可以用 null 替代，自增长的 既可以用 NULL 也可以使用 DEFAULT，有默认值的列，可以使用 DEFAULT。

INSERT INTO tb_user VALUES (3, '安娜', 28, '女');
INSERT INTO tb_user VALUES (4, '安娜2', 28, NULL);

-- 区别，方式二 值的顺序必须与表中字段的顺序一致。


-- 表：批量添加数据 (建议用方式一，使用明显的方式添加数据)

-- 方式一: INSERT INTO 表名 (字段1, 字段2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);

INSERT INTO tb_user (id, name, age, gender) VALUES (5, 'carson1', 30, '男'), (6, 'carson2', 30, '男'), (7, 'carson3', 30, '男');

-- 方式二: INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);

INSERT INTO tb_user VALUES (8, 'Ada-1', 26, '女'), (9, 'Ada-2', 26, '女'), (10, 'Ada-3', NULL, NULL);

-- 将查询结果写入数据表
INSERT [INTO] tbl_name [(col_name, ...)] SELECT ...;

-- 多表更新
UPDATE table_references 
SET col_name1 = {expr1 | DEFAULT}
[, col_name2 = {exper2 | DEFAULT}] ...
[WHERE where_condition]
```

##### 修改数据

注意：修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。

```sql
-- UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [WHERE 条件];

UPDATE tb_user SET id = 55, name = 'carson2-5' WHERE name = 'carson2';
UPDATE tb_user SET id = 56 WHERE name = 'carson3';
UPDATE tb_user SET gender = '男' WHERE id = 1;
UPDATE tb_user SET gender = NULL;
UPDATE tb_user SET gender = NULL WHERE 1 = 1;

-- 在原来的基础上修改 (NULL 不会增加, 还是 NULL)
-- UPDATE 表名 SET 字段名1 = 字段名1 + 增加的值, ... [WHERE 条件];

UPDATE tb_user SET age = age + 1 WHERE id = 1;
```

##### 删除数据

注意：

1. DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。
2. DELETE 语句不能删除某一个字段的值 (可以使用 UPDATE)。

```sql
-- 清空表，并不清空表的结构，数据可以找回
-- DELETE FROM 表名 [WHERE 条件]

DELETE FROM tb_user WHERE name = 'carson2-5';
DELETE FROM tb_user WHERE gender is NULL; -- 删除 gender 是 NULL 的所有数据
DELETE FROM tb_user WHERE gender IS NOT NULL;
DELETE FROM tb_user WHERE gender <=> NULL; -- 兼容 NULL 的情况，不建议使用 仅 mysql 支持。
DELETE FROM tb_user; -- 删除所有数据
DELETE FROM tb_user WHERE 1 = 1; -- 删除所有数据

-- TRUNCATE 关键字可以将某一张表中的所有数据记录全部清空，清空数据之后，重新添加数据时，自动增长的数据会重新开始。
-- DELETE 不会，DELETE 后再添加数据 自动增长的 id 会增长。

-- TRUNCATE TABLE 表名; -- 后边不可以跟表达式

TRUNCATE TABLE tb_user;
```
- 删除 是空的字段值


```sql
-- 删除该字段是 null 的行
delete from 表名 where isnull(字段名);
```

##### delete  与 truncate 的区别

- delete 删除表中的数据，表结构还在，删除后的数据可以找回。

- truncate 删除是把表直接 DROP 掉，然后再创建一个同样的新表。删除的数据不能找回。执行的速度比 delete 快。表的结构依然在。

##### 查询参数占位符

- 语法 .query('SELECT ??, ?? FROM ?? WHERE ?? = ?', ['id', 'username', 'users', 'id', 1]) ??: 字段名、表名。 ?:值。
```javascript
const sql = `selete id, title, done from todos ${where} limit ? offset ?`;
const [todos] = await connection.query(sql, [prepage, (page-1) * prepage]);
```

### DQL

数据查询语言，用来查询数据库中表的记录

数据库执行DQL语句不会对数据进行改变，而是让数据库发送结果集给客户端。

结果集： 通过查询语句查询出来的数据以表的形式展示，我们称这个表为虚拟结果集。存放在内存中。查询返回的结果集是一张虚拟表。

##### 编写顺序

```mysql
-- SELECT [字段列表, 表达式，函数] FROM 表名列表 WHERE 条件列表 GROUP BY 分组字段列表 HAVING 分组后条件列表 ORDER BY 排序字段列表 LIMIT 分页参数;
```

##### 执行顺序

```mysql
-- DQL 执行顺序
-- FROM 表名列表 WHERE 条件列表 GROUP BY 分组字段列表 HAVING 分组后条件列表 SELECT 字段列表 ORDER BY 排序字段列表 LIMIT 分页参数

--  1. 先执行 FROM，通过 FROM 决定查询的是哪一张表的数据
--  2. 执行 WHERE 过滤，通过 WHERE 指定查询的条件
--  3. 通过 GROUP BY 和 HAVING 指定分组 以及分组之后的条件
--  4. 决定查询要返回哪些字段 (SELECT 后边的内容) 执行 SELECT
--  5. 执行 ORDER BY
--  6. 执行 LIMIT
```



```sql
SELECT select_expr[, select_expr, ...] 
[
    FROM table_reference
    [WHERE where_condition]
    [GROUP BY {col_name | position} [ASC | DESC], ...]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position} [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
];
```

##### 基本查询

```sql
-- SELECT * FROM 表名;

SELECT * FROM tb_user;

-- 1. 查询多个字段 SELECT 字段1, 字段2, 字段3, ... FROM 表名

SELECT id, name, age FROM tb_user;

-- 2. 查询时设置别名 SELECT 字段1 [AS 别名1], 字段2 [AS 别名2], ... FROM 表名;

SELECT id, name AS user_name, age FROM tb_user;

-- 省略 AS

SELECT id, name, age user_age FROM tb_user;

-- 3. 去除重复记录 SELECT DISTINCT 字段列表 FROM 表名;

SELECT DISTINCT gender FROM tb_user;
```

##### 条件查询

条件查询就是在查询时给出 where 子句，在 where 子句中可以使用一些运算符及关键字

```sql
-- SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

###### 比较运算符 和 逻辑运算符

比较运算符

- < 小于, <= 小于等于, > 大于, >= 大于等于, = 等于, <> / != 不等于
- BETWEEN ... AND ... 在某个范围之内(含最小、包含最大值)
- IN(...) 在 in 之后的列表中的值，多选一
- LIKE 占位符 模糊匹配(_匹配单个字符, %匹配任意个字符),
- IS NULL 值是 NULL
- IS NOT NULL 值不是 NULL

逻辑运算符

- AND / && 并且(多个条件同时成立)
- OR / || 或者(多个条件任意一个成立)
- NOT / ! 非，不是

###### 条件查询的使用

- 条件查询 where: select 字段名 from 表名 where 表达式;

- and: select 字段名 from 表名 where 表达式1 and 表达式2;

- or: select 字段名 from 表名 where 表达式1 or 表达式2;

- in: select 字段名 from 表名 where 字段 in (值1, 值2, ...);

- not in: select 字段名 from 表名 where 字段 not in (值1, 值2, ...);

- between and: select 字段名 from 表名 where 字段名 between 值1 and 值2;

- is null: select 字段名 from 表名 where 字段 is null;

- is not null: select 字段名 from 表名 where 字段 is not null;

###### 模糊查询

- 根据指定的关键字进行查询

- 使用 LIKE 关键字后跟通配符

通配符

- _ ： 任意一个字母（字符）

- % ： 任意 0～n 个字母（字符、汉字）

例如：

```sql
-- 查询姓名由 5个字母（或汉字） 构成的学生记录
select * from students where name like '_____';

-- 查询姓名由 5个字母构成，并且第 5个字母为 's' 的学生记录
select * from students where name like '____s';

-- 查询姓名以 'm' 开头的学生记录
select * from students where name like 'm%';

-- 查询姓名中第 2个字母为 'u' 的学生记录
select * from students where name like '_u%';

-- 查询姓名中包含 's' 字母的学生记录
select * from students where name like '%s%';

-- 查询 张姓 另个及两个字以上的学习记录
select * from students where name like '张_%';
select * from students where name like '张%'; -- 有可能 会匹配到 一个 '张' 字，所以不对。

-- 查询 包含 % 的记录
select * from students where name like '%1%%' escape '1'; -- 1 不匹配。
```

##### 计算列

```sql
SELECT age - 10 AS new_age FROM tb_user; -- NULL 无法计算，因此计算后值还是 NULL
```

##### 字段控制查询

- 去除重复记录
```sql
SELECT DISTINCT name FROM tb_user;
```
- 把查询字段的结果进行运算，必须都要是数据型。

    + select *, 字段1 + 字段2 from 表名。

    ```sql
    SELECT age + 1 FROM tb_user;
    ```
    + 列有很多记录的值为 null，因为任何东西与 null 相加的结果还是 null，所以 结果可能会出现 null。使用了把 null 转换成数值 0 的函数 ifnull()。
    ```sql
    -- 如果 score 为 null 的话，就变为 0.
    SELECT *, IFNULL(age, 0) + 1 FROM tb_user;
    
    select *, age + ifnull(score, 0) from students;
    ```


##### 别名

查询表达式可以使用 [AS] alias_name 为其赋予别名。

别名可用于 GROUP BY, ORDER BY 或 HAVING 子句。

- 对查询结果起别名（使用 AS 或 不写）

```sql
SELECT name ename FROM tb_user;

-- 或者 使用 AS (建议)

SELECT name AS ename FROM tb_user;
```

##### 排序查询

```sql
-- SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;
```

排序方式

- ASC: 升序 (默认值) (从小到大排序, 默认情况下 NULL 值排在前面)

    ```sql
    SELECT * FROM tb_user ORDER BY age;
    ```

- DESC: 降序 (默认情况下 NULL值 排在后面)

    ```sql
    SELECT * FROM tb_user ORDER BY age DESC;
    ```

多字段排序：当第一个字段值相同时，才会根据第二个字段进行排序。

```sql
-- 按照 age 升序排列，年龄相同按照 id 降序排
SELECT * FROM tb_user ORDER BY age ASC, id DESC;
```

MySQL 内部把 `NULL` 视作**小于任何实际值**。

如果想自定义 NULL 位置

- 升序时 NULL 放末尾

```sql
-- 方式1：IFNULL 转换，NULL 变成极大值
SELECT * FROM tb_user ORDER BY IFNULL(age, 999999) ASC;

-- 方式2：CASE 判断，优先排非 NULL
SELECT * FROM tb_user ORDER BY age IS NULL, age ASC;

-- age IS NULL：NULL=1，非 NULL=0；0 排在 1 前面，有效值先出。
```

- 降序时 NULL 放开头

```sql
-- 方式1：IFNULL 转换，NULL 变成极大值
SELECT * FROM tb_user ORDER BY IFNULL(age, 999999) DESC;

-- 方式2：CASE 判断，优先排非 NULL
SELECT * FROM tb_user ORDER BY age IS NULL DESC, age DESC;
```

##### 聚合函数（分组函数）

对查询的结果进行统计计算

聚合函数是对一组值执行计算并返回单一结果的函数。

```sql
-- SELECT 聚合函数(字段列表) FROM 表名;

SELECT COUNT(*) FROM tb_user; -- 10
SELECT SUM(age) FROM tb_user; -- 244
SELECT SUM(name) FROM tb_user; -- 0
```

注意：NULL 值不参与所有聚合函数运算。

常用聚合函数：

- count() 统计指定列不为 null 的记录行数，（当函数参数为 * 时不会忽略）

- max() 计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算

- min() 计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算

- sum() 计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为 0

- avg() 计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为 0

##### 分组查询

将查询结果按照 1个 或多个字段进行分组，字段值相同的一组。

```sql
-- SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
```

使用：

- selct 字段名 from 表名 group by 字段名;

- 根据 gender（性别） 字段来分组，gender 字段的全部值只有两个('男'和'女')，所以分了两个组。

- 当 group by 单独使用时，只显示出每组的第一条记录。所以 group by 单独使用的实际意义不大。


分组注意事项： 在使用分组时， select 后面直接跟的字段一般都出现在 group by 后。

- **group by + GROUP_CONCAT()**, group_concat(字段名)可以作为一个输出字段来使用，表示分组之后，根据分组结果，使用 group_concat()来放置每一组的某字段的值的集合。

    ```sql
    -- 按照部门列出 每一部门都有哪些人
    select deptno, group_concat(ename) from emp group by deptno;
    
    -- 按照 名字 和 性别同时分组
    select name, gender from emp group by gender, name;
    
    -- 按照年龄列出，每一个年龄都有那些人
    SELECT age, GROUP_CONCAT(name) FROM tb_user GROUP BY age;
    ```

- group by + 聚合函数
    ```sql
    -- 按照部门列出，每一部门的 部门名称 和 工资总和
    select deptno, sum(sal) from emp group by deptno;

    -- 按照部门列出，每一部门的 部门名称 和 最低工资
    select deptno, min(sal) from emp group by deptno;

    -- 查询出 每个部门的部门名称以及每个部门工资大于1500的人数
    -- 先查出所有 工资大于 1500 的人，然后再分组查询
    select deptno, count(*) from emp where sal > 1500 group by deptno;
    ```

**group by + having**

用来分组查询后指定一些条件来输出查询结果

having 作用和 where 一样，但 having 只能用于 group by

```sql
-- 按照部门分组，并且筛选部门工资总和大于 9000 的部门，显示部门号和部门总和

select deptno, sum(sal) from emp group by deptno having sum(sal) > 9000;
```

**WHERE 和 HAVING 的区别**

- 执行时机不同：WHERE 是分组之前进行过滤，不满足 WHERE 条件，不参与分组；而 HAVING 是分组之后对结果进行过滤。
- having 后面可以使用分组函数（统计函数）, where 后面不可以使用分组函数
- 判断条件不同：WHERE 不能对聚合函数进行判断，而 HAVING 可以。

**注意**

- 执行顺序：where ==> 聚合函数 ==> having
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其它字段无任何意义。

```SQL
-- 查询出工资大于2000的人，并且工资总和大于6000的部门名称以及工资和 并且降序排列
    -- 1 查询工资 大于2000 的人
        -- select * from emp where sal > 2000;
    -- 2 各部门工资
        -- select deptno, group_concat(sal) from emp where sal > 2000 group by deptno;
    -- 3 各部门工资总和
        -- select deptno, group_concat(sal), sum(sal) from emp where sal > 2000 group by deptno;
    -- 4 各部门工资总和 大于6000
        -- select deptno, group_concat(sal), sum(sal) from emp where sal > 2000 group by deptno having sum(sal) > 6000;
    -- 5 降序排列

SELECT deptno, group_concat(sal), SUM(sal) FROM emp WHERE sal > 2000 GROUP BY deptno HAVING SUM(sal) > 6000 ORDER BY SUM(sal) DESC;
```

##### LIMIT 分页

```sql
-- SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
```

**注意**

- 起始索引从 0 开始，起始索引 = (查询页码 - 1) * 每页显示记录数。
- 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL 中是 LIMIT。
- 如果查询的是第一页数据，起始索引可以省略，直接简写为 LIMIT 10。

```sql
-- 取 前三条 数据
SELECT * FROM tb_user LIMIT 0, 3;

-- 按照年龄降序排列，取前三条数据
SELECT * FROM tb_user ORDER BY age DESC LIMIT 0, 3;
SELECT * FROM tb_user ORDER BY age DESC LIMIT 3;

-- 按照 日期་降序排列 取 前三条 数据
select * from emp order by hiredate desc limit 0,3;
```

##### SQL 顺序

- 书写顺序 : select --> from --> where --> group by --> having --> order by --> limit

- 执行顺序 : from --> where --> group by --> having --> select --> order by --> limit

先从表里找指定条件的数据，然后分组，分组后再筛选，拿到筛选后的数据，排序，取出指定条数。

### DCL

数据控制语言，用来管理数据库用户、控制数据库的访问 权限。

**注意**

- 主机名可以使用 % 通配。
- 这类 SQL 开发人员操作的比较少，主要是 DBA 使用。

##### 查询用户

```sql
USE mysql;

SELECT * FROM user;
```

##### 创建用户

```sql
-- CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

-- 创建用户 itcast, 只能够在当前主机 localhost 访问，密码 123456;
CREATE USER 'itcast'@'localhost' IDENTIFIED BY '123456'; -- 默认没有任何权限

-- 创建用户 heima, 可以在任意主机访问该数据库，密码 123456；
CREATE USER 'heima'@'%' IDENTIFIED BY '123456';
```

##### 修改用户密码

```sql
-- ALTER USER '用户名'@'主机名' IDENTIFIED BY '新密码';

-- 修改用户 heima 的访问密码为 1234
ALTER USER 'heima'@'%' IDENTIFIED BY '1234';
```

##### 删除用户

```sql
-- DROP USER '用户名'@'主机名';

DROP USER 'heima'@'%';
```

##### 权限控制

**什么是权限**: 限制一个用户能够做什么事情，在 mysql 中，可以设置全局权限，指定数据库权限，指定表权限，指定字段权限。

**注意**

- 多个权限之间，使用逗号分隔。
- 授权时，数据库名和表名可以使用 * 进行通配，代表所有。

常用权限

|        权限         |            说明             |
| :-----------------: | :-------------------------: |
| ALL, ALL PRIVILEGES |          所有权限           |
|       SELECT        |        查询数据权限         |
|       INSERT        |        插入数据权限         |
|       UPDATE        |        修改数据权限         |
|       DELETE        |        删除数据权限         |
|        ALTER        |     修改表 / 字段 权限      |
|        DROP         | 删除数据库 / 表 / 视图 权限 |
|       CREATE        | 创建数据库 / 表 / 索引 权限 |
|        INDEX        |          索引权限           |
|     create view     |        创建视图权限         |
|       execute       |      执行存储过程权限       |

##### 查询权限

```sql
-- SHOW GRANTS FOR '用户名'@'主机名';

SHOW GRANTS FOR 'itcast'@'localhost';
```

##### 授予权限

```sql
-- GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名'；

# *.* 所有的数据库，该库下的所有的表

GRANT SELECT ON itcast.tb_user TO 'itcast'@'localhost';

GRANT SELECT ON *.* TO 'itcast'@'localhost';

-- 拥有查询 itcast 数据库下 所有表的权限

GRANT SELECT ON itcast.* TO 'itcast'@'localhost';

GRANT ALL ON *.* TO 'itcast'@'localhost';

-- 注意：
--  授权后的账户 需要重新进入该库 才能看到权限。
--  新权限会在旧的权限上增加，不会直接替换旧的权限。
```

##### 撤销权限

```mysql
-- REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';

REVOKE SELECT ON *.* FROM 'itcast'@'localhost';

REVOKE ALL ON itcast.* FROM 'itcast'@'localhost';
```

### 函数

函数：是指一段可以直接被另一段程序调用的程序或代码。

说明：事先提供好的一些功能可以直接使用，函数可以用在 select 语句及其子句，也可以用在 update, delete 语句当中。

​	事先提供好的一些功能可以直接使用，函数可以用在 select 语句及其子句，也可以用在 update,delete 语句当中。

**函数分类**

- 字符串函数
- 数值函数
- 日期函数
- 流程函数

##### 字符串函数

+ CONCAT(s1, s2, ...sn); 把 s1,s2...sn 连接成一个字符串。任何字符串与 null 进行链接，结果都是 null
+ concat_ws('|', s1, s2, ...sn); 使用指定的分隔符进行字符连接。返回结果：s1|s2|sn...。
+ format(s1, str); 格式化 s1 并保留小数点后的 str 位。format(12560.75 ,1); 返回：12,560.8。
+ insert(str, x, y, instr) 将 str 从 x 开始 y 个，替换为 instr
+ LOWER(str) 将字符串 str 全部转为小写
+ UPPER(str) 将字符串 STR 全部转为大写
+ Left(str, x) 返回字符串 str 最左边的 x 个字符。如果第二个参数为 null, 那么则不返回任何字符。
+ right(str, x)返回字符串 str 最右边的 x 个字符。如果第二个参数为 null, 那么则不返回任何字符。
+ length(str)：返回 str 的长度（字节长度，一个汉字占三个字节）。如果字符串包含空格，那么空格也算在长度之内。
+ char_length(str)：返回 str 的 字符长度（一个汉字是一个字符）。
+ LPAD(str, n, pad) 左填充，用字符串 pad 对 str 的左边进行填充，达到 n 个字符串的长度
+ RPAD(str, n, pad) 右填充，用字符串 pad 对 str 的右边进行填充，达到 n 个字符串的长度
+ LTRIM(str) 去掉字符串当中 最左侧 的空格
+ RTRIM(str) 去掉字符串当中 最右侧的空格
+ TRIM(str) 去掉字符串头部和尾部的空格
+ select trim (leading '?' from '??Mysql???'); 删除字符串中 前导的 '?'。返回：'Mysql???'。
+ select trim (trailing '?' from '??Mysql???'); 删除字符串中 后导的 '?'。返回：'?Mysql'。
+ select trim (both '?' from '??Mysql???'); 删除字符串中 前导 和 后导 的 '?'。返回：'Mysql'。
+ select trim (both '?' from '??My??sql???'); 返回：'My??sql'。中间的不会删掉。
+ REPEAT(str, x) 返回 str 重复 x 次的结果。
+ replace(str, a, b) 用字符串 b 替换字符串 str 中所有出现的字符串 a
+ SUBSTRING(str, start, len) 返回从字符串 str 从 start 位置起的 len 个长度的字符串 (str 索引 从 1 开始计数)
+ mid(str, x, y) 返回字符串 str 中第 x 位置起 y 个字符长度的字符。

```sql
SELECT CONCAT('hello', 'Mysql'); -- 返回 'helloMysql'
SELECT CONCAT_WS('_', 'HELLO', 'MYSQL', 'TEST'); -- HELLO_MYSQL_TEST

SELECT FORMAT(1234560.733, 1); -- 1,234,560.7
SELECT INSERT('HELLO', 1, 2, 'YY'); -- YYLLO

SELECT LEFT('HELLO', 3); -- HEL
SELECT RIGHT('HELLO', 2); -- LO

SELECT LENGTH('HELLO'); -- 5
SELECT LENGTH('HELL O'); -- 6
SELECT LENGTH('好'); -- 3
SELECT CHAR_LENGTH('HELLO'); -- 5
SELECT CHAR_LENGTH('好'); -- 1

SELECT LOWER('hEllo'); -- 返回 'hello'
SELECT UPPER('hEllo'); -- 返回 'HELLO'

SELECT LPAD('hello', 10, '-'); -- 返回 '-----hello'
SELECT RPAD ('hello', 10, '-'); -- 返回 'hello-----'

SELECT REPEAT('HE', 3); -- 'HEHEHE'
SELECT REPLACE('HELLO', 'L', 'Y'); -- 'HEYYO'

SELECT LTRIM(' HELLO '); -- 'HELLO '
SELECT RTRIM(' HELLO '); -- ' HELLO'
SELECT TRIM('  hello '); -- 返回 'hello'
SELECT TRIM('  he  llo '); -- 返回 'he  llo'

SELECT SUBSTRING('hello', 0, 2); -- 返回 ''
SELECT SUBSTRING('hello', 1, 2); -- 返回 'he'
SELECT SUBSTRING('hello', 2, 2); -- 返回 'el'
SELECT MID('HELLO', 3, 2);  -- 'LL'
```

##### 数值函数

+ ABS(x) 求 x 的绝对值
+ CEIL(x) 向上取整
+ FLOOR(x) 向下取整
+ MOD(x, y) 返回 x/y 的模
+ RAND() 返回 0-1 内的随机值
+ round(x, y)  小数点后 y 位 实现四舍五入。默认返回一个 四舍五入的值。
+ greatest(x1, x2, x3, x4, ...) 返回其中最大的值。

```sql
SELECT ABS(-3); -- 3

SELECT CEIL(-0.3); -- 返回 0
SELECT CEIL(0.3); -- 返回 1
SELECT CEIL(0.6); -- 返回 1
SELECT CEIL(1.0); -- 返回 1
SELECT FLOOR(-0.3); -- 返回 -1
SELECT FLOOR(0.3); -- 返回 0
SELECT FLOOR(0.6); -- 返回 0
SELECT MOD(2, 5); -- 返回 2
SELECT MOD(8, 6); -- 返回 2
SELECT MOD(10, 5); -- 返回 0

SELECT RAND(); -- 返回 0.1657900765219611
SELECT RAND(); -- 返回 0.1992335172362938
SELECT ROUND(0.6321, 2); -- 返回 0.63
SELECT ROUND(0.6371, 2); -- 返回 0.64
SELECT ROUND(0.12, 1); -- 返回 0.1
SELECT ROUND(0.11, 0); -- 返回 0

SELECT GREATEST(12, 13, 16, 18); -- 18
```

##### 日期和时间函数

+ CURDATE() 返回当前的日期，只包含 年月日
+ CURTIME() 返回当前的时间，只包含 时分秒
+ NOW()  / SYSDATE() 返回当前日期和时间，包含 年月日 时分秒
+ UNIX_TIMESTAMP 返回当前日期的时间戳
+ FROM_UNIXTIME(unixtime) 将一个时间戳转换成日期
+ WEEK(DATE) 返回当前是一年中的 第几周
+ YEAR(DATE) 返回 所给日期是哪一年
+ MONTH(date) 获取指定 date 的月份
+ DAY(date) 获取指定 date 的日期
+ HOUR(TIME) 返回当前时间的小时
+ MINUTE(TIME) 返回当前时间的分钟
+ DATE_FORMAT(date, fmt) 按字符串格式化日期 date 的值
+ DATE_ADD(date, INTERVAL expr type) 返回一个日期/时间值加上一个时间间隔 expr 后的时间值
+ DATEDIFF(date1, date2) 返回起始时间 date1 和 结束时间 date2 之间的天数, 计算时 date1 - date2

```sql
SELECT UNIX_TIMESTAMP(); -- 1783503981
SELECT FROM_UNIXTIME(1783503981); -- 2026-07-08 17:46:21

-- 返回当前格式化后的日期
SELECT DATE_FORMAT('2026-07-08 17:46:21', '%Y-%m-%d'); -- 2026-07-08

SELECT CURDATE(); -- 返回 2026-06-14
SELECT CURTIME(); -- 返回 07:05:00
SELECT NOW(); -- 返回 2026-06-14 07:05:25
SELECT SYSDATE(); -- 返回 2026-07-05 18:50:20

SELECT WEEK('2026-07-08 17:46:21'); -- 27
SELECT YEAR(NOW()); -- 返回 2026
SELECT YEAR('2025-06-02'); -- 返回 2025
SELECT MONTH('2025-06-02'); -- 返回 6
SELECT MONTH('2025-02-29'); -- 返回 NULL，原因是 这个日期是一个错误日期
SELECT DAY('2025-06-02'); -- 返回 2
SELECT HOUR('2026-07-08 17:46:21'); -- 17
SELECT MINUTE('2026-07-08 17:46:21'); -- 46

-- 返回 60天 后的时间
SELECT DATE_ADD(NOW(), INTERVAL 60 DAY); -- 返回 2026-08-13 07:15:25
SELECT DATE_ADD(NOW(), INTERVAL 10 DAY); -- 返回 2026-06-24 07:16:52
SELECT DATE_ADD(NOW(), INTERVAL 10 MONTH); -- 返回 2027-04-14 07:16:52
SELECT DATE_ADD(NOW(), INTERVAL 10 YEAR); -- 返回 2036-06-14 07:18:32
SELECT DATE_ADD('2026-06-01', INTERVAL 15 DAY); -- 返回 2026-06-16

SELECT DATEDIFF('2026-06-01', '2026-06-14'); -- 返回 -13
SELECT DATEDIFF('2021-08-01', '2026-06-14'); -- 返回 -1778
SELECT DATEDIFF('2026-07-10', '2026-04-10'); -- 91
```

##### 流程函数

流程控制函数也是很常用的一类函数，可以在 SQL 语句中实现条件筛选，从而提高语句的效率。

|                           函数                            |                             功能                             | 举例                                                    |
| :-------------------------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------- |
|                      IF(value, t, f)                      |           如果 value 为 true, 则返回 t, 否则返回 f           | select if(2 > 3, 'true', 'false');  => 'false           |
|                  IFNULL(value1, value2)                   | 如果 value1 不为空(NULL / null)，返回 value1, 否则返回 value2 |                                                         |
|    CASE WHEN [val1] THEN [res1] ... ELSE [default] END    | 如果 val1 为 true, 返回 res1, ...否则返回 default 默认值。WHEN xxx THEN xxx 可以有多个 | select case when 2 > 3 then '对' else '错' end; => '错' |
| CASE [expr] WHEN [val1] THEN [res1] ...ELSE [default] END | 如果 expr 的值等于 val1, 返回 res1, ...否则返回 default 默认值 |                                                         |

```sql
SELECT IF(true, 'OK', 'Error'); -- 返回 'OK'
SELECT IF(false, 'OK', 'Error'); -- 返回 'Error'
SELECT IFNULL(NULL, 'Error'); -- 返回 'Error'
SELECT IFNULL('OK', 'Error'); -- 返回 'OK'
SELECT IFNULL(false, 'Error'); -- 返回 0 Mysql 没有原生严格布尔类型，而是会先把 false 转换为 0 把 true 转换为 1
SELECT IFNULL(true, 'Error'); -- 返回 1
SELECT IFNULL(0, 'Error'); -- 返回 0
SELECT IFNULL(1, 'Error'); -- 返回 1
SELECT IFNULL('', 'Default'); -- 返回 ''

SELECT name, (CASE workaddress WHEN '北京' THEN '一线城市' WHEN '上海' THEN '一线城市' ELSE '二线城市' end) AS address, workaddress FROM emp;

SELECT name, math, (CASE WHEN math >= 85 THEN '优秀' WHEN math < 85 AND math >= 60 THEN '及格' ELSE '不及格' END) AS math_val FROM score;
```

##### 其它函数

+ DATABASE() 返回当前数据库名 例： select database(); => 当前使用的数据库
+ CONNECTION_ID() 返回当前连接的 id，即：线程的 id。
+ LAST_INSERT_ID() 最后插入记录的 id 号。只能 查看 带 自增的主键，比如 id 值。
+ VERSION() 返回当前数据库版本
+ USER() 返回当前登录用户名
+ MD5() 返回 str 的 MD5 值

```sql
SELECT VERSION(); -- 9.2.0
SELECT CONNECTION_ID(); -- 36;
SELECT USER(); -- root@localhost

SELECT DATABASE(); -- itcast

SELECT MD5('ABC'); -- 902fbdd2b1df0c4f70b4a5d23525e932
```

### 数据的完整性

什么是数据的完整性： 保证用户输入的数据保存到数据库中是正确的。

实体完整性、域完整性及参照完整性分别在行、列、表上实施。

如何添加数据完整性： 在创建表时给表中添加约束

##### 完整性分类

- 实体完整性

- 域完整性

- 引用完整性

##### 实体完整性

什么是实体完整性： 表中的一行（一条记录）代表一个实体（entity）

实体完整性的作用：标识每一行数据不重复。行级约束

约束类型：

- 主键约束 (primary key)

    + 特点： 每个表中要有一个主键，一个表中只能有 一个 主键约束。

    + 数据唯一，且不能为 null

- 唯一约束 (unique)

    + 特点： 指定列的数据不能重复，可以为空值，一个表中可以有多个 unique 约束。
    + 多个列可以组合成一个 唯一约束。
    + mysql 会给唯一约束的列上默认创建一个唯一索引。
    + 格式： create table 表名（字段名1 数据类型， 字段名2 数据类型 unique）
    + 删除约束：alter table 表名 drop 索引(index) 列名;

- 自动增长列 (auto_increment)

    + 特点： 指定列的数据自动增长，即使数据删除，还是从删除的序号继续往下。

    + 格式： create table 表名（字段名1 类型 auto_increment, 字段2 类型）

**主键约束 添加方式**

- create table 表名(字段名1 类型 primary key, 字段2 类型);

- create table 表名(字段名1 类型, 字段2 类型, primary key(要设置主键的字段));

- 联合主键： 当两个字段数据同时相同时，才违反联合主键约束

    + create table 表名(字段名1 类型, 字段2 类型, primary key(主键1, 主键2));

- 先创建表，再去修改表，添加主键

    + Alter table 表名 add constraint primary key (列名);

**删除主键约束**

```sql
ALTER TABLE t_user DROP PRIMARY KEY;
```

##### 域完整性

使用： 限制此单元格的数据正确，不对照此列的其它单元格比较。

域完整性是对数据表中字段属性的约束。

它是由确定表结构时所定义的字段的属性决定的。

限制数据类型，缺省值，规则，约束，是否可以为空。

域完整性可以确保不会输入无效的值。

域完整性约束：

- 数据类型（创建表时定义）： 数值类型、日期类型、字符串类型。

- 非空约束(not null)

    + create table 表名(字段名1 类型，字段名2 类型 not null);

- 默认值约束(default)

    + create table 表名(字段名1 类型，字段名2 类型 default '男');

##### 参照完整性

什么是参照完整性： 是指表与表之间的一种对应关系。通常情况下可以通过设置两表之间的主键，外键关系，或者编写两表的触发器来实现。

有对应参照完整性的两张表格，在对他们进行数据插入、更新、删除的过程中，系统都会将被修改表格与另一张对应表格进行对照，从而阻止一些不正确的数据的操作。

数据库的主键和外键类型一定要一致

设置参照完整性后，外键当中的值，必须得是主键当中的内容。

一个表设置当中的字段设置为主键，设置主键的为主表

```sql
create table stu(
  num int primary key auto_increment,
  name varchar(10)
);
```

创建表时，设置外键，设置外键的为子表。

```sql
create table score(
  num int primary key auto_increment,
  scores int,
  sid int,
  foreign key(sid) references stu(num)
);

-- 或 添加一个约束名 sc_st_fk
create table score(
  num int primary key atuo_increment,
  scores int,
  sid int,
  constraint sc_st_fk foreign key(sid) references stu(num)
);

-- 先创建表，后修改表外建约束
alter table 表名 add constraint 约束名 foreign key(列名) references 主表名(列名);

-- 删除外键约束
ALTER TABLE students DROP FOREIGN KEY 约束名;
```

**要求**

- 禁止使用临时表
- 外键列和参照列必须创建索引，如果外键列不存在索引的话，mysql 将自动创建索引。
- 两个表必须得要是 InnoDB 类型
- 外键列和参照列必须具有相似的数据类型。其中数字的长度或是否有符号位必须相同，而字符的长度则可以不同。

##### 外键约束的参照操作

1. CASCADE：从父表删除或更新且自动删除或更新子表中匹配的行。
2. SET NULL：从父表删除或更新行，并设置子表中的外键列为 NULL。如果使用该选项，必须保证子表列没有指定 NOT NULL。
3. RESTRICT：拒绝对父表的删除或更新操作。
4. NO ACTION：标准 SQL 的关键字，在 mysql 中与 RESTRICT 相同。

### 约束

概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。(可以在创建表/修改表的时候添加约束)

目的：保证数据库中数据的正确性、有效性和完整性。

##### 分类

| 约束     | 描述                                                     | 关键字      |
| -------- | -------------------------------------------------------- | ----------- |
| 非空约束 | 限制该字段的数据不能为 NULL                              | NOT NULL    |
| 唯一约束 | 保证该字段的所有数据都是唯一、不重复的                   | UNIQUE      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                 | PRIMARY KEY |
| 默认约束 | 保存数据时，如果未指定该字段的值，则采用默认值           | DEFAULT     |
| 检查约束 | 保证字段值满足某一个条件                                 | CHECK       |
| 外键约束 | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |

##### 案例

根据需求，完成表结构的创建。

| 字段名 | 字段含义    | 字段类型    | 约束条件                              | 约束关键字                                  |
| ------ | ----------- | ----------- | ------------------------------------- | ------------------------------------------- |
| id     | ID 唯一标识 | INT         | 主键，并且自动增长                    | PRIMARY KEY, AUTO_INCREMENT(只有数字类型有) |
| name   | 姓名        | VARCHAR(10) | 不为空，并且唯一                      | NOT NULL, UNIQUE                            |
| age    | 年龄        | INT         | 大于0，并且小于等于120                | CHECK                                       |
| status | 状态        | CHAR(1)     | 如果没有指定该值，默认为1             | DEFAULT                                     |
| gender | 性别        | CHAR(1)     | 只能从 男 或者 女 选一个，可以为 NULL | NULL                                        |

```mysql
CREATE TABLE user1 (
  id INT PRIMARY KEY AUTO_INCREMENT COMMENT 'id 主键',
  name VARCHAR(10) NOT NULL UNIQUE COMMENT '姓名',
  age INT CHECK (age > 0 && age <= 120) COMMENT '年龄',
  status CHAR(1) DEFAULT '1' COMMENT '状态',
  gender ENUM('男', '女') NULL COMMENT '性别'
) COMMENT '用户表-带字段约束';

-- 主键放在创建的末尾

CREATE TABLE user2 (
  id INT COMMENT 'id 主键',
  name VARCHAR(10) NOT NULL UNIQUE COMMENT '姓名',
  age INT CHECK (age > 0 && age <= 120) COMMENT '年龄',
  status CHAR(1) DEFAULT '1' COMMENT '状态',
  gender ENUM('男', '女') NULL COMMENT '性别',
  PRIMARY KEY(id)
) COMMENT '用户表-带字段约束';

-- 删除表中的主键
ALTER TABLE user2 DROP PRIMARY KEY; -- 删掉 id 的手动主键后，InnoDB 为了聚簇索引，自动复用 name(UNIQUE NOT NULL) 充当内部主键，所以 DESC user2 会显示 name 为 PRI。但它本质依旧是 UNIQUE 唯一索引，不是真正手动定义的 PRIMARY KEY，约束定义没有自动转换。

-- 添加表的主键
ALTER TABLE user2 MODIFY id INT PRIMARY KEY COMMENT '主键'; -- 此时 DESC user2; id 会变成 PRI，并且 name 会回退到 UNI

-- 插入数据
INSERT INTO user1 (name, age, status, gender) VALUES 
('yaya', 28, '0', '女'),
('anna', 29, '1', '女'),
('sky', 31, '0', '男'),
('carson', 29, '1', NULL);

INSERT INTO user1 VALUES (NULL, 'Tom2', 26, '1', '男');

-- 会失败，不满足 age 的 CHECK 约束
INSERT INTO user1 VALUES (NULL, 'Tom3', 130, '1', '男');

-- id 会跳过 上面失败的

INSERT INTO user1 VALUES (NULL, 'Tom3', 80, NULL, '男');
INSERT INTO user1 (name, age, gender) VALUES ('Tom4', 88, '男');

INSERT INTO user1 VALUES (10, 'Tom4', 80, NULL, '男');
INSERT INTO user1 VALUES (NULL, 'Tom5', 80, NULL, '男');
```

**注意**

MySQL InnoDB 特性

1. id 如果是手动插入的，则数据库保存的 id 就是手动插入的id, 如果手动插入的 id 大于当前 AUTO_INCREMENT 值，则后边的 AUTO_INCREMENT 在前面手动插入的基础上 自动增长，否则 后续的 id 则使用 AUTO_INCREMENT 的值。
2. AUTO_INCREMENT 自增计数器只要执行了 INSERT 语句（无论最终成功 / 失败），都会先 + 1，不会回滚。只保证唯一性、不保证连续性。

原因

- MySQL 为了并发性能，自增 ID 分配是预分配、提前占用的，不跟随事务回滚、约束失败而撤销。
- 唯一约束、CHECK、ENUM 校验、主键冲突等导致插入失败，ID 依旧被消耗，出现ID 断号。

##### 联合主键

概念：将数据表中的多列组合在一起设置为表的主键。

```mysql
-- 创建联合主键组成的 学生成绩表
CREATE TABLE grades (
  stu_num CHAR(8) NOT NULL,
  course_id INT,
  score INT,
  PRIMARY KEY(stu_num, course_id) -- 把 stu_num 和 course_id 作为一个联合主键，联合主键 在创建表时 只能这样定义。
) COMMENT '学生成绩表';

-- 插入数据
INSERT INTO grades VALUES (101, 1, 59), (101, 2, 60); 

-- 报错，主键重复。
INSERT INTO grades VALUES (101, 1, 59); -- 报错 Duplicate entry '101-1' for key 'grades.PRIMARY'
```

**注意**

在实际项目的数据库设计中，联合主键使用频率并不高。当一张数据表中没有明确的字段可以作为主键时，我们可以额外添加一个 ID字段作为主键。

##### 外键约束

概念：外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

```mysql
-- 1. 创建表时 直接添加

-- CREATE TABLE 表名 (
--   字段名 数据类型,
--   [CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
-- );

-- 2. 修改表，增加外键

-- ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名);


-- 3. 删除外键

-- ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

**外键约束 更新 / 删除 行为**

| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除 / 更新。(与 RESTRICT 一致) (默认行为) |
| RESTRICT    | 当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除 / 更新。(与 NO ACTION 一致) (默认行为) |
| CASCADE     | 当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除 / 更新外键在子表中的记录。 |
| SET NULL    | 当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为 NULL (这就要求该外键允许取 NULL)。 |
| SET DEFAULT | 父表有变更时，子表将外键列设置成一个默认的值 (INNODB 不支持) |

```mysql
-- 修改表，增加外键时，添加 行为

-- ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表名 (主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

```mysql
-- 新建两个表

CREATE TABLE dept (
  id INT AUTO_INCREMENT COMMENT 'id' PRIMARY KEY,
  name VARCHAR(50) NOT NULL COMMENT '部门名称'
) COMMENT '部门表';

INSERT INTO dept (id, name) VALUES 
(NULL, '研发部'),
(2, '市场部'),
(3, '财务部'),
(4, '销售部'),
(NULL, '总经办');

-- 没有 物理外键约束，则 dept 表中删除数据，emp 表中的数据不会更改。

CREATE TABLE emp (
  id INT AUTO_INCREMENT COMMENT 'ID' PRIMARY KEY,
  name VARCHAR(50) NOT NULL COMMENT '姓名',
  age INT COMMENT '年龄',
  job VARCHAR(20) COMMENT '职位',
  salary INT COMMENT '薪资',
  entrydate DATE COMMENT '入职时间',
  managerid INT COMMENT '直属领导 ID',
  dept_id INT COMMENT '部门 ID'
) COMMENT '员工表';

INSERT INTO emp (name, age, job, salary, entrydate, managerid, dept_id) VALUES 
('金庸', 66, '总裁', 20000, '2000-01-01', null, 5),
('张无忌', 20, '项目经理', 12500, '2005-12-05', 1, 1);

INSERT INTO emp (id, name, age, job, salary, entrydate, managerid, dept_id) VALUES 
(NULL, '杨逍', 33, '开发', 8400, '2000-11-03', 2, 1),
(NULL, '韦一笑', 48, '开发', 11000, '2002-02-05', 2, 1),
(NULL, '常遇春', 43, '开发', 10500, '2004-09-07', 3, 1),
(NULL, '小昭', 19, '程序员鼓励师', 6600, '2004-10-12', 2, 1);

INSERT INTO emp VALUES (8, '郭达', 60, '开发', 10000, '2026-06-06', 1, 1);
INSERT INTO emp VALUES (7, '郭达1', 60, '开发', 10000, '2026-06-06', 1, 1);
INSERT INTO emp VALUES (NULL, '郭达4', 60, '开发', 10000, '2026-06-06', 1, 1);

-- 添加外键约束 (外键名称(fk_emp_dept_id) 不要加 引号) (默认使用 NO ACTION 或 RESTRICT 的行为)

ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept (id);

-- 1. 指定 外键约束 更新 和 删除 使用 CASCADE 行为

ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept (id) ON UPDATE CASCADE ON DELETE CASCADE;

-- a. 修改 dept 表的 id
UPDATE dept SET id = 6 WHERE id = 1;

-- 查询 dept 表，发现 id 由 1 变为 6。
SELECT * FROM dept;

-- 查询 emp 表，发现 原本 dept_id 是 1 的数据 此时 dept_id 会变成 6。
SELECT * FROM emp;

-- b. 删除 dept 表的 id = 6 的数据
DELETE FROM dept WHERE id = 6;

-- 查询 emp 表，发现 dept_id = 6 的数据会被删除掉。
SELECT * FROM emp;

-- 有外键约束时，此时删除表，需要先删除 子表，然后才能删除 父表。
DROP TABLE emp;

DROP TABLE dept;

-- 2. 指定 外键约束 更新 和 删除 使用 SET NULL 行为

ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept (id) ON UPDATE SET NULL ON DELETE SET NULL;

-- 删除 dept 表的 id = 1 的数据
DELETE FROM dept WHERE id = 1;

-- 查询 emp 表，发现原本 dept_id = 1 的数据，它们的 dept_id 值会变成 NULL。
SELECT * FROM emp;

DESC emp; -- dept_id 列会展示 MUL。(外键字段会自动生成普通索引，标识 MUL)

-- 删除外键约束

ALTER TABLE emp DROP FOREIGN KEY fk_emp_dept_id;
```

如果一个想修改父表的 id，但是子表有级联关系 并且 删除/更新 的行为 是默认时，可以通过如下方式修改。

- 先修改子表的 关联 id 为 NULL，然后修改 父表的 id, 最后修改 子表的 关联id 为父表的新的 id。
- 先把子表的 外键关联关系删除，然后修改 父表的 id，修改子表 关联旧的 id 替换为新的 父表id，最后添加外键关联。
- 先把子表的 外键关联关系删除，然后修改 级联的 删除/更新 的默认行为 为 ON UPDATE CASCADE ON DELETE CASCADE。

**生产上 这种级联操作 一般是怎么样的**

生产环境数据库级联操作完整方案（外键级联删除/更新）

先分两种场景：**数据库原生外键级联（ON DELETE/UPDATE CASCADE）**、**业务代码手动级联（生产主流）**，讲清楚各自适用场景、规范、坑、标准写法。

**一、两种级联方案对比（生产选型核心）**

1. 数据库原生级联（外键 CASCADE）

```mysql
-- 定义
-- 建表外键约束，主表删/改时，数据库自动同步子表数据，无需业务代码处理。
-- ```sql
-- -- 示例：订单主表，订单明细子表

-- CREATE TABLE order_main (
--     id BIGINT PRIMARY KEY
-- );

-- CREATE TABLE order_item (
--     id BIGINT PRIMARY KEY,
--     order_id BIGINT,
--     FOREIGN KEY (order_id) REFERENCES order_main(id)
--         ON DELETE CASCADE   -- 删除主订单，自动删明细
--         ON UPDATE CASCADE   -- 主订单ID变更，明细同步更新
-- );

-- 生产优缺点
-- ✅ 优点
-- 1. 逻辑下沉数据库，业务代码极简，不会漏删子表；
-- 2. 原子性：同事务，不会出现主删子留脏数据；
-- 3. 多端操作（后台、定时任务、SQL脚本）统一生效。

-- ❌ 致命缺点（大厂生产极少直接用）
-- 1. **性能灾难**：主表批量删除万级数据，数据库递归删所有子表，锁表、IO飙升、事务超时；
-- 2. **无法加业务逻辑**：不能做软删除、日志记录、数据归档、权限校验；
-- 3. 级联深度不可控：A→B→C→D多层级联，删一条主数据会连锁删全链路，误删无法挽回；
-- 4. 分库分表不支持：Sharding-JDBC、MyCat分布式场景外键失效；
-- 5. 归档/审计需求不满足：真实业务几乎不硬删数据，多用**软删除**，原生CASCADE只支持物理删除。

-- 适用场景（仅小系统、单表简单关联）
-- 内部小型后台、数据量小、无归档审计、不分库分表、允许物理删除。
```

2. 业务代码手动级联（90%中大型生产标准方案）

核心思路：**业务层统一控制删除逻辑，数据库不依赖外键级联**，分软删除、物理删除两套流程。

```mysql
-- 标准生产流程（软删除，主流）
-- 业务统一规范：所有表加 `is_deleted TINYINT(1) DEFAULT 0` 软删除标记
-- 1. 开启事务；
-- 2. 按**从子表到主表**顺序更新（先删下级，再上级，防止关联查询异常）；
--    - 例：订单 → 订单明细 → 订单物流 → 订单优惠券
--    - 先更新明细is_deleted=1，再更新主订单is_deleted=1；
-- 3. 每一步记录操作日志（操作人、时间、数据ID）；
-- 4. 事务提交；异常全部回滚。

-- 物理删除流程（仅临时日志、临时中间表）
-- 1. 事务内先删除所有子表关联数据（分页批量删，避免一次性锁大量行）；
-- 2. 再删除主表；
-- 3. 大数据量必须分页分批，每批100~1000条，分批提交事务，防止长事务阻塞库。

-- 配套生产规范
-- 1. 数据库**不建外键**（仅业务层维护关联关系），提升写入性能、适配分库分表；
-- 2. 多层关联统一封装公共工具类/Service，统一复用，避免各处代码漏写子表删除；
-- 3. 删除前增加校验：是否已归档、是否有未完结业务（如未退款订单禁止删除）；
-- 4. 重要数据删除前先归档到历史表（order_main_hist），再标记删除，满足审计要求。

-- ✅ 优点
-- - 可控：可加校验、日志、归档、限流；
-- - 性能友好：支持分批、分页删除，不会瞬间压垮DB；
-- - 兼容分布式分库分表；
-- - 适配软删除（企业生产标准）；
-- - 误删风险低，代码层可加拦截开关。

-- ❌ 缺点
-- 代码量多，需要维护全链路关联关系，新增子表必须同步修改删除逻辑。
```

**二、大数据量级联删除生产最佳实践（避坑重点）**

问题: 直接 `delete from main where id = xxx` 关联十万条子数据，单事务锁表、binlog暴涨、主从延迟。

标准处理方案（分批删除）

1. 循环分页查询子表关联ID，每次批量100条。
2. 批量删除子表，提交小事务。
3. 循环直至该主ID下所有子数据清空。
4. 最后删除/软删除主表。
5. 大任务异步化：通过定时任务、MQ异步执行删除，不阻塞前端接口。

伪代码示例

```mysql
-- // 异步执行级联删除
-- @Async
-- // order.entity.ts
-- import { Entity, PrimaryGeneratedColumn, OneToMany, DeleteDateColumn } from 'typeorm';
-- import { OrderItem } from './order-item.entity';

-- @Entity('order')
-- export class Order {
--   @PrimaryGeneratedColumn()
--   id: number;

--   // 软删除字段
--   @DeleteDateColumn({ nullable: true })
--   deletedAt: Date;

--   // 一对多，级联软删除
--   @OneToMany(() => OrderItem, item => item.order, {
--     cascade: ['soft-remove'], // 父 softRemove 自动级联子
--     orphanedRowAction: 'soft-delete' // 子脱离父关系时自动软删
--   })
--   items: OrderItem[];
-- }

-- // order-item.entity.ts
-- import { Entity, PrimaryGeneratedColumn, ManyToOne, DeleteDateColumn } from 'typeorm';
-- import { Order } from './order.entity';

-- @Entity('order_item')
-- export class OrderItem {
--   @PrimaryGeneratedColumn()
--   id: number;

--   @DeleteDateColumn({ nullable: true })
--   deletedAt: Date;

--   @ManyToOne(() => Order, order => order.items)
--   order: Order;
-- }

-- async function cascadeDeleteLargeOrder(orderId: number) {
--   return AppDataSource.transaction(async manager => {
--     // 分批删除明细，每次100条
--     const pageSize = 100;
--     while (true) {
--       const items = await manager.find(OrderItem, {
--         where: { order: { id: orderId } },
--         take: pageSize
--       });
--       if (items.length === 0) break;
--       await manager.softRemove(items);
--     }
--     // 最后删主订单
--     const order = await manager.findOneBy(Order, { id: orderId });
--     if (order) await manager.softRemove(order);
--   });
-- }
```

**三、多层级联（A→B→C→D）处理规范**

1. 删除顺序：由最底层子表 → 上层父表  (D → C → B → A)
2. 查询顺序：父表 → 子表 (查询展示用)
3. 维护一张**业务关联关系配置表/枚举**，记录所有父子表依赖，新增表统一维护，防止漏删。

**四、两种特殊生产场景补充**

- 分库分表环境：**禁止使用数据库外键级联**，跨库无法触发约束，必须全部业务代码手动级联，且要按分片维度分别删除各分片子数据
- 数据归档场景（金融/电商强审计）
  - 事务内把子表数据迁移至历史归档表
  - 原表标记软删除
  - 主表同步归档+软删除
  - 归档数据永久保留，支持溯源查询

**五、生产落地总结 (一句话选型标准)**

1. 小型单体、数据量小、不分库、允许物理删除：可少量使用数据库 `ON DELETE CASCADE。`
2. 中大型系统、分布式、需要审计/软删除、数据量大：**统一业务代码手动级联，禁用外键级联**。
3. 大批量级联删除必须异步、分页分批，杜绝单事务一次性删除海量数据。

### 多表查询

##### 多表关系

概述：项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间互相关联，所有各个表结构之间也存在着各种联系，基本上分为三种：

- 一对一
  - 案例：用户和用户详情的关系
  - 关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另外一张表中，以提升操作效率。
  - 实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的 (UNIQUE)
- 一对多 (多对一)
  - 案例：部门和员工的关系
  - 关系：一个部门对应多个员工，一个员工对应多个部门
  - 实现：在多的一方建立外键，指向一的一方的主键。
- 多对多
  - 案例：学生和课程的关系
  - 关系：一个学生可以选修多门课程，一门课程也可以供多个学生选择。
  - 实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键。

##### 一对一数据库设计

**注意**： 外键必须是不重复的

```mysql
-- 方案一：主键关联 —— 两张数据表中主键相同的数据为互相对应的数据
-- 方案二：唯一外键 —— 在任意一张表中添加一个字段，添加外键约束与另一张表主键关联，并且将外键列添加 唯一约束。

CREATE TABLE tb_user (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  name VARCHAR(10) COMMENT '姓名',
  age INT COMMENT '年龄',
  gender CHAR(1) COMMENT '1: 男, 2: 女',
  phone CHAR(11) COMMENT '手机号'
) COMMENT '用户基本信息表';

CREATE TABLE tb_user_edu (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  degree VARCHAR(20) COMMENT '学历',
  major VARCHAR(50) COMMENT '专业',
  primaryschool VARCHAR(50) COMMENT '小学',
  middleschool VARCHAR(50) COMMENT '中学',
  university VARCHAR(50) COMMENT '大学',
  userid INT UNIQUE COMMENT '用户ID', -- 外键必须是不重复的
  CONSTRAINT fk_userid FOREIGN KEY (userid) REFERENCES tb_user (id) -- 创建表时 添加外键约束
) COMMENT '用户教育信息表';

INSERT INTO tb_user (id, name, age, gender, phone) VALUES 
(NULL, '黄渤', 45, '1', '18800001111'),
(NULL, '范冰冰', 35, '2', '18800002222'),
(NULL, '马云', 55, '1', '18800008888'),
(NULL, '李彦宏', 50, '1', '18800009999');

INSERT INTO tb_user_edu VALUES 
(NULL, '本科', '舞蹈', '静安区第一小学', '静安区第一中学', '北京舞蹈学院', 1),
(NULL, '硕士', '表演', '朝阳区第一小学', '朝阳区第一中学', '北京电影学院', 2),
(NULL, '本科', '英语', '杭州市第一小学', '杭州市第一中学', '杭州师范大学', 3),
(NULL, '本科', '应用数学', '阳泉第一小学', '阳泉区区第一中学', '清华大学', 4);
```

##### 多对多数据库设计

```mysql
CREATE TABLE student (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  name VARCHAR(10) COMMENT '姓名',
  no VARCHAR(10) COMMENT '学号'
) COMMENT '学生表';

INSERT INTO student VALUES 
(NULL, '黛绮丝', '2000100101'),
(NULL, '谢逊', '2000100102'),
(NULL, '殷天正', '2000100103'),
(NULL, '韦一笑', '2000100104');

CREATE TABLE course (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  name VARCHAR(10) COMMENT '课程名称'
) COMMENT '课程表';

INSERT INTO course VALUES 
(NULL, 'Java'),
(NULL, 'PHP'),
(NULL, 'MySQL'),
(NULL, 'Hadoop');

-- 中间表可以有多列

CREATE TABLE student_course (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键',
  studentid INT NOT NULL COMMENT '学生ID',
  courseid INT NOT NULL COMMENT '课程ID',
  CONSTRAINT fk_courseid FOREIGN KEY (courseid) REFERENCES course (id),
  CONSTRAINT fk_studentid FOREIGN KEY (studentid) REFERENCES student (id)
) COMMENT '学生表和课程表的中间表';

INSERT INTO student_course VALUES 
(NULL, 1, 1),
(NULL, 1, 2),
(NULL, 1, 3),
(NULL, 2, 2),
(NULL, 2, 3),
(NULL, 3, 4);
```

一对多数据库设计

```sql
CREATE TABLE dept (
  id INT AUTO_INCREMENT COMMENT 'id' PRIMARY KEY,
  name VARCHAR(50) NOT NULL COMMENT '部门名称'
) COMMENT '部门表';

INSERT INTO dept (id, name) VALUES 
(NULL, '研发部'),
(2, '市场部'),
(3, '财务部'),
(4, '销售部'),
(NULL, '总经办'),
(6, '人事部');

CREATE TABLE emp (
  id INT AUTO_INCREMENT COMMENT 'ID' PRIMARY KEY,
  name VARCHAR(50) NOT NULL COMMENT '姓名',
  age INT COMMENT '年龄',
  job VARCHAR(20) COMMENT '职位',
  salary INT COMMENT '薪资',
  entrydate DATE COMMENT '入职时间',
  managerid INT COMMENT '直属领导 ID',
  dept_id INT COMMENT '部门 ID',
  CONSTRAINT fk_dept_id FOREIGN KEY (dept_id) REFERENCES dept (id)

  -- 添加级联行为
  -- CONSTRAINT fk_dept_id FOREIGN KEY (dept_id) REFERENCES dept (id) ON DELETE SET NULL ON UPDATE CASCADE
) COMMENT '员工表';

INSERT INTO emp (name, age, job, salary, entrydate, managerid, dept_id) VALUES 
('金庸', 66, '总裁', 20000, '2000-01-01', null, 5),
('张无忌', 20, '项目经理', 12500, '2005-12-05', 1, 1);

INSERT INTO emp (id, name, age, job, salary, entrydate, managerid, dept_id) VALUES 
(NULL, '杨逍', 33, '开发', 8400, '2000-11-03', 2, 1),
(NULL, '韦一笑', 48, '开发', 11000, '2002-02-05', 2, 1),
(NULL, '常遇春', 43, '开发', 10500, '2004-09-07', 3, 1),
(NULL, '小昭', 19, '程序员鼓励师', 6600, '2004-10-12', 2, 1),
(NULL, '灭绝', 60, '财务总监', 8500, '2002-09-12', 1, 3),
(NULL, '周芷若', 19, '会计', 48000, '2006-06-02', 7, 3),
(NULL, '丁敏君', 23, '出纳', 5250, '2009-05-13', 7, 3),
(NULL, '赵敏', 20, '市场部总监', 12500, '2004-10-12', 1, 2),
(NULL, '鹿杖客', 56, '职员', 3750, '2006-10-03', 10, 2),
(NULL, '鹤笔翁', 19, '职员', 3750, '2007-05-09', 10, 2),
(NULL, '方东白', 19, '职员', 5500, '2009-02-12', 10, 2),
(NULL, '张三丰', 88, '销售总监', 14000, '2004-10-12', 1, 4),
(NULL, '俞莲舟', 38, '销售', 4600, '2004-10-12', 14, 4),
(NULL, '宋远桥', 40, '销售', 4600, '2004-10-12', 14, 4),
(NULL, '陈友谅', 42, NULL, 2000, '2011-10-12', 1, NULL);
```

##### 多表查询语法 (链接查询)

**笛卡尔积**：是指在数学中，两个集合 A集合 和 B集合 的所有组合情况。(在多表查询时，需要消除无效的笛卡尔积。)

`笛卡尔集：` `假设几个 A = {a, b}, 集合 B = {0, 1, 2}，则笛卡尔集为 (a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b,2) 可以扩展到多个集合的情况。`

```mysql
-- 在查询时要把主键和外键保持一致。

SELECT * FROM emp, dept WHERE emp.dept_id = dept.id; -- 99 连接法 (隐式内连接，emp 里 dept_id 是 NULL 的 不会被查询出来)

-- 主表当中的数据参照子表当中的数据。

-- 原理：逐行判断，相等的留下，不相等的全不要。
```

**多表查询 语法分类**

- 连接查询
  - 内连接：相当于查询 A, B 交集部分的数据
  - 外连接：
    - 左外链接：查询左表的所有数据，以及两张表交集部分数据
    - 右外链接：查询右表所有数据，以及两张表交集部分数据
  - 自连接：当前表与自身的链接查询，自连接必须使用表别名
- 子查询

##### 内连接

内连接查询的是两张表交集的部分

**语法**

```mysql
--  1. 隐式内连接

--  SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;

--  2. 显示内连接 (INNER 可以省略) (推荐)

--  SELECT 字段列表 FROM 表1, 表2 [INNER] JOIN 表2 ON 连接条件 ...;

-- WHERE: 先生成笛卡尔积 然后再过滤。
-- ON: 先判断链接条件是否成立，如果成立两张表的数据再进行组合生成一条结果记录
-- ON 后面只写主外键，如果还有条件直接在后面写 WHERE
-- 数据库优化器会自动重写，执行计划无差别。
```

##### 外连接

```mysql
--  左外链接 (实际 生产使用的多)

--  SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件 ...;
--  SELECT * FROM leftTable LEFT JOIN rightTable on 匹配条件 [WHERE 条件];

-- 相当于查询表1 (左表) 的所有数据，包含 表1 和 表2 交集部分的数据

--  右外链接 (右连接会把右表当中的数据全部查出，左表当中只查出满足条件的数据。)

-- SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件 ...;

-- 相当于查询表2 (右表) 的所有数据，包含 表1 和 表2 交集部分的数据
```

##### 自连接

概念： 自己连接自己，起别名。

**语法** (必须给表 A 起别名)

```mysql
-- SELECT 字段列表 FROM 表A 表名A JOIN 表A 别名B ON 条件 ...;

-- 自连接查询，可以是内连接查询，也可以是外连接查询。
```

##### 联合查询 -- UNION, UNION ALL

对于 UNION 查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

```sql
-- 把两个 select 语句的查询结果合并到一起
-- SELECT 字段列表 FROM 表A ... UNION [ALL] SELECT 字段列表 FROM 表B ...;
```

**注意**

- ALL 不会合并记录，会直接拼接起来。而不使用 ALL 则会对重复数据 去重。
- 对于联合查询的 多张表的列数必须保持一致，字段类型也需要保持一致。

**注意**

1. 同一张表，多个筛选条件是 或 关系 -> 用 OR
2. 多张表/多个独立结果集拼接 -> 用 UNION ALL
3. 禁止用 UNION 替代单表 OR 查询，会造成不必要性能损耗。

**隐藏坑**

- 若字段不一致，UNION 直接报错；OR 不存在该问题。
- UNION 会丢失原结果顺序，OR 保留表扫描顺序
- 百万级数据时，UNION 的去重操作会严重拖慢接口响应。

##### 子查询

概念：在 SQL 语句中嵌套 SELECT 语句，称为嵌套查询，又称子查询。

子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个。

```mysql
-- SELECT * FROM t1 WHERE  column1 = (SELECT column1 FROM t2);
```

根据**子查询结果**不同，可以分为以下 四类

- 标量子查询 (子查询返回结果为单个值)
- 列子查询 (子查询返回结果为一列)
-  行子查询 (子查询返回结果为一行)
- 表子查询 (子查询返回结果为多行多列)

根据**子查询位置** 分为

- WHERE 之后 (把 select 查询出来的结果当作另一个 select 的条件值)
- FROM 之后 (把查询出的结果当作一个新表)
- SELECT 之后

###### 标量子查询

概念：子查询返回的记过是单个值 (数字、字符串、日期等), 最简单的形式。

常用的操作符号: =, <>, >, >=, <, <=

###### 列子查询

概念：子查询返回的结果是 一列 (可以是多行)

常用的操作符: IN, NOT IN, ANY, SOME, ALL

| 操作符 | 描述                                        |
| ------ | ------------------------------------------- |
| IN     | 在指定的集合范围之内，多选一                |
| NOT IN | 不在指定的集合范围之内                      |
| ANY    | 子查询返回列表中，有任意一个满足即可        |
| SOME   | 与 ANY 等同，使用 SOME 的地方都可以使用 ANY |
| ALL    | 子查询返回列表的所有值都必须满足            |

###### 行子查询

概念：子查询返回的结果是 一行 (可以是多列)

常用的操作符: =, <>, IN, NOT IN

###### 表子查询

概念：子查询返回的结果是 多行多列

常用的操作符: IN

### 事务

简介：事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些**操作要么同时成功，要么同时失败**。(假设该操作有 ABCD 四个步骤组成，若 ABCD 四个步骤都成功完成，则认为事务成功，若 ABCD 中任意一个步骤操作失败，则认为事务失败。)



默认 MySQL 的事务是自动提交的，也就是说，当执行一条 DML 语句时，MySQL 会立即隐式的提交事务。

```mysql
-- 新建一张表 account

CREATE TABLE account (
  id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  name VARCHAR(10) COMMENT '姓名',
  money INT COMMENT '余额'
) COMMENT '账户表';

INSERT INTO account (id, name, money) VALUES (NULL, '张三', 2000), (NULL, '李四', 2000);

-- 恢复数据
UPDATE account SET money = 2000 WHERE name = '张三' OR name = '李四';

-- 事务操作

-- 方式一: 查看/设置事务提交方式

SELECT @@autocommit; -- 1: 自动提交，0: 手动提交

SET @@autocommit = 0; -- 设置的是当前会话的 事务提交方式，其它会话不受影响。

-- 案例：转账操作 (张三给李四转账 1000) (把整个操作控制在一个事务内)

-- 设置事务为手动提交
SET @@autocommit = 0;

--  1. 查询张三账户余额
SELECT * FROM account WHERE name = '张三';

--  2. 将张三账户余额 -1000
UPDATE account SET money = money - 1000 WHERE name = '张三';

--  3. 将李四账户余额 +1000
UPDATE account SET money = money + 1000 WHERE name = '李四';

-- 成功了就执行 提交事务
COMMIT;

-- 失败了就执行 回滚事务
ROLLBACK;

-- 方式二: 手动开启事务

-- 开启事务
START TRANSACTION;

-- 或

BEGIN;

--  1. 查询张三账户余额
SELECT * FROM account WHERE name = '张三';

--  2. 将张三账户余额 -1000
UPDATE account SET money = money - 1000 WHERE name = '张三';

--  3. 将李四账户余额 +1000
UPDATE account SET money = money + 1000 WHERE name = '李四';

-- 提交事务
COMMIT;

-- 回滚事务
ROLLBACK;
```

##### 事务四大特性 (ACID)

- 原子性(Atomicity): 事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
- 一致性(Consistency): 事务完成时，必须使所有的数据都保持一致状态。(也就是说一个事务执行之前和执行之后都必须处于一致性状态。让数据保持一定上的合理。)
- 隔离性(Isolation): 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
- 持久性(Durability): 事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

##### 并发事务问题

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事务读到另外一个事务还没有提交的数据。(主要是 修改操作)  |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读。 |
| 幻读       | 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了 "幻影"。(主要是 插入操作) |

##### 事务隔离级别

注意：事务隔离级别越高，数据越安全，但是性能越低。

| 隔离级别                   | 脏读  (✅: 会出现, ❎: 不会出现) | 不可重复读 | 幻读 | 解释                                                         |
| -------------------------- | ------------------------------ | ---------- | ---- | ------------------------------------------------------------ |
| Read uncommitted           | ✅                              | ✅          | ✅    | 读未提交                                                     |
| Read committed             | ❎                              | ✅          | ✅    | 读已提交 (事务 T2 只能读取 T1 已经提交的数据)                |
| Repeatable Read(MySQL默认) | ❎                              | ❎          | ✅    | 可重复读 (T2 执行第一次查询之后，在事务结束之前，其他事务不能修改对应的数据) |
| Serializable               | ❎                              | ❎          | ❎    | 同时只允许一个事务对数据表进行操作                           |

**从上到下隔离级别递增**，但是 Serializable (串行化) 性能最差。

**查看事务隔离级别**

```mysql
-- 查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION; -- REPEATABLE-READ

-- 设置事务隔离级别
-- SESSION: 仅针对当前会话窗口有效
-- GLOBAL: 针对所有会话窗口有效

SET [SESSION | GLOBAL] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE}

-- 恢复到 事务默认隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 索引

介绍：索引(index) 是帮助MySQL 高效获取数据的数据结构(有序)。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用(指向)数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。



索引用于快速找出在某个列中有一特定值的行，不使用索引，mysql 必须从第一条记录开始读完整个表，直到找出相关的行，表越大，查询数据所花费的时间就越多，如果表中查询的列有一个索引，mysql 能够快速到达一个位置去搜索数据文件，而不必查看所有数据，那么将会节省很大一部分时间。



##### 索引优缺点

**优点**：提高数据检索的效率，降低数据的 IO 成本。通过索引列对数据进行排序，降低数据排序的成本，降低 CPU 的消耗。

**缺点**：实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占空间的。索引大大提高了查询效率，同时却也降低更新表的速度，如对表进行 INSERT, UPDATE, DELETE 时，效率降低，同时索引也会被更新掉。

##### 索引结构

MySQL 的索引是在存储引擎层实现的，不同的存储引擎有不同的结构，主要包含以下几种

| 索引结构             | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| B+Tree 索引          | 最常见的索引类型，大部分引擎都支持 B+数 索引                 |
| Hash 索引            | 底层数据结构是用哈希实现的，只有精确匹配索引列的查询才有效，不支持范围查询。 |
| R-tree (空间索引)    | 空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少。 |
| Full-text (全文索引) | 是一种通过建立倒排索引，快速匹配文档的方式。类似于 Lucene, Solr, ES |

索引支持情况

| 索引        | InnoDB | MyISAM | Memory |
| ----------- | ------ | ------ | ------ |
| B+tree 索引 | 支持   | 支持   | 支持   |
| hash 索引   | 不支持 | 不支持 | 支持   |
| R-tree 索引 | 不支持 | 支持   | 不支持 |
| Full-text   | 支持   | 支持   | 不支持 |

我们平常所说的索引，如果没有特别指明，都是指 B+树 结构组织的索引。

**二叉树缺点**：顺序插入时，会形成一个链表，查询性能大大降低。大数据量情况下，层级较深，检索速度慢。

**红黑树** (也是二叉树): 可解决二叉树的缺点。但是 大数据量情况下，层级较深，检索速度慢。

**B-Tree** (多路平衡查找树)



**B+Tree 相对于 B-Tree 的区别**：1. 所有的数据都会出现在叶子节点 2. 叶子节点形成一个单向列表

MySQL 索引数据结构对经典的 B+Tree 进行了优化。在原 B+Tree 的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的 B+Tree，提高区间访问的性能。

Hash: 哈希索引就是采用一定的 hash 算法，将键值换算成新的 hash 值，映射到对应的槽位上，然后存储在 hash 表中。

如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了 hash 冲突(也称为 hash 碰撞),可以通过链表来解决。

**Hash 索引特点**

- Hash 索引只能用于对等比较(=, in)，不支持范围查询 (Between, >, <, ...)
- 无法利用索引完成排序操作
- 查询效率高，通常只需要一次检索就可以了，效率通常(只需要检索一次，没有出现 hash 冲突，不需要链表)要高于 B+Tree索引

在 MySQL 中，支持 Hash 索引的是 Memory 引擎，而 **InnoDB 中具有自适应 hash 功能**，hash 索引是存储引擎根据 B+Tree 索引在特定条件下自动构建的。

**为什么 InnoDB 存储引擎选择使用 B+Tree 索引结构？**

- 相对于二叉树，层级更少，搜索效率高
- 对于 B-Tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低。
- 相对于 Hash 索引，B+Tree 支持范围匹配及排序操作。

**说明**

- 在创建数据库表时，将字段声明为主键 (添加主键约束), 会自动在主键字段创建主键索引。
- 在创建数据库表时，将字段声明为唯一键 (添加唯一约束), 会自动在唯一字段创建唯一索引。
- 在表上定义了一个外键时，会自动创建一个普通索引 (单指索引)。

##### 索引分类

| 分类     | 含义                                                 | 特点                     | 关键字   |
| -------- | ---------------------------------------------------- | ------------------------ | -------- |
| 主键索引 | 针对于表中主键创建的索引                             | 默认自动创建，只能有一个 | PRIMARY  |
| 唯一索引 | 避免同一个表中某数据列中的值重复，值可以为 NULL      | 可以有多个               | UNIQUE   |
| 常规索引 | 快速定位特定数据                                     | 可以有多个               |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以有多个               | FULLTEXT |
| 单指索引 | 一个索引值包含单个列，一个表可以有多个单列索引。     |                          |          |
| 复合索引 | 一个索引包含多个列                                   |                          |          |

在 InnoDB 存储引擎中，根据**索引的存储形式**，又可以分为以下两种

无论如何 聚集索引一定存在

| 分类                       | 含义                                                       | 特点                 |
| -------------------------- | ---------------------------------------------------------- | -------------------- |
| 聚集索引 (Clustered Index) | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有，而且只有一个 |
| 二级索引 (Secondary Index) | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个         |

聚集索引选取规则

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE) 索引作为聚集索引。
- 如果表没有主键，或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引。

**回表查询**：先走二级索引找到主键值，再根据主键值 到 聚集索引 中拿到行数据。

1. 以下 SQL 语句，哪个执行效率更高？为什么？

   ```mysql
   -- 备注 id 为主键，name 字段创建的有索引。
   -- 答案：使用 id 查询的效率高，因为使用 name 查询要走 回表查询。
   
   SELECT * FROM user WHERE id = 10;
   
   SELECT * FROM user WHERE name = 'Arm';
   ```

2. InnoDB 主键索引的 B+Tree 高度为多高呢？

   ```mysql
   --  假设:
   --    一行数据大小为 1K, 一页中可以存储16行这样的数据。InnoDB 的指针占用6个字节的空间，主键即使为bigint,占用字节数为8。
   --    高度为 2:
   --        n * 8 + (n + 1) * 6 = 16 * 1024, 算出 n 约为 1170
   --        1171 * 16 = 18736 条
   --    高度为 3：
   --        1171 * 1171 * 16 = 21939856 条 (B+Tree 高度为 3时，可以存储 21939856 条)
   
   --   结论：2千万行以下数据，高度最多 3层，超过 2千万行数据，就要分库分表。
   ```

##### 索引语法

**单列索引**：一个索引只关联一个字段

**联合索引 / 组合索引**：一个索引关联多个字段

```mysql
-- index_name: 索引名
-- table_name: 表名
-- index_col_name: 列名

-- 创建索引
-- CREATE [UNIQUE | FULLTEXT] INDEX index_name ON table_name (index_col_name, ...);

-- 查看索引
-- SHOW INDEX FROM tab_name;

-- 删除索引
-- DROP INDEX index_name ON table_name;

-- 创建一个 user_info 表
CREATE TABLE user_info (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT 'id主键',
    name VARCHAR(20),
    phone CHAR(11) NOT NULL,
    email VARCHAR(50) NOT NULL,
    profession VARCHAR(50) NOT NULL,
    age TINYINT NOT NULL,
    gender TINYINT NOT NULL COMMENT '1男 2女',
    status TINYINT NOT NULL,
    createtime DATETIME NOT NULL
) COMMENT '用户信息表';

INSERT INTO user_info (id, name, phone, email, profession, age, gender, status, createtime)
VALUES
(NULL, '吕布', '17799990000', 'lvbu666@163.com', '软件工程', 23, 1, 6, '2001-02-02 00:00:00'),
(NULL, '曹操', '17799990001', 'caocao666@qq.com', '通讯工程', 33, 1, 0, '2001-03-05 00:00:00'),
(NULL, '赵云', '17799990002', '17799990@139.com', '英语', 34, 1, 2, '2002-03-02 00:00:00'),
(NULL, '孙悟空', '17799990003', '17799990@sina.com', '工程造价', 54, 1, 0, '2001-07-02 00:00:00'),
(NULL, '花木兰', '17799990004', '19980729@sina.com', '软件工程', 23, 2, 1, '2001-04-22 00:00:00'),
(NULL, '大乔', '17799990005', 'daqiao666@sina.com', '舞蹈', 22, 2, 0, '2001-02-07 00:00:00'),
(NULL, '露娜', '17799990006', 'luna_love@sina.com', '应用数学', 24, 2, 0, '2001-02-08 00:00:00'),
(NULL, '程咬金', '17799990007', 'chengyaojin@163.com', '化工', 38, 1, 5, '2001-05-23 00:00:00'),
(NULL, '项羽', '17799990008', 'xiaoyu666@qq.com', '金属材料', 43, 1, 0, '2001-09-18 00:00:00'),
(NULL, '白起', '17799990009', 'baiqi666@sina.com', '机械工程及其自动化', 27, 1, 2, '2001-08-16 00:00:00'),
(NULL, '韩信', '17799990010', 'hanxin520@163.com', '无机非金属材料工程', 27, 1, 0, '2001-06-12 00:00:00'),
(NULL, '荆轲', '17799990011', 'jingke123@163.com', '会计', 29, 1, 0, '2001-05-11 00:00:00'),
(NULL, '兰陵王', '17799990012', 'lanlinwang666@126.com', '工程造价', 44, 1, 1, '2001-04-09 00:00:00'),
(NULL, '狂铁', '17799990013', 'kuangtie@sina.com', '应用数学', 43, 1, 2, '2001-04-10 00:00:00'),
(NULL, '貂蝉', '17799990014', '84958948374@qq.com', '软件工程', 40, 2, 3, '2001-02-12 00:00:00'),
(NULL, '妲己', '17799990015', '2783238293@qq.com', '软件工程', 31, 2, 0, '2001-01-30 00:00:00'),
(NULL, '芈月', '17799990016', 'xiaomin2001@sina.com', '工业经济', 35, 2, 0, '2000-05-03 00:00:00'),
(NULL, '嬴政', '17799990017', '8839434342@qq.com', '化工', 38, 1, 1, '2001-08-08 00:00:00'),
(NULL, '狄仁杰', '17799990018', 'jujiamlm8166@163.com', '国际贸易', 30, 1, 0, '2007-03-12 00:00:00'),
(NULL, '安琪拉', '17799990019', 'jdodmlh@126.com', '城市规划', 51, 2, 0, '2001-08-15 00:00:00'),
(NULL, '典韦', '17799990020', 'ycaunanjian@163.com', '城市规划', 52, 1, 2, '2000-04-12 00:00:00'),
(NULL, '廉颇', '17799990021', 'lianpo321@126.com', '土木工程', 19, 1, 3, '2002-07-18 00:00:00'),
(NULL, '后羿', '17799990022', 'altycj2000@139.com', '城市园林', 20, 1, 0, '2002-03-10 00:00:00'),
(NULL, '姜子牙', '17799990023', '37483844@qq.com', '工程造价', 29, 1, 4, '2003-05-26 00:00:00');

SELECT * FROM user_info;

-- 按照下列的需求，完成索引的创建

-- 1. name 字段为 姓名字段，该字段的值可能会重复，为该字段创建索引。
CREATE INDEX idx_user_name ON user_info (name);

-- 2. phone 手机号字段的值，是非空，且唯一的，为该字段创建唯一索引。
CREATE UNIQUE INDEX idx_user_phone ON user_info (phone);

-- 3. 为 profession, age, status 创建联合索引。
CREATE INDEX idx_user_profession_age_status ON user_info (profession, age, status);

-- 4. 为 email 建立合适的索引来提升查询效率。
CREATE INDEX idx_user_email ON user_info (email);

-- 查看创建的索引
SHOW INDEX FROM user_info;

-- 5. 删除 email 索引
DROP INDEX idx_user_email ON user_info;
```

##### 哪些情况需要创建索引

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该创建索引
- 查询中与其他表关联的字段，外键关系建立索引
- 查询中排序的字段，排序的字段若通过索引去访问将大大提高排序速度
- 查询中统计或者分组字段

##### 哪些情况不需要创建索引

- 表记录太少
- 经常增删改的表
- 如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果
- 频繁更新的字段不适合建立索引，因为每次更新不单单是更新了记录还会更新索引
- WHERE 条件里用不到的字段不创建索引

### SQL 性能分析

##### 查看 SQL 执行频率

MySQL 客户端链接成功后，通过 `SHOW [SESSION | GLOBAL] STATUS` 命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的 INSERT, UPDATE, DELETE, SELECT 的访问频次。

```mysql
SHOW GLOBAL STATUS LIKE 'COM_______'; -- (_ 有七个)

-- 对应的信息: Com_delete, Com_insert, Com_select, Com_update
```

##### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数 (long_query_time, 单位: 秒，默认 10秒) 的所有 SQL 语句的日志。

MySQL 的慢查询日志默认没有开启，需要在 MySQL 的配置文件 (/etc/my.cnf) 中配置如下信息

```mysql
-- 开启 MySQL 慢查询日志查询开关
slow_query_log=1
-- 设置慢日志的时间为 2秒，SQL 语句执行时间超过 2秒，就会视为慢查询，记录慢查询日志。
long_query_time=2
-- 配置完毕之后，重新启动 MySQL 服务器进行测试，查看慢日志文件中记录的信息 /var/lib/mysql/localhost-slow.log。

-- 查看慢查询日志是否开启
SHOW VARIABLES LIKE 'slow_query_log';
```

##### profile 详情

`SHOW PROFILES` 能够在做 SQL 优化时帮助我们了解时间都耗费到哪里去了。通过 have_profiling 参数，能够看到当前 MySQL 是否支持 profile 操作

```mysql
SELECT @@have_profiling;

-- 默认 profiling 是关闭的，可以通过 SET 语句在 SESSION / GLOBAL 级别开启 profiling;

--  查看 profiling 开关是 开启还是关闭。0: 关闭, 1: 开启
SELECT @@profiling;

-- 开启 profiling 开关
SET profiling = 1;

-- 执行一系列的业务 SQL 的操作，然后通过如下指令查看指令的执行耗时：
SELECT * FROM tb_user;

-- 查看每一条 SQL 的耗时基本情况 (单位 秒)
SHOW PROFILES;

-- 查看 指定 query_id 的 SQL 语句各个阶段的耗时情况
SHOW PROFILE FOR QUERY query_id;

-- 查看指定 query_id 的 SQL 语句 CPU 的使用情况
SHOW PROFILE CPU FOR QUERY query_id;
```

##### EXPLAIN 执行计划

EXPLAIN 或者 DESC 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中 表如何连接和连接的顺序。

**语法**

```mysql
-- 语法：
-- 直接在 SELECT 语句之前加上关键字 explain / desc

-- EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件;

EXPLAIN SELECT * FROM user_info;

-- EXPLAIN 执行计划各字段含义：

-- id: SELECT 查询的序列号，表示查询中执行 SELECT 子句或者是操作表的顺序 (id 相同，执行顺序从上到下；id 不同，值越大，越先执行)。
-- select_type: 表示 SELECT 的类型，常见的取值有 SIMPLE (简单表，即不使用表链接或者子查询)、PRIMARY (主查询， 即外层的查询)、UNION (UNION 中的第二个或者后面的查询语句)、SUBQUERY (SELECT / WHERE 之后包含了子查询) 等。
-- type: 表示链接类型，性能由好到差连接类型为 NULL, system, const (id 索引，唯一索引), eq_ref, ref, range, index (用了索引), all (全表扫描，性能低)。
-- possible_key: 显示可能应用在这张表上的索引，一个或多个。
-- key: 实际使用的索引，如果为 NULL, 则没有使用索引。
-- key_len: 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好。
-- rows: MySQL 认为必须要执行查询的行数，在 InnoDB 引擎的表中，是一个估计值，可能并不总是准确的。
-- filtered: 表示返回结果的行数占需读取行数的百分比，filtered 的值越大越好。
-- Extra: 额外展示的信息

-- 重点关注: type, possible_key, key, key_len, Extra

-- Extra 知识小贴士:
--  Using index condition / NULL: 查找使用了索引，但是需要回表查询数据。
--  Using where / Using index: 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据。

-- Extra 值	含义	好坏
-- NULL	纯索引定位，无额外处理	最优
-- Using where	索引只匹配部分条件，取出数据后还要再过滤	一般
-- Using index	覆盖索引，只查索引不回表	优秀
-- Using filesort	需要额外排序	差
-- Using temporary	创建临时表分组 / 去重	很差
-- Using index condition	索引下推 ICP，部分条件存储引擎过滤	良好
```

##### 索引使用

**原则**

###### 1. 最左前缀法则

如果索引了多列 (联合索引)，要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果跳跃某一列，索引将部分失效 (后面的字段索引失效)。(查询时，必须包含索引里 最左边的列)

```mysql
EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 用到了索引(idx_user_profession_age_status) 符合 最左前缀法则

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age = 31; -- 索引生效(idx_user_profession_age_status)，符合 最左前缀法则

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程'; -- 索引生效(idx_user_profession_age_status)，符合 最左前缀法则

EXPLAIN SELECT * FROM user_info WHERE age = 31 AND status = '0';  -- 没有用到索引(联合索引没有剩下), 不符合 最左前缀法则

EXPLAIN SELECT * FROM user_info WHERE status = '0';  -- 没有用到索引(联合索引没有剩下), 不符合 最左前缀法则

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND status = '0';  -- 索引生效(idx_user_profession_age_status)，符合 最左前缀法则 (部分失效)

EXPLAIN SELECT * FROM user_info WHERE age = 31 AND status = '0' AND profession = '软件工程'; -- 索引生效 (索引的字段存在即可，不关心放的位置)
```

###### 2. 范围查询

联合索引中，出现范围查询 (>, <), 范围查询右侧的列索引失效。

```mysql
EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age > 30 AND status = '0'; -- 索引生效(idx_user_profession_age_status)，部分失效 (可以观察 key_len 列)

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age >= 30 AND status = '0'; -- 索引生效(idx_user_profession_age_status)，没有失效部分

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age >= 30 AND age < 50 AND status = '0'; -- 索引生效(idx_user_profession_age_status)，没有失效部分
```

###### 索引列运算

不要在索引列上进行运算操作，索引将失效，性能减低。

```mysql
EXPLAIN SELECT * FROM user_info WHERE phone = '17799990015'; -- 索引生效 (可以观察 key 列)

EXPLAIN SELECT * FROM user_info WHERE substring(user_info.phone, 10, 2) = '15'; -- 索引失效
```

###### 字符串不加引号

字符串类型字段使用时，不加引号，索引将失效。

```mysql
EXPLAIN SELECT * FROM user_info WHERE phone = 17799990015; -- 索引失效，key 列是 NULL 未使用到 索引。

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 正常走索引

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = 0; -- 索引生效(idx_user_profession_age_status)，部分失效 (可以观察 key_len 列)
```

###### 模糊查询

若果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。

```mysql
EXPLAIN SELECT * FROM user_info WHERE profession LIKE '软件%'; -- 正常走索引

EXPLAIN SELECT * FROM user_info WHERE profession LIKE '%工程'; -- 不走索引 (前面加了 % 就不再走索引)

EXPLAIN SELECT * FROM user_info WHERE profession LIKE '%工%'; -- 不走索引
```

###### OR 链接的条件

用 OR 分割开的条件，如果 OR 前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。

```mysql
EXPLAIN SELECT * FROM user_info WHERE id = 10 OR age = 23; -- 不会用到索引

EXPLAIN SELECT * FROM user_info WHERE phone = '17799990017' OR age = 23; -- 不会用到索引

-- 由于 age 没有索引，所以即使 id, phone 有索引，索引也会失效。所以需要针对于 age 也要建立索引。

CREATE INDEX idx_user_age ON user_info (age);

-- 创建完 age 的索引后，上面的两个 SQL 都会走索引。
```

###### 数据分布影响

如果 MySQL 评估使用索引比全表更慢，则不使用索引。

```mysql
EXPLAIN SELECT * FROM user_info WHERE phone >= '17799990020'; -- 正常走索引

EXPLAIN SELECT * FROM user_info WHERE phone >= '17799990000'; -- 不走索引，因为全表 所有的数据都符合 查询出来的是全表，因此扫描了全表 没有走索引。(关注 type 列)

EXPLAIN SELECT * FROM user_info WHERE phone >= '17799990010';-- 不走索引，因为全表 大部分数据都符合条件

EXPLAIN SELECT * FROM user_info WHERE phone >= '17799990013'; -- 正常走索引，只有一部分数据 满足条件，因此走索引更快。

-- IS NULL 和 IS NOT NULL 走不走索引，是取决于表中数据的分布的。

EXPLAIN SELECT * FROM user_info WHERE profession IS NULL; -- 不走索引 (走不走 索引是需要根据 profession 列的情况来了，由于 profession 是 NOT NULL 的，因此 会全表扫描)

EXPLAIN SELECT * FROM user_info WHERE profession IS NOT NULL;
```

###### SQL 提示

SQL 提示，是优化数据的一个重要手段，简单来说，就是在 SQL 语句中加入一些人为的提示来达到优化操作的目的。

```mysql
-- 添加一个 profession 索引

CREATE INDEX idx_user_profession ON user_info (profession);

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程'; -- 关注 key 列，使用的是 idx_user_profession_age_status 这个索引

-- USE INDEX: 告诉数据库用哪个索引 (只是一个建议，MySQL 会判断这个索引执行效率高不高，高了才会使用。)

EXPLAIN SELECT * FROM user_info USE INDEX (idx_user_profession) WHERE profession = '软件工程'; -- 关注 key 列，使用的是 idx_user_profession 这个索引

-- IGNORE INDEX: 告诉数据库不使用哪个索引

EXPLAIN SELECT * FROM user_info IGNORE INDEX (idx_user_profession) WHERE profession = '软件工程'; -- 关注 key 列，使用的是 idx_user_profession_age_status 这个索引

-- FORCE INDEX: 告诉数据库 强制使用哪个索引

EXPLAIN SELECT * FROM user_info FORCE INDEX (idx_user_profession) WHERE profession = '软件工程'; -- 关注 key 列，使用的是 idx_user_profession 这个索引
```

###### 覆盖索引

尽量使用覆盖索引 (查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到)，减少 SELECT *.

```mysql
-- 删除 age, email, profession 单列索引

DROP INDEX idx_user_age ON user_info;
DROP INDEX idx_user_email ON user_info;
DROP INDEX idx_user_profession ON user_info;

SHOW INDEX FROM user_info;

EXPLAIN SELECT id, profession FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 关注 Extra: Using index

EXPLAIN SELECT id, profession, age, status FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 关注 Extra: Using index

EXPLAIN SELECT id, profession, age, status, name FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 关注 Extra: NULL。要索引以外的字段，匹配到索引后必须回表读取完整行；没有额外过滤、排序，所以 Extra 为 NULL。

EXPLAIN SELECT * FROM user_info WHERE profession = '软件工程' AND age = 31 AND status = '0'; -- 关注 Extra: NULL


-- 一张表，有四个字段 (id, username, password, status) 由于数据量大，需要对以下 SQL 语句进行优化，该如何进行才是最优方案：

SELECT id, username, password FROM tb_user WHERE username = 'itcast';

-- 答：建立 username, password 的联合索引
```

###### 前缀索引

当字段类型为 字符串(VARCHAR, TEXT 等) 时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率。此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。

```mysql
-- 语法
-- CREATE INDEX idx_xxx ON table_name (column(n));

-- 前缀长度 (n)
--  可以根据索引的选择性来决定，而选择性是指不重复的索引值 (基数) 和 数据表的记录总数的比值，索引选择性越高 则查询效率越高，唯一索引的选择性是 1，这是最好的索引选择性，性能也是最好的。
-- 可以借助公式

-- SELECT COUNT(DISTINCT email) / COUNT(*) FROM user_info;
-- SELECT COUNT(DISTINCT SUBSTRING(email, 1, 5)) / COUNT(*) FROM user_info; -- 5 是最有性价比的 (截取最小，并且 返回值接近于 1)

CREATE INDEX idx_user_email_5 ON user_info (email(5));

EXPLAIN SELECT * FROM user_info WHERE email = 'chengyaojin@163.com'; -- 使用了索引 idx_user_email_5
```

###### 单列索引与联合索引

单列索引：即一个索引值包含单个列。

联合索引：即一个索引包含了多个列。

```mysql
EXPLAIN SELECT id, phone, name FROM user_info WHERE phone = '17799990010' AND name = '韩信'; -- phone 走索引，name 没有走索引，所以会 回表查询。

-- 创建一个唯一索引 (手机号是唯一索引，因此联合索引一定是 唯一索引。)
CREATE UNIQUE INDEX index_user_phone_name ON user_info (phone, name);

EXPLAIN SELECT id, phone, name FROM user_info USE INDEX (index_user_phone_name) WHERE phone = '17799990010' AND name = '韩信'; -- 走联合索引 index_user_phone_name，不会回表查询，Extra 列是 Using index
```

在业务场景中，如果存在多个查询条件，考虑针对查询字段建立索引时，建议建立联合索引，而非单列索引。

多条件联合查询时，MySQL 优化器会评估哪个字段的索引效率更高，会选择该索引完成本次查询。

###### 索引设计原则

- 针对于数据量较大，且查询比较频繁的表建立索引。
- 针对于常作为查询条件(where)、排序(order by)、分组(group by) 操作的字段建立索引。
- 尽量选择 区分度(重复度)高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高。(区分度比较低的列：性别、状态等)
- 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。
- 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以使用覆盖索引，节省存储空间，避免回表，提高查询效率，遵循最左前缀法则。
- 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也就越大，会影响增删改的效率。
- 如果索引列不能存储 NULL 值，请在创建表时 使用 NOT NULL 约束它。当优化器知道每列是否包含 NULL 值时，它可以更好的确定哪个索引最有效地用于查询。

### SQL优化

##### 插入数据

###### insert 优化

- 批量插入，但是不建议一次性超过一千条。(减少链接次数)
- 手动提交事务

```mysql
-- START TRANSACTION;
-- INSERT INTO tb_test VALUES (1, 'Tom1'), (2, 'Cast1'), (3, 'Jerry1');
-- INSERT INTO tb_test VALUES (1, 'Tom1'), (2, 'Cast1'), (3, 'Jerry1');
-- INSERT INTO tb_test VALUES (1, 'Tom1'), (2, 'Cast1'), (3, 'Jerry1');
-- COMMIT;
```

- 主键顺序插入，顺序插入的性能高于乱序插入。

**大批量数据插入**

如果一次性需要插入大批量数据 (百万)，使用 INSERT 语句插入性能较低，此时可以使用 Mysql 数据库提供的 load 指令进行插入。操作如下

1. 客户端链接服务器端时，加上参数 --local-infile
2. mysql --local-infile -u root -p
3. 设置全局参数 local_file 为 1，开启从本地加载文件导入数据的开关
4. SET GLOBAL local_infile = 1;
5. 执行 load 指令将准备好的数据，加载到表结构中。
6. load data local infile '/root/sql1.log' into table 'tb_user' fields terminated by ',' lines terminated by '\n';

##### 主键优化

**数据组织方式**：在 InnoDB 存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表 (index organized table IOT)。

**页分裂**：页可以为空，也可以填充一半，也可以填充 100%。每个页包含了 2 - N 行数据 (如果一行数据过大，会行溢出)，根据主键排列。

**页合并**：当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记 (flaged) 为删除，并且它的空间变得允许被其他记录声明使用。当页中删除的记录达到 MERGE_THRESHOLD (默认为页的 50%), InnoDB 会开始寻找最靠近的页 (前或后) 看看是否可以将两个页合并以优化空间使用。

MERGE_THRESHOLD: 合并页的阈值，可以自己设置，在创建表或者创建索引时指定。

**主键设计原则**

- 满足业务需求的情况下，尽量降低主键的长度。(二级索引里存储的是 主键ID，主键ID过长会占用大量磁盘空间。)
- 插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键。
- 尽量不要使用 UUID 做主键或者其他自然主键，如身份证号。
- 业务操作时，避免对主键的修改。

##### ORDER BY 优化

**Using filesort**: 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。

**Using index**: 通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index, 不需要额外排序，操作效率高。

```mysql
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age; -- 没有使用索引，并且 Extra 列显示 Using filesort

-- 没有创建索引时，根据 age, phone 进行排序
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age, phone; -- 没有使用索引，并且 Extra 列显示 Using filesort

-- 创建索引
CREATE INDEX idx_user_age_phone_aa ON user_info (age, phone);

-- 创建索引后，根据 age, phone 进行升序排序
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age, phone; -- 使用了索引 idx_user_age_phone_aa，并且 Extra 列显示 Using index

-- 创建索引后，根据 age, phone 进行降序排序
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age DESC, phone DESC; -- 使用了索引 idx_user_age_phone_aa，并且 Extra 列显示 Backward index scan; Using index

-- 违背最左前缀法则
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY phone, age; -- 使用了索引 idx_user_age_phone_aa，但是 Extra 列显示 Using index; Using filesort。 原因是 索引顺序是 age, phone 但是排序顺序是 phone, age

-- 创建索引时，默认是按照升序走。
EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age ASC, phone DESC; -- 使用了索引 idx_user_age_phone_aa，但是 Extra 列显示 Using index; Using filesort。原因是 phone 是降序排列。

SHOW INDEX FROM user_info; -- 里边的 索引 idx_user_age_phone_aa 对应的 Collation 里边显示的 age 和 phone 都是 A, 即可以看到创建索引时是 按照 升序走的。

-- 创建一个针对 age 升序排，phone 降序排列的 索引
CREATE INDEX idx_user_age_phone_ad ON user_info (age ASC, phone DESC);

SHOW INDEX FROM user_info; -- 里边的 索引 idx_user_age_phone_ad 对应的 Collation 里边显示的 age 是 A phone 是 D。

EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age ASC, phone DESC; -- 使用了索引 idx_user_age_phone_ad，Extra 列显示 Using index

EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age, phone; -- 使用了索引 idx_user_age_phone_aa，并且 Extra 列显示 Using index。没有受到索引 idx_user_age_phone_ad，Extra 的影响。

EXPLAIN SELECT id, age, phone FROM user_info ORDER BY age DESC, phone ASC; -- 使用了索引 idx_user_age_phone_ad，并且 Extra 列显示 Backward index scan; Using index

-- SELECT 查询的是 *
EXPLAIN SELECT * FROM user_info ORDER BY age ASC, phone DESC; -- 没有使用索引
```

**优化**

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。
- 尽量使用覆盖索引
- 多字段排序，一个升序 一个降序，此时需要注意联合索引在创建时的规则 (ASC / DESC)。
- 如果不可避免的出现 filesort， 大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size (默认 256 K)。 (SHOW VARIABLES LIKE 'sort_buffer_size'; -- 262144) 如果缓冲区占满了 就会在 磁盘文件中排序。

##### GROUP BY 优化

在分组操作时，可以通过索引来提高效率。

```mysql
-- 删除掉目前的联合索引 idx_user_profession_age_status

DROP INDEX idx_user_profession_age_status ON user_info;
DROP INDEX idx_user_age_phone_aa ON user_info;
DROP INDEX idx_user_age_phone_ad ON user_info;

-- 执行分组操作，根据 profession 字段分组

EXPLAIN SELECT profession, COUNT(*) FROM user_info GROUP BY profession; -- 没有走索引 Extra: Using temporary 走的临时表

-- 创建索引

CREATE INDEX idx_user_profession_age_status ON user_info (profession, age, status);

-- 执行分组操作，根据 profession 分组

EXPLAIN SELECT profession, COUNT(*) FROM user_info GROUP BY profession; -- 走索引 idx_user_profession_age_status

-- 执行分组操作，根据 profession 和 age 字段分组

EXPLAIN SELECT profession, COUNT(*) FROM user_info GROUP BY profession, age; -- 走索引 idx_user_profession_age_status

-- 满足最左前缀法则
EXPLAIN SELECT profession, age, COUNT(*) FROM user_info GROUP BY profession, age; -- 走索引 idx_user_profession_age_status

-- 不满足最左前缀法则
EXPLAIN SELECT age, COUNT(*) FROM user_info GROUP BY age; -- 用到了索引 idx_user_profession_age_status，但是 Extra: Using index; Using temporary

-- 在 WHERE 之后出现了 profession 因此满足 最左前缀法则
EXPLAIN SELECT age, COUNT(*) FROM user_info WHERE profession = '软件工程' GROUP BY age; -- 走索引 idx_user_profession_age_status
```

##### LIMIT 优化

一个常见又非常头疼的问题就是 limit 2000000, 10，此时需要 MySQL 排序前 2000010 记录，仅仅返回 2000000 - 2000010 的记录，其他记录丢弃，查询排序的代价非常大。

优化思路：一般分页查询时，通过创建 覆盖索引 能够比较好的提高性能，可以通过覆盖索引加子查询行形式进行优化。(使用 id + 子查询)

```mysql
SELECT id FROM user_info ORDER BY id LIMIT 2000000, 10;

-- 推荐 (MySQL 不支持)
SELECT * FROM user_info WHERE id IN (SELECT id FROM user_info ORDER BY id LIMIT 2000000, 10);

-- 推荐: 使用 内连接的形式
SELECT * FROM user_info AS u1 INNER JOIN (SELECT id FROM user_info ORDER BY id LIMIT 2000000, 10) AS u2 ON u1.id = u2.id;
```

##### COUNT 优化

InnoDB 引擎 在执行 COUNT(*) 的时候，需要把数据一行一行的从引擎里面读出来，然后累积计数 当数据量大的时候 COUNT(*) 一次也是很耗时的。

MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 COUNT(*) 的时候 (没有 WHERE 查询条件) 会直接返回这个数，效率很高。

**优化思路**：自己计数。(1. 使用 Redis 计数， 2. 新建一个表 存储统计数据)

**COUNT()**: 是一个集合函数，对于返回的结果集，一行行的判断，如果 COUNT 函数的参数不是 NULL，累积值就加 1， 否则不加，最后返回累计值。

```mysql
-- 用法: COUNT(*), COUNT(主键), COUNT(字段), COUNT(1).

-- COUNT(主键): InnoDB 引擎会遍历整张表，把每一行的 主键id 值都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加 (主键不可能为 NULL)。
-- COUNT(字段): 
--    1. 字段 没有 NOT NULL 约束: InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为 NULL, 不为 NULL, 计数累加。
--    2. 字段 有 NOT NULL 约束: InnoDB 引擎会遍历整张表把每行的字段值都取出来，返回给服务层，直接按行进行累加。
-- COUNT(1): InnoDB 引擎会遍历整张表，但是不取值。服务层对于返回的每一行，放一个数字 '1' 进去，直接按行进行累加。 (放 0, -1 都是一样的)
-- COUNT(*): InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。

SELECT COUNT(NULL) FROM user_info; -- 返回 0, NULL 不计数

-- 按照效率排序: COUNT(字段) < COUNT(主键id) ≈ COUNT(1) ≈ COUNT(*), 所以尽量使用 COUNT(*)。
```

##### UPDATE 优化

**注意事项**

- 更新时，要根据 索引字段进行更新 (查询条件要有索引)，否则 行锁就会升级为表锁，锁住整张表，并发性能会降低。(InnoDB 的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁。)

### MySQL 体系结构

##### 分层

**连接层**

最上层是一些客户端和链接服务，主要完成一些类似于链接处理、授权认证、及相关的安全方案。服务器也会为安全接入的每个客户端验证它所具有的操作权限。

**服务层 (SQL接口、解析器、查询优化器、混存)**

第二层架构主要完成大多数的核心服务功能，如 SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。

**引擎层 (索引)**

存储引擎真正的负责了 MYSQL 中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。

**存储层 (系统文件、文件、日志)**

主要是将数据存储在文件系统之上，并完成与存储引擎的交互。

##### 存储引擎

简介：存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可以被称为表类型。

**默认存储引擎**：InnoDB

```mysql
-- 在创建表时，指定存储引擎

-- CREATE TABLE 表名 (
--   字段1 字段1类型 [COMMENT 字段1注释]
-- ) ENGINE = INNODB [COMMENT 表注释];

CREATE TABLE my_myisam (
  name VARCHAR(10) COMMENT '姓名'
) ENGINE = MyISAM COMMENT '测试存储引擎表';

-- 查看当前数据库支持的存储引擎

SHOW ENGINES;
```

##### InnoDB

介绍：InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL5.5 之后，InnoDB 是默认的 MySQL 存储引擎。

**特点**

- DML(数据增删改) 操作遵循 ACID 模型，支持事务
- 行级锁，提高并发访问性能
- 支持外键 FOREIGN KEY 约束，保证数据的完整性和正确性

**文件**

- xxx.ibd: xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构(frm, sdi)、数据和索引
- 参数: innodb_file_per_table

### 视图1

视图 (View) 是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的 SQL 逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条 SQL 查询语句上。

**使用建议**：对复杂查询简化操作，并且不会对数据进行修改的情况下可以使用视图。

##### 视图操作

```mysql
-- 创建视图

CREATE OR REPLACE VIEW tb_user_v_1 AS SELECT id, name FROM tb_user WHERE id <= 10;

-- 查询视图
SHOW CREATE VIEW tb_user_v_1;

-- 查询视图里的数据
SELECT * FROM tb_user_v_1;

SELECT * FROM tb_user_v_1 WHERE id < 3;

-- 修改视图
CREATE OR REPLACE VIEW tb_user_v_1 AS SELECT id, name, age FROM tb_user WHERE id < 10;

ALTER VIEW tb_user_v_1 AS SELECT id, name, age, gender FROM tb_user WHERE id < 10;

-- 查询视图
SHOW CREATE VIEW tb_user_v_1; -- 可以看到视图里 查询的内容变了

-- 删除视图
DROP VIEW IF EXISTS tb_user_v_1;

-- 创建视图
CREATE OR REPLACE VIEW tb_user_v_1 AS SELECT id, name FROM tb_user WHERE id <= 20;

-- 往视图中添加数据
INSERT INTO tb_user_v_1 VALUES(6, 'Tom');

INSERT INTO tb_user_v_1 VALUES(30, 'Tom');

-- 修改视图，添加限制条件
CREATE OR REPLACE VIEW tb_user_v_1 AS SELECT id, name FROM tb_user WHERE id <= 20 WITH CASCADED CHECK OPTION;

-- 有了 CHECK 检查，就不能插入 越界的数值。
INSERT INTO tb_user_v_1 VALUES(33, 'Tom'); -- 失败

-- 原表插入 可以插入成功
INSERT INTO tb_user VALUES(33, 'Tom', 30, '男');

-- 视图的检查选项
--  当使用 WITH CHECK OPTION 子句创建视图时，MySQL 会通过视图检查正在更改的每个行，例如 插入，更新，删除，以使其符合视图的定义。
--  MySQL 允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql 提供了两个选项：CASCADED 和 LOCAL，默认值为 CASCADED。

-- CASCADED: 不仅要满足当前视图，也要满足 当前视图所依赖的视图。

-- 创建视图 1
CREATE OR REPLACE VIEW tb_user_v_1 AS SELECT id, name FROM tb_user WHERE id <= 20;

INSERT INTO tb_user_v_1 VALUES(5, 'Tom'); -- 成功

INSERT INTO tb_user_v_1 VALUES(25, 'Tom'); -- 成功

-- 基于 tb_user_v_1 创建一个新的 v_2 视图，并且添加 CASCADED 检查 (此时，添加数据不光会受到 tb_user_v_2 的约束，也会受到 tb_user_v_1 的约束)

CREATE OR REPLACE VIEW tb_user_v_2 AS SELECT id, name FROM tb_user_v_1 WHERE id >= 10 WITH CASCADED CHECK OPTION;

-- 插入数据
INSERT INTO tb_user_v_2 VALUES(7, 'Tom'); -- 失败 不满足 tb_user_v_2

INSERT INTO tb_user_v_2 VALUES(26, 'Tom'); -- 失败 不满足 tb_user_v_1，虽然 v_1 没有定义检查选项，但是 会受到 v_2 的 CASCADED 作用，导致不能添加成功。

INSERT INTO tb_user_v_2 VALUES(15, 'Tom'); -- 成功 即满足 tb_user_v_2, 也满足 tb_user_v_1

-- 基于 tb_user_v_2 创建一个视图
CREATE VIEW tb_user_v_3 AS SELECT id, name FROM tb_user_v_2 WHERE id <= 15;

INSERT INTO tb_user_v_3 VALUES(11, 'Tom'); -- 成功

INSERT INTO tb_user_v_3 VALUES(17, 'Tom'); -- 成功，满足 tb_user_v_3 (v_3 不定义检查选项), 也满足 tb_user_v_, tb_user_v_1

INSERT INTO tb_user_v_3 VALUES(28, 'Tom'); -- 失败 不满足 tb_user_v_1

-- LOCAL: 需要满足当前视图，当上一级视图 没有添加定义限制时，则不检查条件，有定义检查选项，则检查 那一级的条件。

-- 清空数据
DROP VIEW IF EXISTS tb_user_v_1;
DROP VIEW IF EXISTS tb_user_v_2;
DROP VIEW IF EXISTS tb_user_v_3;
DELETE FROM tb_user WHERE name LIKE 'Tom%';

-- 创建视图

CREATE VIEW tb_user_v_4 AS SELECT id, name FROM tb_user WHERE id <= 15;

INSERT INTO tb_user_v_4 VALUES(5, 'Tom'); -- 添加成功

INSERT INTO tb_user_v_4 VALUES(16, 'Tom'); -- 添加成功, v_4 没有对添加做检查

-- 基于 v_4 视图，添加 LOCAL 定义检查选项
CREATE VIEW tb_user_v_5 AS SELECT id, name FROM tb_user_v_4 WHERE id >= 10 WITH LOCAL CHECK OPTION;

INSERT INTO tb_user_v_5 VALUES(13, 'Tom'); -- 成功 v_5 有定义检查选项，但是 v_4 没有定义检查选项，因此可以添加成功。如果 v_4 定义检查选项，那么还需要检查 v_4 的检查条件。

INSERT INTO tb_user_v_5 VALUES(17, 'Tom'); -- 成功 v_4 没有定义检查选项

-- 基于 v_5 视图, 不定义检查选项
CREATE VIEW tb_ser_v_6 AS SELECT id, name FROM tb_user_v_5 WHERE id < 20;

INSERT INTO tb_user_v_4 VALUES(14, 'Tom'); -- 成功，因为: v_6 没有定义检查选项，v_5 定义了检查选项 但是满足条件，v_4 没有定义检查选项。

-- 视图的更新
--  要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图包含以下任何一项，则该视图不可更新：
--  1. 聚合函数 或 窗口函数 (SUM(), MIN(), MAX(), COUNT() 等)
--  2. DISTINCT
--  3. GROUP BY
--  4. HAVING
--  5. UNION 或者 UNION ALL

-- 如果从视图中删除数据，数据也将对原表中的数据进行删除，但是如果是多表联合 则不可以删除。

-- 创建视图，使用聚合函数
CREATE VIEW tb_user_v_count AS SELECT COUNT(*) FROM tb_user;

INSERT INTO tb_user_v_count VALUES (10); -- 失败: The target table tb_user_v_count of the INSERT is not insertable-into

-- 视图作用

-- 简单
--  视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些经常被使用的查询可以被定义为视图，从而使得用户不必为以后得操作每次指定全部的条件。
-- 安全
--  数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据。

-- 案例

-- 1. 为了保证数据库表的安全性，开发人员在操作 tb_user 表时，只能看到用户的基本操作，屏蔽手机号和邮箱两个字段。

USE itcast;

CREATE VIEW tb_user_info_view AS SELECT id, name, profession, age, gender, status, createtime FROM user_info;

-- 返回的数据里不包含 手机号 和 邮箱
SELECT * FROM tb_user_info_view;

-- 2. 查询每个学生所选修的课程(三张表联查)，这个功能在很多的业务中都有使用到，为了简化操作，定义一个视图。

USE itheima2;

-- 原 SQL
SELECT student.id, student.name AS student_name, student.no AS student_no, course.name AS course_name FROM student INNER JOIN student_course ON student.id = student_course.studentid INNER JOIN course ON student_course.courseid = course.id;

-- 创建视图
CREATE OR REPLACE VIEW tb_student_course_view AS
SELECT
    student.id AS student_id,
    student.name AS student_name,
    student.no AS student_no,
    course.name AS course_name
FROM
    student
    INNER JOIN student_course ON student.id = student_course.studentid
    INNER JOIN course ON student_course.courseid = course.id;

-- 查询视图内容

SELECT * FROM tb_student_course_view;

SELECT * FROM tb_student_course_view WHERE student_id = 1;
```

















### 视图

##### 什么是视图

​	视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。行和列数据来自定义视图的查询所引用的表，并且在引用视图时动态生成。

简单的来说，视图是由 select 结果组成的表。

#### 视图的特性

- 视图是对若干张基本表的引用，一张虚拟表，查询语句执行的结果。
- 不存储具体的数据（基本表数据发生了改变，视图也会跟着改变）。
- 可以跟基本表一样，进行增删改查操作（增删改操作有条件限制）。

#### 视图的作用

- 安全性。
  - 创建一个视图，定义好该视图所操作的数据。之后将用户权限与视图绑定，这样的方式是使用到了一个特性：grant 语句可以针对视图进行授予权限。
- 查询性能提高
- 提高了数据的独立性

#### 创建视图

create  [algorithm] = {undefined|merge|temptable}] view 视图名 [(属性清单)] as select 语句 [with [cascaded|local]check option];

algorighm 参数：指定替换方式。

- merge：处理方式，替代式，可以进行更新真实表中的数据。
- temptable：具化式，由于数据存储在临时表中，所以不可以进行更新操作。
- undefined：没有定义 algorithm 参数，mysql 更倾向于选择替代方式，是因为它更加有效。

with check option：更新数据时不能插入或更新 不符合视图限制条件的记录。

local 和 cascaded：为可选参数，决定了检查测试的范围，默认值为 cascaded。

```sql
-- 把工资大于 2000 的员工的所有信息 保存为一个视图
create view emp_sal_view as (select * from emp where sal > 2000);

-- 从视图中查询出 性别是 男 的员工
SELECT * FROM emp_sal_view where sex = '男';

-- 修改视图
create or replace view 视图名 as delect [...] from [...];

-- 删除视图
drop view emp_sal_view;

-- 该视图的 avg(sal) 字段 不可以修改（更新、删除）。
create algorithm = merge view avg_sal_view as (select ename, sal, avg(sal) from emp);
```

#### 视图机制

- 替代式：操作视图时，视图名直接被视图定义给替换掉。（默认替代式）
- 具化式：mysql 先得到了视图执行的结果，该结果形成一个中间结果暂时存在内存中。外面的 select 语句就调用了这些中间结果（临时表）。

区别：替换方式，将视图公式替换后，当成一个整体 sql 进行处理了。具体化方式，先处理视图结果，后处理外面的查询需求。

#### 视图不可更新部分

- 聚合函数

- distinct 关键字

- group by 子句

- having 子句

- union 运算符

- from 子句中包含多个表

- select 语句中引用了不可更新视图

只要视图当中的数据不是来自于基表，就不能够直接修改。

### 存储过程

##### 什么是存储过程

存储过程是 SQL 语句和控制语句的预编译集合，以一个名词存储并作为一个单元处理。

一组可编程的函数，是为了完成特定功能的 sql 语句集。存储过程就是具有名字的一段代码，用来完成一个特定的功能。创建的存储过程保存在数据字典中。

存储过程是事先经过编译并存储在数据中的一段 SQL 语句的集合，调用存储过场可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装和重用。

**特点**：

- 封装、复用。
- 可以接收参数、也可以返回数据。
- 减少网络交互，效率提升。

##### 为什么要用存储过程

- 将重复性很高的一些操作，封装到一个存储过程中，简化了对这些 sql 的调用
- 批量处理
- 统一接口，确保数据的安全
- 相对于 oracle 数据库来说，mysql 的存储过程相对功能较弱，使用较少。

##### 存储过程的优点

- 增强 SQL 语句的功能和灵活性
- 实现较快的执行速度
- 减少网络流量

##### 创建、调用、查看、删除

delimiter $$：把 ';' 改成了 '$$'。它与存储过程语法无关，delimiter 语句将标准分隔符 - 分号（;）更改为：$$。因为我们想将存储过程作为整体传递给服务器，而不是让 mysql 工具依次解释每个语句。

​	告诉 mysql 解释器，该段命令是否已经结束了，mysql 是否可以执行了，默认情况下，delimiter 是分号，在命令行客户端中，如果有一行命令以分号结束，那么回车后，mysql 将会执行该命令，但有时候，不希望 mysql 这么做，在为可能输入较多的语句，且语句中包含有分号，使用 delimiter $$ ，这样只有当 $$ 出现之后，mysql 解释器才会执行这段语句。

**创建**

```sql
DELIMITER $$
CREATE PROCEDURE show_emp()

BEGIN

-- SQL 语句
SELECT * FROM emp;

END$$
DELIMITER ;
```

**调用**

```sql
-- CALL 名称();
CALL show_emp();
```

**查看**

- 查看所有存储过程

```sql
SHOW PROCEDURE STATUS;
```

- 查看指定数据库中的存储过程

```sql
show procedure status where db='my_test';

SHOW PROCEDURE STATUS WHERE DB='itcast';
```

- 查看指定存储过程源代码

```sql
-- SHOW CREATE PROCEDURE 存储过程名;
SHOW CREATE PROCEDURE show_emp;
```

**删除**

```sql
-- DROP PROCEDURE [IF EXISTS] 存储过程名;
DROP PROCEDURE show_emp;
```

##### 存储过程变量

在存储过程中声明一个变量

```sql
delimiter $$
create procedure show_emp()
begin

-- 声明变量
declare res varchar(255) default '';
declare x,y int default 0;

end$$
delimiter ;
```

使用 declare 语句

```sql
-- declare 变量名 数据类型（大小） default 默认值;

-- 可以声明一个名为 total_sale 的变量，数据类型为 int,默认值为 0
declare total_sale int default 0;

-- 声明共享相同数据类型的两个或多个变量
declare x,y int default 0;
```

分配变量值

要为变量分配一个值，可以使用 set 语句

```sql
delimiter $$
create procedure show_emp()
begin

-- 声明变量
declare res varchar(255) default '';
declare x,y int default 0;

-- 更改变量值
set x = 3;
set y = 4;

-- 或

declare avgRes double default 0;
-- 把查询结果赋值 给 变量
select avg(sal) into avgRes from emp;

end$$
delimiter ;
```

使用 select into 语句将查询的结果分配给一个变量

```sql
delimiter $$
create procedure show_emp()
begin

-- 声明变量
declare res varchar(255) default '';
declare x,y int default 0;

declare avgRes double default 0;
-- 把查询结果赋值 给 变量
select avg(sal) into avgRes from emp;

end$$
delimiter ;
```

**变量的范围**

如果在存储过程中声明一个变量，那么当达到存储过程的 end 语句时，它将超出范围，因此在其它代码块中无法访问。（相当于：作用域）

##### 存储过程参数

三种类型

* in 输入，表示该参数的值必须在调用存储过程时指定。可以输入多个值。
* out 输出，表示该参数的值可以被存储过程改变，并且可以返回。可以返回多个值。
* inout 输入和输出，表示该参数的调用时指定，并且可以被改变和返回。

```sql
-- in

delimiter $$
create procedure getName(in name vatchar(255))
begin

select * from emp where ename = name;

end $$
delimiter ;

-- 调用
call getName('张三'); -- 返回 张三的 所有信息

-- out
-- 给一个名称，可以返回 对应的 薪资
delimiter $$
create procedure getSal(in name varchar(255), out sal int)
begin

select sal into sal from emp where ename = name;

end $$
delimiter ;

-- 调用
call getSal('张三', @s);
select @s; -- 返回张三的 薪资
-- 或
select @s from dual;

-- inout
delimiter $$
create procedure test(inout num int, in inc int)
begin

set num = num + inc;

end $$
delimiter ;

-- 调用
set @num1 = 20;
call test(@num1, 10);

select @num1; --30
```

##### 存储过程语句

- if 语句
- case 语句
- 循环

### 锁

**概述**：锁是计算机协调多个进程或线程 并发访问某一资源的机制。在数据库中，除传统的计算资源(CPU, RAM, I/O) 的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题。锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更复杂。

##### MySQL 锁分类

Mysql 中的锁，按照锁的颗粒度，分为以下三类

- 全局锁：锁定数据库中的所有表。
- 表级锁：每次操作锁住整张表。
- 行级锁：每次操作锁住对应的行数据。

##### 全局锁

全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的 DML 的写语句，DDL 语句，以及更新操作的事务提交语句都将被阻塞。

其典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。

```mysql
-- 开启全局锁
FLUSH TABLES WITH READ LOCK;

-- 这时候执行删除数据会报错
DELETE FROM user WHERE name LIKE 'Tom%'; -- 当前客户端会报错，其它客户端会卡着不动。Can't execute the query because you have a conflicting read lock

-- 只能查询数据
SELECT * FROM user;

-- 备份数据 (不要在 mysql 命令行里执行，要在 终端的 命令行)
mysqldump -u root -p itcast > itcast.sql; -- 备份的 sql 是在当前目录下

-- 解锁
UNLOCK TABLES;
```

**全局锁特点**

数据中加全局锁，是一个比较重的操作，存在以下问题

- 如果在主库上备份，那么在备份期间都不能执行更新和删除，业务基本上就得停摆。
- 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志(binlog)，会导致主从延迟。

在 InnoDB 引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致性数据备份。(底层是通过快照读来实现的)

```mysql
mysqldump --single-transaction -u root -p itcast > itcast2.sql;
```

##### 表级锁

每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在 MyISAM, InnoDB, BDB 等存储引擎中。

对于表级锁，主要分为以下三类

- 表锁
- 元数据锁 (meta data lock, MDL)
- 意向锁

###### 表锁

对于表锁，分为两类

- 表共享读锁 (read lock) 加了之后只能读不能写，并且其它客户端 也只能读 不能写 (会被卡住，只能等待 表释放锁)。并且哪个客户端加的只能自己释放，其它客户端无法释放。
- 表独占写锁 (write lock) 加了之后只能当前客户端进行读取和写入，其它客户端无法读取和写入。

读锁不会阻塞其他客户端的读，但是会阻塞写。写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写。

**语法**

```mysql
-- 1. 加锁
-- LOCK TABLES 表名1, 表名2... READ/WRITE;

LOCK TABLES user READ;

-- 当前客户端 这时候执行删除数据会报错，其它客户端执行会卡住不动。
DELETE FROM user WHERE name LIKE 'Tom%';

-- 当前和其它客户端都可以执行查询操作
SELECT * FROM user;

-- 2. 释放锁
UNLOCK TABLES / 客户端断开连接。

UNLOCK TABLES;

---

LOCK TABLES user WRITE;

-- 当前客户端 可以执行 删除操作，其它客户端执行会卡住不动。
DELETE FROM user WHERE name LIKE 'Tom%';

-- 当前客户端 可以执行 读取操作，其它客户端执行会卡住不动。
SELECT * FROM user;

UNLOCK TABLES;
```

###### 元数据锁 (meta data lock, MDL)

MDL 加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL 锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。

**存在的作用**是 为了避免 DML 与 DDL 冲突，保证读写的正确性。

在 MySQL 5.5 中引入了 MDL，当对一张表进行增删改查的时候，加 MDL 读锁(共享);当对表结构进行变更操作的时候，加 MDL 写锁 (排他)。(在一个客户端执行 事务时，其它客户端不能对表结构 进行改动。 即不能执行 ALTER TABLE... 只能等待执行事务的客户端 事务执行完毕。)

###### 意向锁

为了避免 DML 在执行时，加的行锁与表锁的冲突，在 InnoDB 中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。

分为两类

- 意向共享锁 (IS): 由语句 SELECT ... LOCK IN SHARE MODE 添加。与表锁共享锁(read) 兼容，与表锁排他锁(write) 互斥。
- 意向排他锁 (IX): 由 INSERT, UPDATE, DELETE, SELECT ... FOR update 添加。与表锁共享锁(read) 及排他锁(write) 都互斥。意向锁不会互斥。

事务里边的 增删该操作 都会对当前的表添加 意向排他锁。

##### 行级锁

每次操作锁住对应的行记录。锁定颗粒度最小，发生锁冲突的概率最低，并发度最高。应用在 InnoDB 存储引擎中。

InnoDB 的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

- 行锁 (Record Lock): 锁定单个行记录的锁，防止其他事务对此进行 update 和 delete。在 RC, RR 隔离级别下都支持。
- 间隙锁 (Gap Lock): 锁定索引记录间隙 (不含该记录), 确保索引记录间隙不变，防止其他事务在这个间隙进行 INSERT, 产生幻读。在 RR隔离级别下都支持。
- 临键锁 (Next-Key Lock): 行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙 Gap。在 RR隔离级别下支持。

###### 行锁

InnoDB 实现了以下两种类型的行锁

- 共享锁 (S): 允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。
- 排他锁 (X): 允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。

常见的 增删改查类型 以及所加的 行锁类型

| SQL                           | 行锁类型   | 说明                                        |
| ----------------------------- | ---------- | ------------------------------------------- |
| INSERT ...                    | 排他锁     | 自动加锁                                    |
| UPDATE ...                    | 排他锁     | 自动加锁                                    |
| DELETE ...                    | 排他锁     | 自动加锁                                    |
| SELECT (正常使用)             | 不加任何锁 |                                             |
| SELECT ... LOCK IN SHARE MODE | 共享锁     | 需要手动在 SELECT 之后加 LOCK IN SHARE MODE |
| SELECT ... FOR UPDATE         | 排他锁     | 需要手动在 SELECT 之后加 FOR UPDATE         |

默认情况下，InnoDB 在 REPEATABLE READ 事务隔离级别运行，InnoDB 使用 next-key 锁进行扫描和索引扫描，以防止幻读。

- 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。
- InnoDB 的行锁是针对于索引加的锁，不通过索引条件检索数据，那么 InnoDB 将对表中的所有记录加锁，此时就会 升级为表锁。

###### 间隙锁 / 临键锁

默认情况下，InnoDB 在 REPEATABLE READ 事务隔离级别运行，InnoDB 使用 next-key 锁进行搜索和索引扫描，以防止幻读。

- 索引上的等值查询 (唯一索引), 给不存在的记录加锁时，优化为间隙锁。
- 索引上的等值查询 (普通索引), 向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。
- 索引上的查询范围 (唯一索引) -- 会访问到不满足条件的第一个值为止。

**注意**： 间隙锁唯一的目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。

### 触发器

触发器是与表有关的数据库对象，指在 INSERT / UPDATE / DELETE 之前或之后，触发并执行触发器中定义的 SQL 语句集合。

触发器的这种特性可以协助应用在数据库端保证数据的完整性，日志记录，数据校验等操作。

使用别名 OLD 和 NEW  来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

| 触发器类型      | NEW 和 OLD                                             |
| --------------- | ------------------------------------------------------ |
| INSERT 型触发器 | NEW 表示将要或者已经新增的数据                         |
| UPDATE 型触发器 | OLD 表示修改之前的数据，NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 | OLD 表示将要或者已经删除的数据                         |

```mysql
-- 创建
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tb_name FROM EACH ROW -- 行级触发器
BEGIN

  trigger_stmt;

END;

-- 查看
SHOW TRIGGERS;

-- 删除
DROP TRIGGER [schema_name.] trigger_name; -- 如果没有指定 schema_name, 默认为当前数据库。

-- 通过触发器记录 tb_user 表的数据变更日志，将变更日志插入到日志表 user_logs 中，包含增加，修改，删除。

CREATE TABLE user_info_logs (
  id INT(11) AUTO_INCREMENT PRIMARY KEY COMMENT '主键ID',
  operation VARCHAR(20) NOT NULL COMMENT '操作类型, insert/update/delete',
  operation_time DATETIME NOT NULL COMMENT '操作时间',
  operation_id INT(11) NOT NULL COMMENT '操作的ID',
  operation_params VARCHAR(500) COMMENT '操作参数'
) COMMENT 'tb_user 的日志变动表';

-- 插入数据时触发器

CREATE TRIGGER tb_user_insert_trigger

AFTER INSERT ON user_info FOR EACH ROW

BEGIN

  INSERT INTO  user_info_logs(id, operation, operation_time, operation_id, operation_params) VALUES 
  (NULL, 'insert', NOW(), NEW.id, CONCAT('插入的数据内容为: id=', NEW.id, ', name=', NEW.name, ', phone=', NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession, ', age=', NEW.age, ', gender=', NEW.gender, ', status=', NEW.status));

END;
```



## 练习题

### 数据准备

```mysql
USE itcast3;

CREATE TABLE emp (
    id INT COMMENT '编号',
    workno VARCHAR(10) COMMENT '工号',
    name VARCHAR(10) COMMENT '姓名',
    gender CHAR(1) COMMENT '性别',
    age TINYINT UNSIGNED COMMENT '年龄',
    idcard CHAR(18) COMMENT '身份证号',
    workaddress VARCHAR(50) COMMENT '工作地址',
    entrydate DATE COMMENT '入职时间'
) COMMENT '员工表';

INSERT INTO emp VALUES 
(1, '1', '柳岩', '女', 20, '123456789012315678', '北京', '2000-01-01'),
(2, '2', '张无忌', '男', 18, '123456789010345670', '北京', '2005-09-01'),
(3, '3', '韦一笑', '男', 38, '123456789713345670',  '上海', '2005-08-01'),
(4, '4', '赵敏', '女', 18, '122345675712381670', '北京', '2009-12-01'),
(5, '5', '小昭', '女', 16, '123456769012341678', '上海', '2007-07-01'),
(6, '6', '杨逍', '男', 28, '12345678931234367X', '北京', '2006-01-01'),
(7, '7', '范瑶', '男', 40, '123456789212342670', '北京', '2005-05-01'),
(8, '8', '黛绮丝', '女', 38, '123456789012045678', '天津', '2015-05-01'),
(9, '9', '范亮亮', '女', 45, '123456789019345678', '北京', '2010-04-01'),
(10, '10', '陈友谅', '男', 53, '123456789812345678', '上海', '2011-01-01'),
(11, '11', '张士诚', '男', 55, '123456789712345678', '江苏', '2015-05-01'),
(12, '12', '常遇春', '男', 32, '123456789612345678', '北京', '2004-02-01'),
(13, '13', '张三丰', '男', 88, '123456789512345678', '江苏', '2020-11-01'),
(14, '14', '灭绝', '女', 65, '123456789014345678', '西安', '2019-05-01'),
(15, '15', '胡青牛', '男', 70, '12345678931234567X', '西安', '2018-04-01'),
(16, '16', '周芷若', '女', 18, NULL, '北京', '2012-06-01');

CREATE TABLE score (
    id INT COMMENT 'ID',
    name VARCHAR(20) COMMENT '姓名',
    math INT COMMENT '数学',
    english INT COMMENT '英语',
    chinese INT COMMENT '语文'
) COMMENT '学员成绩表';

INSERT INTO score (id, name, math, english, chinese) VALUES 
(1, 'Tom', 67, 88, 95),
(2, 'Rose', 23, 66, 90),
(3, 'Jack', 56, 98, 76);
```

### 基本查询

```mysql
-- 1. 查询指定字段 name, workno, age 返回

SELECT name, workno, age FROM emp;

-- 2. 查询所有字段返回

SELECT * FROM emp; -- 实际使用中 尽量不用 *，不够直观。

SELECT id, workno, name, gender, age, idcard, workaddress, entrydate FROM emp;

-- 3. 查询所有员工的工作地址，起别名

SELECT workaddress AS work_address FROM emp;

SELECT workaddress work_address2 FROM emp;

-- 4. 查询公司员工 上班地址 (不要重复)

SELECT DISTINCT workaddress FROM emp;

SELECT DISTINCT workaddress AS work_address FROM emp;
```

### 条件查询

```mysql
-- 1. 查询年龄 等于88 的员工信息

SELECT * FROM emp WHERE age = 88;

-- 2. 查询年龄 小于20 的员工信息

SELECT * FROM emp WHERE age < 20;

-- 3. 查询年龄 小于等于20 的员工信息

SELECT * FROM emp WHERE age <= 20;

-- 4. 查询 没有身份证号 的员工信息

SELECT * FROM emp WHERE idcard IS NULL;

-- 5. 查询有身份证号的员工信息

SELECT * FROM emp WHERE idcard IS NOT NULL;

-- 6. 查询年龄 不等于88 的员工信息

SELECT * FROM emp WHERE age != 88;

-- 或

SELECT * FROM emp WHERE age <> 88;

-- 7. 查询年龄在 15岁(包含) 到 20岁(包含) 之间的员工信息

SELECT * FROM emp WHERE age >= 15 AND age <= 20;

-- 或

SELECT * FROM emp WHERE age >= 15 && age <= 20;

-- 或

SELECT * FROM emp WHERE age BETWEEN 15 AND 20;

-- 错误 BETWEEN 后边跟着最小值，AND 后边跟着最大值，顺序不能反。

SELECT * FROM emp WHERE age BETWEEN 20 AND 15;

-- 8. 查询性别为 女 并且 年龄小于25岁 的员工信息

SELECT * FROM emp WHERE gender = '女' AND age < 25;

-- 9. 查询年啦等于18 或 20 或 40 的员工信息

SELECT * FROM emp WHERE age = 18 OR age = 20 OR age = 40;

-- 或

SELECT * FROM emp WHERE age = 18 || age = 20 || age = 40;

-- 或

SELECT * FROM emp WHERE age IN (18, 20, 40);

-- 10. 查询姓名为 两个字的员工信息

SELECT * FROM emp WHERE name LIKE '__';

-- 11. 查询姓名为 三个字的员工信息

SELECT * FROM emp WHERE name LIKE '___';

-- 12. 查询身份号最后一位是 X 的员工信息

SELECT * FROM emp WHERE idcard LIKE '%X';
```

### 聚合函数

```mysql
-- 1. 统计该企业 员工数量

SELECT COUNT(*) FROM emp;

SELECT COUNT(id) FROM emp;

-- 2. 统计该企业 有身份证号的 员工数量

SELECT COUNT(idcard) FROM emp; -- 会排除 NULL 值

-- 或者 

SELECT COUNT(*) FROM emp WHERE idcard IS NOT NULL;

-- 3. 统计该企业员工的 平均年龄

SELECT AVG(age) FROM emp;

-- 4. 统计该企业员工的 最大年龄

SELECT MAX(age) FROM emp;

-- 5. 统计该企业员工的 最小年龄

SELECT MIN(age) FROM emp;

-- 5.2 查找该企业员工 年龄最小的人 返回他的 name 和 age (子查询)

SELECT name, age FROM emp WHERE age = (SELECT MIN(age) FROM emp);

-- 6. 统计西安地区员工的年龄之和

SELECT SUM(age) FROM emp WHERE workaddress = '西安';
```

### 分组查询

```mysql
-- 1. 查询所有的性别

SELECT gender FROM emp GROUP BY gender;

-- 或者

SELECT DISTINCT gender FROM emp;

-- 2. 根据性别分组，统计男性员工 和 女性员工的 数量

SELECT gender, COUNT(*) FROM emp GROUP BY gender;

-- 3. 根据性别分组，统计男性员工 和 女性员工的 平均年龄

SELECT gender, AVG(age) FROM emp GROUP BY gender;

-- 4. 查询年龄 小于45 的员工，并根据工作地址分组，获取员工数量大于等于3 的工作地址

SELECT workaddress, COUNT(*) FROM emp WHERE age < 45 GROUP BY workaddress HAVING COUNT(*) >= 3;

-- AS 起别名
SELECT workaddress, COUNT(*) AS total FROM emp WHERE age < 45 GROUP BY workaddress HAVING total >= 3;

-- 5. 根据工作地址分组，获取员工数量大于等于3 的工作地址

SELECT workaddress, COUNT(*) FROM emp GROUP BY workaddress HAVING COUNT(*) >= 3;
```

### 排序查询

```mysql
-- 1. 根据年龄对公司的员工进行 升序排序

SELECT * FROM emp ORDER BY age;

-- 或

SELECT * FROM emp ORDER BY age ASC;

-- 2. 根据入职时间，对员工进行 降序排序

SELECT * FROM emp ORDER BY entrydate DESC;

-- 3. 根据年龄对公司的员工进行升序排序，年龄相同 再按照入职时间进行降序排序

SELECT * FROM emp ORDER BY age, entrydate DESC;
```

### 分页查询

```mysql

-- 1. 查询 第1页 员工数据，每页展示 10条 记录

SELECT * FROM emp LIMIT 0, 10;

-- 或

SELECT * FROM emp LIMIT 10;

-- 2. 查询 第2页 员工数据，每页展示 10条 记录

SELECT * FROM emp LIMIT 10, 10;
```

### 综合查询 (基本、条件、聚合、分组、排序、分页)

```mysql
-- 1. 查询年龄为 20, 21, 22, 23岁 的员工信息。

SELECT * FROM emp WHERE age IN (20, 21, 22, 23);

-- 2. 查询年龄为 20, 21, 22, 23岁 的女性员工信息。

SELECT * FROM emp WHERE age IN (20, 21, 22, 23) AND gender = '女';

-- 3. 查询性别为 男，并且年龄在 20-40岁(含) 以内的员工姓名为三个字的员工。

SELECT * FROM emp WHERE gender = '男' AND (age BETWEEN 20 AND 40) AND name LIKE '___';

-- 或
SELECT * FROM emp WHERE gender = '男' AND age BETWEEN 20 AND 40 AND name LIKE '___';

-- 或
SELECT * FROM emp WHERE gender = '男' AND age >= 20 && age <= 40 AND name LIKE '___';

-- 或
SELECT * FROM emp WHERE gender = '男' AND age >= 20 AND age <= 40 AND name LIKE '___';

-- 4. 统计员工表中，年龄小于 60岁 的，男性员工和女性员工的人数。

SELECT gender, COUNT(*) AS total FROM emp WHERE age < 60 GROUP BY gender;

-- 5. 查询所有年龄小于等于 35岁 员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序。

SELECT name, age FROM emp WHERE age <= 35 ORDER BY age ASC, entrydate DESC;

-- 或
SELECT name, age FROM emp WHERE age <= 35 ORDER BY age, entrydate DESC;

-- 6. 查询性别为男，且年龄在 20-40岁(含) 以内的前五个员工信息，对查询结果按年龄升序排序，年龄相同的按入职时间升序排序。

SELECT * FROM emp WHERE gender = '男' AND age BETWEEN 20 AND 40 ORDER BY age ASC, entrydate ASC LIMIT 0, 5;

-- 或
SELECT * FROM emp WHERE gender = '男' AND age BETWEEN 20 AND 40 ORDER BY age, entrydate LIMIT 5;

-- 7. 查询年龄 大于15岁 的员工 姓名、年龄，并根据年龄进行升序排序。(该查询 反应了 DQL 的查询循序)

SELECT name, age FROM emp WHERE age > 15 ORDER BY age;

-- e: emp 的别名

SELECT e.name AS e_name, e.age AS e_age FROM emp AS e WHERE e.age > 15 ORDER BY e_age ASC;

-- 或
SELECT e.name e_name, e.age e_age FROM emp e WHERE e.age > 15 ORDER BY e_age ASC;

-- 报错，SELECT 在 WHERE 之后执行，因此 WHERE 里的条件 不能用 SELECT 里边的 别名
-- SELECT e.name AS e_name, e.age AS e_age FROM emp e WHERE e_age > 15 ORDER BY age ASC;
```

### 函数

```mysql
-- 1. 由于业务需求变更，企业员工的工号，统一为 5位数，目前不足 5位数的全部在前面补 0。比如: 1号员工的工号应该为 00001。

-- 查询
SELECT name, LPAD(workno, 5, '0') FROM emp;

-- 修改
UPDATE emp SET workno = LPAD(workno, 5, '0');

-- 2. 通过数据库的函数，生成一个六位数的随机验证码。

-- 位数不够 6位时，需要前边进行补 0 操作
SELECT LPAD(ROUND(RAND() * 1000000, 0), 6, '0'); -- 返回 043447

-- 错误，有可能会返回 五位 65071
SELECT ROUND(RAND() * 1000000, 0); -- 返回 881860

-- 错误
-- 1. 返回不稳定，2. 数字生成概率不均匀。
-- 返回 9319 或 608770 或者 253153 不稳定。因为 RAND() 返回的数据 有可能 是 0.0012345 这种。
SELECT CEIL(RAND() * 1000000);

-- 3. 查询所有员工的入职天数，并根据入职天数倒序排序。

SELECT name, DATEDIFF(NOW(), entrydate) AS entry_days FROM emp ORDER BY entry_days DESC;

-- 4. 统计班级各个学员 各科的成绩 并展示，展示的规则如下：
--  >=85, 展示优秀
--  >=60, 展示及格
--  否则, 展示不及格

-- 只展示数学
SELECT name, math, (CASE WHEN math >= 85 THEN '优秀' WHEN math < 85 AND math >= 60 THEN '及格' ELSE '不及格' END) AS math_val FROM score;

-- 或
SELECT name, math, (CASE WHEN math >= 85 THEN '优秀' WHEN math >= 60 THEN '及格' ELSE '不及格' END) AS math_val FROM score;

-- 数学、英语、语文一起展示

SELECT
    name,
    math,
    (CASE WHEN math >= 85 THEN '优秀' WHEN math < 85 AND math >= 60 THEN '及格' ELSE '不及格' END) AS math_val,
    english,
    (CASE WHEN english >= 85 THEN '优秀' WHEN english < 85 && english >= 60 THEN '及格' ELSE '不及格' END) AS english_val,
    chinese,
    (
        CASE
            WHEN chinese >= 85 THEN '优秀'
            WHEN chinese >= 60 THEN '及格'
            ELSE '不及格'
        END
    ) AS chinese_val
FROM score;
```

### 多表查询

##### 内连接

```mysql
-- 1. 查询每一个员工的姓名，以及关联的部门名称 (隐式内连接实现)

SELECT emp.name, dept.name FROM emp, dept WHERE emp.dept_id = dept.id;

-- 或

SELECT e.name AS e_name, d.name AS d_name FROM emp AS e, dept AS d WHERE e.dept_id = d.id;

-- 2. 查询每一个员工的姓名，及关联的部门的名称 (显示内连接实现)

SELECT emp.name, dept.name FROM emp INNER JOIN dept ON emp.dept_id = dept.id;

-- 或

SELECT e.name AS e_name, d.name AS d_name FROM emp AS e INNER JOIN dept AS d ON e.dept_id = d.id;

-- 或

SELECT e.name e_name, d.name d_name FROM emp e INNER JOIN dept d ON e.dept_id = d.id;

-- 或者 省略 INNER

SELECT e.name e_name, d.name d_name FROM emp e JOIN dept d ON e.dept_id = d.id;
```

##### 外连接

```mysql
-- 1. 查询 emp 表的所有数据，和对应的部门信息 (左外链接)

SELECT emp.*, dept.name FROM emp LEFT OUTER JOIN dept ON emp.dept_id = dept.id;

-- 或 省略 OUTER

SELECT emp.*, dept.name FROM emp LEFT JOIN dept ON emp.dept_id = dept.id;

-- 或

SELECT e.*, d.name AS d_name FROM emp AS e LEFT JOIN dept AS d ON e.dept_id = d.id;

-- 2. 查询 dept 表的所有数据，和对应的员工信息 (右外链接)

SELECT * FROM emp RIGHT OUTER JOIN dept ON emp.dept_id = dept.id;

-- 或 省略 OUTER

SELECT dept.*, emp.* from emp RIGHT JOIN dept on emp.dept_id = dept.id;
```

##### 自连接

```mysql
-- 1. 查询员工信息 及其 所属领导的名字

SELECT e1.*, e2.name FROM emp AS e1 JOIN emp AS e2 ON e1.managerid = e2.id;

-- 或

SELECT a.*, b.name FROM emp a, emp b WHERE a.managerid = b.id;

-- 2. 查询所有员工 emp 及其领导的名字 emp，如果员工没有领导，也需要查询出来。

SELECT e1.*, e2.name FROM emp AS e1 LEFT OUTER JOIN emp AS e2 ON e1.managerid = e2.id;
```

##### 联合查询

```mysql
-- 1. 将薪资低于 5000 的员工，和 年龄大于50岁 的员工全部查出来。

-- 当某一条记录符合 两个条件时，会把该条记录 只列出来一次。

SELECT * FROM emp WHERE salary < 5000 UNION SELECT * FROM emp WHERE age > 50;

-- 或 (单表 “或” 条件，更推荐用 OR)

SELECT * FROM emp WHERE salary < 5000 OR age > 50;

-- 当某一条记录符合 两个条件时，会把该条记录 列出来两次，不会合并。

SELECT * FROM emp WHERE salary < 5000 UNION ALL SELECT * FROM emp WHERE age > 50;

-- 错误：返回的列数不同 无法合并。

SELECT * FROM emp WHERE salary < 5000 UNION SELECT name FROM emp WHERE age > 50;

-- 正确

SELECT name FROM emp WHERE salary < 5000 UNION SELECT name FROM emp WHERE age > 50;
```

##### 子查询

###### 标量子查询

```mysql
-- 1. 查询 "销售部" 的所有员工信息

-- 步骤分解
--  a. 查询 '销售部' 部门ID
--  b. 根据 '销售部' 部门ID 查询 员工表 获取员工信息

-- 适合 单表字段

SELECT * FROM emp WHERE dept_id = (SELECT id FROM dept WHERE name = '销售部');

-- 或

SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE name = '销售部');

-- 或者使用 内连接 (推荐)

SELECT emp.* FROM emp INNER JOIN dept ON emp.dept_id = dept.id WHERE dept.name= '销售部';

-- 2. 查询在 "方东白" 入职之后的员工信息

SELECT * FROM emp WHERE entrydate > (SELECT entrydate FROM emp WHERE name = '方东白');
```

###### 列子查询

```mysql
-- 1. 查询 "销售部" 和 "市场部" 的所有员工信息

SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE dept.name IN ('销售部', '市场部'));

-- 2. 查询比财务部所有人工资都高的员工信息

-- 步骤拆解

-- a. 找财务部 dept_id
SELECT id FROM dept WHERE dept.name = '财务部';

-- b. 找到所有的财务部人员工资
SELECT salary FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '财务部');

-- c. 查询 财务部最高工资

SELECT MAX(salary) FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '财务部');

-- d. 查询比财务部所有人工资都高的员工信息
SELECT * FROM emp WHERE salary > ALL (SELECT salary FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '财务部'));

-- 或者

SELECT * FROM emp WHERE salary > (SELECT MAX(salary) FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '财务部'));

-- 3. 查询比研发部其中任意一人工资高的员工信息

SELECT id FROM dept WHERE dept.name = '研发部';
SELECT salary FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '研发部');

SELECT * FROM emp WHERE salary > ANY (SELECT salary FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '研发部'));

-- 或

SELECT * FROM emp WHERE salary > SOME (SELECT salary FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '研发部'));

-- 或

SELECT * FROM emp WHERE salary > (SELECT MIN(salary) FROM emp WHERE dept_id = (SELECT id FROM dept WHERE dept.name = '研发部'));
```

###### 行子查询

```mysql
-- 1. 查询 与"张无忌" 的薪资和直属领导相同的员工信息

-- a. 查询 张无忌的 薪资 和 直属领导
SELECT salary, managerid FROM emp WHERE emp.name = '张无忌';

SELECT * FROM emp WHERE (salary, managerid) = (12500, 1);

-- b. 查询 与"张无忌" 的薪资和直属领导相同的员工信息
SELECT * FROM emp WHERE (salary, managerid) = (SELECT salary, managerid FROM emp WHERE emp.name = '张无忌');
```

###### 表子查询

```mysql
-- 1. 查询与 "鹿杖客", "宋远桥" 的职位和薪资相同的员工信息 (即：查询与 鹿杖客 职位和薪资都相同的员工信息，查询与 宋远桥 职位和薪资都相同的员工信息)

-- a. 先查询 "鹿杖客", "宋远桥" 的职位 和 薪资
SELECT job, salary FROM emp WHERE emp.name = '鹿杖客' OR emp.name = '宋远桥';

-- b. 查询与 "鹿杖客", "宋远桥" 的职位和薪资相同的员工信息
SELECT * FROM emp WHERE (job, salary) IN (SELECT job, salary FROM emp WHERE emp.name = '鹿杖客' OR emp.name = '宋远桥');

-- 2. 查询入职日期是 "2006-01-01" 之后的员工信息，及其部门信息。

-- a. 先查询 入职日期是 "2006-01-01" 之后的员工信息
SELECT * FROM emp WHERE emp.entrydate > '2006-01-01';

-- b. 查询入职日期是 "2006-01-01" 之后的员工信息，及其部门信息。

-- 子查询的结果作为一张临时表存在
SELECT e.*, d.* FROM (SELECT * FROM emp WHERE emp.entrydate > '2006-01-01') AS e LEFT JOIN dept AS d ON e.dept_id = d.id;
```

### 多表查询综合案例

```mysql
-- 添加一个表

CREATE TABLE salgrade (
  grade INT COMMENT '薪资等级',
  losal INT COMMENT '最低薪资',
  hisal INT COMMENT '最高薪资'
) COMMENT '薪资等级表';

INSERT INTO salgrade VALUES 
(1, 0, 3000),
(2, 3001, 5000),
(3, 5001, 8000),
(4, 8001, 10000),
(5, 10001, 15000),
(6, 15001, 20000),
(7, 20001, 25000),
(8, 25001, 30000);


-- 1. 查询员工的姓名、年龄、职位、部门信息。(使用左外链接)

SELECT emp.name, emp.age, emp.job, dept.name AS dept_name FROM emp LEFT JOIN dept ON emp.dept_id = dept.id;

-- 1.2 查询员工的姓名、年龄、职位、部门信息。(使用隐式内连接)

SELECT emp.name, emp.age, emp.job, dept.name FROM emp, dept WHERE emp.dept_id = dept.id;

-- 2. 查询年龄小于30岁的员工姓名、年龄、职位、部门信息。(使用左外链接)

SELECT e.name, e.age, e.job, dept.name FROM (SELECT * FROM emp WHERE emp.age < 30) AS e LEFT JOIN dept ON e.dept_id = dept.id;

-- 2.2 查询年龄小于30岁的员工姓名、年龄、职位、部门信息。(使用显示内连接)

SELECT emp.name, emp.age, emp.job, dept.name FROM emp INNER JOIN dept ON emp.dept_id = dept.id WHERE emp.age < 30;

-- 3. 查询拥有员工的部门ID、部门名称。(使用隐式内连接)

SELECT DISTINCT dept.id, dept.name FROM emp, dept WHERE emp.dept_id = dept.id;

-- 4. 查询所有年龄大于40岁的员工，及其归属的部门名称；如果员工没有分配部门，也需要展示出来。(使用左外连接)

SELECT emp.*, dept.name FROM emp LEFT JOIN dept ON emp.dept_id = dept.id WHERE age > 40;

-- 5. 查询所有员工的工资等级。(使用隐式内连接)
-- 链接条件: emp.salary >= salgrade.losal AND emp.salary <= salgrade.hisal;

SELECT emp.name, emp.salary, salgrade.grade, salgrade.losal, salgrade.hisal FROM emp, salgrade WHERE emp.salary >= salgrade.losal AND emp.salary <= salgrade.hisal;

-- 或

SELECT emp.name, emp.salary, salgrade.grade, salgrade.losal, salgrade.hisal FROM emp, salgrade WHERE emp.salary BETWEEN salgrade.losal AND salgrade.hisal;

-- 6. 查询 "研发部" 所有员工的信息及工资等级。(使用隐式内连接)
-- 链接条件: emp.salary BETWEEN salgrade.losal AND salgrade.hisal;
-- 链接条件: emp.dept_id = dept.id;
-- 查询条件: dept.name = "研发部";

SELECT emp.*, dept.name, salgrade.* FROM emp, dept, salgrade WHERE (emp.dept_id = dept.id) AND (emp.salary BETWEEN salgrade.losal AND salgrade.hisal) AND (dept.name = '研发部');

-- 7. 查询 "研发部" 员工的平均工资。

-- 隐式内连接
SELECT AVG(emp.salary) AS avg_salary FROM emp, dept WHERE emp.dept_id = dept.id AND dept.name = '研发部'; -- 9800.0000

-- 或 (显示式内连接 推荐)
SELECT AVG(emp.salary) AS avg_salary FROM emp INNER JOIN dept ON emp.dept_id = dept.id WHERE dept.name = '研发部';

-- 或

SELECT AVG(salary) avg_salary FROM emp WHERE emp.dept_id = (SELECT id FROM dept WHERE dept.name = '研发部'); -- 9800.0000

-- 8. 查询工资比 "灭绝" 高的员工信息。(使用 标量 子查询)

SELECT * FROM emp WHERE emp.salary > (SELECT salary FROM emp WHERE emp.name = '灭绝');

-- 9. 查询比平均薪资高的员工信息。(使用 标量 子查询)

SELECT * FROM emp WHERE emp.salary > (SELECT AVG(salary) avg_salary FROM emp);

-- 10. 查询低于本部门平均工资的员工信息。

-- a. 查询各个部门平均工资

SELECT e1.*, (SELECT AVG(e2.salary) FROM emp AS e2 WHERE e2.dept_id = e1.dept_id) AS dept_avg_salary FROM emp AS e1 WHERE e1.salary < (SELECT AVG(e2.salary) FROM emp AS e2 WHERE e2.dept_id = e1.dept_id);

-- 11. 查询所有的部门信息，并统计部门的员工人数。(SELECT 之后也可以出现子查询)

SELECT dept.id, dept.name, (SELECT COUNT(*) FROM emp WHERE emp.dept_id = dept.id) AS total FROM dept;

-- 12. 查询所有学生的选课情况，展示出学生名称、学号、课程名称。

-- 使用的表: student, course, student_course
-- 链接条件: student.id = student_course.studentid, course.id = student_course.courseid

SELECT student.name, student.no, course.name FROM student, student_course, course WHERE student.id = student_course.studentid AND course.id = student_course.courseid;

-- 或 (推荐,使用显示内连接)

SELECT student.name, student.no, course.name FROM student INNER JOIN student_course ON student.id = student_course.studentid INNER JOIN course ON course.id = student_course.courseid;

-- 隐式内连接 和 显示内连接 总结
--  1. 结果、性能：无任何区别；
--  2. 语法风险：逗号隐式 JOIN 容易漏关联条件，产生笛卡尔积；
--  3. 可读性：INNER JOIN + ON 分层清晰，便于维护；
--  4. 扩展性：JOIN 支持左 / 右 / 全外连接，逗号语法不支持；
--  5. 开发建议：一律使用 INNER JOIN ... ON 标准写法，放弃逗号多表写法。

-- 13. 查询选修了 MySQL 课程的学生 (要求使用 子查询)

SELECT id FROM course WHERE course.name = 'MySQL'; -- 3

SELECT studentid FROM student_course WHERE student_course.courseid = 3; -- 1, 2

SELECT student.name, student.no FROM student WHERE student.id IN (1,2);

-- 子查询实现

SELECT student.name, student.no FROM student WHERE student.id IN (SELECT studentid FROM student_course WHERE student_course.courseid = (SELECT id FROM course WHERE course.name = 'MySQL'));

-- 查看 该 SQL 执行计划

EXPLAIN SELECT student.name, student.no FROM student WHERE student.id IN (SELECT studentid FROM student_course WHERE student_course.courseid = (SELECT id FROM course WHERE course.name = 'MySQL'));
```







---

END

