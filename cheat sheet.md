# SQL Cheat Sheet

# 模版一

```sql
SELECT XXX
FROM (
    SELECT XXXX
    FROM XXXX
    GROUP BY XXXX
) AS 
JOIN XXX ON XXX
WHERE XXX
AND XXX = (
    SELECT XXXX
    FROM XXXX
    JOIN XXXX ON XXXX
    WHERE XXXX AND XXXX AND XXXX
)
GROUP BY XXX
HAVING XXX
ORDER BY XXX
```

# 模板二

```sql

SELECT
CASE WHEN XXX THEN XXX
ELSE XXX
END AS XXX

FROM XXX
JOIN XXX
ON XXX AND XXX
ORDER BY XXX;
```

# 1. **基础查询语句 (Basic Queries)**

- **SELECT**: 选择数据。
    
    ```sql
    SELECT column1, column2 FROM table_name;
    ```
    
- **SELECT DISTINCT**: 选择唯一不同的数据。
    
    ```sql
    SELECT DISTINCT column1 FROM table_name;
    ```
    
- **WHERE**: 数据过滤条件。
    
    ```sql
    SELECT column1 FROM table_name WHERE condition;
    ```
    
- **AND, OR, NOT**: 多重过滤条件。
    
    ```sql
    SELECT column1 FROM table_name WHERE condition1 AND condition2;
    SELECT column1 FROM table_name WHERE condition1 OR condition2;
    SELECT column1 FROM table_name WHERE NOT condition;
    ```
    

# 2. **排序和聚合 (Sorting & Aggregations)**

- **ORDER BY**: 对结果进行排序。
    
    ```sql
    SELECT column1 FROM table_name ORDER BY column1 ASC|DESC;
    ```
    
- **GROUP BY**: 按照某一列的值分组数据。
    
    ```sql
    SELECT column1, COUNT(column2) FROM table_name GROUP BY column1;
    ```
    
- **HAVING**: 对聚合后的结果进行过滤。
    
    ```sql
    SELECT column1, COUNT(column2) FROM table_name GROUP BY column1 HAVING COUNT(column2) > 10;
    ```
    

# 3. **函数 (Functions)**

## 1. **聚合函数 (Aggregate Functions)**

聚合函数对一系列值进行计算，并返回单一的值。

- **`COUNT()`**: 计算行数。
    
    ```sql
    SELECT COUNT(column_name) FROM table_name;
    
    ```
    
- **`SUM()`**: 计算数值总和。
    
    ```sql
    SELECT SUM(column_name) FROM table_name;
    
    ```
    
- **`AVG()`**: 计算数值的平均值。
    
    ```sql
    SELECT AVG(column_name) FROM table_name;
    
    ```
    
- **`MAX()`**: 返回一组值中的最大值。
    
    ```sql
    SELECT MAX(column_name) FROM table_name;
    
    ```
    
- **`MIN()`**: 返回一组值中的最小值。
    
    ```sql
    SELECT MIN(column_name) FROM table_name;
    
    ```
    
- **`STDDEV()`**: 计算标准偏差。
    
    ```sql
    SELECT STDDEV(column_name) FROM table_name;
    
    ```
    
- **`VAR_POP()`**: 计算总体方差。
    
    ```sql
    SELECT VAR_POP(column_name) FROM table_name;
    
    ```
    

## 2. **数学函数 (Mathematical Functions)**

用于执行数值计算的函数。

- **`ABS()`**: 返回数的绝对值。
    
    ```sql
    SELECT ABS(column_name) FROM table_name;
    
    ```
    
- **`CEIL()` 或 `CEILING()`**: 向上取整到最近的整数。
    
    ```sql
    SELECT CEIL(column_name) FROM table_name;
    
    ```
    
- **`FLOOR()`**: 向下取整到最近的整数。
    
    ```sql
    SELECT FLOOR(column_name) FROM table_name;
    
    ```
    
- **`ROUND()`**: 四舍五入到指定的小数位。
    
    ```sql
    SELECT ROUND(column_name, decimals) FROM table_name;
    
    ```
    
- **`SQRT()`**: 返回数的平方根。
    
    ```sql
    SELECT SQRT(column_name) FROM table_name;
    
    ```
    
- **`EXP()`**: 计算 e（自然对数的底数）的指数。
    
    ```sql
    SELECT EXP(column_name) FROM table_name;
    
    ```
    
