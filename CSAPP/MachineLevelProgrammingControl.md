# MachineLevelProgrammingControl

## Condition code 条件码
条件指令的基础
- `%rip` 指令指针，正在执行的指令的地址

## 条件指令
- `cmpq Src2,Scr1`
- `cmpq b,a` 就像计算 `a-b` 但没设定destination
  - `CF`(carry flag):  无符号数溢出
  - `SF`(sign flag): if t < 0, `SF` 被置为 1
  - `ZF`(zero flag): if t == 0
  - `OF`(overflow flag): 有符号数溢出(`(a>0 && b<0 && (a-b) < 0) || (a<0 && b>0 && (a-b) > 0)` )
- `testq Src2,Src1` 当你有一个值，你想看看它是什么
- `testq b,a` 就像计算 `a&b` 但没设定destination
  - `ZF`(zero flag): if a&b == 0
  - `SF`(sign flag): if a&b < 0

### jumping
- `jmp`` 无条件跳转
  - `je` 当为`ZF`时跳转
  - `js` 当为`SF`时跳转

## Loop
- `do while` 循环(无论如何都会进行至少一次loop)
- 很适合用汇编的`goto`来表示
```c
do
  Body
  while (Test);
```
```MIPS
loop:
  Body
  if (Test)
    goto loop
```
- `while` 循环(jump to middle)
```MIPS
  goto test;
loop:
  Body
test:
  if (Test)
    goto loop;
done:
```
- `for` 循环
  - 先变成`while` 循环
- `switch`
  - `switch` 中的 `case` 如果没有 `break` 会继续进行下去
  - `if-else`结构不一定是`switch`的机器码形式
  - 汇编层设计了一个 `jump table`
    - `goto *JTab[x]`
    - 先找到最小值和最大值，使其以外的范围对应到默认情况
    - 这种方式下获取所需位置的code的时间复杂度是O(1)
  - 如果范围很大，又比较稀疏，编译器会编译成`if-else`
    - 设计出一个跳转树
    [jumptable](imgs/jumptable.png)
    
## 跳转的三种方式
- 条件跳转
- 条件移动
- 跳转表