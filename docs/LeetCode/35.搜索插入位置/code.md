# [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

## 方法一：顺序查找

时间复杂度：O(N)

> 执行用时：4 ms, 在所有 Go 提交中击败了90.20%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func searchInsert(nums []int, target int) int {
	index := 0
	for i := 0; i < len(nums); i++ {
		if nums[i] > target {
			break
		}
		index = i
	}
	if len(nums) != 0 && nums[index] < target {
		index += 1
	}
	return index
}
```



## 方法二：自己写的二分查找

思路：题目中说了是有序数组，需要进行后处理，因为可能最后一个元素小于目标值，我们需要插入到最后一个元素索引的下一个，所以需要和最后一个元素比较一下。

> 执行用时：4 ms, 在所有 Go 提交中击败了90.20%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func searchInsert(nums []int, target int) int {

	if len(nums) == 0 {
		return 0
	}

	left, right := 0, len(nums)-1

	for left < right {
		mid := left + (right - left) >> 1
		if nums[mid] > target {
			right = mid
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			return mid
		}
	}

	//后处理
	//需要验证nums[left]是否为目标值，如果不是说明没有目标值，且目标值需要放在最后一个位置
	if nums[left] < target {
		left += 1
	}

	return left
}

```

## 方法三：LeetCode上的二分查找

思路：使用了二分查找模板I

> 执行用时：4 ms, 在所有 Go 提交中击败了90.20%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func searchInsert(nums []int, target int) int {

	left, right := 0, len(nums)-1

	for left <= right {
		mid := left + (right - left) >> 1
		if nums[mid] > target {
			right = mid - 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			return mid
		}
	}

	return left
}

```

