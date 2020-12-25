



# [290. 单词规律](https://leetcode-cn.com/problems/word-pattern/)

关键点：**建立双向关系**，例如`abba` 与`dog dog dog dog`是匹配的，但是如果反过来就不匹配了，因此我们需要考虑双向关系。
		**题目中说明了：pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。**因此我们**定义两个map**使得pattern中的字母可以合法匹配str中的每个非空单词，以及str中的每个非空单词可以合法匹配pattern中的字母

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
			内存消耗：2 MB, 在所有 Go 提交中击败了100.00%的用户


```go
func wordPattern(pattern string, str string) bool {
	strs := strings.Split(str, " ")
	if len(strs) != len(pattern) {
		return false
	}

	mymap := make(map[uint8]string)

	for i := 0; i < len(pattern); i++ {
		if val, ok := mymap[pattern[i]]; ok {
			if val != strs[i] {
				return false
			}
		} else { //不存在于map中
			mymap[pattern[i]] = strs[i]
		}
	}

	mymap2 := make(map[string]uint8)

	for i := 0; i < len(strs); i++ {
		if val, ok := mymap2[strs[i]]; ok {
			if val != pattern[i] {
				return false
			}
		} else { //不存在于map中
			mymap2[strs[i]] = pattern[i]
		}
	}


	return true
}
```



