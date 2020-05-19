# 实践指南：函数的艺术

## 如何编写良好的函数
- 如何编写良好的函数
  - 每个函数都应该只做一个任务
    - 任务使用短小的名称来定义，使用一行文本来标识
  - 不要重复劳动（DRY）是软件工程的中心法则。
    - 多个代码段不应该描述重复的逻辑
  - 函数应该定义得泛型
    - 任何情况下能可以使用

##  文档字符串
- 描述这个函数的文档，叫做文档字符串
  - 使用三个引号
    - 第一行描述函数的任务
    - 描述参数
- [文档字符串准则](https://www.python.org/dev/peps/pep-0257/)

```python
def pressure(v, t, n):
    """Compute the pressure in pascals of an ideal gas.

    Applies the ideal gas law: http://en.wikipedia.org/wiki/Ideal_gas_law

    v -- volume of gas, in cubic meters
    t -- absolute temperature in degrees kelvin
    n -- particles of gas
    """
    k = 1.38e-23  # Boltzmann's constant
    return n * k * t / v

help(pressure) # 查看文档字符串
```

## 参数默认值
```python
k_b=1.38e-23  # Boltzmann's constant
def pressure(v, t, n=6.022e23):
    return n * k_b * t / v
```
- 函数体的大多数数据
  - 应该表示为具名参数的默认值
- 永远不会改变的值
  - 就像基本常数k_b，应该定义在全局帧中。