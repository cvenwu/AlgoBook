# [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)





## 方法一：对撞指针


思路：采用对撞指针每次判断两边的字母数字字符，如果相等进入到下一个字母数字字符，否则返回false

步骤：
1. 将所有字母小写
2. 之后left,right分别从左右两边开始移动，直到left = right就停止
      l和r指向的如果不同，就返回false。否则l和r移动到右边和左边的位置，
      继续如上循环


> 执行用时：4 ms, 在所有 Go 提交中击败了96.81%的用户
> 		内存消耗：2.8 MB, 在所有 Go 提交中击败了13.93%的用户

```go
func isPalindrome(s string) bool {
	//空字符串定义为有效的回文串。
	if len(s) <= 1 {
		return true
	}
	s = strings.ToLower(s)
	l, r := 0, len(s)-1
	for l < r {
		for l < r && !isValidCharacter(s[l]) {
			l += 1
		}
		for l < r && !isValidCharacter(s[r]) {
			r -= 1
		}

		if s[l] != s[r] {
			return false
		}

		l, r = l + 1, r - 1
	}

	//走到这里说明是回文串
	return true

}

func isValidCharacter(num uint8) bool {
	if (num >= 97 && num <= 122) || (num >= 48 && num <= 57) {
		return true
	}

	return false
}
```

