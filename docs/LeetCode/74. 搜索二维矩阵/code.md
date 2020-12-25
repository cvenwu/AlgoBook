# [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

## 方法：
思路：从矩阵右上角开始查找，之后不断收缩边界

> 执行用时：8 ms, 在所有 Go 提交中击败了83.31%的用户
		内存消耗：3.7 MB, 在所有 Go 提交中击败了56.70%的用户

```go
func searchMatrix(matrix [][]int, target int) bool {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	i, j := 0, len(matrix[0])-1


	for i < len(matrix) && j >= 0 {
		if j >= 0  &&  matrix[i][j] > target {
			j--
		} else if i < len(matrix) && matrix[i][j] < target {
			i++
		} else {
			return true
		}
	}

	return false
}
```

