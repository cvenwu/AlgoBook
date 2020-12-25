# [477. 汉明距离总和](https://leetcode-cn.com/problems/total-hamming-distance/)

## 【超时】方法一：暴力


> 时间复杂度：O(n^2)

```go
func totalHammingDistance(nums []int) int {

	sum := 0
	for i := 0; i < len(nums)-1; i++ {
		for j := i+1; j < len(nums); j++ {
			sum += hammingDistance(nums[i], nums[j])
		}
	}
	return sum
}

func hammingDistance(m, n int) int {
	count, xorRet := 0, m ^ n
	for xorRet != 0 {
		count += 1
		xorRet &= (xorRet - 1)
	}
	return count
}
```

## 【推荐】方法二：考虑二进制中的每一位

思路：因此，我们考虑数组中每个数二进制的第 i 位，假设一共有 t 个 0 和 n - t 个 1，那么显然在第 i 位的汉明距离的总和为 t * (n - t)。

步骤：
1. 统计所有数在某一位上位1的数目，如果某一位上有k个相同，剩下n-k个不相同，那么该位上的汉明距离为k*(n-k)，
2. 依次类推求出32位上的各个位的汉明距离，求和并返回

关键点：如何统计每一位上位1的数目，右移固定长度之后与上1


```go

func totalHammingDistance(nums []int) int {
	total := 0
	n := len(nums)
	for i := 0; i <= 31; i++ {
		bitCount := 0
		for _, num := range nums {
			if (num >> i & 1) == 1 {
				bitCount += 1
			}
		}
		total += bitCount * (n - bitCount)
	}

	return total
}

```

