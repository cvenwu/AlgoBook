# [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)

## 方法一：

思路：每次将当前数字 与运算 二进制的11 得到后两位，如果为0或3说明相邻位相同，返回false 。否则将当前数字右移一位继续判断，直到最后为0

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 ms, 在所有 Go 提交中击败了13.33%的用户

```go
func hasAlternatingBits(n int) bool {
	for n != 0 {
		if n & 0b11 == 0 || n & 0b11 == 3 {
			return false
		}
		n >>= 1
	}
	return true
}
```

## 方法二：

思路：每次将当前数字 与运算 二进制的11 得到后两位，如果为0或3说明相邻位相同，返回false 。否则将当前数字右移一位继续判断，直到最后为0

改进点：上面的思路需要每次两两判断相邻位，而本方法直接一次性可以直接判断

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 ms, 在所有 Go 提交中击败了13.33%的用户

```go
func hasAlternatingBits(n int) bool {
	ret := n ^ (n >> 1)  //因为n是相邻二进制位不同，所以右移1位之后与之前的n正好插值。此时异或后得到的后面全是1
	return ret & (ret + 1) == 0	//+1得到一个1后面全为0，同时与上之前的数字一定为0
}
```

