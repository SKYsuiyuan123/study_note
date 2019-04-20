# mysql 数据库

## 数据库介绍

- 什么是数据库： 数据库是按照数据结构来组织、存储和管理数据的仓库。

## RDBMS （关系型数据库） 专业术语：

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
- MEMORY memory 使用存储在内存中的内容来创建表。每个 memory 表实际对应一个磁盘文件，格式是 .frm MEMORY类型的表访问非常快，因为它的数据是放在内存中的，并且默认使用 HASH 索引，但是一旦服务器关闭，表中的数据就会丢失，但是表还会继续存在。



|   特点   | MyISAM | InnoDB | Memory | Archive |
| :------: | :----: | :----: | :----: | :-----: |
| 存储限制 | 256TB  |  64TB  |   有   |   无    |
| 事务安全 |   -    |  支持  |   -    |    -    |
| 支持索引 |  支持  |  支持  |  支持  |         |
|  锁粒度  |  表锁  |  行锁  |  表锁  |  行锁   |
| 数据压缩 |  支持  |   -    |   -    |  支持   |
| 支持外键 |   -    |  支持  |   -    |    -    |



## MySQL 语句规范

- 关键字与函数名称全部大写
- 数据库名称、表名称、字段名称全部小写
- SQL 语句必须以分号结尾

## 数据库命令

- 登录数据库  输入命令后回车，填写密码。

```sql
mysql -uroot -p
```

- 查看数据库  查看都有哪些数据库

```sql
-- SHOW {DATABASES | SCHEMAS} [LIKE 'pattern' | WHERE expr]
show databases;
```

- 查看数据库的创建详细信息

```sql
show create database 数据库名称;
```

- 创建数据库

```sql
-- CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name

-- IF NOT EXISTS 忽略警告信息（不会直接把警告信息显示在客户端）
-- SHOW WARNINGS; -- 查看警告信息
create database 数据库的名称;

-- 创建指定编码格式的数据库
create database 数据库名称 character set utf8;
```

- 修改数据库的字符类型

```sql
alter database 数据库的名称 character set 编码名;
```

- 删除数据库

```sql
-- DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;
drop database 数据库的名称;
```

- 使用数据库

```sql
use 数据库的名称;
```

- 查看当前在使用哪个数据库

```sql
select database();

-- 或者
select database()\s;
```

- 查看数据库值都有哪些表

```sql
-- 先 use 数据库名，进入数据库后 再查看有哪些表。
show tables;
```

## SQL

- SQL(struct query language 结构化查询语言)

- SQL 是专为数据库而建立的操作命令集，是一种功能齐全的数据库语言，在使用它时，只需发出 ‘做什么’ 的命令，‘怎么做’不是使用者考虑的。

- SQL 语言的功能包括查询、操纵、定义和控制，是一个综合的、通用的关系数据库语言。

### SQL功能划分

- DDL： 数据定义语言 用来定义数据库对象： 创建或删除 库，表，列等。(create, drop, alter)

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

- 查看数据库  查看都有哪些数据库

```sql
show databases;
```

- 创建数据库

```sql
create database 数据库的名称;

-- 创建指定编码格式的数据库
create database 数据库名称 character set utf8;
```

- 查看数据库的创建详细信息

```sql
show create database 数据库名称;
```

- 修改数据库的字符类型

```sql
alter database 数据库的名称 character set 编码名;
```

- 删除数据库

```sql
drop database 数据库的名称;
```

- 查看都有哪些表

```sql
-- SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]; 查看表
-- FROM db_name 查看指定数据库的所有表,当前所在的数据库不会被改变。
show tables;
```

- 创建表

```sql
create table 表名(字段名1 类型 [约束]， 字段名2 类型 [约束]，...);

create table 表名(字段名1 类型 [约束] comment '该字段备注信息'， 字段名2 类型 [约束]，...);

-- 创建数据表同时将查询结果写入到数据表
CREATE TABLE [IF NOT EXISTS] tbl_name [(create_definition, ...)] select_statement;
```

- 修改表名

```sql
rename table 旧表名 to 新表名;
```

- 修改表的字符集

```sql
alter table 表名 character set 编码名;
```

- 查看创建表的完整语句

```sql
show create table 表名;
```

- 查看表结构

```mysql
desc 表名;
-- 或者 SHOW COLUMNS FROM tal_name;
```

- 删除表

```sql
drop table 表名;
```

- 添加字段

```sql
-- 添加 一列字段，并且添加至 指定列的后边。省略指定的列，则会添加到最后一列。若只写 first 不写指定的列名，则添加为 表里的 第一列。
-- ALTER TABLE tab_name ADD [COLUMN] col_name column_definition [FIRST | AFTER col_name];

-- 添加多列。只能依次添加到表的下方，不能指定位置。
-- ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition, ...);

alter table 表名 add 字段名 类型;
```

- 修改字段名

```sql
alter table 表名 change 旧字段名 新字段名 数据类型;
```

- 修改字段类型

```sql
alter table 表名 modify 字段名 新的类型;
```

- 删除字段

