# [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)

这道题最核心的便是构造mask，来求得数字的补数

**如何构造mask：**异或操作其实相当于取反，但是如果构造一个从num最高位的1开始的那一位到最后全为1的mask，之后与数字的补数异或便得到数字的补数

以下两种解法，实质上都是构造同一个mask，只是构造方式不同罢了

## 方法一：

思路：找到num最高位的1所在位左边那位，新建一个数字，让该位右边全为1，此时将这个数与num进行异或。得到的便是我们要的结果

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了42.86%的用户

```go
func findComplement(num int) int {
	mask := 1
	for mask <= num {
		mask <<= 1
	}

	return (mask - 1) ^ num
}
```

## 方法二：

思路：通过最开始的全1，最后一直左移，直到找到num最高位的1左边的位，新建一个数字，让该位(包含)左边全为1，此时将这个数取反后与num进行异或。得到的便是我们要的结果。

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了42.86%的用户


```go
func findComplement(num int) int {
	mask := ^0
	for mask & num > 0 {
		mask <<= 1
	}
	return ^mask ^ num
}
```

