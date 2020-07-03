# 命令行环境
- 使用一些方法改善您的工作流
  - 执行多个不同的进程并追踪它们的状态
  - 如何停止或暂停某个进程
  - 如何使进程在后台运行
- 改善 `shell` 及其他工具的工作流的方法
  - 定义别名
  - 基于配置文件对其进行配置
  - 例如，仅需要执行一些简单的命令，我们就可以在所有的主机上使用相同的配置。
- 如何使用 `SSH` 操作远端机器
 
## 任务控制
使用 `Ctrl-C` 来停止命令的执行
- 它的工作原理
- 为什么有的时候会无法结束进程？

### 结束进程
`shell` 使用 `UNIX` 提供的信号机制 执行 进程间通信
- 当一个进程接收到信号(信号是一种软件中断)时，它会
  - 停止执行
  - 处理该信号
  - 基于信号传递的信息来改变其执行
  - 输入 `Ctrl-C` 时，`shell` 会发送一个 `SIGINT` 信号到进程
- 尽管 `SIGINT` 和 `SIGQUIT` 都常常用来发出和终止程序相关的请求。
- `SIGTERM` 则是一个更加通用的、也更加优雅地退出信号。
  -  `kill -TERM <PID>`。  
  
  
### 暂停和后台执行进程
```shell script
root@cyx:~# nohup sleep 2000 & # & 后台运行， nohup 关闭终端不停止
[1] 207
root@cyx:~# jobs
[1]+  Running                 nohup sleep 2000 &
root@cyx:~# sleep 1000 &
[2] 208
root@cyx:~# jobs
[1]-  Running                 nohup sleep 2000 &
[2]+  Running                 sleep 1000 &
root@cyx:~# pgrep sleep
207
208
root@cyx:~# kill 207
root@cyx:~# jobs
[1]-  Terminated              nohup sleep 2000
[2]+  Running                 sleep 1000 &
root@cyx:~# jobs
[2]+  Running                 sleep 1000 &
root@cyx:~# kill -STOP %2  # %2 代指第二个任务
[2]+  Stopped                 sleep 1000
root@cyx:~# jobs
[2]+  Stopped                 sleep 1000
```

#### 暂停 
- 进程暂停 `SIGSTOP`  `Ctrl-Z`
  - 使用 `fg`(在前台继续) 或 `bg`(background在后台继续) 恢复暂停的工作
#### 未完成的任务
- `jobs` 
  - 用 `pgrep` 找出 `pid`。
  - 使用百分号 + 任务编号来选取该任务。
  - `$!` 最近的一次任务。
#### 后台执行进程
- 命令的 `&` 后缀
  - 不过它此时还是会使用 `shell` 的标准输出（这种情况可以使用 `shell` 重定向处理）。
- 让已经在运行的进程转到后台运行
  - 键入`Ctrl-Z` ，然后紧接着再输入 `bg`
#### 关闭终端后使得程序继续运行
  - 一旦您关闭终端（会发送另外一个信号 `SIGHUP`），这些后台的进程也会终止。
  - 您可以使用 `nohup` (忽略 `SIGHUP` ) 来运行程序。
  - 针对已经运行的程序，可以使用 `disown` 。

## 终端多路复用
- 分离当前终端会话并在将来重新连接
- 在最流行的终端多路器是 `tmux`

###tmux
会话：
- 每个会话都是一个独立的工作区，其中包含一个或多个窗口
 - **`tmux`** 开始一个新的会话
 - **`tmux new -s NAME`** 以指定名称开始一个新的会话
 - **`tmux ls`** 列出当前所有会话
- **`<C-b> d`** ，将当前会话分离
- **`tmux a`** 重新连接最后一个会话。您也可以通过 **`-t`** 来指定具体的会话
窗口：
- 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分
 - **`<C-b> c`** 创建一个新的窗口(create)
 - **`<C-b> <0-9>`** 跳转到第 N 个窗口
 - `<C-b> p` 切换到前一个窗口(preview)
 - `<C-b> n` 切换到下一个窗口(next)
 - `<C-b> ,` 重命名当前窗口
 - `<C-b> w` 列出当前所有窗口
面板： 
- 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell
 - **`<C-b> "`** 水平分割
 - **`<C-b> %`** 垂直分割
 - **`<C-b> <方向>`** 切换到指定方向的面板
 - **`<C-b> z`** 切换当前面板的缩放
 - `<C-b> [` 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
 - `<C-b> <空格>` 在不同的面板排布间切换
 
## 别名
- 语法：`alias alias_name="command_to_alias arg1 arg2"`
- 在默认情况下 `shell` 并不会保存别名。
  - 需要将配置放进 `shell` 的启动文件里，像是`.bashrc` 或 `.zshrc`
```shell script
# 创建常用命令的缩写
alias ll="ls -alF"

# 能够少输入很多
alias gs="git status"
alias gc="git commit"
alias v="vim"

# 重新定义一些命令行的默认行为
alias mv="mv -i"           # -i prompts before overwrite
alias mkdir="mkdir -p"     # -p make parent dirs as needed
alias df="df -h"           # -h prints human readable format

# 在忽略某个别名
\ls
# 或者禁用别名
unalias la

# 获取别名的定义
alias ll
# 会打印 ll='ls -lh'
```

## 配置文件（Dotfiles）
- 编辑 `.bashrc` 或 `.bash_profile` 来进行配置
- 确保程序能够被 shell 找到
  - `export PATH="$PATH:/path/to/program/bin"`
  