

# [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

## 方法：

参考左神的书籍上的思路

遍历题目给我们的二维数组，

  1. 如果某一个值为1，我们从当前位置开始进行感染，将与该值相连的1全部感染成2

2. 最后要的岛屿的数量就是我们感染的次数

   最后返回我们感染的次数即可

   

   假设输入的矩阵为M\*N
   **时间复杂度：O(M\*N)**

代码如下：

```go

func infect(matrix [][]byte, i int, j int) {
	//递归结束条件，以及如果当前我们遍历到的值不是1我们直接退出
	if i < 0 || i >= len(matrix) || j < 0 || j >= len(matrix[0]) || matrix[i][j] != '1' {
		return
	}
	//到达这里说明当前值是1我们开始进行感染，需要将值修改为2
	matrix[i][j] = '2'
	//之后我们继续向上下左右4个方向开始进行感染
	infect(matrix, i-1, j)
	infect(matrix, i+1, j)
	infect(matrix, i, j-1)
	infect(matrix, i, j+1)
}

func numIslands(grid [][]byte) int {
	if grid == nil || len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	ret := 0
	for i := 0; i < len(grid); i++ {
		for j := 0; j < len(grid[0]); j++ {
			//如果为1说明之前没有被感染或者说是一个新的1的群体
			if grid[i][j] == '1' {
				ret++
				infect(grid, i, j)
			}
		}
	}

	return ret
}

```





