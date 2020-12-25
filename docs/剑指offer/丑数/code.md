# [剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

## 方法一【自己的代码】：动态规划


步骤：
1. base case : `dp[0] = 1`   因为第1个丑数是0
2. 状态定义：`dp[i]`表示第i+1个丑数
3. 状态转移方程：`dp[i] = min(dp[a]*2, dp[b]*3, dp[c]*5) `要求`dp[a]*2, dp[b]*3, dp[c]*5` 都要大于`dp[i]`
4. 结果：返回dp数组的最后一个元素即可


```go
func nthUglyNumber(n int) int {
	//意味着输入不合法
	if n <= 0 {
		return -1
	}
	dp := []int{1}

	a, b, c := 0, 0, 0
	for i := 1; i < n; i++ {
		min, minVal := a, 2
		//找到dp[a]*2, dp[b]*3, dp[c]*5中最小的并返回
		if dp[a] * 2 > dp[b] * 3 {
			min, minVal = b, 3
		}
		if dp[min] * minVal > dp[c] * 5 {
			min, minVal = c, 5
		}

		dp = append(dp, dp[min] * minVal)

		//谁指向的数乘以质因子最小谁就+1
		if dp[i] == dp[a]*2 {
			a++
		}
		if dp[i] == dp[b]*3 {
			b++
		}
		if dp[i] == dp[c]*5 {
			c++
		}
	}
	return dp[n-1]
}
```

## 方法二：参考LeetCode他人解法


> 执行用时：4 ms, 在所有 Go 提交中击败了 63.93% 的用户
> 内存消耗：6.3 MB, 在所有 Go 提交中击败了 5.19% 的用户

> 注意：因为b会指向4，而a会指向6，所以到时候dp[a]*2 == dp[b]*3 所以，此时将值加入之后，我们要让a,b都要右移

```go
func nthUglyNumber(n int) int {
	//意味着输入不合法
	if n <= 0 {
		return -1
	}

	dp := []int{1}

	a, b, c := 0, 0, 0

	for i := 1; i < n; i++ {

		n2, n3, n5 := dp[a]*2, dp[b]*3, dp[c]*5
		dp = append(dp, int(math.Min(math.Min(float64(n2), float64(n3)), float64(n5))))

		//谁指向的数乘以质因子最小谁就+1
		if dp[i] == n2 {
			a++
		}

		if dp[i] == n3 {
			b++
		}

		if dp[i] == n5 {
			c++
		}
	}
	fmt.Println(dp)
	return dp[n-1]
}
```

