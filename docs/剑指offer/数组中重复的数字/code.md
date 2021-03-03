# 数组中重复的数字

## 方法一：参考剑指offer思路
```go
func findRepeatNumber(nums []int) int {
	//遍历数组中的每个元素
	for i := 0; i < len(nums); i++ {
		//如果当前遍历到的数不在最终的索引上，说明还没有放置好
		for i != nums[i] {
			if nums[i] == nums[nums[i]] {  //如果当前数与最终应该放置位置的数相同，说明找到了重复元素
				return nums[i]
			}

			//否则，交换当前数与最终应该放置位置的数
			nums[i], nums[nums[i]] = nums[nums[i]], nums[i]
		}
	}

	return -1
}
```