- **`LOG()`**: 计算数的自然对数。
    
    ```sql
    SELECT LOG(column_name) FROM table_name;
    
    ```
    
- **`POWER()`**: 返回数的指定次幂。
    
    ```sql
    SELECT POWER(column_name, power) FROM table_name;
    
    ```
    
- **`MOD()`**: 返回除法的余数。
    
    ```sql
    SELECT MOD(column_name, divisor) FROM table_name;
    
    ```
    
- **`RAND()`**: 生成一个随机数。
    
    ```sql
    SELECT RAND() FROM table_name;
    
    ```
    

## 3. **字符串函数 (String Functions)**

处理和操作字符串数据的函数。

- **`CONCAT()`**: 连接两个或多个字符串。
    
    ```sql
    SELECT CONCAT(column1, column2) FROM table_name;
    
    ```
    
- **`UPPER()`**: 将字符串转换为大写。
    
    ```sql
    SELECT UPPER(column_name) FROM table_name;
    
    ```
    
- **`LOWER()`**: 将字符串转换为小写。
    
    ```sql
    SELECT LOWER(column_name) FROM table_name;
    
    ```
    
- **`LENGTH()`**: 返回字符串的长度。
    
    ```sql
    SELECT LENGTH(column_name) FROM table_name;
    
    ```
    
- **`SUBSTRING()`**: 从字符串中提取子串。
    
    ```sql
    SELECT SUBSTRING(column_name FROM start FOR length) FROM table_name;
    
    ```
    
- **`REPLACE()`**: 替换字符串中的某部分。
    
    ```sql
    SELECT REPLACE(column_name, 'old_string', 'new_string') FROM table_name;
    
    ```
    
- **`TRIM()`**: 去除字符串两端的空格或指定字符。
    
    ```sql
    SELECT TRIM(column_name) FROM table_name;
    
    ```
    
- **`LTRIM()`**: 去除字符串左端的空格或指定字符。
    
    ```sql
    SELECT LTRIM(column_name) FROM table_name;
    
    ```
    
- **`RTRIM()`**: 去除字符串右端的空格或指定字符。
    
    ```sql
    SELECT RTRIM(column_name) FROM table_name;
    
    ```
    

## 4. **日期和时间函数 (Date and Time Functions)**

操作和格式化日期和时间值的函数。

- **`NOW()`**: 返回当前日期和时间。
    
    ```sql
    SELECT NOW() FROM table_name;
    
    ```
    
- **`CURDATE()`**: 返回当前日期。
    
    ```sql
    SELECT CURDATE() FROM table_name;
    
    ```
    
- **`CURTIME()`**: 返回当前时间。
    
    ```sql
    SELECT CURTIME() FROM table_name;
    
    ```
    
- **`DATE()`**: 提取日期部分。
    
    ```sql
    SELECT DATE(column_name) FROM table_name;
    
    ```
    
- **`MONTH()`**: 提取月份。
    
    ```sql
    SELECT MONTH(column_name) FROM table_name;
    
    ```
    
- **`YEAR()`**: 提取年份。
    
    ```sql
    SELECT YEAR(column_name) FROM table_name;
    
    ```
    
- **`DAY()` 或 `DAYOFMONTH()`**: 提取月份中的日。
    
    ```sql
    SELECT DAY(column_name) FROM table_name;
    
    ```
    
- **`HOUR()`**: 提取小时。
    
    ```sql
    SELECT HOUR(column_name) FROM table_name;
    
    ```
    
- **`MINUTE()`**: 提取分钟。
    
    ```sql
    SELECT MINUTE(column_name) FROM table_name;
    
    ```
    
- **`SECOND()`**: 提取秒。
    
    ```sql
    SELECT SECOND(column_name) FROM table_name;
    
    ```
    
- **`DATEDIFF()`**: 计算两个日期之间的差异（天数）。
    
    ```sql
    SELECT DATEDIFF(date1, date2) FROM table_name;
    
    ```
    

## 5. **转换函数 (Conversion Functions)**

这些函数用于数据类型之间的转换。

- **`CAST()`**: 显式地将一个表达式转换为某种数据类型。
    
    ```sql
    SELECT CAST(column_name AS data_type) FROM table_name;
    
    ```
    
- **`CONVERT()`**: 在某些数据库中，用于类型转换。
    
    ```sql
    SELECT CONVERT(data_type, column_name) FROM table_name;
    
    ```
    

## 6. **条件表达式 (Conditional Expressions)**

