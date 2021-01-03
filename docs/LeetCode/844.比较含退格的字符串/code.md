# [844.比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)


## 方法：栈
!> 思路：使用两个栈去遍历我们的字符串
			1. 如果遇到退格，如果栈不空，则弹出栈顶，否则继续遍历下一个元素
			2. 如果没有遇到退格，我们直接将元素加入栈中
		    3. 最后比较两个栈的内容是否相等即可，如果长度不相等，那么直接返回false，如果有某一个元素不等，直接返回false
```go
//✅
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2 MB, 在所有 Go 提交中击败了66.96%的用户
func backspaceCompare(S string, T string) bool {
	var sStack, tStack []byte

	for i := 0; i < len(S); i++ {
		if S[i] == '#' {
			//如果栈不为空，弹出栈顶
			if len(sStack) != 0 {
				sStack = sStack[:len(sStack)-1]
			}
			continue
		}

		sStack = append(sStack, S[i])
	}
	http.StatusAccepted

	for i := 0; i < len(T); i++ {
		if T[i] == '#' {
			//如果栈不为空，弹出栈顶
			if len(tStack) != 0 {
				tStack = tStack[:len(tStack)-1]
			}
			continue
		}

		tStack = append(tStack, T[i])
	}

	//比较两个栈是否相等，如果不想等返回false
	if len(sStack) != len(tStack) {
		return false
	}


	for i := 0; i < len(sStack); i++ {
		if sStack[i] != tStack[i] {
			return false
		}
	}

	return true
}
```