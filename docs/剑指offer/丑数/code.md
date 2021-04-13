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

## 【推荐】方法三：便于理解的三指针解法

思路：
1. 创建一个数组用于保存我们已经得到的丑数，数组的第一个元素为1
2. 创建3个指针(twoIndex, threeIndex, fiveIndex)指向我们的数组最开始位置，同时新建一个变量表示当前我们要找的丑数所在的位置cur
3. 开始遍历(cur < n)：
	3.1 如果twoIndex指向的位置不是我们当前要填入的位置并且twoIndex指向的位置对应的元素乘以2比我们当前已经填入的最大的数字小，那么twoIndex不断加一，直到不满足上述两个条件为止
	3.2 如果threeIndex指向的位置不是我们当前要填入的位置并且threeIndex指向的位置对应的元素乘以3比我们当前已经填入的最大的数字小，那么threeIndex不断加一，直到不满足上述两个条件为止
	3.3 如果fiveIndex指向的位置不是我们当前要填入的位置并且fiveIndex指向的位置对应的元素乘以5比我们当前已经填入的最大的数字小，那么fiveIndex不断加一，直到不满足上述两个条件为止
	3.4 此时我们当前位置cur应该要填入的数字就是我们min(twoIndex指向的位置的数 乘以 2，threeIndex指向的位置的数 乘以 3，fiveIndex指向的位置的数 乘以 5)
4. 最后返回我们数组中的最后一个数即可
```go

func nthUglyNumber(n int) int {
	ret := make([]int, n)
	ret[0] = 1
	twoIndex, threeIndex, fiveIndex := 0, 0, 0
	cur := 1
	for cur < n {
		for twoIndex < cur && ret[twoIndex]*2 <= ret[cur-1] {
			twoIndex++
		}
		for threeIndex < cur && ret[threeIndex]*3 <= ret[cur-1] {
			threeIndex++
		}
		for fiveIndex < cur && ret[fiveIndex]*5 <= ret[cur-1] {
			fiveIndex++
		}
		//当前指向的位置应该是cur
		ret[cur] = min(ret[twoIndex]*2, ret[threeIndex]*3, ret[fiveIndex]*5)
		cur++
	}
	return ret[len(ret)-1]
}


func min(a, b, c int) int {
	ret := a
	if a > b {
		ret = b
	}
	if ret > c {
		ret = c
	}
	return ret
}
```
