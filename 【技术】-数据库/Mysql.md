# Mysql

## SQL语句结构

### 数据查询语句（Data Query Language）

select\where\order by\group by\having

### 数据操作语言（Data Manipulation Language）

Insert\update\delect

### 事务处理语言（TPL）

保证被DML语句影响的表的所有行能及时更新

Begin transaction\commit\rollback

### 数据控制语言 （DCL）

语句通过grant\revoke获得许可，确定当个用户和用户组对数据库对象的访问

### 数据定义语言（DDL）

Create/drop

### 指针控制语言（CCL）

Declear cursor /fetch into/update where current

## SQL数据类型

| **数据类型**                         | **描述**                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| CHARACTER(n)                         | 字符/字符串。固定长度 n。                                    |
| VARCHAR(n) 或   CHARACTER VARYING(n) | 字符/字符串。可变长度。最大长度 n。                          |
| BINARY(n)                            | 二进制串。固定长度 n。                                       |
| BOOLEAN                              | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或   BINARY VARYING(n)  | 二进制串。可变长度。最大长度 n。                             |
| INTEGER(p)                           | 整数值（没有小数点）。精度 p。                               |
| SMALLINT                             | 整数值（没有小数点）。精度 5。                               |
| INTEGER                              | 整数值（没有小数点）。精度 10。                              |
| BIGINT                               | 整数值（没有小数点）。精度 19。                              |
| DECIMAL(p,s)                         | 精确数值，精度 p，小数点后位数 s。例如：decimal(5,2) 是一个小数点前有 3 位数小数点后有 2 位数的数字。 |
| NUMERIC(p,s)                         | 精确数值，精度 p，小数点后位数 s。（与 DECIMAL 相同）        |
| FLOAT(p)                             | 近似数值，尾数精度 p。一个采用以 10 为基数的指数计数法的浮点数。该类型的 size 参数由一个指定最小精度的单一数字组成。 |
| REAL                                 | 近似数值，尾数精度 7。                                       |
| FLOAT                                | 近似数值，尾数精度 16。                                      |
| DOUBLE PRECISION                     | 近似数值，尾数精度 16。                                      |
| DATE                                 | 存储年、月、日的值。                                         |
| TIME                                 | 存储小时、分、秒的值。                                       |
| TIMESTAMP                            | 存储年、月、日、小时、分、秒的值。                           |
| INTERVAL                             | 由一些整数字段组成，代表一段时间，取决于区间的类型。         |
| ARRAY                                | 元素的固定长度的有序集合                                     |
| MULTISET                             | 元素的可变长度的无序集合                                     |
| XML                                  | 存储 XML 数据                                                |

### 字符型（VARCHARVS CHAR）

- VARCHARVS和CHAR用于存储字符串长度小于255的字符
- VARCHARVS会去除空白字符，而CHAR按照定义自动填充空白字符

### 文本型（TEXT）

​	可存放超过二十亿个字。尽量**避免**使用。一旦向文本型字段输入任何数据（甚至是NULL），就会有2K空间自动被分配给该数据。至于删除该数据才能释放空间

### 数值型（INT、NUMERIC、MONEY）

#### INT\SAMLLINT\TINYINT

- INT：4BYTE
- TINYINT：1BYTE

#### NUMERIC

-10^38 – 10^38

#### MONEY\SMALLMONEY

- SMALLMONEY: -214,748.3648-214,748.3647 
- MONEY: -922,337,203,685,477.5808-922,337,203,685,477.5807

### 逻辑型（BIT）

0/1

### 日期型

- DATETIME: 1753年1月1日第一毫秒到9999年12月31日最后一毫秒
- SMALLDATETIME: 从1900年1月1日到2079年6月6日的日期，它只能精确到秒

### SQL MS ACCESS\MYSQL\SQL SERVER数据类型

#### Microsoft Access 数据类型

