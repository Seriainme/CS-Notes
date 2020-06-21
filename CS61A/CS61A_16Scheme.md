# sheme
## quote
- 阻止记号被求值
  - 将符号原封不动地传递到程序，而不是求值后的其他内容
```scheme
> (+ 2 3)
5
> (quote (+ 2 3))
(+ 2 3)
> '(+ 2 3)
(+ 2 3)
```
## cons ,car ,cdr
- 一对 pair 是 cons
- car 是前面的
- cdr 是后面的

