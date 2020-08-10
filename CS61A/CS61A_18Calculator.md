# Calculator
- Parsing (解析)  
`Text -> Lexical analysis (词法分析) -> Token (标记) -> Syntactic analysis (句法分析) -> Expression`
- `scheme` 可用递归进行句法分析
  - `Base case`：符号和数字
  - `Recursive call`：`scheme_read`子表达式，并将其组合起来
  - `Syntactic analysis `：确定了一个表达的层次结构，可以是嵌套
  - 每次调用`scheme_read`都会消耗一个表达式的输入标记。
  
## Scheme syntax calculator
- 计算器表达式的值是递归定义的。
  - `Primitive`。一个数字对其自身进行评估。
  - `Call`。调用表达式对其参数值进行运算。
    - `+`: 参数值之和
    - `*`: 各项参数的乘积
    - `-`: 如果只有一个参数，取负。如果有多个参数，则从第一个参数中减去其余参数。
    - `/`: 如果有一个参数，就把它反过来。如果超过一个参数，则从第一个参数中除以其余部分。
    
## eval function
- `eval`函数计算一个表达式的值，它总是一个数字
- 它是一个通用函数，根据表达式的类型（`primitive` 或 `call`）进行分配。
  
```python
def calc_eval(exp):
    if type(exp) in (int, float): # A number evaluates to itself
        return exp

    # A call expression evaluates to its argument values combined by an operator 
    # (调用表达式对其参数值进行评估，由一个运算符组合而成。)
    elif isinstance(exp, Pair):  

        # calc_eval: Recursive call returns a number for each operand 
        # (递归调用为每个操作数返回一个数字。)
        arguments = exp.second.map(calc_eval)  

        # exp.first: '+', '-', '*', '/'
        # arguments: A Scheme list of numbers
        return calc_apply(exp.first, arguments)

    else:
        raise TypeError

def calc_apply(operator, args):
    if operator == '+':
        return reduce(add, args, 0)
    elif operator == '-':
        ...
    elif operator == '*':
        ...
    elif operator == '/':
        ...
    else:
        raise TypeError
```

## Interactive Interpreters
- 读取 —— 执行 — 打印循环
  - 许多编程语言的用户界面是一个交互式解释器。
    1. 打印一个提示
    2. 读取用户输入的文本
    3. 将输入的文本解析为一个表达式
    4. 评价这个表达式
    5. 如果发生任何错误，请报告这些错误。
    6. 否则打印表达式的值，并重复

```python
def read_print_loop():
    """Run a read-print loop for Scheme expressions."""
    while True:
        try:
            src = buffer_input()
            while src.more_on_line:
                expression = scheme_read(src)
                print(repr(expression))
        except (SyntaxError, ValueError) as err:  # ValueError: 2.3.4; SyntaxError: 额外的‘)’
            print(type(err).__name__ + ':', err)
        except (KeyboardInterrupt, EOFError):  # <Control>-D, etc. 真正的停止方式
            return
```