```sql
-- 删除 一列
-- ALTER TABLE tbl_name DROP [COLUMN] col_name

-- 删除多列
-- ALTER TABLE tbl_name DROP [COLUMN] col_name， DROP [COLUMN] col_name, ...;

alter table 表名 drop 字段名;
```

### DML

##### 插入数据

列名与列值的类型、个数、顺序要一 一对应。值不要超出列定义的长度。插入的日期和字符一样都使用引号括起来。

```sql
-- 插入数据
-- INSERT [INTO] tbl_name [(col_name), ...] VALUES(val, ...);

-- 字符串类型，日期类型要加引号
-- 日期格式：'2019/10/12' 或 '2019-10-12'
insert into 表名(列名1, 列名2, ...) values(数据1, 数据2,...);

-- 简写
insert into 表名 values(所有的值); -- 没有值的字段可以用 null 替代，自增长的 既可以用 null 也可以使用 default，有默认值的列，可以使用 default。

-- 一次插入多行数据
insert into 表名 values(第一行所有的值), (第二行所有的值), ...;
-- 或
insert into 表名(列名1, 列名2, ...) values(数据1, 数据2, ...), (数据1, 数据2, ...), ...;

-- 将查询结果写入数据表
INSERT [INTO] tbl_name [(col_name, ...)] SELECT ...;

-- 多表更新
UPDATE table_references 
SET col_name1 = {expr1 | DEFAULT}
[, col_name2 = {exper2 | DEFAULT}] ...
[WHERE where_condition]
```

##### 修改数据

```sql
-- 修改所有的记录 字段名 = 字段值 可以修改多个字段
update 表名 set 字段名 = 新的字段值, ...;

-- 修改符合条件的 字段名 = 新值
update 表名 set 字段名1 = 新的字段值1, 字段名2 = 新的字段值2, ... where num = 3;

-- 在原来的基础上修改
update 表名 set 字段名1 = 字段名1 + 增加的值, ... where 条件;
```

##### 删除数据

```sql
-- 清空表，并不清空表的结构，数据可以找回
delete from 表名;

-- 加上条件判断 清除符合条件的 行
delete from 表名 where 条件表达式;

--truncate 关键字可以将某一张表中的所有数据记录全部清空，清空数据之后，重新添加数据时，自动增长的数据会重新开始。
-- delete 不会，delete后数据再添加会增长。
truncate table 表名; -- 后边不可以跟表达式
```
- 删除 是空的字段值


```sql
-- 删除该字段是 null 的行
delete from 表名 where isnull(字段名);
```

##### delete  与 truncate 的区别：

- delete 删除表中的数据，表结构还在，删除后的数据可以找回。

- truncate 删除是把表直接 DRDP 掉，然后再创建一个同样的新表。删除的数据不能找回。执行的速度比 delete 快。表的结构依然在。

##### 查询参数占位符

- 语法 .query('SELECT ??, ?? FROM ?? WHERE ?? = ?', ['id', 'username', 'users', 'id', 1]) ??: 字段名、表名。 ?:值。
```javascript
const sql = `selete id, title, done from todos ${where} limit ? offset ?`;
const [todos] = await connection.query(sql, [prepage, (page-1) * prepage]);
```

### 更改用户的密码（SQL语句）

```sql
use mysql;          -- 进入数据库
update user set authentication_string = password('123456') where user = 'root' and Host = 'localhost';        -- 更新 root 的密码, host可以省略
flush privileges;   -- 刷新 MySQL 的系统权限相关表
exit;               -- 退出 或者 quit;

-- 或者使用 mysql 命令 （不用进入数据库） 123456 新密码
mysqladmin -u root -p password 123456; -- 输入原始密码。
-- 密码更新完成，可以用新密码连接登录了。
```

### DQL

数据库执行DQL语句不会对数据进行改变，而是让数据库发送结果集给客户端。

结果集： 通过查询语句查询出来的数据以表的形式展示，我们称这个表为虚拟结果集。存放在内存中。查询返回的结果集是一张虚拟表。

查询格式： select [字段列表， 表达式， 函数] from 表名;

字段列表：

- 查询所有列： SELECT * FROM 表名;

- 查询指定的列： SELECT 列1, 列2, ... FROM 表名;

表达式： select 表达式 from 表名;

- select 字段名*12 from 表名;

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

#### 条件查询

条件查询就是在查询时给出 where 子句，在 where 子句中可以使用一些运算符及关键字

条件查询关键字：

- =, !=（不等于）, <>（不等于）, <, <=, >, >=;

- BETWEEN...AND; 在...范围之内。

- IN(set); 在固定的范围值

- NOT IN(set); 不在固定的范围值

- IS Null; (为空) IS NOT NULL; (不为空)

- AND; 与

- OR; 或

- NOT; 非

条件查询的使用

- 条件查询 where: select 字段名 from 表名 where 表达式;

- and: select 字段名 from 表名 where 表达式1 and 表达式2;

- or: select 字段名 from 表名 where 表达式1 or 表达式2;

- in: select 字段名 from 表名 where 字段 in (值1, 值2, ...);

- not in: select 字段名 from 表名 where 字段 not in (值1, 值2, ...);

