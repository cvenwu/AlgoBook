#### [1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/)

## 方法一：自己的解法

思路：将除以2使用右移运算符替代

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了84.85%的用户



```go
func numberOfSteps (num int) int {
	ret := 0
	for num != 0 {
		if num & 1 == 1 {
			num -= 1
		} else {
			num >>= 1
		}
		ret += 1
	}
	return ret
}
```

## 方法二：参照其他人的解法

思路：将除以2使用右移运算符替代

**改进：相比于方法一，如果我们这个数是奇数，那么减去1，其实可以和1进行异或运算**

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了54.55%的用户



```go
func numberOfSteps (num int) int {
	ret := 0
	for num != 0 {
		if num & 1 == 1 {
			num ^= 1
		} else {
			num >>= 1
		}
		ret += 1
	}
	return ret
}
```

