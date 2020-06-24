# Aggregation(聚合)
- 我们也可以使用相同的 `SELECT` 语句对多条记录进行聚合操作
  - 对 a group of rows 进行计算
- 我们可以使用 `MAX、MIN、COUNT和SUM` 函数从我们的初始表中获取更多的信息。
  - `COUNT` 计算 `row` 的个数
    - `count(distinct name)` -> 计算 `name` 的种类 
  - 聚合函数也会取 `table` 中的某一个 `row`
    - 但结果有多个 `row` 对应，则会取其中任意一行
    - `avg` 函数有可能取不出 `row`
```SQLite
sqlite> SELECT name, MAX(salary) FROM records;
Oliver Warbucks|150000
```
- 使用特殊 `COUNT(*)` 语法，我们可以统计表中的行数，以查看该公司的员工人数。
```SQLite
sqlite> SELECT COUNT(*) from RECORDS;
9
```

## Group
到目前位置的所有 `select` 都只有一个 `group`,`the only one final group`  
`table` 中的 `rows` 可以被 `grouped`, `aggregation` 在各个组起作用  
`select [columns] from [table] group by [expression] having [expression];`
- `group by [expression]`
  - `group` 的数量是 `group` 的 `[expression]` 中独特值的个数(数的种类)
- 可以通过 `HAVING` 子句过滤被聚合的组的集合
  - 与 `WHERE` 子句不同的是，
    - `WHERE` 子句过滤掉的是行，
    -  `HAVING` 子句过滤掉的是整个组。
```SQLite
sqlite> SELECT title FROM records GROUP BY title HAVING count(*) > 1; -- 过滤组数少于 1 的组
Programmer
```

## 用 Python 描述 SQL Aggregation
我们可以使用GROUP BY和HAVING 将行划分为组，并仅选择组的子集。
```python
output_table = []                                 # 输出多个 table
for input_group in GROUP_BY(FROM(*input_tables)): # GROUP_BY 划分了子集
    output_group = []
    for row in input_group:
        if WHERE(row):
            output_group += [row]                 # 每个子集内部的 row 用 WHERE 筛选
    if HAVING(output_group):                      # group 本身用 HAVING 筛选
        output_table += [SELECT(output_group)]

if ORDER_BY:
    output_table = ORDER_BY(output_table)
if LIMIT:
    output_table = output_table[:LIMIT]
```