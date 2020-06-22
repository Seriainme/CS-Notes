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
- 一对 pairs 是 `cons`
- `car` 是前面的
- `cdr` 是后面的
- `nil` 是 empty

## if ,and ,define
- `if`表达式
  - `(if <predicate> <consequent> <alternative>) `
- `and` 和 `or`
  - `(and <e1> ... <en>), (or <e1> ... <en>)`
- 绑定符号。
  -  `(define <symbol> <expression>)`
- 新程序。
  - `(define (<symbol> <formal parameters>) <body>)`


## lambda
- ` (lambda (<formal-parameters>) <body>)`
  - 举例：
    - `(define (plus4 x) (+ x 4))` 
    - `(define plus4 (lambda (x) (+ x 4)))`
    
## 注意
- `scheme` 中只有 ` #f, false, False ` 代表假值
- `=, eq?, equal?`
  - `=` 只能用于比较数字。
  - `eq?`在Python中类似于 `==`，用于比较两个 `non-pairs` (数字、布尔运算等)。否则，`eq?` 的行为就像在Python中的 `is` 一样。
  - `equal?` 比较 `pairs`, 通过比较它们的 `cars` 是否相等，它们的 `cdrs` 是否相等。否则，`equal?` 的行为就像 `eq?` 一样。
```scheme
scm> (eq? '(1 2 3) '(1 2 3))
#f
scm> (equal? '(1 2 3) '(1 2 3))
#t
scm> (= '(1 2 3) '(1 2 3))
Traceback (most recent call last):
  0     (= (quote (1 2 3)) (quote (1 2 3)))
Error: operand 0 ((1 2 3)) is not a number
```