| **数据类型**  | **描述**                                                     | **存储** |
| ------------- | ------------------------------------------------------------ | -------- |
| Text          | 用于文本或文本与数字的组合。最多 255 个字符。                |          |
| Memo          | Memo 用于更大数量的文本。最多存储 65,536 个字符。**注释：**无法对 memo 字段进行排序。不过它们是可搜索的。 |          |
| Byte          | 允许 0 到 255 的数字。                                       | 1 字节   |
| Integer       | 允许介于 -32,768 与 32,767 之间的全部数字。                  | 2 字节   |
| Long          | 允许介于 -2,147,483,648 与 2,147,483,647 之间的全部数字。    | 4 字节   |
| Single        | 单精度浮点。处理大多数小数。                                 | 4 字节   |
| Double        | 双精度浮点。处理大多数小数。                                 | 8 字节   |
| Currency      | 用于货币。支持 15 位的元，外加 4 位小数。**提示：**您可以选择使用哪个国家的货币。 | 8 字节   |
| AutoNumber    | AutoNumber 字段自动为每条记录分配数字，通常从 1 开始。       | 4 字节   |
| Date/Time     | 用于日期和时间                                               | 8 字节   |
| Yes/No        | 逻辑字段，可以显示为 Yes/No、True/False 或 On/Off。在代码中，使用常量 True 和 False （等价于 1 和 0）。**注释：**Yes/No 字段中不允许 Null 值 | 1 比特   |
| Ole Object    | 可以存储图片、音频、视频或其他 BLOBs（Binary Large OBjects）。 | 最多 1GB |
| Hyperlink     | 包含指向其他文件的链接，包括网页。                           |          |
| Lookup Wizard | 允许您创建一个可从下拉列表中进行选择的选项列表。             | 4 字节   |

####  MySQL 数据类型

​	在 MySQL 中，有三种主要的类型：Text（文本）、Number（数字）和 Date/Time（日期/时间）类型。

**Text 类型**：

