# Data Wrangeling(数据整理)
- 如何通过命令行直接轻松快速地修改、查看、解析、绘制和计算数据和文件。
- 不再从日志文件拷贝 粘贴。不再手动统计数据。不再用电子表格画图。
- 将某种格式存储的数据转换成另外一种格式

## 从远处服务器取数据
- 日志处理通常是一个比较典型的使用场景
  - 在日志中查找信息
```shell script
ssh myserver journalctl # 这样是把 myserver 的整个网络日志发往本地
```
内容太多了，现在让我们把涉及 `sshd` 的信息过滤出来
```shell script
# 内容太多了，现在让我们把涉及 `sshd` 的信息过滤出来
ssh myserver journalctl | grep sshd 
# 使用管道将一个远程服务器上的文件传递给本机的 grep 程序
```
我们只要`Disconnected from` 的数据，而且以 `less` 实现
```shell script
ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less
# 这里的引号是指在远端机器上先过滤文本内容，再把结果传送回来
# less 为我们创建来一个文件分页器，通过翻页的方式浏览较长的文本
```
输入到文件中，后续就不需要再次通过网络访问该文件了
```shell script
$ ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' > ssh.log
$ less ssh.log
```

## sed (流编辑器stream editor)
`sed` 是一个基于文本编辑器构建的”流编辑器”(一次处理一行) 
```shell script
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed 's/.*Disconnected from //' # 把 .*Disconnected from 替换为 空
```
- `s`，即替换命令
- `s/REGEX/SUBSTITUTION/`
  - `REGEX` 部分是我们需要使用的正则表达式
  - `SUBSTITUTION` 是用于替换匹配结果的文本

## 正则表达式
- [在线正则表达式测试](https://regex101.com/)
- [learn-regex](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)
- [正则表达式可视化](https://regexper.com/)
- `*` 匹配前面字符零次或多次(贪心)
  - `+?`或`*?`表示非贪婪匹配，即尽可能少的匹配(这个`sed`不支持，`perl`支持)
  - `Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from invalid user Disconnected from 46.97.239.16 port 55920 [preauth]`
  - 正则表达式： `.*Disconnected from`  -> `Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from invalid user Disconnected from`
  - 正则表达式： `.*?Disconnected from` -> `Jan 17 03:13:00 thesquareplanet.com sshd[2631]: Disconnected from`(非全局)
- `+` 匹配前面字符一次或多次
- `.` 除换行符“\n”之外的”任意单个字符”
  - `.*` 所有字符
- `[abc]` 匹配 `a`, `b` 和 `c` 中的任意一个
  - `24.61.9.143` 匹配：`[24619143.]` 或者 `[0-9.]`
  - `(RX1)` 捕获组，任何能够匹配 `RX1` 的结果 
    - 现代语法，要加上 `-E` 才能执行
    - 如果没有地方加 `-E` ,要用`\`表示我要它具有特殊含义 `\(RX1\)`
    - `root@cyx:~# echo "ababacab" | sed 's/\(ab\)*//g'`
      - 结果是 `ac`
      - `g` 代表全局替换，如果没有只会替换第一次
    - `(RX1|RX2)` 任何能够匹配 `RX1` 或 `RX2` 的结果
      - `root@cyx:~# echo "ababacab" | sed 's/\(ab\|ca\)*//g'`
      -  结果是`ab` ~~ab ab~~ a ~~ca~~ b
- `^` 行首
- `$` 行尾

## 用于数据整理的 bash
- sort 排序
  - `| sort` 对其输入数据进行排序
  - `sort -n` 会按照数字顺序对输入进行排序
  - （默认情况下是按照字典序排序 `-k1,1` 则表示“仅基于以空格分割的第一列进行排序”
- uniq 组合
  - `| uniq -c` 把连续出现的行折叠为一行并使用出现次数作为前缀
- wc 计算字数
  - `wc -l` 计算行数
- paste
  - `paste -sd,`
    - 合并行(`-s`)
    - 指定一个分隔符进行分割 (`-d`)，

## awk 基于列的流处理器
事实上，我们完全可以抛弃 `grep` 和 `sed` ，因为 `awk` 就可以解决所有问题
```shell script
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd,  # 对于每一行文本，打印其第二个部分，也就是用户名。
```
- `awk` 程序接受一个模式串（可选）以及一个代码块，指定当模式匹配时应该做何种操作
-  在代码块中，`$0` 表示正行的内容，`$1` 到 `$n` 为一行中的 n 个区域
- 引用代码块 `''`
- 区域的分割基于 `awk` 的域分隔符（默认是空格，可以通过`-F`来修改
- 精准匹配 `==`
  - `awk '$1=="zhengxh" {print $0}' filename        #输出第一列等于zhengxh的行`
- 模糊匹配 `~`
- 匹配代码块，可以是字符串或正则表达式`//`   
- 命令代码块，包含一条或多条命令 `{}`        

让我们统计一下所有
- 以 c 开头，以 e 结尾，
- 仅尝试过 1 次登陆的用户
```shell script
|awk '$1 == 1 && $2 ~ /^c.*e$/ {print $2 } ' | wc -l
```

```shell script
root@cyx:~# cat ssh.log | grep sshd | grep "Disconnected from"  | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/' | sort | uniq -c | sort -nk1,1 |awk '$1 == 1 && $2 ~ /^c.*e$/ {print
 $2 } ' |paste -sd,
caffe,caroline,cherie,clarice,clive,core,counterstrike,course
```

- 既然 `awk` 是一种编程语言，那么则可以这样：
```shell script
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```
- `BEGIN` 也是一种模式，它会匹配输入的开头（ `END` 则匹配结尾）。
- 然后，对每一行第一个部分进行累加，最后将结果输出。

### 分析数据
- 将每行的数字加起来
```shell script
| paste -sd+ | bc -l # bc -> berkeley calculation
```

## xargs
`xargs` 是给命令传递参数的一个过滤器
```shell script
somecommand |xargs -item  command
```

## 整理二进制数据
- `ffmpeg` 图片