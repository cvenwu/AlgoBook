# 二维数组的查找


## 方法一：迭代
!> 思路：每次从二维数组的右上角开始搜索，注意数组第一个维度或第二个维度为空的情况
```go
func findNumberIn2DArray(matrix [][]int, target int) bool {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	rows, cols := len(matrix), len(matrix[0])
	i, j := 0, cols-1
	for i < rows && j >= 0 {
		if matrix[i][j] > target { //向左一列
			j--
		} else if matrix[i][j] < target { //向下一行
			i++
		} else {
			return true
		}
	}

	return false
}

```