| **数据类型**     | **描述**                                                     |
| ---------------- | ------------------------------------------------------------ |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。**注释：**如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs（Binary  Large OBjects）。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs（Binary  Large OBjects）。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs  (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许您输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。  **注释：**这些值是按照您输入的顺序排序的。  可以按照此格式输入可能的值： ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，不同的是，SET 最多只能包含 64 个列表项且 SET 可存储一个以上的选择。 |

**Number 类型**：

| **数据类型**    | **描述**                                                     |
| --------------- | ------------------------------------------------------------ |
| TINYINT(size)   | -128 到 127 常规。0 到 255 无符号*。在括号中规定最大位数。   |
| SMALLINT(size)  | -32768 到 32767 常规。0 到 65535 无符号*。在括号中规定最大位数。 |
| MEDIUMINT(size) | -8388608 到 8388607 普通。0 to 16777215 无符号*。在括号中规定最大位数。 |
| INT(size)       | -2147483648 到 2147483647 常规。0 到  4294967295 无符号*。在括号中规定最大位数。 |
| BIGINT(size)    | -9223372036854775808 到 9223372036854775807 常规。0 到  18446744073709551615 无符号*。在括号中规定最大位数。 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在 size 参数中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在 size 参数中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。在 size 参数中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |

*这些整数类型拥有额外的选项 UNSIGNED。通常，整数可以是负数或正数。如果添加 UNSIGNED 属性，那么范围将从 0 开始，而不是某个负数。

**Date 类型**：

| **数据类型** | **描述**                                                     |
| ------------ | ------------------------------------------------------------ |
| DATE()       | 日期。格式：YYYY-MM-DD  **注释：**支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()   | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS  **注释：**支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP()  | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的秒数来存储。格式：YYYY-MM-DD HH:MM:SS  **注释：**支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()       | 时间。格式：HH:MM:SS  **注释：**支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()       | 2 位或 4 位格式的年。  **注释：**4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

*即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。

### SQL Server 数据类型

**String 类型**：

| **数据类型**   | **描述**                                             | **存储**                  |
| -------------- | ---------------------------------------------------- | ------------------------- |
| char(n)        | 固定长度的字符串。最多 8,000 个字符。                | Defined width             |
| varchar(n)     | 可变长度的字符串。最多 8,000 个字符。                | 2 bytes + number of chars |
| varchar(max)   | 可变长度的字符串。最多 1,073,741,824 个字符。        | 2 bytes + number of chars |
| text           | 可变长度的字符串。最多 2GB 文本数据。                | 4 bytes + number of chars |
| nchar          | 固定长度的 Unicode 字符串。最多 4,000 个字符。       | Defined width x 2         |
| nvarchar       | 可变长度的 Unicode 字符串。最多 4,000 个字符。       |                           |
| nvarchar(max)  | 可变长度的 Unicode 字符串。最多 536,870,912 个字符。 |                           |
| ntext          | 可变长度的 Unicode 字符串。最多 2GB 文本数据。       |                           |
| bit            | 允许 0、1 或 NULL                                    |                           |
| binary(n)      | 固定长度的二进制字符串。最多 8,000 字节。            |                           |
| varbinary      | 可变长度的二进制字符串。最多 8,000 字节。            |                           |
| varbinary(max) | 可变长度的二进制字符串。最多 2GB。                   |                           |
| image          | 可变长度的二进制字符串。最多 2GB。                   |                           |

**Number 类型**：

| **数据类型** | **描述**                                                     | **存储**    |
| ------------ | ------------------------------------------------------------ | :---------- |
| tinyint      | 允许从 0 到 255 的所有数字。                                 | 1 字节      |
| smallint     | 允许介于 -32,768 与 32,767 的所有数字。                      | 2 字节      |
| int          | 允许介于 -2,147,483,648 与 2,147,483,647 的所有数字。        | 4 字节      |
| bigint       | 允许介于 -9,223,372,036,854,775,808 与 9,223,372,036,854,775,807 之间的所有数字。 | 8 字节      |
| decimal(p,s) | 固定精度和比例的数字。  允许从 -10^38 +1 到 10^38 -1 之间的数字。  p 参数指示可以存储的最大位数（小数点左侧和右侧）。p 必须是 1 到 38 之间的值。默认是 18。  s 参数指示小数点右侧存储的最大位数。s 必须是 0 到 p 之间的值。默认是 0。 | 5-17 字节   |
| numeric(p,s) | 固定精度和比例的数字。  允许从 -10^38 +1 到 10^38 -1 之间的数字。  p 参数指示可以存储的最大位数（小数点左侧和右侧）。p 必须是 1 到 38 之间的值。默认是 18。  s 参数指示小数点右侧存储的最大位数。s 必须是 0 到 p 之间的值。默认是 0。 | 5-17 字节   |
| smallmoney   | 介于  -214,748.3648 与  214,748.3647 之间的货币数据。        | 4 字节      |
| money        | 介于  -922,337,203,685,477.5808 与 922,337,203,685,477.5807 之间的货币数据。 | 8 字节      |
| float(n)     | 从 -1.79E  + 308 到 1.79E +  308 的浮动精度数字数据。  n 参数指示该字段保存 4 字节还是 8 字节。float(24) 保存 4 字节，而 float(53) 保存 8 字节。n 的默认值是 53。 | 4 或 8 字节 |
| real         | 从 -3.40E  + 38 到 3.40E +  38 的浮动精度数字数据。          | 4 字节      |

**Date 类型**：

| **数据类型**   | **描述**                                                     | **存储**  |
| -------------- | ------------------------------------------------------------ | --------- |
| datetime       | 从 1753 年 1 月 1 日 到 9999 年 12 月 31 日，精度为 3.33 毫秒。 | 8 字节    |
| datetime2      | 从 1753 年 1 月 1 日 到 9999 年 12 月 31 日，精度为 100 纳秒。 | 6-8 字节  |
| smalldatetime  | 从 1900 年 1 月 1 日 到 2079 年 6 月 6 日，精度为 1 分钟。   | 4 字节    |
| date           | 仅存储日期。从 0001 年 1 月 1 日 到 9999 年 12 月 31 日。    | 3 bytes   |
| time           | 仅存储时间。精度为 100 纳秒。                                | 3-5 字节  |
| datetimeoffset | 与  datetime2 相同，外加时区偏移。                           | 8-10 字节 |
| timestamp      | 存储唯一的数字，每当创建或修改某行时，该数字会更新。timestamp 值基于内部时钟，不对应真实时间。每个表只能有一个 timestamp 变量。 |           |

其他数据类型：

| **数据类型**     | **描述**                                                     |
| ---------------- | ------------------------------------------------------------ |
| sql_variant      | 存储最多 8,000 字节不同数据类型的数据，除了 text、ntext 以及 timestamp。 |
| uniqueidentifier | 存储全局唯一标识符 (GUID)。                                  |
| xml              | 存储 XML 格式化数据。最多 2GB。                              |
| cursor           | 存储对用于数据库操作的指针的引用。                           |
| table            | 存储结果集，供稍后处理。                                     |

## 一些重要的SQL语句

| **SQL** **语句** | **语法**                                                     |
| ---------------- | ------------------------------------------------------------ |
| AND / OR         | SELECT column_name(s)   FROM table_name   WHERE condition   AND\|OR condition |
| ALTER TABLE      | ALTER TABLE table_name    ADD column_name datatype  or  ALTER TABLE table_name    DROP COLUMN column_name |
| AS (alias)       | SELECT column_name AS column_alias   FROM table_name  or  SELECT column_name   FROM table_name AS table_alias |
| BETWEEN          | SELECT column_name(s)   FROM table_name   WHERE column_name   BETWEEN value1 AND value2 |
| CREATE DATABASE  | CREATE DATABASE database_name                                |
| CREATE TABLE     | CREATE TABLE table_name   (   column_name1 data_type,   column_name2 data_type,   column_name2 data_type,   ...   ) |
| CREATE INDEX     | CREATE INDEX index_name   ON table_name (column_name)  or  CREATE UNIQUE INDEX index_name   ON table_name (column_name) |
| CREATE VIEW      | CREATE VIEW view_name AS   SELECT column_name(s)   FROM table_name   WHERE condition |
| DELETE           | DELETE FROM table_name   WHERE some_column=some_value  or  DELETE FROM table_name    (**Note:** Deletes the entire table!!)  DELETE * FROM table_name    (**Note:** Deletes the entire table!!) |
| DROP DATABASE    | DROP DATABASE database_name                                  |
| DROP INDEX       | DROP INDEX table_name.index_name (SQL Server)   DROP INDEX index_name ON table_name (MS Access)   DROP INDEX index_name (DB2/Oracle)   ALTER TABLE table_name   DROP INDEX index_name (MySQL) |
| DROP TABLE       | DROP TABLE table_name                                        |
| GROUP BY         | SELECT column_name, aggregate_function(column_name)   FROM table_name   WHERE column_name operator value   GROUP BY column_name |
| HAVING           | SELECT column_name, aggregate_function(column_name)   FROM table_name   WHERE column_name operator value   GROUP BY column_name   HAVING aggregate_function(column_name) operator value |
| IN               | SELECT column_name(s)   FROM table_name   WHERE column_name   IN (value1,value2,..) |
| INSERT INTO      | INSERT INTO table_name   VALUES (value1, value2, value3,....)  *or*  INSERT INTO table_name   (column1, column2, column3,...)   VALUES (value1, value2, value3,....) |
| INNER JOIN       | SELECT column_name(s)   FROM table_name1   INNER JOIN table_name2    ON table_name1.column_name=table_name2.column_name |
| LEFT JOIN        | SELECT column_name(s)   FROM table_name1   LEFT JOIN table_name2    ON table_name1.column_name=table_name2.column_name |
| RIGHT JOIN       | SELECT column_name(s)   FROM table_name1   RIGHT JOIN table_name2    ON table_name1.column_name=table_name2.column_name |
| FULL JOIN        | SELECT column_name(s)   FROM table_name1   FULL JOIN table_name2    ON table_name1.column_name=table_name2.column_name |
| LIKE             | SELECT column_name(s)   FROM table_name   WHERE column_nameLIKE pattern |
| ORDER BY         | SELECT column_name(s)   FROM table_name   ORDER BY column_name [ASC\|DESC] |
| SELECT           | SELECT column_name(s)   FROM table_name                      |
| SELECT *         | SELECT *   FROM table_name                                   |
| SELECT DISTINCT  | SELECT DISTINCT column_name(s)   FROM table_name             |
| SELECT INTO      | SELECT *   INTO new_table_name [IN externaldatabase]   FROM old_table_name  *or*  SELECT column_name(s)   INTO new_table_name [IN externaldatabase]   FROM old_table_name |
| SELECT TOP       | SELECT TOP number\|percent column_name(s)   FROM table_name  |
| TRUNCATE TABLE   | TRUNCATE TABLE table_name                                    |
| UNION            | SELECT column_name(s) FROM table_name1   UNION   SELECT column_name(s) FROM table_name2 |
| UNION ALL        | SELECT column_name(s) FROM table_name1   UNION ALL   SELECT column_name(s) FROM table_name2 |
| UPDATE           | UPDATE table_name   SET column1=value, column2=value,...   WHERE some_column=some_value |
| WHERE            | SELECT column_name(s)   FROM table_name   WHERE column_name operator value |

- SELECT - 从数据库中提取数据
- UPDATE - 更新数据库中的数据
- DELETE - 从数据库中删除数据
- INSERT INTO - 向数据库中插入新数据
- CREATE DATABASE - 创建新数据库
- ALTER DATABASE - 修改数据库
- CREATE TABLE - 创建新表
- ALTER TABLE - 变更（改变）数据库表
- DROP TABLE - 删除表
- CREATE INDEX - 创建索引（搜索键）
- DROP INDEX - 删除索引

### SELECT DISTINCT

### INSERT INTO

- INSERT INTO TABLE_NAME VALUES(,,,)—全表插入
- INSERT INTO TABLE_NAME(,,,) VALUES(,,,)—指定插入

### 检测NULL

IS NULL/IS NOT NULL

### SELECT TOP

- MY SQL-LIMIT
- ORACLE-ROWNUM

- Select top number column_name from table_name where condition;
- Select column_name from table_name where condition limit number;
- Select column_name from table_name where condition and rownum <=number;

### SQL LIKE

- %-零个或多个字符
- _-单个字符
- [charlist]-定义要匹配的字符的集合和范围
- [^charlist]或[!charlist]-定义不匹配字符的字符的集合和范围

Select * from table_name where column_name like ‘[!a-c]’;

### JOIN

1. INNER JOIN/JOIN
2. LEFT JOIN
3. RIGHT JOIN
4. FULL JOIN

### UNION

相似数据结构组合，如果允许重复使用UNION ALL

### SELECT INTO

Select into复制一个表的数据插入到新表中

```mysql
Select * 
Into new_table_name [in database]
From table_name;
```

若只想复制结构，不复制值，使用

```mysql
Select * 
Into newtable
From table_name
Where 1=0;
```

### INSERT INTO SELECT

```mysql
Insert into newtable(column)
Select column from table
Where condition;
```

### ALTER TABLE

```mysql
ALTER TABLE TABLE_NAME ADD COLUMN_NAME DATATYPE;
ALTER TBALE TABLE_NAME DROP COLUMN COLUMN_NAME;
ALTER TABLE TBALE_NAME
ALTER /MODIFY COLUMN COLUMN_NAME DATATYPE;
```

### AUTO INCREMENT

### VIEW

```mysql
CREATE VIEW view_name AS SELECT * ;
DROP VIEW viewname;
WITH CHECK OPTION-检查条件
CREATE VIEW CUSTOMERS_VIEW AS
SELECT name, age
FRM CUSTOMER
WHERE age IS NOT
WITH CHECK OPTION;
```

- SELECT 子句不能包含 DISTINCT 关键字
- SELECT 子句不能包含任何汇总函数（summary functions）
- SELECT 子句不能包含任何集合函数（set functions）
- SELECT 子句不能包含任何集合运算符（set operators）
- SELECT 子句不能包含 ORDER BY 子句
- FROM 子句中不能有多个数据表
- WHERE 子句不能包含子查询（subquery）
- 查询语句中不能有 GROUP BY 或者 HAVING
- 计算得出的列不能更新
- 视图必须包含原始数据表中所有的 NOT NULL 列，从而使 INSERT 查询生效。

### Date

| **函数**                                                     | **描述**                            |
| ------------------------------------------------------------ | ----------------------------------- |
| [NOW()](https://www.w3cschool.cn/mysql/func-now.html)        | 返回当前的日期和时间                |
| [CURDATE()](https://www.w3cschool.cn/mysql/func-curdate.html) | 返回当前的日期                      |
| [CURTIME()](https://www.w3cschool.cn/mysql/func-curtime.html) | 返回当前的时间                      |
| [DATE()](https://www.w3cschool.cn/mysql/func-date.html)      | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.w3cschool.cn/mysql/func-extract.html) | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.w3cschool.cn/mysql/func-date-add.html) | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.w3cschool.cn/mysql/func-date-sub.html) | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.w3cschool.cn/mysql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.w3cschool.cn/mysql/func-date-format.html) | 用不同的格式显示日期/时间           |

### SQL约束

- NOT NULL 约束：保证列中数据不能有 NULL 值
- DEFAULT 约束：提供该列数据未指定时所采用的默认值
- UNIQUE 约束：保证列中的所有数据各不相同
- 主键约束：唯一标识数据表中的行/记录外键约束：唯一标识其他表中的一条行/记录
- CHECK 约束：此约束保证列中的所有值满足某一条件
- 索引：用于在数据库中快速创建或检索数据
- 删除约束：ALTER TABLE EMPLOYEES DROP CONSTRAINT EMPLOYEES_PK;
- SQL NOT NULL约束：P-id int NOT NULL;
- SQL UNIQUE：UNIQUE(P-id);
- SQL PRIMARY KEY约束：唯一标识。唯一，不能包含NULL。有且仅有一个
- SQL FOREIGN KEY约束：一个表的forei key指向另一个表的primary key
- SQL DEFAULT约束
- SQL CHECK约束：CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')；

### 撤销约束

ALTER TABLE Persons

DROP CHECK chk_Person

### SQL INTERSECT

返回重合部分

### SQL EXPECT

### SQL克隆表格

- SHOW CREATE TABLE table_name
- CREATE TABLE clone_name() ;
- INSERT INTO clone_name() SELECT * FROM table_name;

### SQL INDEX

CREATE (UNIQUE) INDEX index_name on table_name(column_name);

何时避免使用索引：

- 小的数据表不应当使用索引；
- 需要频繁进行大批量的更新或者插入操作的表；
- 如果列中包含大数或者 NULL 值，不宜创建索引；
- 频繁操作的列不宜创建索引。

### SQL子查询

- 子查询必须括在圆括号中。
- 子查询的 SELECT 子句中只能有一个列，除非主查询中有多个列，用于与子查询选中的列相比较。
- 子查询不能使用 ORDER BY，不过主查询可以。在子查询中，GROUP BY 可以起到同 ORDER BY 相同的作用。
- 返回多行数据的子查询只能同多值操作符一起使用，比如 IN 操作符。
- SELECT 列表中不能包含任何对 BLOB、ARRAY、CLOB 或者 NCLOB 类型值的引用。
- 子查询不能直接用在集合函数中。
- BETWEEN 操作符不能同子查询一起使用，但是 BETWEEN 操作符可以用在子查询中。

### SQL注入

- 模式匹配用户输入
- 转义那些对数据库有意义的特殊字符
- 要破解 LIKE 困境，必须有一种专门的转义机制，将用户提供的 '%' 和 '_' 转换为字面值

### SQL HAVING子句

Having子句指定顾虑条件，对GROUP BY子句施加约束；必须紧跟GROUP BY子句，在ORDER BY之前。

```mysql
SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2
```

## SQL事务

事务是任务序列，可以用户手工执行，也可由数据库程序自行执行。

### 属性

ACID-Atomicity\Consistency\Isolatin\Durability

- 原子性：保证任务所有操作都执行完毕，否则全部回滚
- 一致性：事务成功执行，数据库的状态得到正确的转变
- 隔离性：不用事务相互独立、透明
- 持久性：系统出现问题，之前执行成功的事务也会保留

### 事务控制

- COMMIT
- ROLLBACK
- SAVEPOINT

### 创建还原点

```mysql
SAVEPOINT SAVEPOINT_NAME;
ROLLBACK TO SAVEPOINT_NAME;
RELEASE SAVEPOINT;
#### SET TRANSACTION
命名事务，可以初始化事务，并制定事务的属性
SET TRANSACTION [ READ WRITE | READ ONLY ];
```

### 创建临时表

CREATE TEMPORARY TABLE;

## SQL函数

### SQL AGGREGATE函数

SQL MAX()/MIN()/COUNT()/AVG()/SUM()/FIRST()/LAST()

### SQL SCALAR函数

- UCASE() - 将某个字段转换为大写
- LCASE() - 将某个字段转换为小写
- MID() - 从某个文本字段提取字符
- LEN() - 返回某个文本字段的长度
- ROUND() - 对某个数值字段进行指定小数位数的四舍五入
- NOW() - 返回当前的系统日期和时间
- FORMAT() - 格式化某个字段的显示方式

### SQL日期函数

| 名称                                   | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| ADDDATE()                              | 增加日期                                                     |
| ADDTIME()                              | 增加时间                                                     |
| CONVERT_TZ()                           | 将当前时区更改为另一时区                                     |
| CURDATE()                              | 返回当前日期                                                 |
| CURRENT_DATE(), CURRENT_DATE           | CURDATE() 的别名                                             |
| CURRENT_TIME(), CURRENT_TIME           | CURTIME() 的别名                                             |
| CURRENT_TIMESTAMP(), CURRENT_TIMESTAMP | NOW() 的别名                                                 |
| CURTIME()                              | 返回当前时间                                                 |
| DATE_ADD()                             | 将两个日期相加                                               |
| DATE_FORMAT()                          | 按照指定格式格式化日期                                       |
| DATE_SUB()                             | 将两个日期相减                                               |
| DATE()                                 | 从 date 或者 datetime 表达式中提取出日期部分                 |
| DATEDIFF()                             | 将两个日期相减                                               |
| DAY()                                  | DAYOFMONTH() 的别名                                          |
| DAYNAME()                              | 返回某天在用星期中的名称                                     |
| DAYOFMONTH()                           | 返回某天是当月的第几天 （1-31）                              |
| DAYOFWEEK()                            | 返回某天是该星期的第几天                                     |
| DAYOFYEAR()                            | 返回某天是一年中的第几天（1-366）                            |
| EXTRACT                                | 提取日期中的某一部分                                         |
| FROM_DAYS()                            | 将天数转换为日期                                             |
| FROM_UNIXTIME()                        | 将某个日期格式化为 UNIX 时间戳                               |
| HOUR()                                 | 提取小时                                                     |
| LAST_DAY                               | 返回参数日期所在月份的最后一天                               |
| LOCALTIME(), LOCALTIME                 | NOW() 的别名                                                 |
| LOCALTIMESTAMP, LOCALTIMESTAMP()       | NOW() 的别名                                                 |
| MAKEDATE()                             | 利用年份和某天在该年所处的天数来创建日期                     |
| MAKETIME                               | MAKETIME()                                                   |
| MICROSECOND()                          | 由参数返回微秒                                               |
| MINUTE()                               | 由参数返回分钟                                               |
| MONTH()                                | 返回日期参数的月份                                           |
| MONTHNAME()                            | 返回月份的名字                                               |
| NOW()                                  | 返回当前日期和时间                                           |
| PERIOD_ADD()                           | 向年月格式的日期数据之间添加一段时间                         |
| PERIOD_DIFF()                          | 返回两个年月格式的日期数据之间的月份数                       |
| QUARTER()                              | 返回日期参数所在的季度                                       |
| SEC_TO_TIME()                          | 将秒数转换为 'HH:MM:SS' 格式                                 |
| SECOND()                               | 返回参数中的秒数 (0-59)                                      |
| STR_TO_DATE()                          | 将字符串转换为日期数据                                       |
| SUBDATE()                              | 以三个参数调用的时候是 DATE_SUB() 的同义词                   |
| SUBTIME()                              | 减去时间                                                     |
| SYSDATE()                              | 返回函数执行的时的时刻                                       |
| TIME_FORMAT()                          | 格式化时间                                                   |
| TIME_TO_SEC()                          | 将时间参数转换为秒数                                         |
| TIME()                                 | 返回参数表达式中的时间部分                                   |
| TIMEDIFF()                             | 将两个时间相减                                               |
| TIMESTAMP()                            | 只有一个参数时，该函数返回 date 或者 datetime 表达式。当有两个参数时，将两个参数相加。 |
| TIMESTAMPADD()                         | 在 datetime 表达式上加上一段时间                             |
| TIMESTAMPDIFF()                        | 在 datetime 表达式上减去一段时间                             |
| TO_DAYS()                              | 将日期参数转换为天数                                         |
| UNIX_TIMESTAMP()                       | 返回 UNIX 时间戳                                             |
| UTC_DATE()                             | 返回当前 UTC 日期                                            |
| UTC_TIME()                             | 返回当前 UTC 时间                                            |
| UTC_TIMESTAMP()                        | 返回当前 UTC 日期和时间                                      |
| WEEK()                                 | 返回参数的星期数                                             |
| WEEKDAY()                              | 返回日期参数时一个星期中的第几天                             |
| WEEKOFYEAR()                           | 返回日期参数是日历上的第几周 (1-53)                          |
| YEAR()                                 | 返回日期参数中的年份                                         |
| YEARWEEK()                             | 返回年份和星期                                               |

### SQL FIELD()

SELECT FIELD(STR,STR1,STR2,…);

在STR后面的STR1等寻找STR,找到返回索引，从1开始，若没找到返回0；

### SQL SQRT()

求方根

### SQL RAND()

生成0-1随机数

### SQL CONTACT()

将两个字符串连接成一个字符串

### SQL ISNULL()\NVL()\IFNULL()\COALESCE()

### SQL REPLACE()

replace(original-string，search-string，replace-string)

### SQL TRIM()

- MySQL: TRIM( ), RTRIM( ), LTRIM( )
- Oracle: RTRIM( ), LTRIM( )
- SQL Server: RTRIM( ), LTRIM( )

TRIM ( [ [位置] [要移除的字串] FROM ] 字串): [位置] 的可能值为 LEADING (起头), TRAILING (结尾), or BOTH (起头及结尾)。 这个函数将把 [要移除的字串] 从字串的起头、结尾，或是起头及结尾移除。如果我们没有列出 [要移除的字串] 是什么的话，那空白就会被移除。

- LTRIM(字串): 将所有字串起头的空白移除。
- RTRIM(字串): 将所有字串结尾的空白移除。

## SQL反模式—BILL Karwin

### 解决树形结构的查询和存储

#### 1.6.1.1 路径枚举

定义一个path varchar(1000)用来保存路径如/1/2/4/7

- 获取子节点：Select *from comment as c where c.path like ‘1/2/4’ || ‘%’
- 获得祖先路径：Select * from comment as c where ‘1/2/4/7’ like c.path || ‘%’

#### 嵌套集

#### 闭包表

### MySql存储机制

- MyISAM:对事务支持不好
- InnoDB：提供事务安全的存储

如果需要支持，需要在建表作业后加上ENGINE=MyISAM/InnoDB

### 常用指令

- show databses;
- create database [if not exits] databasename;
- use databasename;
- show tables;
- desc tablename;