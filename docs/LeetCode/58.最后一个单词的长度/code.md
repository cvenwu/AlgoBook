# [58. 最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

## 方法一：调用库函数末尾去空格

思路：

1. 使用一个指针指向最后一个字符串，
   1. 如果指针指向的字符为空格，此时直接返回length
   2. 如果指针指向的不是空格，每次length+1,并且不断左移
   

注意：输入的参数中末尾有可能有空格

>执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
>		内存消耗：2.1 ms, 在所有 Go 提交中击败了27.14%的用户


```go
func lengthOfLastWord(s string) int {

	length := 0
	//记得去掉首尾两边的空格
	s = strings.Trim(s, " ")
	for i := len(s) - 1; i >= 0; i-- {
		if s[i] == ' ' {
			break
		}
		length += 1
	}
	return length
}
```

## 方法二：改进。自己实现末尾去空格

**改进：自己用一个循环去掉末尾的空格**

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 ms, 在所有 Go 提交中击败了75.25%的用户

```go
func lengthOfLastWord(s string) int {

	length := 0
	//自己手动实现去掉尾部的空格
	i := len(s)-1
	for i >= 0 && s[i] == ' ' {
		i -= 1
	}
	for ; i >= 0; i-- {
		if s[i] == ' ' {
			break
		}
		length += 1
	}
	return length
}
```

