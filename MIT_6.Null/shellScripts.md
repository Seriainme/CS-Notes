# Shell scripts
shell脚本针对shell所从事的相关工作进行来优化
- bash中为变量赋值的语法是`foo=bar`，访问变量中存储的数值，其语法为 `$foo`
  - `foo = bar` （使用括号隔开）是不能正确工作的，因为解释器会调用程序 `foo` 并将 `=` 和 `bar` 作为参数
  - `'`定义的字符串为原义字符串，其中的变量不会被转义，而 `"` 定义的字符串会将变量值进行替换。
  ```shell script
  foo=bar
  echo "$foo"
  # 打印 bar
  echo '$foo'
  # 打印 $foo
  ```
## `if, case, while, for`,函数
### 函数
```shell script
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```