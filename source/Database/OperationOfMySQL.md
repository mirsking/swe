# SQL基础操作

## SQL语句分类
* DDL(Data Defination Languages)数据定义语句
    用于定义不同的数据段、数据库、表、列、索引等数据库对象。如create/drop/alter
* DML(Data Manipulation Languages)数据操纵语句
    用于添加、删除、更新和查询数据库记录，并检查数据完整性，如insert/delete/update/select
* DCL(Data Control Language)数据控制语句
    用于控制不同数据段直接的许可和访问级别的语句，这些语句定义了数据库、表、字段、用户的访问权限和安全级别。如grant/revoke

## DDL
1. 数据库的新建、删除、选择、显示
  ```
  CREATE DATABASE dbname; # 新建数据库
  DROP DATABASE dbname; # 删除数据库
  USE dbname; # 选择数据库
  SHOW DATABASES; # 列出数据库
  ```

2. 表的新建、删除、修改
    * 新建表
        ```
        CREATE TABEL tablename(
        column_name_1 column_type_1 constraints,
        column_name_2 column_type_2 constraints,
        ...
        column_name_n column_type_n constraints);
        ```

    * 删除表
        ```
        DROP TABLE tablename
        ```

    * 修改表
        * 修改列
            ```
            ALTER TABLE tablename MODIFY [COLUMN] column_name column_defination [FIRST | AFTER col_name];
            ```
        * 增加列
            ```
            ALTER TABLE tablename ADD [COLUMN] column_name column_defination [FIRST | AFTER col_name];
            ```
        * 删除列
            ```
            ALTER TABLE tablename DROP [COLUMN] column_name;
            ```
        * 列改名
            ```
            ALTER TABLE tablename CHANGE [COLUMN] old_col_name new_col_name column_defination [FIRST | AFTER col_name];
            ```
        * 修改列排列顺序
            add/modify/change 中的[first | after col_name]可以修改列的顺序
        * 更改表名
            ```
            ALTER TABLE tablename RENAME [TO] new_tablename;
            ```

## DML
1. 插入记录
  ```
  INSERT INTO tablename (field1, field2, ..., fieldn)
  VALUES
  (record1_value1, record1_value2, ... , record1_valuen),
  (record2_value1, record2_value2, ... , record2_valuen),
  (record3_value1, record3_value2, ... , record3_valuen)
  ;
  ```

2. 更新记录
    * 更新单个表的数据
        ```
        UPDATE tablename SET field1 = value1, field2 = value2,... fieldn = value [WHERE CONDITITION];
        ```
    * 更新多个表的数据
        ```
        UPDATE t1,t2,...,tn SET t1.field1=expr1, ..., tn.fieldn=exprn [WHERE CONDITION];
        ```
3. 删除记录
    * 删除单个记录
        ```
        DELETE FROM tablename [WHERE CONDITION];
        ```
    * 删除多个记录
        ```
        DELETE t1, t2, ..., tn FROM t1, t2, ..., tn [WHERE CONDITION];
        ```
4. 查询记录
     ```
     SELECT * FROM tablename [WHERE CONDITION];
     ```
    1. 查询不重复的记录
        ```
        SELECT DISTINCT * FROM tablename [WHERE CONDITION];
        ```
    2. 条件查询
        * where中的条件可以使用```> < >= <= !=```等比较运算符；
        * 多个条件之间可以使用```or/and```进行条件联合
    3. 排序
        ```
        SELECT * FROM tablename [WHERE CONDITION] [ORDER BY field1 [DESC/ASC], field2 [DESC/ASC], ..., fieldn [DESC/ASC]];
        ```
    4. 限制
        ```
        SELECT ... [LIMIT offset_start, row_count];
        ```
    5. 聚合
        ```
        SELECT [field1, field2, ..., fieldn] fun_name
        FROM tablename
        [WHERE condition]
        [GROUP BY field1, field2, ..., fieldn]
        [WITH ROLLUP]
        [HAVING condition]
        ```
        * fun_name 表示要做的聚合操作，也就是聚合函数，常用的有sum/count/max/min
        * GROUP BY 表示要进行聚合的列
        * WITH ROLLUP表明是否对聚合后的结果进行再汇总
        * HAVING表示对分类后的结果再进行条件的过滤。 having和where的区别在于，having是对聚合后的结果进行条件过滤，where是在聚合前对记录进行过滤。如果逻辑允许，尽可能用where先过滤，这样结果集减少，将对聚合的效率大大提高，最后根据逻辑看是否用having进行再过滤。
    6. 表连接
        表连接包括左连接和右连接：
        * 左连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录。
        * 右连接：包含所有的右边表中的记录甚至是坐标表中没有和它匹配的记录。
        ```
        SELECT [field1, ..., fieldn] FROM dbname1 LEFT/RIGHT JOIN dbname2 ON WHERE CONDITION
        ```
    7. 子查询
        ```
        SELECT * FROM tablename WHERE field1 in/= (SELECT ...)
        ```
    8. 记录联合
        ```
        SELECT ...
        UNION/ UNION ALL
        SELECT ...
        ```

## DCL

## 帮助的使用
```
? contents
```
