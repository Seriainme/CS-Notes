# Interpreters
## The Structure of an Interpreter
有两个function：
- `Eval` 求表达式的值(Evaluate a Calculator expression.)
- `Apply` 当 `eval` 找到一个表达式,用 `apply` 来求出其具体的值(Apply the named operator to a list of args.)
- `Eval`
  - Base cases
    - 原始值（数字）
    - 查询与符号绑定的值 
  - Recursive calls: 
    - `Eval(operator(操作符), operands(操作数))` of call expressions
    - `Apply(procedure, arguments)`
    - `Eval(sub-expressions)` of special forms
- `Apply`
  - Base cases
    - Built-in primitive procedures
  - Recursive calls: 
    - `Eval(body)` (用户定义程序)

## Special Forms
- `scheme_eval` 函数根据表达式选择行为
  - `symbols` 在当前环境中绑定为 `values`
  - `self-evaluating` 表达式作为值返回
  - 所有其他合法表达式都以 `Scheme lists` 的形式表示，称为 `combinations`

### Logical Special Forms
- `if`
- `and` `or`
- `cond`

## Quotation
- `quote`特殊形式 `evaluates` 被引用的表达式(which is not evaluated)
- `expression` 本身就是被求出的值

## Lambda expressions
- Lambda 表达式对用户定义的过程进行求值

```python
class LambdaProcedure:
    def __init__(self, formals, body, env):
        self.formals = formals  # A scheme list of symbols
        self.body = body        # A scheme list of expressions
        self.env = env          # A Frame instance
```

## Frame
- 一个 `frame` 通过有一个`parent frame`来代表一个环境
- `Frames` 是 `Python` 实例，有方法 `lookup` 和 `define` 。

### Define
- `Define` 将一个 `symbol` 绑定到 `current environment` 的 `first frame` 中的值。
- `Procedure definition`(过程定义)是带有 lambda 表达式的定义的简写。  
`(define (<name> <formal parameters>) <body>)`  
`(define <name> (lambda (<formal parameters>) <body>))`