- between and: select 字段名 from 表名 where 字段名 between 值1 and 值2;

- is null: select 字段名 from 表名 where 字段 is null;

- is not null: select 字段名 from 表名 where 字段 is not null;

#### 模糊查询

- 根据指定的关键字进行查询

- 使用 LIKE 关键字后跟通配符

通配符：

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

-- 查询 包含 % 的记录
select * from students where name like '%1%%' escape '1'; -- 1 不匹配。
```

#### 字段控制查询

- 去除重复记录
```sql
select distinct name from students;
```
- 把查询字段的结果进行运算，必须都要是数据型。

    + select *, 字段1 + 字段2 from 表名。

    ```sql
    select age + score from students;
    ```
    + 列有很多记录的值为 null，因为任何东西与 null 相加的结果还是 null，所以 结果可能会出现 null。使用了把 null 转换成数值 0 的函数 ifnull()。
    ```sql
    -- 如果 score 为 null 的话，就变为 0.
    select *, age + ifnull(score, 0) from students;
    ```


#### 别名

查询表达式可以使用 [AS] alias_name 为其赋予别名。

别名可用于 GROUP BY, ORDER BY 或 HAVING 子句。

- 对查询结果起别名（使用 as 或 不写）

```sql
select name ename form emp;
-- 或者 使用 as
select name as ename from emp;
```

#### 排序

排序类型：

- 升序 ASC 从小到大，默认。

    ```sql
    select * from emp order by sal; -- 把 所有数据 按 sal 默认 升序排列
    ```

- 降序 DESC 从大到小

    ```sql
    select * from emp order by sal desc; -- 把所有数据 按照 sal 降序排列
    ```

```sql
-- 按照 sal 降序排列，当 sal 相同时，按照 id 降序排列
select * from emp order by sal desc, id desc;
```

#### 聚合函数（分组函数）

对查询的结果进行统计计算

常用聚合函数：

- count() 统计指定列不为 null 的记录行数，（当函数参数为 * 时不会忽略）

- max() 计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算

- min() 计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算

- sum() 计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为 0

- avg() 计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为 0

#### 分组查询

将查询结果按照 1个 或多个字段进行分组，字段值相同的一组。

使用：

- selct 字段名 from 表名 group by 字段名;

- 根据 gender（性别） 字段来分组，gender 字段的全部值只有两个('男'和'女')，所以分了两个组。

- 当 group by 单独使用时，只显示出每组的第一条记录。

- 所以 group by 单独使用的实际意义不大。

分组注意事项： 在使用分组时， select 后面直接跟的字段一般都出现在 group by 后。

- group by + group_concat()
    ```sql
    -- 按照部门列出 每一部门都有哪些人
    select deptno, group_concat(ename) from emp group by deptno;

    -- 按照 名字 和 性别同时分组
    select name, gender from emp group by gender, name;
    ```
    + group_concat(字段名)可以作为一个输出字段来使用，表示分组之后，根据分组结果，使用 group_concat()来放置每一组的某字段的值的集合。

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

- group by + having

    + 用来分组查询后指定一些条件来输出查询结果

    + having 作用和 where 一样，但 having 只能用于 group by

```sql

-- 按照部门分组，并且筛选部门工资总和大于 9000 的部门，显示部门号和部门总和
select deptno, sum(sal) from emp group by deptno having sum(sal) > 9000;

```

having 和 where 的区别

- having 是在分组后对数据进行过滤， where 是在分组前对数据进行过滤

- having 后面可以使用分组函数（统计函数）, where 后面不可以使用分组函数

- where 是对分组前记录的条件，如果某行记录没有满足 where 子句的条件，那么这行记录不会参加分组。having 是对分组后数据的约束。

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
select deptno, group_concat(sal), sum(sal) from emp where sal > 2000 group by deptno having sum(sal) > 6000 order by sum(sal) desc;

```

#### SQL 顺序

- 书写顺序 : select --> from --> where --> group by --> having --> order by --> limit

- 执行顺序 : from --> where --> group by --> having --> select --> order by --> limit

先从表里找指定条件的数据，然后分组，分组后再筛选，拿到筛选后的数据，排序，取出指定条数。

#### limit

从哪一行开始查，总共要查几行

- limit 参数1，参数2...

- 格式： select * from 表名 limit 参数1, 参数2;

- 角标是从 0 开始

```sql
-- 取 前三条 数据
select * from emp limit 0,3;

-- 按照 日期་降序排列 后取 前三条 数据
select * from emp order by hiredate desc limit 0,3;
```

分页思路：

```sql
int cutPage = 1;    -- 当前页，（默认是第一页），该数据由客户端每次请求时发送过来
int pageSize = 10;  -- 每页多少条数据

-- 当前页为 1， 第一页从 0 开始 (1 - 1) * 10 = 0;
-- 当前页为 2， 第一页从 10 开始 (2 - 1) * 10 = 10;
-- 当前页为 3， 第一页从 20 开始 (3 - 1) * 10 = 20;
    .
    .
    .
slect * from emp limit (cutPage - 1) * pageSize, pageSize;
```

