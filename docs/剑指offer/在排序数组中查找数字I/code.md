# 在排序数组中查找数字I


## 方法一：
!> 思路：获得这个数第一次出现的索引以及最后一次出现的索引，如果没有出现就返回0，否则就是最后一次出现的索引减去第一次出现的索引+1

```go

//找到数字在数组中出现的第一次以及最后一次的索引，最后一次的索引减去第一次的索引然后+1就是我们的次数
func search(nums []int, target int) int {
	if getFirstIndexOfTarget(nums, target) == -1 {
		return 0
	} else {
		return getLastIndexOfTarget(nums, target) - getFirstIndexOfTarget(nums, target) + 1
	}
}


func getFirstIndexOfTarget(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		m := l + (r - l) >> 1
		if nums[m] > target {
			r = m - 1
		} else if nums[m] < target {
			l = m + 1
		} else if nums[m] == target{
			if m != 0 && nums[m-1] == target {
				r = m - 1
			} else {
				return m
			}
		}
	}

	//说明没找到
	return -1
}


func getLastIndexOfTarget(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		m := l + (r - l) >> 1
		if nums[m] > target {
			r = m - 1
		} else if nums[m] < target {
			l = m + 1
		} else if nums[m] == target{
			if m != len(nums)-1 && nums[m+1] == target {
				l = m + 1
			} else {
				return m
			}
		}
	}

	//说明没找到
	return -1
}
```