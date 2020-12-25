# [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

## 方法：前缀和 + 哈希表

思路：使用一个哈希表保存前缀和，其中key为前缀和的值，val为出现的次数
		步骤：

1. 首先对哈希表加入一个特殊的键值对，即前缀和为0的出现了1次 加入0-1，避免我们其中某一个元素就是我们的目标值，但是前缀和为0的不存在map中
2. 遍历数组中的每个数
   2.1 将当前数加入到sum中
   2.2 map中键值对修改，key为sum对应的val+1
   2.3 查看sum - k 在map中出现的次数，并让ret加上（因为我们map中求到的是以当前遍历的数为止的前缀和，所以不用担心顺序问题，也就是说我们此时加的是连续子数组以当前数结尾并且值为k出现的次数）
3. 返回ret

注意点：
   1. 最开始的时候，对哈希表加入一个特殊的键值对，即前缀和为0的出现了1次 加入0-1，避免我们其中某一个元素就是我们的目标值，但是前缀和为0的不存在map中
   2. 之后每次我们需要计算以当前遍历到的元素结尾的和为k的连续子数组出现的次数


> 执行用时：28 ms, 在所有 Go 提交中击败了74.93%的用户
> 		内存消耗：6.5 ms, 在所有 Go 提交中击败了26.19%的用户

```go
func subarraySum(nums []int, k int) int {
	//如果数组长度为0直接返回
	if len(nums) == 0 {
		return 0
	}

	//用来记录到当前数的和
	sum := 0
	//记录结果，也就是我们要返回的多少个和为k的连续子数组
	ret := 0
	//使用map保存前缀和，key为前缀和,value 为前缀和出现的次数
	prefixSumMap := make(map[int]int)
	prefixSumMap[0] = 1

	//遍历数组中的每个元素，如果k-当前元素出现在前缀和map中，ret就加上次数就是我们想要的，之后设置map让当前前缀和出现次数+1
	for i := 0; i < len(nums); i++ {
		sum += nums[i]
		//如果出现在map中
		if val, ok := prefixSumMap[sum - k]; ok {
			ret += val
		}
		prefixSumMap[sum] += 1
	}

	return ret
}
```

