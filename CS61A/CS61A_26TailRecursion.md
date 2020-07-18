# Tail Recursion
如果一个函数中所有递归形式的调用都出现在函数的末尾，我们称这个递归函数是尾递归的
1. 在尾部调用的是函数自身 (Self-called)；
2. 可通过优化，使得计算仅占用常量栈空间 (Stack Space)。
   - 跳过所有不需要的 frame 以实现常数空间
   - 如果 tail call 没有额外计算，则可以是 tail recursion
  
## tail call optimization
- tail call 的返回值是返回当前程序调用的 return value
- 因此，tail call 不应该增加 env 的数量


```javascript
// 非尾递归
function fact(n) {
    if (n <= 0) {
        return 1;
    } else {
        return n * fact(n - 1);
    }
}
/*
6 * fact(5)
6 * (5 * fact(4))
6 * (5 * (4 * fact(3))))
// two thousand years later...
6 * (5 * (4 * (3 * (2 * (1 * 1)))))) // <= 最终的展开
*/


// 尾递归
function fact(n, r) {
    if (n <= 0) {
        return 1 * r;
    } else {
        return fact(n - 1, r * n);
    }
}
/*
fact(6, 1) // 1 是 fact(0) 的值，我们需要手动写一下
fact(5, 6)
fact(4, 30)
fact(3, 120)
fact(2, 360)
fact(1, 720)
720 // <= 最终的结果
*/
```
