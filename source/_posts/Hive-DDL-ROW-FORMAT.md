---
title: Hive DDL ROW FORMAT
categories: 大数据
tags: Hive
abbrlink: acf44460
date: 2020-06-17 13:38:22
---

### DDL语法规则

- CREATE DATABASE/SCHEMA, TABLE, VIEW, FUNCTION, INDEX（创建 数据库/模式，表，视图，函数，索引）
- DROP DATABASE/SCHEMA, TABLE, VIEW, INDEX（删除 数据库/模式，表，视图，索引）
- TRUNCATE TABLE（清空 表）
- ALTER DATABASE/SCHEMA, TABLE, VIEW（修改 数据库/模式，表，视图）
- MSCK REPAIR TABLE (or ALTER TABLE RECOVER PARTITIONS)（MSCK修复表或ALTER TABLE恢复分区）
- SHOW DATABASES/SCHEMAS, TABLES, TBLPROPERTIES, VIEWS, PARTITIONS, FUNCTIONS, INDEX[ES], COLUMNS, CREATE TABLE（查看）
- DESCRIBE DATABASE/SCHEMA, table_name, view_name（查看 数据库/模式描述信息）

创建表时需要指定数据切分格式，会用到ROW FORMAT关键字。可以参考Hive官网关于ROW FORMAT的用法。

下面通过一个例子说明数据之间分隔符用法。比如有两条数据：

```
1,Lilei,book-tv-code,beijing:chaoyang-shanghai:pudong
2,Hanmeimei,book-Lilei-code,beijing:haidian-shanghai:huangpu
```

先看JAVA集合框架图，明确每个字段数据类型，再看数据格式，指定分隔符。

### 分隔符类型

限定符开始语句----------ROW FORMAT DELIMITED

每个字段之间由[ , ]分割----------FIELDS TERMINATED BY ','

第二个字段是Array形式，元素与元素之间由[ - ]分割----------COLLECTION ITEMS TERMINATED BY '-'

第三个字段是K-V形式，每组K-V对内部由[ : ]分割，每组K-V对之间由[ - ]分割----------MAP KEYS TERMINATED BY ':'

每条数据之间由换行符分割（默认[ \n ]），如果是其它分割方式（比如[ ; ]）----------LINES TERMINATED BY ';'

### 完整建表语句

```sql
create table psn (
id INT,
name STRING,
hobbies ARRAY <STRING>,
address MAP <STRING, STRING>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '-'
MAP KEYS TERMINATED BY ':'; 
```

