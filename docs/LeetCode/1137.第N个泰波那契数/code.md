# [1137. 第 N 个泰波那契数](https://leetcode-cn.com/problems/n-th-tribonacci-number/)

## 方法一：普通递归

思路：直接使用递归进行计算，可以得到结果，但是效率比较低，并且**涉及到了重复计算，因此我们可以使用dp进行改进**

缺点：超时

```go
func tribonacci(n int) int {
	if n == 0 {
		return 0
	} else if n == 1 {
		return 1
	} else if n == 2 {
		return 1
	}


	return tribonacci(n-1) + tribonacci(n-2) + tribonacci(n-3)
}
```

## 方法二：dp

思路：复用3个变量进行计算

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了97.44%的用户

```go
func tribonacci(n int) int {
	if n == 0 {
		return 0
	} else if n == 1 {
		return 1
	} else if n == 2 {
		return 1
	}
	p, q, r := 0, 1, 1

	for i := 3; i <= n; i++ {
		p, q, r = q, r, p+q+r
	}

	return r
}
```

## 总结：

还有其他解法，根据面试官的不同要求来实现，如果需要快速获取，我们可以使用一个map来存储对应的值，如果希望节省空间，那么我们就可以使用3个变量来复用进行计算，具体哪种方法最好取决于面试官的需求以及实际场景的应用要求。