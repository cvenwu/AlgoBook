# [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

## 方法一：普通dp

也就是使用一个二维数据进行状态转移，这里不再进行赘述

## 方法二：状态压缩的dp

步骤：
1. base case 使用一个一维数组，初始全为1，长度为列数
2. 状态定义：matrix[i] 到达第i列的走法
3. 选择：可以选择向下或者向右
4. dp方程：matrix[i] = matrix[i-1] + matrix[i]

**最终的结果为matrix的最后一个元素**

时间复杂度：O(m * n)
		空间复杂度：O(n)



> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了 89.04% 的用户

```go
func uniquePaths(m int, n int) int {
	path := make([]int, n)

	//初始化状态矩阵
	for i := 0; i < n; i++ {
		path[i] = 1
	}

	//之后不断走路并进行选择
	//直接跨过第一行，因为我们初始化的时候就是第一行
	for i := 1; i < m; i++ {
		//这里直接跨过第1列，因为第1列所有的值都是1
		for j := 1; j < n; j++ {
			path[j] = path[j] + path[j-1]
		}
	}

	return path[len(path)-1]
}
```

