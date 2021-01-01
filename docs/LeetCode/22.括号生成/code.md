# [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

## 方法：回溯

```go
func isValid(path string) bool {
	stack := []uint8{}
	for i := 0; i < len(path); i++ {
		if path[i] == ')' && stack[len(stack)-1] == '(' {
			stack = stack[:len(stack)-1]
		} else {
			stack = append(stack, path[i])
		}
	}
	return len(stack) == 0
}

func backtrack(path string, n int, ret *[]string, symbol string) {
	if len(path) == n*2 && isValid(path) {
		*ret = append(*ret, path)
		return
	}
	for i := 0; i < len(symbol); i++ {
		path += string(symbol[i])
		backtrack(path, n, ret, symbol)
		path = path[:len(path)-1]
	}

}

func generateParenthesis(n int) []string {
	if n <= 0 {
		return nil
	}
	ret := []string{}
	symbol := "()"
	backtrack("", n, &ret, symbol)
	return ret
}
```