## 数据的完整性

什么是数据的完整性： 保证用户输入的数据保存到数据库中是正确的。

如何添加数据完整性： 在创建表时给表中添加约束

##### 完整性分类

- 实体完整性

- 域完整性

- 引用完整性

### 实体完整性

什么是实体完整性： 表中的一行（一条记录）代表一个实体（entity）

实体完整性的作用：标识每一行数据不重复。行级约束

约束类型：

- 主键约束 (primary key)

    + 特点： 每个表中要有一个主键，一个表中只能有 一个 主键约束。

    + 数据唯一，且不能为 null

- 唯一约束 (unique)

    + 特点： 指定列的数据不能重复，可以为空值，一个表中可以有多个 unique 约束。

    + 格式： create table 表名（字段名1 数据类型， 字段名2 数据类型 unique）

- 自动增长列 (auto_increment)

    + 特点： 指定列的数据自动增长，即使数据删除，还是从删除的序号继续往下。

    + 格式： create table 表名（字段名1 类型 auto_increment, 字段2 类型）

##### 主键约束 添加方式

- create table 表名(字段名1 类型 primary key, 字段2 类型);

- create table 表名(字段名1 类型, 字段2 类型, primary key(要设置主键的字段));

- 联合主键： 当两个字段数据同时相同时，才违反联合主键约束

    + create table 表名(字段名1 类型, 字段2 类型, primary key(主键1, 主键2));

- 先创建表，再去修改表，添加主键

    + Alter table 表名 add constraint primary key (列名);

### 域完整性

使用： 限制此单元格的数据正确，不对照此列的其它单元格比较。

域完整性约束：

- 数据类型（创建表时定义）： 数值类型、日期类型、字符串类型。

- 非空约束(not null)

    + create table 表名(字段名1 类型，字段名2 类型 not null);

- 默认值约束(default)

    + create table 表名(字段名1 类型，字段名2 类型 default '男');

### 参照完整性

什么是参照完整性： 是指表与表之间的一种对应关系。通常情况下可以通过设置两表之间的主键，外键关系，或者编写两表的触发器来实现。

有对应参照完整性的两张表格，在对他们进行数据插入、更新、删除的过程中，系统都会将被修改表格与另一张对应表格进行对照，从而阻止一些不正确的数据的操作。

数据库的主键和外键类型一定要一致

设置参照完整性后，外键当中的内值，必须得是主键当中的内容。

一个表设置当中的字段设置为主键，设置主键的为主表

```sql
create table stu(num int primary key auto_increment, name varchar(10));
```

创建表时，设置外键，设置外键的为子表。

```sql
create table score(num int primary key auto_increment, scores int, sid int, foreign key(sid) references stu(num));

-- 或 添加一个约束名 sc_st_fk
create table score(num int primary key atuo_increment, scores int, sid int, constraint sc_st_fk foreign key(sid) references stu(num));

-- 先创建表，后修改表外建约束
alter table 表名 add constraint 约束名 foreign key(列名) references 主表名(列名);
```

要求：

- 禁止使用临时表
- 外键列和参照列必须创建索引，如果外键列不存在索引的话，mysql 将自动创建索引。
- 两个表必须得要是 InnoDB 类型
- 外键列和参照列必须具有相似的数据类型。其中数字的长度或是否有符号位必须相同，而字符的长度则可以不同。

##### 外键约束的参照操作

1. CASCADE：从父表删除或更新且自动删除或更新子表中匹配的行。
2. SET NULL：从父表删除或更新行，并设置子表中的外键列为 NULL。如果使用该选项，必须保证子表列没有指定 NOT NULL。
3. RESTRICT：拒绝对父表的删除或更新操作。
4. NO ACTION：标准 SQL 的关键字，在 mysql 中与 RESTRICT 相同。

## 表与表之间的关系

- 一对一

- 一对多 （添加外键的形式）

- 多对多 （一个老师可以有多个学生，一个学生也可以有多个老师）

    * 1、创建老师表

    * 2、创建学生表

    * 3、创建学生与老师关系表

    * 4、添加外键 （在关系表中）

`为什么要拆分表：` `避免大量冗余数据的出现。`

## 合并结果集

概念： 把两个 select 语句的查询结果合并到一起

`合并结果集的两种方式：`

- union 合并时去除重复记录。

- union all 合并时不去除重复记录

`格式：`

```sql
select * from 表1 union select * from 表2

select * from 表1 union all select * from 表2
```

`注意事项：` `被合并的两个结果： 列数、数据类型必须相同。`

## 链接查询

`什么是连接查询：` `连接查询也叫跨表查询，需要关联多个表进行查询`

`笛卡尔集：` `假设几个 A = {a, b}, 集合 B = {0, 1, 2}，则笛卡尔集为 (a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b,2) 可以扩展到多个集合的情况。`

`同时查询两个表，出现的就是笛卡尔集结果。`

`查询时给表起别名：` `select * from stu st, score sc`

`多表联查，如何保证数据正确：`

- 在查询时要把主键和外键保持一致。

```sql
select * from stu st, score sc where st.id = sc.sid; -- 99 连接法
```

