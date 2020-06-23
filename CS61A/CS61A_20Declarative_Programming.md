# Declarative Programming (声明式编程)
- 声明式语言
  - "程序"是对预期结果的描述
  - `interpreter` 想出如何产生结果
- 命令式语言
  -  "程序"是对计算过程的描述
  - `interpreter` 执行 `execution/evaluation` 规则

## SQL
- SQL(Structured Query Language 结构化查询语言)
  - `select` 语句可以从头开始创建一个新表，也可以通过投射表来创建一个新表
  - `create table` 语句给表起了一个全局名称
- 还有很多其他语句：`analyze, delete, explain, insert, replace, update`等。
- 大多数重要的操作是在 `select` 语句中的

### select
- `select` 语句总是包含一个以逗号分隔的列描述列表。
- 列的描述是一个表达式，后面可选择as和列名
- `select [expression] as [name]`
- 两个选择语句的 `union` 是一个包含它们两个结果的行的表。
```SQLite
select "delano" as parent, "herbert" as child union
select "abraham"         , "barack"           union
select "abraham"         , "clinton"          union
select "fillmore"        , "abraham"          union
select "fillmore"        , "delano"           union
select "fillmore"        , "grover"           union
select "eisenhower"      , "fillmore";
```

- select 的结果只呈现给用户，不储存
- `create table` 语句给结果一个名字
- `create table [name] as [select statement];`
```SQLite
create table parents as
    select "delano" as parent, "herbert" as child union
    select "abraham"         , "barack"           union
    select "abraham"         , "clinton"          union
    select "fillmore"        , "abraham"          union
    select "fillmore"        , "delano"           union
    select "fillmore"        , "grover"           union
    select "eisenhower"      , "fillmore";
```

## Projecting Tables(投影表)
- `select` 呈现现有表格
- 一个 `select` 语句可以使用 `from` 子句指定一个输入表
  - ` select [columns] from [table] where [condition] order by [order]`
  - `[column]` 指的是 `select [expression] as [name], [expression] as [name], ... ;` 中的 `[name]`
  - `select child from parents where parent = "abraham";`
  - `select parent from parents where parent > child;`
- 可以使用 `where` 子句选择输入表中的行的子集。
- 可以使用一个 `order by` 子句来声明剩余行的排序。
- 列的描述决定了每条输入行如何投影到结果行。

## Arithmetic(算术)
- 在 `select` 表达式中， `column` 名是 `row` 值。
- 算术表达式可以将 `row` 值和常量相结合
```SQLite
create table lift as
 select 101 as chair, 2 as single, 2 as couple union
 select 102 , 0 , 3 union
 select 103 , 4 , 1;

select chair, single + 2 * couple as total from lift;
```
|char|total|
|:---:|:---:|
|101|6|
|102|6|
|103|6|