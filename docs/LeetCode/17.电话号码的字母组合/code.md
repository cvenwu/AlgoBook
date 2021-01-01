# [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

## 方法：回溯

```go
func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}
	ret := []string{}
	matrix := [][]byte{
		{'a', 'b', 'c'},
		{'d', 'e', 'f'},
		{'g', 'h', 'i'},
		{'j', 'k', 'l'},
		{'m', 'n', 'o'},
		{'p', 'q', 'r', 's'},
		{'t', 'u', 'v'},
		{'w', 'x', 'y', 'z'},
	}

	backtrack(matrix, 0, digits, []byte{}, &ret)

	return ret
}

func backtrack(matrix [][]byte, index int, digits string, path []byte, ret *[]string) {
	if index == len(digits) {
        temp := make([]byte, len(path))
		copy(temp, path)
		*ret = append(*ret, string(temp))
		return
	}

	for i := 0; i < len(matrix[digits[index]-'2']); i++ {
		path = append(path, matrix[digits[index]-'2'][i])
		backtrack(matrix, index+1, digits, path, ret)
		path = path[:len(path)-1]
	}
}
```

