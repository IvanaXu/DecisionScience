### 1.1.1 SQL简介
SQL 是用于访问和处理数据库的标准的计算机语言。

- SQL，指结构化查询语言，全称是 Structured Query Language。
- SQL 让您可以访问和处理数据库。
- SQL 是一种 ANSI（American National Standards Institute 美国国家标准化组织）标准的计算机语言。

### 1.1.2 SQL 能做什么

- SQL 面向数据库执行查询
- SQL 可从数据库取回数据
- SQL 可在数据库中插入新的记录
- SQL 可更新数据库中的数据
- SQL 可从数据库删除记录
- SQL 可创建新数据库
- SQL 可在数据库中创建新表
- SQL 可在数据库中创建存储过程
- SQL 可在数据库中创建视图
- SQL 可以设置表、存储过程和视图的权限

### 1.1.3 SQL 语法
#### 1.1.3.1 数据库表
一个数据库通常包含一个或多个表。每个表有一个名字标识（例如:"Websites"）,表包含带有数据的记录（行）。
在本教程中，我们在 MySQL 的 RUNOOB 数据库中创建了 Websites 表，用于存储网站记录。
我们可以通过以下命令查看 "Websites" 表的数据：

```python
mysql> use RUNOOB;
Database changed

mysql> set names utf8;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT * FROM Websites;
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
+----+--------------+---------------------------+-------+---------+
5 rows in set (0.01 sec)

```

- **use RUNOOB;** 命令用于选择数据库；
- **set names utf8;** 命令用于设置使用的字符集；
- **SELECT * FROM Websites;** 读取数据表的信息；
- 上面的表包含五条记录（每一条对应一个网站信息）和5个列（id、name、url、alexa 和country）；

#### 1.1.3.2 SQL大小写
SQL 对大小写不敏感：SELECT 与 select 是相同的；

#### 1.1.3.3 SQL语句分号
某些数据库系统要求在每条 SQL 语句的末端使用分号。分号是在数据库系统中分隔每条 SQL 语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的 SQL 语句。

#### 1.1.3.4 SQL拆解
结构化查询语言SQL包含6个部分：

（1）数据查询语言（DQL：Data Query Language）其语句，也称为“数据检索语句”，用以从表中获得数据，确定数据怎样在应用程序给出。保留字SELECT是DQL（也是所有SQL）用得最多的动词，其他DQL常用的保留字有WHERE，ORDER BY，GROUP BY和HAVING。这些DQL保留字常与其它类型的SQL语句一起使用。

（2）数据操作语言（DML：Data Manipulation Language）其语句包括动词INSERT、UPDATE和DELETE。它们分别用于添加、修改和删除，

（3）事务控制语言（TCL）它的语句能确保被DML语句影响的表的所有行及时得以更新。包括COMMIT（提交）命令、SAVEPOINT（保存点）命令、ROLLBACK（回滚）命令。

（4）数据控制语言（DCL）它的语句通过GRANT或REVOKE实现权限控制，确定单个用户和用户组对数据库对象的访问。某些RDBMS可用GRANT或REVOKE控制对表单个列的访问，

（5）数据定义语言（DDL）其语句包括动词CREATE,ALTER和DROP。在数据库中创建新表或修改、删除表（CREAT TABLE 或 DROP TABLE）；为表加入索引等。

（6）指针控制语言（CCL）它的语句，像DECLARE CURSOR，FETCH INTO和UPDATE WHERE CURRENT用于对一个或多个表单独行的操作。

最主要的，
SQL (结构化查询语言)是用于执行查询的语法。但是 SQL 语言也包含用于更新、插入和删除记录的语法。
查询和更新指令构成了 SQL 的 DML 部分：

- _SELECT_ - 从数据库表中获取数据
- _UPDATE_ - 更新数据库表中的数据
- _DELETE_ - 从数据库表中删除数据
- _INSERT INTO_ - 向数据库表中插入数据

