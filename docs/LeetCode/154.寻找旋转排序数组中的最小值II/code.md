# [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)





具体思路[看自己的博客文章](http://www.sivan.tech/2020/12/08/%E5%AF%BB%E6%89%BE%E6%97%8B%E8%BD%AC%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E5%80%BC/)

## 方法：二分



```go

func findMin(nums []int) int {
	if len(nums) == 0 {
		return 0
	}

	l, r := 0, len(nums)-1
	for l <= r {
		if nums[l] < nums[r] {
			return nums[l]
		}
		mid := l + (r-l)>>1
		if nums[mid] > nums[r] {
			l = mid + 1
		} else if nums[mid] < nums[r] {
			r = mid
		} else if nums[mid] == nums[r] {
			r = r - 1
		}
	}

	//最后结束时一定有l = r + 1，所以因为r向左移动了一步就直接返回l
	return nums[l]
}
```