- 主表当中的数据参照子表当中的数据。

- 原理： 逐行判断，相等的留下，不相等的全不要。

## 根据连接方式分类

#### 内连接

显示左表及右表符合连接条件的记录。（交集）

+ 等值连接

    `两个表同时出现的 id 号（值）才显示`

    ```sql
    -- inner 可以省略
    select * from stu st inner join score sc on st.id = sc.id;
    
    -- 查询大于 70 分的
    select * from stu st join score sc on st.id = sc.id where sc.score > 70;
    
    -- 查询大于 70分的 男生
    select * from stu st join score sc on st.id = sc.id where sc.score > 70 and st.sex='男';
    ```
    与多表联查约束主外键是一样的，只是写法改变了。

    on 后面只写主外键

    如果还有条件直接在后面写 where

    多表联查后面还有条件就直接写 and

+ 非等值连接

    ```sql
    -- 查询出 所有员工的姓名、工资、所在部门的名称以及工资的等级。
    -- between...and  在...之间（不等值连接）
    select e.name, e.sal, d.name, s.grade from emp e, dept d, salgrade s where e.deptno = d.deptno and e.sal between s.losal and s.hisal;
    
    -- 或
    select e.name, e.sal, d.name, s.grade from emp e join dept d on e.deptno = d.depto join salgrade s on e.sal between s.losal and s.hisal;
    ```

+ 自连接

#### 外连接

显示 左表（使用左连接）或 右表（使用右连接）的全部记录及 右表 或 左表 符合连接条件的 记录。

1. 显示 左表（使用左连接） 的全部记录及 右表 符合连接条件的 记录。
2. 显示 右表（使用右连接）的全部记录及 左表 符合连接条件的 记录。

+ 左外连接（左连接） 两表满足条件相同的数据查出来，如果左边表当中有不相同的数据，也把左边表当中的数据查出来。右边表当中，只查出满足条件的内容。

    ```sql
    -- 查询出 所有的学生信息，以及其对应的 分数。（有的学生可能没有分数，所有要使用 左连接，显示时，没有分数的会在相应的位置 显示 null）
    select * from stu st left outer join score sc on st.id = sc.sid;
    ```

+ 右外连接（右连接） 右连接会把右表当中的数据全部查出，左表当中只查出满足条件的数据。

    ```sql
    -- outer 可以省略
    select * from stu st right outer join score sc on st.id = sc.sid;
    ```

+ 多表连接

    + 99 连接法
    + 内联查询

（站在表的角度去看，使用左连接就把左边表当中的内容全部查出，右边查出满足条件的。）

（使用右连接，就把右边表当中的数据全部查出，左边查出满足条件的。）

#### 自然连接

其实也是 一个 内连接。

连接查询会产生无用笛卡尔集，我们通常使用主外键关系等式来去除它。

而自然连接无需你去给出主外键等式，它会自动找到这一等式。也就是说，不要去写条件。

要求： 两张连接的表中列名称和类型完全一致的列作为条件（若是两个表中只有一列的 列名和其字段名 相同，则只把 该列作为条件，若是两个表中有两列 的列名和其字段名相同，则 把这两列同时作为 条件。）。

```sql
select * from emp natural join dept;

-- 等价于：
select * from emp, dept where emp.deptno = dept.deptno;
```

会自动去除相同的列

## 子查询

概念： 一个 select 语句中包含另一个完整的 select 语句。或两个以上 select，那么就是子查询语句了。

`子查询出现的位置：`

- where 后，把 select 查询出来的结果当作另一个 select 的条件值。

```sql
-- 查询出与 'KING' 在同一部门的人
select * from emp where deptno = (select deptno from emp where ename = 'KING');
```

- from 后，把查询出的结果当作一个新表。

```sql
-- 查询出 部门号为 30号，并且薪水大于等于 1500 的所有员工
select * from (select * from emp where deptno = 30) s where s.sal >= 1500;

-- 查询出 薪水大于等于 1500 并且部门号为 30号 的所有员工
select * from (select * from emp where sal >= 1500) s where s.deptno = 30;
```

- 使用 ANY, SOME, ALL 关键字

当子查询返回多个结果时，可以 使用 ANY, SOME, ALL 这几个关键字来 修饰。

```sql
operand comparison_operstor ANY (subquery);
operand comparison_operstor SOME (subquery);
operand comparison_operstor ALL (subquery);
```

取值：

|        |  ANY   |  SOME  |  ALL   |
| :----: | :----: | :----: | :----: |
| >, >=  | 最小值 | 最小值 | 最大值 |
| <, <=  | 最大值 | 最大值 | 最小值 |
|   =    | 任意值 | 任意值 |        |
| <>, != |        |        | 任意值 |

`练习`

