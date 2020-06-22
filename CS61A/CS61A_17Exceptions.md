# Exceptions
未处理的异常会导致 `Python` 停止执行，并打印一个堆栈跟踪。

- 异常是对象。
- 异常处理往往很慢。

## Raise Excepitons
- `assert`
  - `assert <expression>, <string>`
  - 导致 `AssertionError`
- `raise <expression>`
  - `<expression>`必须为 `BaseException` 的一个子类或一个实例。
  - 异常的构造就像其他对象一样。例如，`TypeError('Bad argument!')`
    - `TypeError` -- 一个函数被传递了错误的参数数量/类型。
    - `NameError` -- 没有找到一个名字。
    - `KeyError` -- 在字典中没有找到一个键。
    - `RecursionError` -- 递归调用太多。
 
## handle exceptions
```python
try:
 <try suite>
except <exception class> as <name>:
 <except suite>
...
```
- 先执行 `<try suite>`
- 如果在执行 `<try suite>` 的过程中引起异常
- 并且如果异常的类继承自`<exception class>`，
- 那么执行`<except suite>`，将`<name>`绑定到异常上

```python
>>> try:
        x = 1/0
    except ZeroDivisionError as e:
        print('handling a', type(e))
        x = 0

handling a <class 'ZeroDivisionError'>
>>> x
0
```
- 多个 handle 时，执行最近的

## 经典用例：
- `reduce`
  - `def reduce(f, s, initial):` 用 `f` 对 `s` 的元素进行组合，从`initial`开始。
  - 如果 `f` 是 division, 需要处理除以 0 的情况
  