# shell
计算机的文字接口

```shell script
missing:~$ 
```
- 你的主机名是 missing 
- 您当前的工作目录（”current working directory”）或者说您当前所在的位置是~ (表示 “home”)
- $符号表示您现在的身份不是root用户

- `pwd` 打印当前目录
- `cd -` 去前一个工作目录，可以方便地在两个目录变化 
```shell script
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```
- 本行第一个字符 d 表示 missing 是一个目录
- 九个字符，每三个字符构成一组。 (rwx). 
  - 它们分别代表了文件所有者(missing)，用户组 (users) 以及其他所有人具有的权限
  - 只有文件所有者可以**修改**(w) missing 文件夹 （例如，添加或删除文件夹中的文件）。
  - 为了**进入**某个文件夹，用户需要具备该文件夹以及其父文件夹的“搜索”权限（以“可执行”：x）权限表示。
    - /bin目录下的程序在最后一组，即表示所有人的用户组中，均包含x权限，也就是说任何人都可以执行这些程序。
  - 为了**列出它的包含的内容**，用户必须对该文件夹具备读权限（r）

## 在程序间创建连接
在shell中，程序有两个主要的“流”：他们的输入流和输出流
- 当程序尝试读取信息时，它们会从输入流中进行读取
- 当程序打印信息时，它们会将信息输出到输出流中。
最简单的重定向是 < file 和 > file
```shell script
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```
- 使用 >> 来向一个文件追加内容
- 使用管道（ pipes ），我们能够更好的利用文件重定向
  - |操作符允许我们将一个程序的输出和另外一个程序的输入连接起来
```shell script
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```
## 用户
- 根用户（root用户）。
  - 根用户几乎不受任何限制，他可以创建、读取、更新和删除系统中的任何文件。
  - 并不会以根用户的身份直接登录系统，因为这样可能会因为某些错误的操作而破坏系统
  - 在需要的时候使用 sudo 命令
    - 让您可以以su（super user 或 root的简写）的身份do一些事情
    - 向 sysfs 文件写入内容,如笔记本亮度在`/sys/class/backlight`
    ```shell script
    $ sudo echo 3 > brightness
    An error occurred while redirecting file 'brightness'
    open: Permission denied
    ```
    - echo 等程序并不知道|的存在，它们只知道从自己的输入输出流中进行读写
    ```shell script
    $ echo 3 | sudo tee brightness
    ```
  - `chmod 777` 改用户权限
    - `rwx` 对应的是二进制数