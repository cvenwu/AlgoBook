# 左旋转字符串

## 方法一：剑指offer

思路：*剑指offer思路，先旋转前n个字符串，之后旋转剩下的，最后旋转整个字符串*

> *// 执行用时：4 ms, 在所有 Go 提交中击败了 12.19% 的用户*
>
> *// 内存消耗：4.9 MB, 在所有 Go 提交中击败了 14.03% 的用户*

```go
func reverseLeftWords(s string, n int) string {
	if len(s) <= n {
		return s
	}

	//首先反转左边
	Reverse(&s, 0, n-1)
	Reverse(&s, n, len(s)-1)
	Reverse(&s, 0, len(s)-1)

	return s
}

func Reverse(s *string, start, end int) {

	tempS := []rune(*s)
	for start < end {
		tempS[start], tempS[end] = tempS[end], tempS[start]
		start, end = start+1, end-1
	}
	*s = string(tempS)
}
```

**2021-03-05改进**

```go
func reverseLeftWords(s string, n int) string {
	content := []byte(s)
	//步骤1：旋转字符串s的前n个字符与剩下的字符
	reverseWords(content, 0, n-1)
	reverseWords(content, n, len(content)-1)
	//步骤2：旋转字符串s
	reverseWords(content, 0, len(content)-1)
	//步骤3：返回旋转后的
	return string(content)
}

func reverseWords(content []byte, start, end int) {
	for start < end {
		content[start], content[end] = content[end], content[start]
		start, end = start+1, end-1
	}
}

```

## 方法二：使用切片

> *// 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户*
>
> *// 内存消耗：3.3 MB, 在所有 Go 提交中击败了 78.95% 的用户*



```go
func reverseLeftWords(s string, n int) string {
	return s[n:] + s[:n]
}
```

