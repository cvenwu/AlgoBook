# [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

## 方法：对撞指针


思路：对撞指针，一个指针从0开始另一个指针从该数的平方根下取整开始一直到满足条件为止，如果中间发现i>=j就可以退出了

两个点：

1. **从0开始的原因**：题目中说了非负整数
2. **为什么要到平方根呢**：因为是两数的平方和等于该数，因此这两个数字一定小于等于该数的平方根

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 ms, 在所有 Go 提交中击败了36.49%的用户


```go
func judgeSquareSum(c int) bool {
	// 对撞指针思想
	// 双指针只用一个指针指向1，另一个指针从该数的平方根开始
	i := 0
	j := int(math.Sqrt(float64(c)))
	tempRet := 0
	for ; i < j; {
		tempRet = i*i + j*j
		if tempRet > c {
			j -= 1
		} else if tempRet < c {
			i += 1
		} else {
			return true
		}
	}

	return false
}

```



**其他方法参考LeetCode**