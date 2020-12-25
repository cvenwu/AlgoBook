# [1295. 统计位数为偶数的数字](https://leetcode-cn.com/problems/find-numbers-with-even-number-of-digits/)

## 方法一：确定位数为偶数的数字所在范围

因为要统计位数为偶数，所以在10-99以及1000-9999以及数字为100000的数字位数都是偶数
	**题目中给定了数组中每个数字的范围从0-10^5**

> 执行用时：4 ms, 在所有 Go 提交中击败了90.45%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func findNumbers(nums []int) int {

	count := 0
	for _, num := range nums {
		if num >= 10 && num <= 99 {
			count += 1
		} else if num >= 1000 && num <= 9999 {
			count += 1
		} else if num == 100000 {
			count += 1
		}

	}
	return count
}
```

## 另外两种解法[参考](https://leetcode-cn.com/problems/find-numbers-with-even-number-of-digits/solution/tong-ji-wei-shu-wei-ou-shu-de-shu-zi-by-leetcode-s/)

