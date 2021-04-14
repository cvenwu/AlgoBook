# [867. 转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

## 方法一：

思路：因为转置前后，行数变成列数，列数变成行数，因此我们则可以使用转置后的列数和行数来循环，同时将内容添加进去，原来在`A[i][j]`现在则在`A[j][i]`

```go
//之前的行数变成了列数
//之前的列数变成了行数
func transpose(matrix [][]int) [][]int {
    rows, cols := len(matrix), len(matrix[0])
    ret := make([][]int, cols)
    for i := 0; i < cols; i++ {
        ret[i] = make([]int, rows)
    }
    for i := 0; i < cols; i++ {
        for j:= 0; j < rows; j++ {
			//这里加入的时候要把之前的行列互换
            ret[i][j] = matrix[j][i]
        }
    }
    return ret
}
```

