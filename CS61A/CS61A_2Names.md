# Names

binds names to values

```python
def <name>(<formal parameters>): # 函数签名
    return <return expression>
```
def 执行的步骤
- 创建一个 function，其签名为 ```<name>(<formal parameters>)```
- 设置函数的主体（第一行后的所有内容），但并未执行函数主体
- 把 ```<name>``` 和当前 frame 的 function 绑定
  
## summary
- envrionment 是一系列的 frames
- [局部环境帧与全局环境帧](https://wizardforcel.gitbooks.io/sicp-py/content/1.3.html)

![sum_squares](https://wizardforcel.gitbooks.io/sicp-py/content/img/evaluate_sum_squares_3.png)

- 名称绑定到值上面，它延伸到许多局部帧中；
- 局部帧在唯一的全局帧之上；
- 全局帧包含共享名称；
- 表达式为树形结构；
- 以及每次子表达式（包含用户定义函数）调用时，环境必须被扩展