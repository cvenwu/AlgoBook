# [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

## 方法一：DFS


通过visited记录是否访问过
> 执行用时：8 ms, 在所有 Go 提交中击败了 30.06% 的用户
			内存消耗：3.7 MB, 在所有 Go 提交中击败了 11.61% 的用户


```go
func exist(board [][]byte, word string) bool {
	row, col := len(board), len(board[0])
	visited := make([][]bool, row)
	for i := 0; i < row; i++ {
		visited[i] = make([]bool, col)
	}
	for i := 0; i < row; i++ {
		for j := 0; j < col; j++ {
			if backtrack(visited, board, word, i, j, 0, row, col) {
				return true
			}
		}
	}
	return false
}

//第3,4个参数表示当前从board的哪个位置开始判断
//start表示当前是从word的第几个字符开始判断
func backtrack(visited [][]bool, board [][]byte, word string, rowIndex, colIndex, start, row, col int) bool {
	//如果当前越界了需要返回false
	//如果当前访问过了需要返回false
	//如果没有越界没有访问过并且值不相等，需要返回false
	if rowIndex >= row || rowIndex < 0 || colIndex >= col || colIndex < 0 || word[start] != board[rowIndex][colIndex] || visited[rowIndex][colIndex] {
		return false
	}

	//如果当前已经到了最后一个字符，并且走到这里说明满足上面的条件
	if len(word) == start+1 {
		return true
	}
	//表明这里访问过
	visited[rowIndex][colIndex] = true
	//如果走到这里说明还没有匹配完并且当前字符是匹配的，我们需要进行深层次的递归，只要有一个符合就返回true
	haspath := backtrack(visited, board, word, rowIndex+1, colIndex, start+1, row, col) ||
		backtrack(visited, board, word, rowIndex-1, colIndex, start+1, row, col) ||
		backtrack(visited, board, word, rowIndex, colIndex+1, start+1, row, col) ||
		backtrack(visited, board, word, rowIndex, colIndex-1, start+1, row, col)
	//走到这里说明这个字符虽然可以走，但是走了这个字符后面没有路径可以到达目的地
	if !haspath {
		visited[rowIndex][colIndex] = false
	}
	return haspath
}
```

## 方法二：dfs改进

改进点：使用一个变量临时存储我们要遍历的字符，如果访问过将该节点的值置换为'/'，之后重新回到该节点将其替换为原来的值


> 执行用时：4 ms, 在所有 Go 提交中击败了 98.06% 的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了 85.61% 的用户

```go
func exist(board [][]byte, word string) bool {
	row, col := len(board), len(board[0])
	for i := 0; i < row; i++ {
		for j := 0; j < col; j++ {
			if backtrack(board, word, i, j, 0, row, col) {
				return true
			}
		}
	}
	return false
}

//第3,4个参数表示当前从board的哪个位置开始判断
//start表示当前是从word的第几个字符开始判断
func backtrack(board [][]byte, word string, rowIndex, colIndex, start, row, col int) bool {
	//如果当前越界了需要返回false
	//如果当前访问过了需要返回false
	//如果没有越界没有访问过并且值不相等，需要返回false
	if rowIndex >= row || rowIndex < 0 || colIndex >= col || colIndex < 0 || word[start] != board[rowIndex][colIndex] || board[rowIndex][colIndex] == '/' {
		return false
	}

	//如果当前已经到了最后一个字符，并且走到这里说明满足上面的条件
	if len(word) == start+1 {
		return true
	}
	//标记这里访问过
	tmp := board[rowIndex][colIndex]
	board[rowIndex][colIndex] = '/'
	//如果走到这里说明还没有匹配完并且当前字符是匹配的，我们需要进行深层次的递归，只要有一个符合就返回true
	haspath := backtrack(board, word, rowIndex+1, colIndex, start+1, row, col) ||
		backtrack(board, word, rowIndex-1, colIndex, start+1, row, col) ||
		backtrack(board, word, rowIndex, colIndex+1, start+1, row, col) ||
		backtrack(board, word, rowIndex, colIndex-1, start+1, row, col)
	//走到这里说明这个字符虽然可以走，但是走了这个字符后面没有路径可以到达目的地
	board[rowIndex][colIndex] = tmp
	return haspath
}
```