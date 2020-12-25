# [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/) 

## 方法一：dp


1. 按照如下模板来做题：（如果实在做不到就直接暴力穷举然后优化）
2. 明确basecase dp[0] = 0
3. 状态定义：dp[i]表示前i天获取的最大利润
4. 选择：当天是卖或者不卖
5. 状态转移方程：dp[i] = max(dp[i-1], 第i天卖掉可以获得的利润)

> 执行用时：4 ms, 在所有 Go 提交中击败了96.81%的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了13.93%的用户

```go
func maxProfit(prices []int) int {
	if len(prices) <= 1 {
		return 0
	}

	dp := make([]int, len(prices))
	//用来保存最低价格
	minPrice := prices[0]
	for i := 1; i < len(prices); i++ {
		if minPrice > prices[i] {
			minPrice = prices[i]
		}
		dp[i] = max(dp[i-1], prices[i]-minPrice)
	}

	return dp[len(prices)-1]
}

func max(num1, num2 int) int {
	if num1 > num2 {
		return num1
	}

	return num2
}
```



## 方法二：参照LeetCode上的另外一种解法

改进：只使用一个变量来记录最小价格，同时使用一个变量来保存最大利润。其实如果把价格用曲线图画出来之后，我们要找到如下的两个点


1. 曲线图中的最小点
2. 曲线图中的最大点
3. 满足最大点位于最小点的后方，这样利润可以为正

> 执行用时：4 ms, 在所有 Go 提交中击败了96.81%的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func maxProfit(prices []int) int {
	if len(prices) <= 1 {
		return 0
	}

	minPrice, maxProfit := prices[0], 0

	for i := 1; i < len(prices); i++ {
		if minPrice > prices[i] {
			minPrice = prices[i]
		}
		if maxProfit < prices[i]-minPrice {
			maxProfit = prices[i] - minPrice
		}
	}
	return maxProfit
}
```

