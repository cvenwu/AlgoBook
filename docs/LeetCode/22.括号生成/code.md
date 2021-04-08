# [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

## 方法：回溯

思路：使用两个变量分别表示左右括号各自的个数，然后不断回溯
1. 如果当前左括号的个数小于0或右括号的个数小于0，直接return
2. 如果当前左括号的个数大于右括号的个数小于0，直接return：因为在后面我们就会多出左括号无法匹配
3. 如果左右括号个数都为0，说明此时已经是一种满足题意的情况，直接加入到我们的结果中
4. 假设当前选择左括号，然后递归，然后撤销选择
5. 假设当前选择右括号，然后递归，然后撤销选择

```go

func generateParenthesis(n int) []string {
	var ret []string
	backtrack(&ret, n, n, []byte{})
	return ret
}

//分别我们要返回的结果，左右括号的个数以及当前要加入的括号所在的位置，以及当前我们的路径
func backtrack(ret *[]string, left, right int, path []byte) {
	//如果左括号的个数小于0 或者 右括号的个数小于0 或者 左括号的个数大于右括号的个数
	if left < 0 || right < 0 || left > right {
		return
	}

	//此时如果我们的左右括号个数都等于0，说明此时生成的括号满足题意可以加入我们的结果中
	if left == 0 && right == 0 {
		*ret = append(*ret, string(path))
		return
	}

	//当前选择左括号
	path = append(path, '(')
	//递归到下一层
	backtrack(ret, left-1, right, path)
	//撤销加入的左括号
	path = path[:len(path)-1]

	//当前选择右括号
	path = append(path, ')')
	//递归到下一层
	backtrack(ret, left, right-1, path)
	//撤销加入的右括号
	path = path[:len(path)-1]
}

```

