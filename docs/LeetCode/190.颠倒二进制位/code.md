# [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

## 方法：

思路：不断让num右移，同时每次获取num的最后一位


> 执行用时：4 ms, 在所有 Go 提交中击败了61.69%的用户
> 		内存消耗：2.6 MB, 在所有 Go 提交中击败了5.21%的用户


```go
func reverseBits(num uint32) uint32 {
	var ret uint32
	for i := 0; i < 32; i++ {
		ret = (ret<<1 | num&1)
		num >>= 1
	}
	return ret
}
```

