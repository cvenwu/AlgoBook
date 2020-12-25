# 数组中数值和下标相等的元素

题目描述：假设一个单调递增的数组里的每个元素都是整数并且是唯一的。请编程实现一个函数，找出数组中任意一个数值等于其下标的元素。例如，在数组[-3, -1, 1, 3, 5]中，数字3和它的下标相等

思路：

**利用性质**：

1. 当数字的值大于对应的下标时，那么该数字右边一定也是数字的值大于下标
2. 当数字的值小于对应的下标时，那么该数字左边的元素的值一定小于下标

例子：假设数字的值m > 下标值i时，对k > 0 有 m + k > i + k，因而下标值为i时，值大于对应下标时，右边所有的值一定都大于对应的下标

```go
package main

import "fmt"

//找不到就返回-1 ，因为我们要找数值和下标相等的元素，因此如果找到结果肯定>=0
func search(nums []int, target int) int {
	if len(nums) <= 0 {
		return -1
	}

	start, end := 0, len(nums)-1
	for start <= end {
		mid := start + (end-start)>>1
		if nums[mid] == mid {
			return mid
		} else if nums[mid] < mid { //说明mid以及mid左边对应的下标一定大于对应的值
			start = mid + 1
		} else { //说明mid以及mid右边对应的下标一定小于对应的值
			end = mid - 1
		}
	}
	//执行到这里说明没有找到
	return -1
}

func main() {
	nums := []int{5, 7, 7, 8, 8, 10}
	fmt.Println(search(nums, 8))
	fmt.Println(nums == nil)
}

```

