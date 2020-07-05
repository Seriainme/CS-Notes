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
    
## 完整内存寻址模式
`D(Rb,Ri,S)`
- `Mem[Reg[Rb]+S*Reg[Ri]+D]`
  - `D`  常量 1、2或4个字节
  - `Rb` 基址寄存器：16个整数寄存器中的任何一个
  - `Ri` 索引寄存器：任何，除了`%rsp`
  - `S`  Scale 1、2、4或8

## 地址计算
`leaq` load effective address
- `leaq Src, Dst`
  - 计算地址但不引用其内存
  - `p = &x[i]`
- `mov` 计算内存地址，然后把 内存地址里面的值 放在寄存器里
- `lea` 计算内存地址，然后把   内存地址本身  放进寄存器里
  - `lea 8(ebx),eax`  将`ebx+8`这个值直接赋给`eax`
  - `mov 8(ebx),eax`  把内存地址为`ebx+8`处的数据赋给`eax`

## 一些运算符
Two Operand Instructions:
```MIPS
addq	Src,Dest	Dest = Dest + Src
subq	Src,Dest	Dest = Dest - Src
imulq	Src,Dest	Dest = Dest * Src
salq	Src,Dest	Dest = Dest << Src
sarq	Src,Dest	Dest = Dest >> Src	
shrq	Src,Dest	Dest = Dest >> Src	
xorq	Src,Dest	Dest = Dest ^ Src
andq	Src,Dest	Dest = Dest & Src
orq 	Src,Dest	Dest = Dest | Src
```
One Operand Instructions
```MIPS
incq	Dest	Dest = Dest + 1
decq	Dest	Dest = Dest - 1
negq	Dest	Dest = -Dest
notq	Dest	Dest = ~Dest
```
