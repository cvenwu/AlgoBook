# [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

## 方法一：直接dp

思路：覆盖传入的triangle


1. basecase：`dp[0][0]=grid[0][0],`
          `dp[0][i]=dp[0][i-1]+grid[0][i],`
          `dp[i][0]=dp[i-1][0]`
2. 状态定义：dp[i][j]表示从左上角到该点的最小路径和
3. 选择：每次可以向右或向下
4. 状态转移方程：dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]

> 执行用时：12 ms, 在所有 Go 提交中击败了17.20%的用户
			内存消耗：5 MB, 在所有 Go 提交中击败了5.36%的用户

```go
func minimumTotal(triangle [][]int) int {
	if len(triangle) == 0 || len(triangle[0]) == 0 {
		return 0
	}


	//更新第1列
	for i := 1; i < len(triangle); i++ {
		triangle[i][0] = triangle[i-1][0] + triangle[i][0]
	}

	//更新其他
	for i := 1; i < len(triangle); i++ {
		for j := 1; j <= i; j++ {
			if i == j {  //说明是最后一列
				triangle[i][j] = triangle[i-1][j-1] + triangle[i][j]
			} else {
				triangle[i][j] = min(triangle[i-1][j-1], triangle[i-1][j])+ triangle[i][j]
			}
		}
	}

	//从最后一行找到最小值并返回
	mi := triangle[len(triangle)-1][0]
	for i := 1; i < len(triangle[len(triangle)-1]); i++ {
		if mi > triangle[len(triangle)-1][i] {
			mi = triangle[len(triangle)-1][i]
		}
	}

	return mi
}
```

## 方法二【推荐】：状态压缩的dp

空间复杂度：O(N)

思路：从下到上进行动态规划，


1. basecase：dp数组初始化为triangle的最后一行
2. 状态定义：dp[j]表示从最后一行到达当前位置的最小路径和
3. 选择：每次可以向上或左上
4. 状态转移方程：`dp[j] = min(dp[j], dp[j+1])` j从0->i，而i代表行号，从倒数第2行到第1行

> 执行用时：4 ms, 在所有 Go 提交中击败了94.80%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了83.71%的用户

**最终我们想要的结果存放在dp[0]**

```go
func minimumTotal(triangle [][]int) int {
	if len(triangle) == 0 || len(triangle[0]) == 0 {
		return 0
	}

	//初始化为最后一行
	dp := triangle[len(triangle)-1]

	for i := len(triangle)-2; i >= 0; i-- {
		for j := 0; j <= i; j++ {
			dp[j] = min(dp[j], dp[j+1]) + triangle[i][j]
		}
	}

	return dp[0]
}
```

