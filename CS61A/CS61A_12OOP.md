## OOP
- 组织模块化编程的方法
  - 数据抽象
  - 将信息和相关行为捆绑在一起
- 一种对使用**分布式状态**的计算的隐喻
  - 每个对象都有自己的本地状态
  - 每个对象也知道如何管理自己的本地状态(基于方法调用)
- 方法调用 是 对象之间传递 的 信息
- 几个对象可能都是一个共同类型的实例。
- 不同的类型可能相互关联

## Classes
- 一个类作为它的实例的模板。
```python
class <name>:
    <suite> # 当类语句执行时，该 suite 被执行。
```
- 当类语句执行时， suite 被执行。
- 类声明创建了一个新的类，并将该类绑定到当前环境的第一个框架中的```<name>```。
- ```<suite>```中的```Assignment & def```语句创建了类的属性（而不是框架中的名称）。
```python
class Clown:
    """An illustration of a class statement. This class is not useful.

    >>> Clown.nose # 被立即执行了
    'big and red'
    >>> Clown.dance()
    'No thanks'
    """
    nose = 'big and red'
    def dance():
        return 'No thanks'
```

## Object Construction

当一个 class 被调用
- 该 class 的一个新实例被创建 (self 绑定为这个 instance)
- 类的__init__方法被调用，新对象作为它的第一个参数(命名为self)，以及在调用表达式中提供的任何附加参数。
```python
class Account:
    """An account has a balance and a holder.
    All accounts share a common interest rate.

    >>> a = Account('John')
    >>> a.holder
    'John'
    >>> a.balance
    10
    >>> a.interest
    0.02
    >>> Account.interest = 0.04
    >>> a.interest
    0.04
    """

    interest = 0.02  # A class attribute

    def __init__(self, account_holder):  # self 绑定 实例
        self.holder = account_holder     # 实例里有 holder
        self.balance = 0                 # 实例里有 balance
```

## method
- 所有被调用的方法都可以通过 self 参数访问对象，因此它们都可以访问和操作对象的状态。
- 点符号自动提供方法的第一个参数。
```python
class Account:
    """
    >>> a = Account('John')
    >>> a.deposit(100) 
    100
    >>> a.withdraw(90)
    10
    >>> a.withdraw(90)
    'Insufficient funds'
    """

    interest = 0.02  # A class attribute

    def __init__(self, account_holder):
        self.holder = account_holder
        self.balance = 0

    def deposit(self, amount):
        """Add amount to balance."""
        self.balance = self.balance + amount
        return self.balance
```

## Attributes(属性)
- 使用 ```getattr(<instance>,<attribute(string)>)``` 获得对应属性的值
- 使用 ```hasattr(<instance>,<attribute(string)>)``` 判断对应属性是否存在
- 在一个对象中查找一个属性名可能会返回。
  - 对象的一个实例属性
  - 一个类属性

## Methods 和 Functions
- 绑定方法(Bound methods)，它将函数和将被调用的对象结合在一起。
  - Object + Function = Bound Method
```python
>>> type(Account.deposit)
<class 'function'>
>>> type(tom_account.deposit)
<class 'method'>
```

## Class Attributes
- 类的属性在类的所有实例中是 "共享 "的，因为它们是类的属性，而不是实例的属性。
- 类的属性没有 copy 到实例中
- 它改变了所有实例的类属性都改变了