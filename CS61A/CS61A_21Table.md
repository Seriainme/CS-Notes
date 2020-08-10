# Table
## Joining Tables
- 通过将多张表连接成一张表来组合数据，这是数据库系统中的基本操作。

- 我们在本类中只重点介绍一种方法（内部连接）。

- 如果连接两个表，左边的表有 m 条记录，右边的表有 n 条记录，那么连接后的表将有 mn 条记录

  
```SQLite
CREATE TABLE dogs AS
 SELECT "abraham" AS name, "long" AS fur UNION
 SELECT "barack"         , "short"       UNION
 SELECT "clinton"        , "long"        UNION
 SELECT "delano"         , "long"        UNION
 SELECT "eisenhower"     , "short"       UNION
 SELECT "fillmore"       , "curly"       UNION
 SELECT "grover"         , "short"       UNION
 SELECT "herbert"        , "curly";

CREATE TABLE parents AS
 SELECT "abraham" AS parent, "barack" AS child UNION
 SELECT "abraham"          , "clinton"         UNION
 ...;
```


- 两张表 A和B 用逗号连接，得出 A和B中某行 的所有组合。
  
  - Select the parents of curly-furred dogs
  
    
```SQLite
SELECT parent FROM parents, dog WHERE child = name AND fur = "curly";
```

## 用 Python 描述 SQL join
```python
def FROM(table_1, table_2):
    for row_1 in table1:
        for row_2 in table2:
            yield row_1 + row_2
```
联接就像创建嵌套`for`循环。
- 每一行`table_1`和每一行`table_2`，联接将对行的每种可能的组合进行迭代，并将其视为输入表

## Aliases and Dot Expressions(别名和点表达式)
- 两个表可能共用一个列名，所以我们需要一种方法来区分表的列名。
- 点表达式和别名 可以消除列值的歧义。
- SQL允许我们在 `FROM` 子句中使用关键字 `AS` 给表赋予别名，
- 使用点表达式来引用特定表内的列。  
`SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order];`
- `[table]` 是一个以逗号分隔的表名列表，可选择别名
- 选择所有相同 parent 的 child
```SQLite
SELECT a.child AS first, b.child AS second
 FROM parents AS a, parents AS b  -- 这是一个别名
 WHERE a.parent = b.parent AND a.child < b.child;
```

## Numerical Expressions
- 表达式可以包含函数调用和算术运算符
- `SELECT [columns] FROM [table] WHERE [expression] ORDER BY [expression];`
- Combine values: ` +, -, *, /, %, and, or `
- Transform values: ` abs, round, not, -`
- Compare values:  ` <, <=, >, >=, <>, !=, =` 
  - `<>, !=` ANSI标准中是用`<>`(所以建议用`<>`)
  - ` = ` 因为 SQL 没有赋值，所有用 `=` 比较

## String Expressions
```SQLite
sqlite> SELECT "hello," || " world"; -- 字符串值可以组合成较长的字符(使用 || )。
hello, world

sqlite> CREATE TABLE phrase AS SELECT "hello, world" AS s;
sqlite> SELECT substr(s, 4, 2) || substr(s, instr(s, " ")+1, 1) FROM phrase; -- SQL中内置了基本的字符串操作，但与Python不同。
low

sqlite> CREATE TABLE lists AS SELECT "one" AS car, "two,three,four" AS cdr;
sqlite> SELECT substr(cdr, 1, instr(cdr, ",")-1) AS cadr FROM lists; -- 字符串可以用来表示结构化的值，但这样做不是个好主意
two

```