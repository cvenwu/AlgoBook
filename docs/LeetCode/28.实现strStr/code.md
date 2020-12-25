# [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

## 方法一：调用库函数

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 MB, 在所有 Go 提交中击败了99.56%的用户


```go
func strStr(haystack string, needle string) int {
	return strings.Index(haystack, needle)
}
```

## 方法二：自己的解法，双指针

思路：

1. 第1个指针指向第1个字符串的字符，第2个指针指向第2个字符串，每次比较
2. 如果相等，则继续比较第1个字符串后面的字符与第2个字符串后面的字符是否相等，
3. 不相等，第1个指针走到下一个位置，同时第2个指针回到起点位置。继续判断

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 MB, 在所有 Go 提交中击败了99.56%的用户


```go
func strStr(haystack string, needle string) int {

	if len(haystack) == 0 || len(needle) == 0 {
		return 0
	}

	if len(haystack) < len(needle) {
		return -1
	}

	for i := 0; i <= len(haystack) - len(needle); i++ {
		j := 0
		for ; j < len(needle); j++ {
			if (i + j) < len(haystack) && haystack[i+j] != needle[j] {
				break
			}
		}
		if j == len(needle) {
			return i
		}
	}

	//走到这里说明没找到
	return -1
}
```

## 其他方法：

KMP，参考leetCode的官方题解以及评论区