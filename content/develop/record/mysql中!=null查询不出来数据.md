+++
title = 'Mysql中!=null查询不出来数据'
tags = ['mysql']
date = 2024-10-15T11:34:55+08:00
draft = false
+++
## 问题描述
今天排查一个问题,sql语句查询不到数据，明明有数据却查不出任务数据sql类似于
```sql
select * for XXX where XX != null;
```
正确的语句是
```sql
select * for XXX where XX is not null;
```

## 原因
在 MySQL 中，NULL 用于表示缺失的或未知的数据，处理 NULL 值需要特别小心，因为在数据库中它可能会导致不同于预期的结果。

为了处理这种情况，MySQL提供了三大运算符:

- <strong>IS NULL</strong>: 当列的值是 NULL,此运算符返回 true。
- <strong>IS NOT NULL</strong>: 当列的值不为 NULL, 运算符返回 true。
- <=>: 比较操作符（不同于 = 运算符），当比较的的两个值相等或者都为 NULL 时返回 true

关于 NULL 的条件比较运算是比较特殊的。你不能使用 = NULL 或 != NULL 在列中查找 NULL 值 。

在 MySQL 中，NULL 值与任何其它值的比较（即使是 NULL）永远返回 NULL，即 NULL = NULL 返回 NULL 。

## 可能涉及的null值处理
1. 检查是否为 NULL
   要检查某列是否为 NULL，可以使用 IS NULL 或 IS NOT NULL 条件。
```sql
SELECT * FROM employees WHERE department_id IS NULL;
```
2. 使用 COALESCE 函数处理 NULL
   COALESCE 函数可以用于替换为 NULL 的值，它接受多个参数，返回参数列表中的第一个非 NULL 值：
```sql
SELECT product_name, COALESCE(stock_quantity, 0) AS actual_quantity
FROM products;
```

3. 使用 IFNULL 函数处理 NULL
   IFNULL 函数是 COALESCE 的 MySQL 特定版本，它接受两个参数，如果第一个参数为 NULL，则返回第二个参数。
```sql
SELECT product_name, IFNULL(stock_quantity, 0) AS actual_quantity
FROM products;
```

4. 使用 <=> 操作符进行 NULL 比较
   <=> 操作符是 MySQL 中用于比较两个表达式是否相等的特殊操作符，对于 NULL 值的比较也会返回 TRUE。它可以用于处理 NULL 值的等值比较。
```sql
SELECT * FROM employees WHERE commission <=> NULL;
```

5. 注意聚合函数对 NULL 的处理
   在使用聚合函数（如 COUNT, SUM, AVG）时，它们会忽略 NULL 值，因此可能会得到不同于预期的结果。如果希望将 NULL 视为 0，可以使用 COALESCE 或 IFNULL。
```sql
SELECT AVG(COALESCE(salary, 0)) AS avg_salary FROM employees;
```