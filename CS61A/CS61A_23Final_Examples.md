# Final Examples

- 两个常用的递归模板
  - return the accumulation(累计) so far，用递归函数的 return 值来跟踪你想要的返回值
  - 初始化某个 accumulation 为 0 或 空列表， 然后按需要填充

```python
def bigs(t):
    """Return the number of nodes in t that are larger than all their ancestors.
    >>> a = Tree(1, [Tree(4, [Tree(4), Tree(5)]), Tree(3, [Tree(0, [Tree(2)])])])
    >>> bigs(a)
    4
    """
    def f(a, x):
        if a.label > x:
            return 1 + sum([f(b, a.label) for b in a.branches])
        else: 
            return sum([f(b, x) for b in a.branches]) 
    return f(t,t.label - 1)
```

```python
def bigs(t):
    """Return the number of nodes in t that are larger than all their ancestors."""
    n = 0
    def f(a, x):
        nonlocal n
        if a.label > x:
            n += 1
        for b in a.branches:
            f(b, max(a.label, x))
    f(t,t.label - 1)
    return n
```

## 设计 functions 
[How to Design Programs, Second Edition](https://htdp.org/2018-01-06/Book/)
- 从问题分析到数据定义 (From Problem Analysis to Data Definitions)
  - 确定必须表示的信息，以及如何在所选语言中表示该信息
  - 制定数据定义，并用例子呈现他们
- Signature(函数签名), Purpose Statement(目标陈述), Header(函数存根)
  - Signature: 描述了函数所需参数和产生的结果
  - Purpose Statement: 描述了函数的作用：这个函数计算了什么
  - Header: 将函数的参数替换为具体的合法数据，并提供了输出结果的具体示例
- Functional Examples
  - 使用样例来呈现函数的目的
- Function Template
  - 将数据定义转化为函数的概要，类似于实现伪代码
  - 写模板，比如自己想要使用是么样子的函数，函数中又包括了什么函数，用到了什么结构语句
- Function Definition
  - 填补函数模板的空白
- Testing
  - 以测试的方式阐述例子，并确保函数全部通过
  - 测试也是对例子的补充，因为它们可以帮助别人阅读和理解


