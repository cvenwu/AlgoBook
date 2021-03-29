# 数组中出现次数超过一半的数字

## 方法一：摩尔投票法

```go
//使用摩尔投票法
func majorityElement(nums []int) int {
	num, times := nums[0], 1
	for i := 1; i < len(nums); i++ {
		if times == 0 {
			num, times = nums[i], 1
			continue
		}

		if num == nums[i] {
			times++
		} else {
			times--
		}
	}

	return num
}
```