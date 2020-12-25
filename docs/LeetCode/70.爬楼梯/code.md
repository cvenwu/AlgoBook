# [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

## 方法一：递归

> 提交后超时

```go
func climbStairs(n int) int {
	if n == 1 {
		return 1
	} else if n == 2 {
		return 2
	} else {
		return climbStairs(n-1) + climbStairs(n-2)
	}
}
```

## 方法二：dp

**使用一个数组来存储每个数对应的斐波那契数。**

时间复杂度：O(N)

空间复杂度：O(N)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了53.78%的用户

```go
func climbStairs(n int) int {
	if n == 1 {
		return 1
	}

	ret := make([]int, n)

	//初始化状态
	ret[0], ret[1] = 1, 2

	for i := 2; i < n; i++ {
		ret[i] = ret[i-1] + ret[i-2]
	}

	return ret[n-1]
}

```

## 方法三：对dp进行改进

**改进点：对方法二中的dp进行状态压缩**，只使用两个变量即可

时间复杂度：O(N)

空间复杂度：O(1)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func climbStairs(n int) int {

	if n == 1 {
		return 1
	}

	oneStep, twoStep := 1, 2

	for i := 2; i < n; i++ {
		oneStep, twoStep = twoStep, oneStep+twoStep
	}

	return twoStep
}
```

## 方法四：矩阵快速幂解决

时间复杂度：O(logN)

**具体请[参考](https://leetcode-cn.com/problems/climbing-stairs/solution/pa-lou-ti-by-leetcode-solution/)**