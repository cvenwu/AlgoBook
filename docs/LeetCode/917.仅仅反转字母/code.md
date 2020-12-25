# [917. 仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)

## 方法：对撞指针思想进行反转


思路：使用对撞指针思想进行反转，每次让两个指针指向字母，之后进行反转


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.6 ms, 在所有 Go 提交中击败了36.00%的用户

```go
func reverseOnlyLetters(S string) string {

	s := []byte(S)
	start, end := 0, len(s)-1

	for start < end {
		for start < end && !isValidCharacter(s[start]) {
			start += 1
		}

		for start < end && !isValidCharacter(s[end]) {
			end -= 1
		}
		s[start], s[end] = s[end], s[start]
	}

	return string(s)
}


func isValidCharacter(num uint8) bool {
	if (num >= 97 && num <= 122) || (num >= 65 && num <= 90) {
		return true
	}

	return false
}

```

