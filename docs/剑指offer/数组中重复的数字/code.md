# 数组中重复的数字

## 方法一：参考剑指offer思路


自己将思路总结成如下伪代码：
```

遍历数组中的元素,设当前遍历到的元素的下标为i
	1. 如果下标i对应的元素与i不等
		1.1 如果nums[i]与nums[nums[i]]相等，则返回nums[i]：
				说明此时有重复，没有必要进行交换，如果交换的话一直会执行当前的if条件，从而造成死循环
		1.2 否则交换nums[i]与nums[nums[i]]

走到这里说明没有重复数字，直接返回-1即可。
```

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