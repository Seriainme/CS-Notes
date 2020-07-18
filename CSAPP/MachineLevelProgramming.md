# Machine Level Programming
每个指令只做一件事
- `*dest = t` 对应的机器级语言 `movq %rax, (%rbx)`
  -  `t`    => 寄存器 `%rax`
  - `dest`  => 寄存器 `%rbx`
  - `*dest` => 内存  `M[%rbx]`

## 反汇编
-  `objdump -d <file(s)>`:  反汇编test中的需要执行指令的那些section
- `gdb` : 可以单步检查程序
  - `disassemble <function name>`

## x86-64 Integer 寄存器
- `%rax`(64位)
  - `%eax` (32位)
- `%rsp`: 栈指针
- `(%rax)`: 内存
- `%0x400`: 常量
- `%rdi``%rsi` 第1,2个参数寄存器


## movq
`movq Imm/Reg/Mem Reg/Mem`(Mem 只能 to Reg)
  - `movq $0x4 %rax`      -> `temp = 0x4;`
  - `movq $-147 (%rax)`   -> `*p = -147;`
  - `movq %rax %rdx`      -> `temp2 = temp1;`
  - `movq %rax (%rdx)`    -> `*p = temp;`
  - `movq (%rax) %rdx`    -> `temp = *p;`
    - `movq 8(%rbp) %rdx` -> `temp = p[1];`
    
