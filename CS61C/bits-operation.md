# bits operation
摘录自[Matrix67: The Aha Moments: 位运算简介及实用技巧（一）：基础篇
](http://www.matrix67.com/blog/archives/263)

## 位运算符号
|C|Pascal|
|:--:|:--:|
|a&b|a and b|
|a\|b|a or b|
|a^b|a xor b|
|~a|not a|
|a<<b|a shl b|
|a>>b|a shr b|

## and & 运算
& 主要用于取位计算:
- ```num & 1``` 的结果就是取二进制的最末位，用来判断一个整数的奇偶
## or | 运算
| 主要用于无条件赋值:
- 用于二进制特定位上的无条件赋值
  - ```num | 1```的结果就是把二进制最末位强行变成1
  - 如果需要把二进制最末位变成0，```num|1 - 1```
  - 可用于把这个数强行变成最接近的偶数

## xor ^ 运算
异或定义：0和1 xor 0都不变，xor 1则取反。  
- ^主要用于对二进制的特定一位进行取反操作。
-  xor运算的逆运算是它本身，也就是说两次异或同一个数最后结果不变 ```(a xor b) xor b = a```
-  用于简单的加密
-  用于无中间变量的swap
```c
   a =a ^ b;
   b =a ^ b;
   a =a ^ b;
```

## not ~ 运算 
not运算的定义是把内存中的0和1全部取反。
- 需要注意整数类型有没有符号
- 无符号整数可用来求类型上界

## shl << 运算
a shl b就表示把a转为二进制后左移b位
-  在二进制数后添一个0就相当于该数乘以2
-  ```a << 1 == a * 2 ```

## shr >> 运算
a shr b表示二进制右移b位（去掉末b位）
- 相当于a除以2的b次方（取整）
- 

```c
unsigned num = 1 << 2;  # num = 1 << 2 = 0b100 = 4
unsigned num = 0b100 >> 2;  # num = 0b100 >> 2 = 0b001 = 1
```

## 示例

|功能|位运算 pascal|位运算 c|示例|
|:--:|:--:|:--:|:--:|
|去掉最后一位 / 除以2| x shr 1| x >> 1| 0b1011101 -> 0b101110|
|在最后加一个0 / 乘以2| x shl 1| x << 1| 0b1011101 -> 0b10111010|
|把最后一位变成1 / 改变二进制数中最后一个的值，将其变为 1| x or 1| x \| 1| 0b1011101 -> 0b1011101|
|最后一位取反|x xor 1| x^1|0b1011101 -> 0b1011100|
|把右起第k位变成1 (k从0开始算)|x or (1 shl k)| x \|(1 << k)|(0b101001 -> 0b101001,k=3)|
|把右起第k位变成0 (k从0开始算)|x & not (1 shl k)| x & ^(1 << k)|(0b101001 -> 0b100001,k=3)|
|把左边数第一个1变为了0|n and (n-1)|n&(n-1)|0b1100->0b1000|

## 练习
```c
// Return the nth bit of x.
// Assume 0 <= n <= 31
// Return the nth bit of x.
// Assume 0 <= n <= 31
unsigned get_bit(unsigned x, unsigned n);

// Set the nth bit of the value of x to v.
// Assume 0 <= n <= 31, and v is 0 or 1
void set_bit(unsigned * x, unsigned n,unsigned v);

// Flip the nth bit of the value of x.
// Assume 0 <= n <= 31
void flip_bit(unsigned * x, unsigned n);
```

解答：
```c
unsigned get_bit(unsigned x, unsigned n) {
    // 取二进制的最末位: num & 1
    return  (x >> n) & 1;
}
// Set the nth bit of the value of x to v.
// Assume 0 <= n <= 31, and v is 0 or 1
void set_bit(unsigned * x, unsigned n,unsigned v) {
    // 把右起第k位变成0: x & ^(1 << k)
    // 把最后一位变成1 : x or 1
    *x = (*x & (~(1 << n))) | (v << n);
}
// Flip the nth bit of the value of x.
// Assume 0 <= n <= 31
void flip_bit(unsigned * x, unsigned n) {
    // 最后一位取反 : x xor 1
    *x = *x ^ (1 << n);
}
```

## 应用：
- [线性反馈移位寄存器 LFSR](https://en.wikipedia.org/wiki/Linear-feedback_shift_register): 伪随机数生成，伪噪声序列

