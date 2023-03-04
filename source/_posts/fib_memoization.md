---
title: fib recursion-memoization 装饰器实现
categories: Algorithm
---

# fib recursion-memoization 装饰器实现

原题：[LeetCode explore: recursion-memoization](https://leetcode.com/explore/learn/card/recursion-i/255/recursion-memoization/1661/)

<!-- more -->

```python
import functools
class Solution:
    def fib_helper(fn):
        cache = {}
        @functools.wraps(fn)
        def wrapper(*args, **kw):
            n = args[1]
            if n in cache:
                return cache[n]
            result = fn(*args, **kw)
            cache[n] = result
            return result
        return wrapper
    
    @fib_helper
    def fib(self, N: int) -> int:
        if N < 2:
            return N
        return self.fib(N - 1) + self.fib(N - 2)
```

**注：** 上述方法存在一个问题：cache是static的，即不是每次调用都更新。所以上述递归的实现类似于一种打表。
