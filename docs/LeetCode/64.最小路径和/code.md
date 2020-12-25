# [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)



## 方法一：dp



步骤：

1. basecase：`dp[0][0]=grid[0][0]`,
          `dp[0][i]=dp[0][i-1]+grid[0][i]`,
          `dp[i][0]=dp[i-1][0]`
2. 状态定义：dp[i][j]表示从左上角到该点的最小路径和
3. 选择：每次可以向右或向下
4. 状态转移方程：`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`


> 执行用时：12 ms, 在所有 Go 提交中击败了17.20%的用户
> 		内存消耗：5 MB, 在所有 Go 提交中击败了5.36%的用户

```go
func minPathSum(grid [][]int) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}
	dp := make([][]int, len(grid))

	dp[0] = append(dp[0], grid[0][0])
	//初始化第1行
	for i := 1; i < len(grid[0]); i++ {
		dp[0] = append(dp[0], dp[0][i-1] + grid[0][i])
	}

	//初始化第1列
	for i := 1; i < len(grid); i++ {
		dp[i] = append(dp[i], dp[i-1][0] + grid[i][0])
	}

	//状态转移
	for i := 1; i < len(grid); i++ {
		for j := 1; j < len(grid[0]); j++ {
			dp[i] = append(dp[i], min(dp[i][j-1], dp[i-1][j]) + grid[i][j])
		}
	}
	return dp[len(grid)-1][len(grid[0])-1]
}

func min(num1, num2 int) int{
	if num1 < num2 {
		return num1
	}

	return num2
}
```



## 方法二：dp改进

改进：复用输入数组进行状态压缩

> 执行用时：8 ms, 在所有 Go 提交中击败了90.93%的用户
> 		内存消耗：3.9 MB, 在所有 Go 提交中击败了82.14%的用户

步骤：

1. basecase：`dp[0][0]=grid[0][0]`,
        `dp[0][i]=dp[0][i-1]+grid[0][i]`,
        `dp[i][0]=dp[i-1][0]`
2. 状态定义：dp[i][j]表示从左上角到该点的最小路径和
3. 选择：每次可以向右或向下
4. 状态转移方程：`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`

```go
func minPathSum(grid [][]int) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	//初始化第1行
	for i := 1; i < len(grid[0]); i++ {
		grid[0][i] = grid[0][i-1] + grid[0][i]
	}

	//初始化第1列
	for i := 1; i < len(grid); i++ {
		grid[i][0] = grid[i-1][0] + grid[i][0]
	}

	//状态转移
	for i := 1; i < len(grid); i++ {
		for j := 1; j < len(grid[0]); j++ {
			grid[i][j] = min(grid[i][j-1], grid[i-1][j]) + grid[i][j]
		}
	}
	return grid[len(grid)-1][len(grid[0])-1]
}

func min(num1, num2 int) int{
	if num1 < num2 {
		return num1
	}

	return num2
}
```

