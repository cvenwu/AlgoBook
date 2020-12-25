# [1480. 一维数组的动态和](https://leetcode-cn.com/problems/running-sum-of-1d-array/)

## 方法一：dp

思路：

1. 初始状态：ret[0] = nums[0]

2. 状态转移方程ret[i] = ret[i-1] + nums[i]


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了84.85%的用户

```go
func runningSum(nums []int) []int {

	if len(nums) <= 0 {
		return []int{}
	}

	ret := make([]int, len(nums))
	//初始化状态
	ret[0] = nums[0]

	//进行状态转移
	for i := 1; i < len(nums); i++ {
		ret[i] = ret[i-1] + nums[i]
	}

	return ret
}
```



## 方法二：dp + 状态压缩

> 改进：直接修改传入的数组(**需要征得面试官同意**)。主要思路同方法一

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：1.9 MB, 在所有 Go 提交中击败了84.85%的用户



```go
func runningSum(nums []int) []int {

	if len(nums) <= 0 {
		return []int{}
	}

	//进行状态转移
	for i := 1; i < len(nums); i++ {
		nums[i] = nums[i-1] + nums[i]
	}

	return nums
}
```





## 注意点

1. **面试修改传入的数组，需要征得面试官同意**