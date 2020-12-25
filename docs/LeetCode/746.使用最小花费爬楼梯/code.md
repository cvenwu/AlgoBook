





# [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

## 方法一：dp


步骤：
1. 明确base case：dp[0]=nums[0],
            dp[1]=nums[1]
       
2. 状态：dp[i]表示到达第i个阶梯的最小花费

3. 选择：每次选择走一步或两步

4. 状态转移方程：
      dp[i] = min(dp[i-2] + cost[i], dp[i-1])  ，i为最后一个值时
      dp[i] = min(dp[i-1], dp[i-2]) + cost[i] , i 从2到倒数第2个值


> 执行用时：4 ms, 在所有 Go 提交中击败了93.12%的用户
> 		内存消耗：3.3 ms, 在所有 Go 提交中击败了5.17%的用户

```go
func minCostClimbingStairs(cost []int) int {
	if len(cost) <= 0 {
		return 0
	}


	if len(cost) == 1 {
		return cost[0]
	}


	dp := []int{cost[0], cost[1]}

	for i := 2; i < len(cost)-1; i++ {
		dp = append(dp, min(dp[i-1], dp[i-2]) + cost[i])
	}
	if len(cost) >= 3 {
		dp = append(dp, min(dp[len(cost)-3] + cost[len(cost)-1], dp[len(cost)-2]))
	}
	return dp[len(cost)-1]
}
```

