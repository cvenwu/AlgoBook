# [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

## 方法一：直接反转

> 执行用时：36 ms, 在所有 Go 提交中击败了97.98%的用户
> 		内存消耗：6.3 MB, 在所有 Go 提交中击败了60.28%的用户

```go
func reverseString(s []byte)  {
	for i := 0; i < (len(s) >> 1); i++ {
		s[i], s[len(s)-1-i] = s[len(s)-1-i], s[i]
	}
}
```

## 方法二：使用双指针进行反转

> 执行用时：36 ms, 在所有 Go 提交中击败了97.98%的用户
> 		内存消耗：6.3 MB, 在所有 Go 提交中击败了60.28%的用户

```go
func reverseString(s []byte)  {
	l, r := 0, len(s)-1
	for l < r {
		s[l], s[r] = s[r], s[l]
		l, r = l + 1, r - 1
	}
}
```

