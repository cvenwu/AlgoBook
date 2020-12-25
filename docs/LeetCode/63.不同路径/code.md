# [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

## 方法一：dp


这里自己觉得不可以使用状态压缩

步骤：
1. 明确base case ：第一行遇到一个障碍物，则该障碍物开始一直到该行结束的走法为0
2. 状态：`dp[i][j]` 表示走到i,j的走法
3. 选择：可以选择向右或者向下，但是如果右边是障碍物只可以向下
4. dp方程：
      如果当前位置就有障碍物，直接进入下一个，因为有障碍物不用计算使用默认的0就可以
      `dp[i][j] = dp[i-1][j] + dp[i][j-1]` 要求两边都不可以有障碍物
      `dp[i][j] = dp[i-1][j]` 如果左边有障碍物
      `dp[i][j] = dp[i][j-1]` 如果右边有障碍物

其实dp方程可以简写为
   `dp[i][j] = dp[i-1][j] + dp[i][j-1]` 如果i,j上没有障碍物则= 0 如果i,j上没有障碍物

最终的结果我们就放置在了dp的最后一行的最后一列中

![ZcriLR](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/ZcriLR.png)

> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：2.6 MB, 在所有 Go 提交中击败了 20.64% 的用户



```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {

	//输入非法判断
	if len(obstacleGrid) <= 0 || len(obstacleGrid[0]) <= 0 {
		return 0
	}

	matrix := make([][]int, len(obstacleGrid))
	for i := 0; i < len(obstacleGrid); i++ {
		matrix[i] = make([]int, len(obstacleGrid[0]))
	}

	//初始化第一行
	for i := 0; i < len(obstacleGrid[0]); i++ {
		//说明有障碍物，直接退出
		if obstacleGrid[0][i] == 1 {
			break
		}
		matrix[0][i] = 1
	}

	//初始化第1列
	for i := 0; i < len(obstacleGrid); i++ {
		//说明有障碍物，直接退出
		if obstacleGrid[i][0] == 1 {
			break
		}
		matrix[i][0] = 1
	}

	//接下来开始走路
	for i := 1; i < len(obstacleGrid); i++ {
		for j := 1; j < len(obstacleGrid[0]); j++ {
			//如果当前就是一个障碍物直接跳过不用管
			if obstacleGrid[i][j] == 1 {
				continue
			}

			//如果上边没有障碍物
			if obstacleGrid[i-1][j] != 1 {
				matrix[i][j] += matrix[i-1][j]
			}
			//如果左边没有障碍物
			if obstacleGrid[i][j-1] != 1 {
				matrix[i][j] += matrix[i][j-1]
			}
		}
	}

	return matrix[len(matrix)-1][len(matrix[0])-1]

}


```

## 方法二：dp进行代码改进

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {

	//输入非法判断
	if len(obstacleGrid) <= 0 || len(obstacleGrid[0]) <= 0 {
		return 0
	}

	matrix := make([][]int, len(obstacleGrid))
	for i := 0; i < len(obstacleGrid); i++ {
		matrix[i] = make([]int, len(obstacleGrid[0]))
	}

	//代码改进点1：初始化第一行,如果有障碍物，直接退出
	for i := 0; i < len(obstacleGrid[0]) && obstacleGrid[0][i] != 1; i++ {
		matrix[0][i] = 1
	}

	//代码改进点1：初始化第1列,如果有障碍物，直接退出
	for i := 0; i < len(obstacleGrid) && obstacleGrid[i][0] != 1; i++ {
		matrix[i][0] = 1
	}

	//接下来开始走路
	for i := 1; i < len(obstacleGrid); i++ {
		for j := 1; j < len(obstacleGrid[0]); j++ {
			//如果当前就是一个障碍物直接跳过不用管
			if obstacleGrid[i][j] == 1 {
				continue
			}
			//代码改进点2：
			matrix[i][j] = matrix[i][j-1] + matrix[i-1][j]
		}
	}
	return matrix[len(matrix)-1][len(matrix[0])-1]
}

```

