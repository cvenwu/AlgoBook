# [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

思路：**异或之后使得不同二进制位上会为1，之后统计异或结果中二进制表达式中1的数目**

> 执行用时：4 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func hammingDistance(x int, y int) int {
	xorRet, count := x ^ y, 0
	for xorRet != 0 {
		count += 1
		xorRet &= (xorRet - 1)
	}

	return count
}
```