```sql

-- 查询薪水大于等于 'ALLEN'薪水 的所有员工
select * from emp where sal >= (select sal from emp where ename = 'ALLEN');

-- 查询 工资高于 30号部门所有人 的员工信息
    -- 1、先查出 30 部门的最高工资
    -- select max(sal) from emp where deptno = 30;
    -- 2、再到整张表中，查询大于 30号部门的最高工资 的所有人
    select * from emp where sal > (select max(sal) from emp where deptno = 30);

-- 查询 工作和工资与 'SCOTT' 完全相同的员工信息 (推荐方式)
select * from emp where (job, sal) in (select job, sal from emp where ename = 'SCOTT');

-- 或者：
select * from emp, (select job, sal from emp where ename = 'SCOTT') s where emp.job = s.job and emp.sal = s.sal;

-- 查询 有两个以上直接下属 的员工信息
    -- 1、先对 mgr 分组
    -- select mgr from emp group by mgr;
    -- 2、再看有两个以上下属的编号
    -- select mgr from emp group by mgr having count(mgr) >= 2;
    -- 3、根据编号 查找信息
select * from emp where empno in(select mgr from emp group by mgr having count(mgr) >= 2);

-- 查询 员工编号为 7788 的员工名称、员工工资、部门名称、部门地址。
-- 1、多表联合查询
select e.ename, e.sal, d.dname, d.loc from emp e, dept d, where e.deptno = d.deptno and e.empno = 7788;

-- 或 2、使用自然连接
select e.ename, e.sal, d.dname, d.loc from emp e natural join dept d where e.empno = 7788;
```

## 自连接

概念： 自己连接自己，起别名。

给同一张表起不同的别名。

```sql
-- 查询 编号为7369员工的 编号，姓名，经理人编号，经理人姓名。
select e1.empno, e1.ename, e2.empno, e2.ename from emp e1, emp e2 where e1.mgr = e2.empno and e1.empno = 7369;
```

## 函数

说明：

​	事先提供好的一些功能可以直接使用，函数可以用在 select 语句及其子句，也可以用在 update,delete 语句当中。

函数分类：

#### 字符串函数

+ concat(s1, s2, ...sn); 把 s1,s2...sn 连接成一个字符串。任何字符串与 null 进行链接，结果都是 null
+ concat_ws('|', s1, s2, ...sn); 使用指定的分隔符进行字符连接。返回结果：s1|s2|sn...。
+ format(s1, str); 格式化 s1 并保留小数点后的 str 位。format(12560.75 ,1); 返回：12,560.8。
+ insert(str, x, y, instr) 将 str 从 x 开始 y 个，替换为 instr
+ Lower(str) 和 Upper(str)
+ Left(str, x) 和 right(str, x) 分别返回字符串 str 最左边的 x 个字符和最右边的 x 个字符。如果第二个参数为 null, 那么则不返回任何字符。
+ length(str)：返回 str 的长度。如果字符串包含空格，那么空格也算在长度之内。
+ LPAD(str, n, pad) 和 RPAD(str, n, pad) 用字符串 pad 对 str 最左边和最右边进行填充，直到长度为 n 个字符长度。
+ ltrim(str) 和 rtrim(str) 去掉字符串当中 最左侧和最右侧的空格。
+ trim(str) 去掉字符串左右的空格。
+ select trim (leading '?' from '??Mysql???'); 删除字符串中 前导的 '?'。返回：'Mysql???'。
+ select trim (trailing '?' from '??Mysql???'); 删除字符串中 后导的 '?'。返回：'?Mysql'。
+ select trim (both '?' from '??Mysql???'); 删除字符串中 前导 和 后导 的 '?'。返回：'Mysql'。
+ select trim (both '?' from '??My??sql???'); 返回：'My??sql'。中间的不会删掉。
+ REPEAT(str, x) 返回 str 重复 x 次的结果。
+ replace(str, a, b) 用字符串 b 替换字符串 str 中所有出现的字符串 a
+ substring(str, x, y) 返回字符串 str 中第 x 位置起 y 个字符长度的字符。

#### 数值函数

+ ABS(x) 求 x 的绝对值
+ CEIL(x) 向上取整
+ FLOOR(x) 向下取整
+ MOD(x, y) 返回 x/y 的模
+ RAND() 返回 0-1 内的随机值

#### 日期和时间函数

+ CURDATE() 返回当前的日期，只包含 年月日
+ CURTIME() 返回当前的时间，只包含 时分秒
+ NOW() 返回当前日期和时间，包含 年月日 时分秒
+ UNIX_TIMESTAMP 返回当前日期的时间戳
+ FROM_UNIXTIME(unixtime) 将一个时间戳转换成日期
+ WEEK(DATE) 返回当前是一年中的 第几周
+ YEAR(DATE) 返回 所给日期是哪一年
+ HOUR(TIME) 返回当前时间的小时
+ MINUTE(TIME) 返回当前时间的分钟
+ DATE_FORMAT(date, fmt) 按字符串格式化日期 date 的值
+ DATE_ADD(date, interval exper type) 计算日期间隔
+ DATEDIFF(date1, date2) 计算两个日期的相差天数

```sql
-- 返回当前格式化后的日期
select date_format(now(), '%M', '%D', '%Y');
-- 返回三天后的时间
select date_add(now(), interval 3 day);
```

#### 流程函数

+ if(value, t, f) 如果 value 是真，返回 t,否则返回 f。 select if(2 > 3, 'true', 'false');  => 'false

