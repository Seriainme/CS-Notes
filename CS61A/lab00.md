# lab00
- ls：列出当前目录中的所有文件
- cd <path to directory>：更改为指定目录
- mkdir <directory name>：使用给定名称创建新目录
- mv <source path> <destination path>：将给定源处的文件移动到给定目标

## python
- 浮点除法（/）：将第一个数字除以第二个数字，即使数字均分，也算出带小数点的数字
- 底数除法（//）：将第一个数字除以第二个数字，然后**删去小数**，得出整数
```python
3/3 # 1.0
99//10 # 9
7 % 4 # 余数 (remainder of 7 // 4)
3
```

## doctest 测试
```python

def twenty_twenty():
    """Come up with the most creative expression that evaluates to 2020,
    using only numbers and the +, *, and - operators.

    >>> twenty_twenty()
    2020
    """
    return 2020
```    
通过```python -m doctest [filename]```调用，如果想看到测试通过的信息可以用```python -m -v doctest [filename]```