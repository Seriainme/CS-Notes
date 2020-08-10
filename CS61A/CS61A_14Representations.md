# Representations
探索一些用于组合和操作**不同类型对象**的方式。
## String Representations
- 所有对象有两个 String representation
  - `str` 设计为人类易读
    - 结果是用 `print` 调用得出的结果
  - `repr` 易于 python 解释器
    - `eval(repr(object))`打印的和 iteractive 模式一样的内容

## 多态函数
- 适用于多种数据类型的函数
  - 比如 str 和 repr
    - repr 调用一个 0 参数的方法 `__repr__`(此名字指的是内置函数)
      - 忽略 实例的 `__repr__`,只调用类的 `__repr__`
      ```python
      def repr(x):
          return type(x).__repr__(x)
      ```
    - str 调用一个 0 参数的方法 `__str__`
      - 忽略 实例的 `__str__`,只调用类的 `__str__`
      - 如果没有`__str__`, 则使用 `repr`
      ```python
      def str(x):
          t = type(x)
          if hasattr(t,'__str__'):
              return t.__str__(x)
          else:
              return repr(x)
      ```
      
## 接口：
消息传递并不仅仅提供用于组装行为和数据的方式。它也允许不同的数据类型以不同方式响应相同消息。来自不同对象，产生相似行为的共享消息是抽象的有力手段。  
接口是共享消息的集合，带有它们含义的规定
- 信息传递。对象之间通过互相查找各自的属性进行交互
- 属性查找规则允许对不同的数据类型响应相同的消息
- 一个共享的消息（属性名），可以从不同的对象中引出类似的行为，这是一种强大的抽象方法
- 一个接口是一组共享的消息，以及它们的含义的规范。
  - 实现`__repr__`和`__str__`方法的类，返回 Python-interpretable 和 human-readable 字符串，实现了一个用于生成字符串表示的接口。
好处：
- 用于每个表示的类可以独立开发；它们只需要遵循它们所共享的属性名称。


## Special Method Names
- 某些名称是特殊的，因为它们有内置的方法
- 这些名称总是以两个下划线开始和结束
  - `__init__` : 构造对象时自动调用的方法
    `__repr__` : 被调用的方法，以Python表达式的形式显示一个对象。
    `__add__` : 调用方法将一个对象添加到另一个对象中
      - `>>> Ratio(1, 3) + Ratio(1, 6) # Ratio(1, 2)
      - Python 会检查表达式的左操作数`__add__`和右操作数`__radd__`上的特殊方法  
    `__bool__` : 被调用的方法，用于将一个对象转换为True或False。
    `__float__` : 调用方法将对象转换为float（实数）。
- 利用 `isinstance` 方法和类型转换来进行对象间的交互

## 泛用函数
- 如何使用相同的概念，定义不同种类、并且不共享通用结构的参数上的泛用操作？(如将复数与有理数相加)
  - 类型分发。一种处理跨类型操作的方式是**为每种可能的类型组合设计不同的函数**，操作可用于这种类型。
    - 编写一个函数，首先检测接受到的参数类型，之后执行适用于这种类型的代码
    - 可以通过以字典实现类型分发
    - 这个基于字典的分发方式是递增的，可扩展。任何新的数值类型可以将自己“安装”到现存的系统中，通过向这些字典添加新的条目。  
  - 数据导向编程。将**任意运算符作用于任意类型**，并且使用字典来储存多种组合的实现。
    - 优点：
      
      - 管理了跨类型运算符的复杂性
    - 缺点：
      - 十分麻烦
      
      - 引入新类型的开销不仅仅是为类型编写方法，还有实现跨类型操作的函数的构造和安装
      
        ```python
        # 这是字典 -> ('计算方法', ('数据类型1', '数据类型2')): 对应具体函数
        >>> apply.implementations = {('mul', ('com', 'com')): mul_complex,
                                     ('mul', ('com', 'rat')): mul_complex_and_rational,
                                     ('mul', ('rat', 'com')): mul_rational_and_complex,
                                     ('mul', ('rat', 'rat')): mul_rational}
        
        # 泛用函数，支持各种数据类型 [key]->('计算方法', ('数据类型1', '数据类型2'))
        # apply.implementations -> 字典
        # apply.implementations[key] -> 函数
        >>> def apply(operator_name, x, y):
                tags = (type_tag(x), type_tag(y))
                key = (operator_name, tags)
                return apply.implementations[key](x, y)
        
        # 求值
        >>> apply('add', ComplexRI(1.5, 0), Rational(3, 2))
        ComplexRI(3.0, 0)
        ```
  - 强制转换。将参数强制转换为**相同类型**的值，之后仅仅调用运算符(仅需要相同类型的运算符的字典)
    - 设计强制转换函数
    - 可以定义强制转换函数的字典
    - 优点：
      - 仅需要为每对类型编写一个函数，而不是为每个类型组合和每个泛用方法编写不同的函数
      - 可以让两个不同类型强制转换为第三个来降低了程序所需的转换函数总数
    - 缺点：
      - 会丢失信息
  
  
## Property Method
- 用于从零个参数的函数凭空计算属性（Attribute）
- `@property` 装饰器允许函数不使用标准调用表达式语法来调用
  
  - ```python
    class s:
        # a getter function 
        @property
        def magnitude(self):
            return ...
    
        # a setter function 
        @magnitude.setter
        def magnitude(self,value):
          ... = value
  
    s.magnitude      # 可以这么调用 getter function
    s.magnitude = .. # 可以这么调用 setter function
    ```