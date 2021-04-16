# 股票的最大利润

## 方法一：

思路：假设在当天买入，在后面找到最大值，差值与之前计算过的最大利润比较，如果大就更新。**含有大量的重复计算**

> *//执行用时：292 ms, 在所有 Go 提交中击败了5.2% 的用户*
>
> *//内存消耗：3.1 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func maxProfit(prices []int) int {
	//这里没必要判断，因为for循环第1个会判断
	//如果传入进来的价格数组长度为0,zh
	length := len(prices)
	//计算最大利润
	maxProfit := 0
	//哪天买
	for i := 0; i < length - 1; i++ {
		//哪天卖
		for j := i+1; j < length; j++ {
			if maxProfit < prices[j] - prices[i] {
				maxProfit = prices[j] - prices[i]
			}
		}
	}
	return maxProfit
}
```

## 方法二：

**思路：假设当天卖出股票，那么在遍历到当天之前，我们需要使用minValue记录最小的值，同时记录maxProfit**

> *//执行用时：4 ms, 在所有 Go 提交中击败了97.06% 的用户*
>
> *//内存消耗：3.1 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func maxProfit(prices []int) int {
	//如果传入进来的价格数组长度为0
	length := len(prices)
	//if length <= 0 {
		//return 0
	//}

	//获取最大利润
	minValue, maxProfit := prices[0], 0
	for i := 1; i < length; i++ {
		//首先比较最大利润
		if prices[i] - minValue > maxProfit {
			maxProfit = prices[i] - minValue
		}
		//之后比较最小值
		if minValue > prices[i] {
			minValue = prices[i]
		}
	}
	return maxProfit
}
```

!> **2021-04-16更新题解**
```go
func maxProfit(prices []int) int {
	profit := 0
	//记录我们股票的最低价格
	minPrice := math.MaxInt32
	for i := 0; i < len(prices); i++ {
		if prices[i] < minPrice {
			minPrice = prices[i]
		}
		//如果利润小于我们的当前卖出与之前买入的最低价就更新
		if profit < prices[i]-minPrice {
			profit = prices[i] - minPrice
		}
	}
	//返回我们的利润
	return profit
}
```

总结：**两种方法如上所示，法一时间复杂度为o(n^2)，法二时间复杂度为o(n)**

这道题目其实就是找到在数组中找到两个值，要求满足如下条件：

1. 两个值有先后顺序
2. 后面值减去前面值尽可能大，如果都为负直接返回0