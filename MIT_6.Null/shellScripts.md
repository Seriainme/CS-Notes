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
  
## 变量
- `$1-$9`: 输入的9个参数
- `$_`: 输入的最后一个参数
- `$?`: 最后一个发生的错误

`$_` 和 `$?` 的含义
```shell script
root@cyx:/home# mkdir test
root@cyx:/home# cd $_ # 上一个参数 -> test
root@cyx:/home# echo $_ # 上一个参数 -> test
test
root@cyx:/home# echo $? # 错误代码 0 -> 正常运行，像 C 那样
0
root@cyx:/home# grep foobar mcd.sh
root@cyx:/home# echo $? # 错误代码 1 -> grep 未在 mcd.sh 找到 foobar
1
```
`$(pwd)`
- 以变量的形式获取一个命令的输出
- 通过 命令替换 (command substitution)实现
- 获取 `pwd` 命令下的结果
```shell script
root@cyx:/home# echo $(pwd)
/home
root@cyx:/home# echo "We are in $(pwd)"
We are in /home
```
`<(pwd)`
- 进程替换（process substitution）
- 执行 `pwd` 并将结果输出到一个临时文件中
- 在希望返回值通过文件而不是 `STDIN` 传递时很有用
```shell script
diff <(ls foo) <(ls bar) # 显示文件夹 foo 和 bar 中文件的区别。
```

`$?`用法
```shell script
root@cyx:/home# true
root@cyx:/home# echo $?
0
root@cyx:/home# false
root@cyx:/home# echo $?
1

```
短路
```shell script
root@cyx:/home# false || echo "oop, fail"
oop, fail
root@cyx:/home# echo $?
0
root@cyx:/home# true || echo "oop, fail"  # 后面的不会执行
root@cyx:/home# true ; echo "oop, fail"  #  分号用来串联命令
oop, fail
```
!! 
- 完整到上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。

### 函数
```shell script
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```
`source a.sh`: 在当前shell内去读取、执行a.sh，而a.sh不需要有"执行权限"
`source` 命令可以简写为`.`:`. a.sh` 注意要用空格


### examples
- 遍历我们提供的参数，使用grep 搜索字符串 foobar，
- 如果没有找到，则将其作为注释追加到文件中
```shell script
#!/bin/bash

echo "Starting program at $(date)"                       # date会被替换成日期和时间
echo "Running program $0 with $# arguments with pid $$"  # $0->脚本名 $#->参数个数 $$->进程识别码
for file in $@; do                                       # file 是变量                                   # $@->所有参数
    grep foobar $file > /dev/null 2> /dev/null           # 如果模式没有找到，则grep退出状态为 1
                                                         # 我们将标准输出流(>)和标准错误流(2>)重定向到/dev/null，因为我们并不关心这些信息  
    
    if [[ $? -ne 0 ]]; then                              # 比较的时候建议使用[[]]
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"                       # 将其作为注释追加到文件中
    fi
done
```
## 通配符
```shell script
>>> ls *.sh # 列出所有 .sh 文件

>>> ls ?.sh # ? 代表只能对应一个字符

```

## convert
- convert 支持格式转换
```shell script
>>> convert image.png image.jpg # 改变格式
```

## {}
- {} 拓展功能
- 支持一行里面使用两个{},就像笛卡尔坐标那样
```shell script
root@cyx:~# touch foo{,1,2,10}
root@cyx:~# ls
foo  foo1  foo10  foo2
root@cyx:~#

root@cyx:~# touch a{1..10}  
root@cyx:~# ls
a1  a10  a2  a3  a4  a5  a6  a7  a8  a9  foo  foo1  foo10  foo2
root@cyx:~#
```

## diff 
- 检查两个里面的区别
```shell script
root@cyx:~# diff <(ls a1) <(ls a2)
1c1
< a1
---
> a2
root@cyx:~#
```

## python and bash
使用 `import sys` 操作 bash
```python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)

>>> python scripts.py a b c
c
b
a

# 要想 ./scripts.py a b c 也起作用，需要在最顶上加一行
#!/usr/bin/env python 
# 指的是 运行 “env” 命令
```

## debug bash scripts
使用 shellcheck
```shell script
>>> shellcheck mcd.sh
```

## man /  tldr (帮助手册)
- man 是详细的帮助手册
- tldr 给了某命令的常用方法，简易版man，很好用

## find (查找文件)
- 查找
  - find
    - 用途广泛
    - 它的语法却比较难以记忆
  - fd
    - 输出着色
    - 默认支持正则匹配
    - 支持unicode
  - locate
    - 更高效的方法
    - 基于数据库
    - 使用 updatedb 来更新
```shell script
>>> find . -name src -type d # 我们要在当前目录下找 名字是 src, 格式是 文件夹 的文件
# 会递归地去找每个文件夹里面符合这个要求的文件
>>> find . -path '**/test/*.py' -type f # 找所有test文件夹里面的python脚本
>>> find . -mtime -1 # 找所有在最后一天修改的文件
>>> find . -name *.temp -exec rm {} \; # 找到所有名为 .temp 的文件如何把他们都删除

# 简易的查找：
>>> fd ".*py" # 找当前文件夹及子文件夹的 python 文件

# 更高效的方法-> locate
# 1. locate的速度比find快，因为它并不是真的查找文件，而是查数据库
# 2. locate的查找并不是实时的，而是以数据库的更新为准，一般是系统自己维护
>>> updatedb # 更新locate数据库
>>> locate "*py"

```
## grep (查找文件)
Linux grep 命令用于查找文件里符合条件的字符串。
- `-C`：获取查找结果的上下文（Context）
- `-v` 将对结果进行反选（Invert），也就是输出不匹配的结果
- 替代品 `rp`
```shell script
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match "^#!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

- 1、在当前目录中，查找后缀有 file 字样的文件中包含 test 字符串的文件，并打印出该字符串的行。此时，可以使用如下命令：
```shell script
grep test *file 
```
- 2、以递归的方式查找符合条件的文件。例如，查找指定目录/etc/acpi 及其子目录（如果存在子目录的话）下所有文件中包含字符串"update"的文件，并打印出该字符串所在行的内容，使用的命令为：
```shell script
grep -r update /etc/acpi 
```

## history (打印你曾经输过的命令)
```shell script
>>> history

>>> history | grep convert  # 使用管道来找所有用过的包含 convert 的命令

>>> history 100 # 最近输入的 100 个命令
```
- 按  `Ctrl+R` 对命令历史记录进行回溯搜索
- `fzf` 是一个通用对模糊查找工具，它可以和很多命令一起使用
  - `cat .viminfo | fzf`
- 为了避免隐私泄露，在命令的开头加上一个空格，它就不会被加进shell记录中
  - 当你输入包含密码或是其他敏感信息的命令时会用到这一特性。
  - 如果你不小心忘了在前面加空格，可以通过编辑 `bash_history` 或 `.zhistory` 来手动地从历史记录中移除那一项。

## 文件夹导航(高效地在目录间随意切换)
- `ls -R` 递归输出所有文件夹和子文件夹的文件
  - 使用 `tree` 更加具有可读性
  - 使用 `broot` 可交互的 文件 `tree`
  - 使用 `nnn` 可交互的 文件 `tree`(跟 `windows` 那种差不多)
