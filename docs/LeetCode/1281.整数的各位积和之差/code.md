# [1281. 整数的各位积和之差](https://leetcode-cn.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer/)

## 方法：直接模拟操作即可

>执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
>		内存消耗：1.9 MB, 在所有 Go 提交中击败了40.00%的用户


```go
func subtractProductAndSum(n int) int {
	if n == 0 {
		return 0
	}

	product, sum := 1, 0
	for n != 0 {
		product *= (n % 10)
		sum += (n % 10)
		n /= 10
	}
	return product - sum
}
```