+ ifnull(value1, value2) 如果 value1 不为空，返回 value1 否则返回 value2.

+ case when ... then end. 例： select case when 2 > 3 then '对' else '错' end; => '错'

#### 其它函数

+ DATABASE() 返回当前数据库名 例： select database(); => 当前使用的数据库
+ CONNECTION_ID() 返回当前连接的 id，即：线程的 id。
+ LAST_INSERT_ID() 最后插入记录的 id 号。只能 查看 带 自增的主键，比如 id 值。
+ VERSION() 返回当前数据库版本
+ USER() 返回当前登录用户名
+ PASSWORD(str) 对 str 进行加密
+ MD5() 返回 str 的 MD5 值

## 事务

#### 什么是事务：

​	不可分割的操作，假设该操作有 ABCD 四个步骤组成，若 ABCD 四个步骤都成功完成，则认为事务成功，若 ABCD 中任意一个步骤操作失败，则认为事务失败。

mysql 默认是开启事务的，每条 SQL 语句都是一个事务。

事务只对 DML语句 有效，对于 DQL 无效。

#### 事务的 ACID：

- 原子性(Atomicity) 原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚。

- 一致性(Consistency) 一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。让数据保持一定上的合理。

- 隔离性(Isolation) 事务之间存在隔离性。

- 持久性(Durability) 事务提交后无法更改。

#### 事务的使用：

1. 开启事务 start transaction
2. 回滚事务 rollback
3. 提交事务 commit

事务 要么，失败回滚，要么，成功提交。

```sql
-- 1、开启事务
start transaction;

-- 2、张三减钱
update zs_account set money = money - 2000;

-- 3、李四加钱
update ls_account set money = money + 2000;

-- 此时并未提交，符合事务的隔离性，即：别的客户端访问时，还是原来的值。

-- 4、提交事务
commit;

-- 或

-- 4、回滚事务
rollback;
```

回滚事务： rollback 当遇到一突发情况，撤销执行的 sql 语句，回到开启事务前的状态。

提交事务： commit 所有语句全部执行完毕，没有发生异常，提交事务，更新到数据库当中。

#### 事务的并发问题

- 脏读： 解决办法： Read committed; 读提交，能解决脏读问题。

- 不可重复读： 一个事务范围内两个相同的查询却返回了不同的数据，这就是不可重复读。 解决办法： Repeatable read。

- 重复读： 事务开启，不允许其他事务的 update 修改操作。

- 幻读： 解决办法： Serializable 一般不使用，效率低。

#### 事务隔离级别

由低到高排列：

- Read uncommitted. 就是一个事务可以读取另一个未提交事务的数据

- Read committed 一个事务要等另一个事务提交后才能读取数据

- Repeatable read (mysql 默认级别)

- Serializable

```sql
-- 查看隔离级别
select @@global.tx_isolation, @@tx_isolation;

-- 设置隔离级别
-- 全局的：
set global transaction isolation level read committed;
```

#### 事务的并发问题与事务隔离级别的对应关系

|        事务隔离级别         | 脏读  | 不可重复读 | 幻读  |
| :-------------------------: | :---: | :--------: | :---: |
| 读未提交 (read-uncommitted) | true  |    true    | true  |
| 不可重复读 (read-committed) | false |    true    | true  |
| 可重复读 (repeatable-read)  | false |   false    | true  |
|    串行化 (serializable)    | false |   false    | false |

## 权限

#### 什么是权限：

​	限制一个用户能够做什么事情，在 mysql 中，可以设置全局权限，知道数据库权限，指定表权限，指定字段权限。

create 创建数据库、表或索引权限。

drop 删除数据库 或 表权限

alter 更改表，比如添加字段、索引等

delete 删除数据权限

index 索引权限

insert 插入权限

select 查询权限

update 更新权限

create view 创建视图权限

execute 执行存储过程权限

#### 创建用户

```sql
create user 'myxq'@'localhost' identified by '密码'; -- 默认没有任何权限
```

## 视图

#### 什么是视图：

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

## 存储过程

#### 什么是存储过程：

​	存储过程是 SQL 语句和控制语句的预编译集合，以一个名词存储并作为一个单元处理。

​	一组可编程的函数，是为了完成特定功能的 sql 语句集。存储过程就是具有名字的一段代码，用来完成一个特定的功能。创建的存储过程保存在数据字典中。

#### 为什么要用存储过程

- 将重复性很高的一些操作，封装到一个存储过程中，简化了对这些 sql 的调用
- 批量处理
- 统一接口，确保数据的安全
- 相对于 oracle 数据库来说，mysql 的存储过程相对功能较弱，使用较少。

#### 存储过程的优点

- 增强 SQL 语句的功能和灵活性
- 实现较快的执行速度
- 减少网络流量

#### 存储过程的创建、调用、查看、删除

delimiter $$：把 ';' 改成了 '$$'。它与存储过程语法无关，delimiter 语句将标准分隔符 - 分号（;）更改为：$$。因为我们想将存储过程作为整体传递给服务器，而不是让 mysql 工具依次解释每个语句。

