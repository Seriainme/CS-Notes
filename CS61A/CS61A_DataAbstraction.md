# data abstraction
复合数据 看做 单个概念单元
- 分离程序使用数据的两部分
  - 如何表示数据
  - 如何处理数据
- 分数是用 一对数字 来表示的经典例子

```python
>>> pair = [1,2]
>>> pair
[1,2]

>>> x,y = pair # unpacking a list
>>> x
1
>>> y
2
```
- 抽象屏障 (Abstraction Barriers)
  - 选择适合的抽象层方法，不要用不属于自己层次的抽象层方法
  - 使得程序更容易被更改
  - 可以改变 data representation 无需重写整个程序

例如：
```python
def rational_mul(x,y):
    return rational(numer(x) * numer(y), denom(x) * denom(y))

#---------用列表表示-------------------

def rational(x,y):
    return [x,y]

def numer(x):
    return x[0]

def denom(x):
    return x[1]

#---------用HOF表示-------------------

def rational(x,y):
    def select(keyword):
        if keyword == "n":
            return x
        if keyword == "d":
            return y
    return select

def numer(x):
    return x("n")

def denom(x):
    return x("d")

```

## dict
- 无顺序
- 键值对
- 两个 key 不能相同
- key 不能是列表
```python
>>> numberals = {'I':1,'V':5,'X':10}    # dict 没有顺序
>>> numberals
{'I': 1, 'V': 5, 'X': 10}

>>> numberals.keys()
dict_keys(['I', 'V', 'X'])

>>> numberals.values()
dict_values([1, 5, 10])

>>> numberals.items()
dict_items([('I', 1), ('V', 5), ('X', 10)])

>>> items = [('I', 1), ('V', 5), ('X', 10)]
>>> dict(items)
{'I': 1, 'V': 5, 'X': 10}
>>> dict(items)['X']
10

>>> 'X' in numberals
True
>>> 'X-ray' in numberals
False

>>> numberals.get('X',0)
10
>>> numberals.get('X-ray',0)
0

>>> {x:x*x for x in range(5)}
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

>>> {1:2,1:3}
{1: 3}
>>> {1:[2,3]}
{1: [2, 3]}
>>> {[1]:2}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