这些表达式用于在查询中执行逻辑判断。

- **`COALESCE()`**: 返回参数列表中第一个非 NULL 值。
    
    ```sql
    SELECT COALESCE(column1, column2, ...) FROM table_name;
    ```
    
- **`NULLIF()`**: 如果两个表达式相等，则返回 NULL；否则返回第一个表达式的值。
    
    ```sql
    SELECT NULLIF(expression1, expression2) FROM table_name;
    ```
    
- **`CASE`**: 在 SQL 语句中提供 IF-THEN-ELSE 功能。
    
    ```sql
    SELECT CASE
           WHEN condition THEN result1
           WHEN condition2 THEN result2
           ELSE result3
           END
    FROM table_name;
    ```
    

## 7. **加密和哈希函数 (Cryptographic and Hashing Functions)**

在处理数据安全和完整性时使用。

- **`MD5()`**: 返回字符串的 MD5 哈希值。
    
    ```sql
    SELECT MD5(string) FROM table_name;
    
    ```
    
- **`SHA1()`, `SHA2()`**: 生成字符串的 SHA-1 或 SHA-2 哈希。
    
    ```sql
    SELECT SHA1(string) FROM table_name;
    SELECT SHA2(string, bit_length) FROM table_name;
    
    ```
    

## 8. **窗口函数 (Window Functions)**

用于执行复杂的数据分析和计算，这些函数关联于 SQL 标准的扩展部分。

- **`ROW_NUMBER()`**: 分配唯一的行号。
    
    ```sql
    SELECT ROW_NUMBER() OVER (ORDER BY column_name) FROM table_name;
    ```
    
- **`RANK()`**: 对记录进行排名。
    
    ```sql
    SELECT RANK() OVER (ORDER BY column_name) FROM table_name;
    ```
    
- **`DENSE_RANK()`**: 类似 `RANK()`，但排名连续。
    
    ```sql
    SELECT DENSE_RANK() OVER (ORDER BY column_name) FROM table_name;
    ```
    
- **`LEAD()`**, **`LAG()`**: 访问结果集中当前行的前一行或后一行。
    
    ```sql
    SELECT LEAD(column_name) OVER (ORDER BY column_name), LAG(column_name) OVER (ORDER BY column_name) FROM table_name;
    ```
    

# 4. **联接 (Joins)**

- **INNER JOIN**: 只返回两个表中匹配的记录。
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1 INNER JOIN table2 ON table1.common_field = table2.common_field;
    ```
    
- **LEFT JOIN**: 返回左表所有记录，即使右表中没有匹配。
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1 LEFT JOIN table2 ON table1.common_field = table2.common_field;
    ```
    
- **RIGHT JOIN**: 返回右表所有记录，即使左表中没有匹配。
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1 RIGHT JOIN table2 ON table1.common_field = table2.common_field;
    ```
    
- **FULL OUTER JOIN**: 返回两个表中所有匹配的记录。
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1 FULL OUTER JOIN table2 ON table1.common_field = table2.common_field;
    ```
    

# 5. **高级操作 (Advanced Operations)**

- **子查询 (Subqueries)**:
    
    ```sql
    SELECT column1 FROM table_name WHERE column2 IN (SELECT column2 FROM table_name2);
    ```
    
- **WITH Clause (Common Table Expressions)**:
    
    ```sql
    WITH subquery_name AS (SELECT column1 FROM table_name) SELECT * FROM subquery_name;
    ```
    

大多数情况下，子查询都可以被CTE代替。所以在练习时可以尽可能多的使用CTE。

- **结果集合并 (Set Operations)**:
    - **UNION**:
        - 合并两个或多个查询的结果集，并移除重复行。
        
        ```sql
        SELECT column_name FROM table1
        UNION
        SELECT column_name FROM table2;
        ```
        
    - **UNION ALL**:
        - 合并两个或多个查询的结果集，包括所有重复行。
        
        ```sql
        SELECT column_name FROM table1
        UNION ALL
        SELECT column_name FROM table2;
        ```
        

# 6. **DDL 语句 (Data Definition Language)**

- **CREATE TABLE**:
    
    ```sql
    CREATE TABLE table_name (column1 datatype, column2 datatype);
    ```
    
- **ALTER TABLE**:
    
    ```sql
    ALTER TABLE table_name ADD column_name datatype;
    ```
    
- **DROP TABLE**:
    
    ```sql
    DROP TABLE table_name;
    ```