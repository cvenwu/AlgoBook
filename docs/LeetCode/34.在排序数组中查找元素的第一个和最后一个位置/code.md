# [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)


## 方法一：二分

!> 2019-09写的代码

```go
func searchRange(nums []int, target int) []int {
	return []int{getFirstIndex(nums, target), getLastIndex(nums, target)}
}

//得到target最后一次出现的位置
func getLastIndex(nums []int, target int) int {
	for len(nums) <= 0 {
		return -1
	}

	left, right := 0, len(nums)-1
	for left < right {
		mid := left + (right-left)>>1
		if nums[mid] > target {
			right = mid - 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			//如果已经到了最后一个或者说是最后一个出现该值的直接返回
			if mid == len(nums) - 1 || nums[mid+1] != target {
				return mid
			}
			left = mid + 1 //说明此时既没有到最后，也不是最后一个目标值
		}
	}
	//如果此时nums[left] != target 说明返回-1
	if nums[left] == target {
		return left
	}
	return -1
}

//得到target第一次出现的位置
func getFirstIndex(nums []int, target int) int {
	for len(nums) <= 0 {
		return -1
	}

	left, right := 0, len(nums) - 1
	for left < right {
		mid := left + (right - left) >> 1
		if nums[mid] > target {
			right = mid - 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			//如果已经到了最开始或者是第1个出现的
			if mid == 0 || nums[mid-1] != target {
				return mid
			}

			right = mid - 1
		}
	}

	if nums[left] == target {
		return left
	}

	return -1

}

```

!> 2021-02-25编写的代码
```go
func searchRange(nums []int, target int) []int {

	return []int{getFirstIndex(nums, target), getLastIndex(nums, target)}
}

//得到目标元素第一次出现的位置
func getFirstIndex(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		//首先检查mid是否为目标值
		if nums[mid] > target {
			r = mid - 1
		} else if nums[mid] < target {
			l = mid + 1
		} else if nums[mid] == target {
			if mid == 0 || nums[mid-1] != target {
				return mid
			}
			r = mid - 1
		}
	}
	return -1
}

//得到目标元素的最后一个索引
func getLastIndex(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		mid := l + (r-l)>>1
		//首先检查mid是否为目标值
		if nums[mid] > target {
			r = mid - 1
		} else if nums[mid] < target {
			l = mid + 1
		} else if nums[mid] == target {
			if mid == len(nums)-1 || nums[mid+1] != target {
				return mid
			}
			l = mid + 1
		}
	}
	return -1
}

```