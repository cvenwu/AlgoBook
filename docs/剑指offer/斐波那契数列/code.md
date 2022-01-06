<!--
 * @Author: yirufeng
 * @Date: 2020-07-15 09:09:25
 * @LastEditTime: 2022-01-06 10:56:12
 * @LastEditors: yirufeng
 * @Description: 
 * @FilePath: /AlgoBook/docs/剑指offer/斐波那契数列/code.md
-->
# 斐波那契数列

注意题目要求：**答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。** 原因：[参考](https://www.zhihu.com/question/49374703/answer/207733726)

## 方法一：递归

> 提交到LeetCode上超时

```go
func fib(n int) int {
	//递归终止条件1
	if n <= 0 {
		return 0
	}
	//递归终止条件2
	if n == 1 {
		return 1
	}

	return fib(n-1) + fib(n-2)
}
```

!> 涉及到大量的重复计算，效率比较低

```c++
class Solution {
public:
    int fib(int n) {
        return this->fib(n-1) + this->fib(n-2);
    }
};
```

## 方法二：动态规划

```go
// 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
// 内存消耗：1.9 MB, 在所有 Go 提交中击败了100.00%的用户
func fib(n int) int {
	if n < 0 {
		return 0
	}
	if n <= 1 {
		return n
	}

	prev, prevPrev, ret := 0, 1, 0
	for i := 2; i <= n; i++ {
		ret = prev + prevPrev
		ret %= 1000000007
		prev = prevPrev
		prevPrev = ret
	}

	return ret
}
```

```c++
class Solution {
public:
    int fib(int n) {
        
        if (n <= 1) return n;

        int oneStep = 0, twoStep = 1;
        int temp;
        for (int i = 0; i < n-1; i++)
        {
            temp = oneStep;
            oneStep = twoStep;
            twoStep = (twoStep+temp)%1000000007;
        }
        return twoStep;
    }
};
```

对方法2代码的改进，[参考](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/solution/zui-jian-dan-de-dong-tai-gui-hua-fei-bo-na-qi-go-b/)

```go
func fib(n int) int {
	a, b := 0, 1

    for i := 0; i < n; i++{
        a, b = b, (a+b) % 1000000007
    }
    
    return a
}
```

## 额外的补充：矩阵快速幂

除以上的两种方法之外还有另外的一种时间复杂度为O(log n)的解法，这里不再进行赘述。


## 总结
!> 采用最简单的递归中涉及到大量的重复计算，因此我们可以考虑使用两个变量交替更新来模拟我们的动态规划的过程

**总结：采用递归的思路分析问题，采用由下往上的思路解决问题**