SQL 的数据定义语言 (DDL) 部分使我们有能力创建或删除表格。我们也可以定义索引（键），规定表之间的链接，以及施加表间的约束。
SQL 中最重要的 DDL 语句:

- _CREATE DATABASE_ - 创建新数据库
- _ALTER DATABASE_ - 修改数据库
- _CREATE TABLE_ - 创建新表
- _ALTER TABLE_ - 变更（改变）数据库表
- _DROP TABLE_ - 删除表
- _CREATE INDEX_ - 创建索引（搜索键）
- _DROP INDEX_ - 删除索引

### 1.1.4 SQL 快速参考
| **SQL 语句** | **语法** |
| --- | --- |
| AND / OR | SELECT column_name(s) FROM table_name WHERE condition AND&#124;OR condition |
| ALTER TABLE | ALTER TABLE table_name  ADD column_name datatypeorALTER TABLE table_name  DROP COLUMN column_name |
| AS (alias) | SELECT column_name AS column_alias FROM table_nameorSELECT column_name FROM table_name AS table_alias |
| BETWEEN | SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2 |
| CREATE DATABASE | CREATE DATABASE database_name |
| CREATE TABLE | CREATE TABLE table_name ( column_name1 data_type, column_name2 data_type, column_name2 data_type, ... ) |
| CREATE INDEX | CREATE INDEX index_name ON table_name (column_name)orCREATE UNIQUE INDEX index_name ON table_name (column_name) |
| CREATE VIEW | CREATE VIEW view_name AS SELECT column_name(s) FROM table_name WHERE condition |
| DELETE | DELETE FROM table_name WHERE some_column=some_valueorDELETE FROM table_name  (**Note:** Deletes the entire table!!)DELETE * FROM table_name  (**Note:** Deletes the entire table!!) |
| DROP DATABASE | DROP DATABASE database_name |
| DROP INDEX | DROP INDEX table_name.index_name (SQL Server) DROP INDEX index_name ON table_name (MS Access) DROP INDEX index_name (DB2/Oracle) ALTER TABLE table_name DROP INDEX index_name (MySQL) |
| DROP TABLE | DROP TABLE table_name |
| GROUP BY | SELECT column_name, aggregate_function(column_name) FROM table_name WHERE column_name operator value GROUP BY column_name |
| HAVING | SELECT column_name, aggregate_function(column_name) FROM table_name WHERE column_name operator value GROUP BY column_name HAVING aggregate_function(column_name) operator value |
| IN | SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,..) |
| INSERT INTO | INSERT INTO table_name VALUES (value1, value2, value3,....)_or_INSERT INTO table_name (column1, column2, column3,...) VALUES (value1, value2, value3,....) |
| INNER JOIN | SELECT column_name(s) FROM table_name1 INNER JOIN table_name2  ON table_name1.column_name=table_name2.column_name |
| LEFT JOIN | SELECT column_name(s) FROM table_name1 LEFT JOIN table_name2  ON table_name1.column_name=table_name2.column_name |
| RIGHT JOIN | SELECT column_name(s) FROM table_name1 RIGHT JOIN table_name2  ON table_name1.column_name=table_name2.column_name |
| FULL JOIN | SELECT column_name(s) FROM table_name1 FULL JOIN table_name2  ON table_name1.column_name=table_name2.column_name |
| LIKE | SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern |
| ORDER BY | SELECT column_name(s) FROM table_name ORDER BY column_name [ASC&#124;DESC] |
| SELECT | SELECT column_name(s) FROM table_name |
| SELECT * | SELECT * FROM table_name |
| SELECT DISTINCT | SELECT DISTINCT column_name(s) FROM table_name |
| SELECT INTO | SELECT _ INTO new_table_name [IN externaldatabase] FROM old_table_name_or*SELECT column_name(s) INTO new_table_name [IN externaldatabase] FROM old_table_name |
| SELECT TOP | SELECT TOP number&#124;percent column_name(s) FROM table_name |
| TRUNCATE TABLE | TRUNCATE TABLE table_name |
| UNION | SELECT column_name(s) FROM table_name1 UNION SELECT column_name(s) FROM table_name2 |
| UNION ALL | SELECT column_name(s) FROM table_name1 UNION ALL SELECT column_name(s) FROM table_name2 |
| UPDATE | UPDATE table_name SET column1=value, column2=value,... WHERE some_column=some_value |
| WHERE | SELECT column_name(s) FROM table_name WHERE column_name operator value |


### 1.1.5 SQL书籍

- 《数据库系统概论（第四版）王珊等》

### 1.1.6 SQL语句规范
参考《Java开发手册（阿里巴巴）-MySQL数据库- SQL语句》

（1）【强制】不要使用 count(列名)或 count(常量)来替代 count(_)，count(_)是 SQL92 定义的标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。说明：count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行。

（2）【强制】count(distinct col) 计算该列除 NULL 之外的不重复行数，注意 count(distinct col1, col2) 如果其中一列全为 NULL，那么即使另一列有不同的值，也返回为 0。

（3）【强制】当某一列的值全是 NULL 时，count(col)的返回结果为 0，但 sum(col)的返回结果为NULL，因此使用 sum()时需注意 NPE 问题。
正例：可以使用如下方式来避免 sum 的 NPE 问题：SELECT IFNULL(SUM(column), 0) FROM table;

（4）【强制】使用 ISNULL()来判断是否为 NULL 值。
说明：NULL 与任何值的直接比较都为 NULL。
1） NULL<>NULL 的返回结果是 NULL，而不是 false。
2） NULL=NULL 的返回结果是 NULL，而不是 true。
3） NULL<>1 的返回结果是 NULL，而不是 true。
反例：在 SQL 语句中，如果在 null 前换行，影响可读性。select * from table where column1 is null and column3 is not null; 而`ISNULL(column)`是一个整体，简洁易懂。从性能数据上分析，`ISNULL(column)`执行效率更快一些。

（5）【强制】代码中写分页查询逻辑时，若 count 为 0 应直接返回，避免执行后面的分页语句。

（6）【强制】不得使用外键与级联，一切外键概念必须在应用层解决。
说明：（概念解释）学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新，即为级联更新。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险；外键影响数据库的插入速度。

（7）【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

（8）【强制】数据订正（特别是删除或修改记录操作）时，要先 select，避免出现误删除，确认无误才能执行更新语句。

（9）【强制】对于数据库中表记录的查询和变更，只要涉及多个表，都需要在列名前加表的别名（或表名）进行限定。
说明：对多表进行查询记录、更新记录、删除记录时，如果对操作列没有限定表的别名（或表名），并且操作列在多个表中存在时，就会抛异常。
正例：select t1.name from table_first as t1 , table_second as t2 where t1.id=t2.id;
反例：在某业务中，由于多表关联查询语句没有加表的别名（或表名）的限制，正常运行两年后，最近在某个表中增加一个同名字段，在预发布环境做数据库变更后，线上查询语句出现出 1052 异常：Column 'name' in field list is ambiguous。

（10）【推荐】SQL 语句中表的别名前加 as，并且以 t1、t2、t3、...的顺序依次命名。
说明：1）别名可以是表的简称，或者是根据表出现的顺序，以 t1、t2、t3 的方式命名。2）别名前加 as 使别名更容易识别。
正例：select t1.name from table_first as t1, table_second as t2 where t1.id=t2.id;

（11）【推荐】in 操作能避免则避免，若实在避免不了，需要仔细评估 in 后边的集合元素数量，控制在 1000 个之内。

（12）【参考】因国际化需要，所有的字符存储与表示，均采用 utf8 字符集，那么字符计数方法需要注意。
说明：
SELECT LENGTH("轻松工作")； 返回为 12
SELECT CHARACTER_LENGTH("轻松工作")； 返回为 4
如果需要存储表情，那么选择 utf8mb4 来进行存储，注意它与 utf8 编码的区别。

（13）【参考】TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE无事务且不触发 trigger，有可能造成事故，故不建议在开发代码中使用此语句。
说明：TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同。


