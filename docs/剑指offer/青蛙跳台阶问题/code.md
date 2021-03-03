# 青蛙跳台阶问题

题目：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 `n` 级的台阶总共有多少种跳法。

## 方法一：

> 思路与斐波那契数列一致，但是注意斐波那契数列中fib(0)=0，而这里numWays(0)返回1

**注意斐波那契数列中fib(0)=0，而这里numWays(0)返回1**

```go
func numWays(n int) int {
	minusOneStep, minusTwoStep := 1, 1
	for i := 0; i < n; i++ {
		minusTwoStep, minusOneStep = minusOneStep, (minusOneStep + minusTwoStep) % 1000000007
	}
	return minusTwoStep
}
```

注意题目中要求：**答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。**



# 【推荐】方法二：
```go
func numWays(n int) int {
    if n == 0 {
        return 1
    } else if n == 1 {
        return 1
    }

    one, two := 1, 1
    for i := 2; i <= n; i++ {
        one, two = two, (one + two)%1000000007
    }
    return two
}
```