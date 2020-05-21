# lab 01
```bash
python3 -i  // 在交互式运行Python文件时对其进行编辑
python3 -m doctest  // 文档测试
```

```python
>>> positive = 28
>>> while positive: # If this loops forever, just type Infinite Loop
...    print("positive?")
...    positive -= 3
```
- 无限循环
  - 只有 0、None、False 为假值
  
- x or y
  - x 为真： x
  - x 为假： y
- x and y
  - x 为真： y
  - x 为假： x
  
## 调试技巧
- [Debugging](https://cs61a.org/articles/debugging.html)
- 使用print语句
  - 短期调试
- 长期调试
  - 使用全局 debug 变量
  ```python
  debug = True
  
  def foo(n):
  i = 0
  while i < n:
      i += func(i)
      if debug:
          print('DEBUG: i is', i)
  ```
  - 只要我们想进行一些调试，就可以将全局debug变量设置 为True
  - （这样的变量称为“标志”）
- 交互式调试
  - ```python -i file.py```
- 使用assert语句
  - ```assert isinstance(x, int), "The input to double(x) must be an integer"```
  - assert语句的一个主要优点是它们不仅仅是调试工具，您可以将它们永久保留在代码中
  - 代码崩溃通常比产生不正确的结果要好
  - 并且在代码中声明内容会使代码更容易崩溃
- PythonTutor调试
  - ```python ok -q <question name> --trace```
  - [pythontutor](http://pythontutor.com/composingprograms.html#mode=edit)
