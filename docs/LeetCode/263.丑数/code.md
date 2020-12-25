# [263. 丑数](https://leetcode-cn.com/problems/ugly-number/)

## 方法一：自己的解法


> 执行用时：4ms, 在所有 Go 提交中击败了58.00%的用户
> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了5.06%的用户

```go
func isUgly(num int) bool {
	if num <= 0 {
		return false
	}
	for num != 1 {
		if num % 5 == 0 {
			num /= 5
		} else if num % 3 == 0 {
			num /= 3
		} else if num % 2 == 0 {
			num >>= 1
		} else if num & 1 != 0 { //如果不能被以上3个整除并且此时不为1，直接返回false
			return false
		}
	}
	return true
}
```

## 方法二：

思路：如果能够被5，3，2整除就一直除下去，直到最终判断是否为1即可


> 执行用时：0ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了5.06%的用户


```go
func isUgly(num int) bool {
	/*负数以及0肯定不是丑数*/
	if num <= 0 {
		return false
	}
	/*
	之后如果能够被5，3，2整除就一直除下去，直到最终判断是否为1即可
	 */
	for num % 5 == 0 {
		num /= 5
	}
	for num % 3 == 0 {
		num /= 3
	}
	for num % 2 == 0 {
		num /= 2
	}
	return num == 1
}
```

