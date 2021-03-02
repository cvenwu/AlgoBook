# [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

## 方法一：回溯

注意：注意代码块1`if index == len(word) {`一定要放在代码块2`if x >= len(board) || x < 0 || y >= len(board[0]) || y < 0 {`之前，不然我们下一次访问的时候已经满足题意，但是x和y中肯定有一个越界，所以我们要让结果判断(代码块1)在前

```go
func exist(board [][]byte, word string) bool {
	if len(board) == 0 || len(board[0]) == 0 {
		return false
	}

	rows, cols := len(board), len(board[0])

	used := make([][]bool, rows)
	for i := 0; i < rows; i++ {
		used[i] = make([]bool, cols)
	}

	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if existCore(board, word, 0, used, i, j) {
				return true
			}
		}
	}

	return false
}

//第三个参数是表示当前要找目标字符串的第几个字符
//第四个参数是判断是否使用过
//第五个参数以及第六个参数表示当前遍历到的位置
func existCore(board [][]byte, word string, index int, used [][]bool, x, y int) bool {

	//说明此时已经搜索到了结果
	//代码块1
	if index == len(word) {
		return true
	}

	//递归结束条件
	//代码块2
	//todo: 注意代码块1一定要放在代码块2之前，不然我们下一次访问的时候已经满足题意，但是x和y中肯定有一个越界，所以我们要让结果判断(代码块1)在前
	if x >= len(board) || x < 0 || y >= len(board[0]) || y < 0 {
		return false
	}
	
	//说明这个已经搜索过了，我们直接返回false
	if used[x][y] {
		return false
	}

	//当前位置与我们的字符不匹配直接返回false
	if word[index] != board[x][y] {
		return false
	}

	//走到这里说明当前字符与我们匹配，我们进行下一层递归
	used[x][y] = true
	ret := existCore(board, word, index+1, used, x, y+1) || existCore(board, word, index+1, used, x, y-1) ||
		existCore(board, word, index+1, used, x+1, y) || existCore(board, word, index+1, used, x-1, y)
	used[x][y] = false
	return ret
}

```

## 【推荐】方法二：回溯
改进点：如果访问过将该节点的值置换为'/'，之后重新回到该节点将其替换为原来的值

> 执行用时：4 ms, 在所有 Go 提交中击败了 98.25% 的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了 94.02% 的用户

!> 优化之后的代码：**置换我们原来的标记位，遍历之后再复原，可以少用一个二维数组的空间**

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