​	告诉 mysql 解释器，该段命令是否已经结束了，mysql 是否可以执行了，默认情况下，delimiter 是分号，在命令行客户端中，如果有一行命令以分号结束，那么回车后，mysql 将会执行该命令，但有时候，不希望 mysql 这么做，在为可能输入较多的语句，且语句中包含有分号，使用 delimiter $$ ，这样只有当 $$ 出现之后，mysql 解释器才会执行这段语句。

##### 创建

```sql
delimiter $$
create procedure show_emp()
begin

-- 语句
select * from emp;

end$$
delimiter ;
```

##### 调用

```sql
-- call 名称();
call show_emp();
```

##### 查看

- 查看所有存储过程

```sql
show procedure status;
```

- 查看指定数据库中的存储过程

```sql
show procedure status where db='my_test';
```

- 查看指定存储过程源代码

```sql
-- show create procedure 存储过程名;
show create procedure show_emp;
```

##### 删除

```sql
-- drop procedure 存储过程名;
drop procedure show_emp;
```

#### 存储过程变量

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
declare 变量名 数据类型（大小） default 默认值;

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

变量的范围

​	如果在存储过程中声明一个变量，那么当达到存储过程的 end 语句时，它将超出范围，因此在其它代码块中无法访问。（相当于：作用域）

#### 存储过程参数

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

#### 存储过程语句

- if 语句
- case 语句
- 循环

## 索引

#### 什么是索引：

​	索引用于快速找出在某个列中有一特定值的行，不使用索引，mysql 必须从第一条记录开始读完整个表，直到找出相关的行，表越大，查询数据所花费的时间就越多，如果表中查询的列有一个索引，mysql 能够快速到达一个位置去搜索数据文件，而不必查看所有数据，那么将会节省很大一部分时间。

#### 索引的优势与劣势

##### 优势：

​	类似于大学图书馆建书目索引，提高数据检索效率，降低数据库的 IO 成本。通过索引对数据进行排序，降低数据排序的成本，降低了 cpu 的消耗。

##### 劣势：

​	实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占空间的。虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行 insert、update、delete。

#### 索引的分类

- 单值索引：即一个索引只包含单个列，一个表可以有多个单列索引。
- 唯一索引：索引列的值必须唯一，但允许有空值。
- 复合索引：一个索引包含多个列。index multildx(id, name, age)
- 全文索引：只有在 myisam 引擎上才能使用，只能在 char, varchar, text 类型字段上使用全文索引。
- 空间索引：空间索引是对空间数据类型的字段建立的索引。

#### 索引操作

##### 创建索引：

```sql
-- create index 索引名称 on table (column [, column]...);
create index salary_index on emp(salary);
```

##### 删除索引：

```sql
-- drop index 索引名称 on 表名;
```

##### 查看索引：

```sql
-- show index from 表名;
```

##### 自动创建索引：

​	在表上定义了主键时，会自动创建一个对应的唯一的索引。在表上定义了一个外键时，会自动创建一个普通索引（单值索引）。

#### EXPLAIN

用来查看索引是否正在被使用，并且输出其使用的索引的信息。

```sql
explain select * from emp where id = 101\G;
```

#### 索引结构

会先对数据进行排序

##### Btree 索引

B+ 树索引

B+ 树是一个平衡的多叉树，从根节点到每个叶子节点的高度差值不超过1，而且同层级的节点间有指针相互链接。

##### Hash 索引

哈希索引就是采用一定的哈希算法，把键值换算成新的哈希值，检索时不需要类似 B+ 树那样从根节点到子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。

hash 索引结构的热属性，其检索效率非常高，索引的检索可以一次定位，不像 B-Tree 索引需要从根节点到枝节点，最后才能访问到页节点这样多次的 IO 访问，所以 Hash 索引的查询效率要远高于 B-Tree 索引。

mysql 的索引默认是 Btree 不支持 Hash

#### 哪些情况需要创建索引

* 主键自动建立唯一索引
* 频繁作为查询条件的字段应该创建索引
* 查询中与其他表关联的字段，外键关系建立索引
* 频繁更新的字段不适合建立索引，因为每次更新不单单是更新了记录还会更新索引
* where 条件里用不到的字段不创建索引
* 查询中排序的字段，排序的字段若通过索引去访问将大大提高排序速度
* 查询中统计或者分组字段

#### 哪些情况不需要创建索引

* 表记录太少
* 经常增删改的表
* 如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果

* 频繁更新的字段不适合建立索引，因为每次更新不单单是更新了记录还会更新索引
* where 条件里用不到的字段不创建索引

## 并发控制

当多个连接对记录进行修改时保证数据的一致性和完整性。

#### 锁

##### 共享锁（读锁）：

​	在同一时间段内，多个用户可以读取同一个资源，读取过程中数据不会发生任何改变。

##### 排他锁（写锁）：

​	在任何时候只能有一个用户写入资源，当进行写锁时会阻塞其他的读锁或者写锁操作。

#### 锁颗粒

* 表锁：是一个开销最小的锁策略。
* 行锁：是一种开销最大的锁策略。