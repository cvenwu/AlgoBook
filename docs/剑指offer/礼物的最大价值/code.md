# 礼物的最大价值



> 思路：问题是求最大价值(最值)，同时是遍历一个二维数组，涉及到了大量的重复计算，因而我们可以考虑dp，
>
> 最入门的dp可以使用二维数组来存储结果
>
> 这里我们进行状态压缩，使用一维数组来存储结果



`dp[i][j]`表示第i, j位置的礼物最大价值

**dp公式如下：**

`dp[i][j] = grid[i][j])` 位于第一行第一列，只能赋值

`dp[i][j] = dp[i][j-1] + grid[i][j]` 位于第一行非第一列，只能从左边过来

`dp[i][j] = dp[i-1][j] + grid[i][j]` 位于其他行的第一列，只能从上边下来

`dp[i][j] = max(dp[i-1][j] + grid[i][j], dp[i][j-1] + grid[i][j])`  当不在第一行第一列时



> // 执行用时：8 ms, 在所有 Go 提交中击败了 89.81% 的用户
>            // 内存消耗：3.9 MB, 在所有 Go 提交中击败了 100.00% 的用户



```go

func maxValue(grid [][]int) int {
	//如果行和列两个维度只要有一个为0就返回0
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	valueArray := make([]int, len(grid[0]))

	//遍历多少次
	for i := 0; i < len(grid); i++ {
		//修改多少列
		for j := 0; j < len(grid[0]); j++ {
			//第1个值
			if i == 0 && j == 0 {
				valueArray[j] = grid[i][j]
			} else if i == 0 {	//第1行
				valueArray[j] = valueArray[j-1] + grid[i][j]
			} else if j == 0 {  //第一列
				valueArray[j] = valueArray[j] + grid[i][j]
			} else {
				valueArray[j] = max(valueArray[j] + grid[i][j], valueArray[j-1] + grid[i][j])
			}
		}
	}
    return valueArray[len(valueArray)-1]
}

func max(a int, b int) int{
	if a > b {
		return a
	} 
	return b
}
```
