





# [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

## 方法一：

思路：新建一个数组，存储不重复的元素。之后将该数组的元素复制给原数组

> 执行用时：12 ms, 在所有 Go 提交中击败了37.98%的用户
> 		内存消耗：5.2 ms, 在所有 Go 提交中击败了5.26%的用户

```go
func removeDuplicates(nums []int) int {
	if len(nums) <= 1 {
		return len(nums)
	}

	num := []int{nums[0]}

	for _, val := range nums[1:] {
		if val != num[len(num)-1] {
			num = append(num, val)
		}
	}

	for i := 0; i < len(num); i++ {
		nums[i] = num[i]
	}

	return len(num)
}
```

## 方法二【推荐】：

思路：使用双指针，两个指针都存储元素的下标，第一个指针指向第一个重复的元素，第二个指针指向下一个要放的元素。

具体细节看代码注释即可


> 执行用时：4 ms, 在所有 Go 提交中击败了99.35%的用户
> 		内存消耗：4.6 ms, 在所有 Go 提交中击败了99.91%的用户


```go
func removeDuplicates(nums []int) int {
	if len(nums) <= 1 {
		return len(nums)
	}

	left, right := 0, 1

	for right <= len(nums)-1 {
		//让right指向left指向的下一个不重复元素
		//为什么使用nums[left]>=nums[right]，因为数组升序，并且left是已经放置到最终位置的元素
		//因此我们下一个要放置的元素一定比left指向的元素大
		for right <= len(nums)-1 && nums[left] >= nums[right] {
			right += 1
		}

		//如果right已经越界，说明此时已经有序
		if right >= len(nums) {
			break
		}

		//说明left指向的元素没有重复元素
		if left == right-1 {
			left += 1
		} else {
			nums[left+1], nums[right] = nums[right], nums[left+1]
			left += 1
		}
	}

	return left + 1
}

```

