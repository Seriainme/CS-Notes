# 树型递归问题
考虑下面这个问题：如果给你半美元、四分之一美元、十美分、五美分和一美分，一美元有多少种找零的方式？更通常来说，我们能不能编写一个函数，使用一系列货币的面额，计算有多少种方式为给定的金额总数找零？

## 分析
- 我们可以递归将给定金额的找零问题，归约为使用更少种类的硬币为更小的金额找零的问题。
- 使用n种硬币找零的方式为：
  - `without`：使用所有除了第一种之外的硬币为`a`找零的方式
  - `within`：使用`n`种硬币为更小的金额`a - d`找零的方式，其中d是第一种硬币的面额。
- 终止条件
  - 如果a正好是零，那么有一种找零方式。
  
  - 如果a小于零，那么有零种找零方式。
  
  - 如果n小于零，那么有零种找零方式。
  
    
```python
>>> def count_change(a, kinds=(50, 25, 10, 5, 1)):
        """Return the number of ways to change amount a using coin kinds."""
        if a == 0:
            return 1
        if a < 0 or len(kinds) == 0:
            return 0

        d = kinds[0]
        return count_change(a, kinds[1:]) + count_change(a - d, kinds)

>>> count_change(100)
292
```