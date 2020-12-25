# [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

## 方法一：动态规划

```go
func maxProduct(nums []int) int {
	//special case
	if len(nums) <= 0 {
		return 0
	} else if len(nums) == 1 {
		return nums[0]
	}

	//定义两个数组，分别用于存放以当前数结尾的最大连续子数组的最大乘积与最小乘积
	maxs := make([]int, len(nums))
	mins := make([]int, len(nums))
	ret := nums[0]
	//base case
	if nums[0] < 0 {
		maxs[0] = 1
		mins[0] = nums[0]
	} else if nums[0] > 0 {
		maxs[0] = nums[0]
		mins[0] = 1
	} else {
		maxs[0] = 0
		mins[0] = 0
	}

	for i := 1; i < len(nums); i++ {
		maxs[i] = max(maxs[i-1]*nums[i], nums[i], mins[i-1]*nums[i])
		mins[i] = min(maxs[i-1]*nums[i], nums[i], mins[i-1]*nums[i])
		if maxs[i] > ret {
			ret = maxs[i]
		}
	}

	return ret
}

func max(a, b, c int) int {
	ret := a
	if b > a {
		ret = b
	}

	if c > ret {
		ret = c
	}

	return ret
}

func min(a, b, c int) int {
	ret := a
	if a > b {
		ret = b
	}

	if ret > c {
		ret = c
	}

	return ret
}


```

## 方法二【推荐】：代码优化


对方法一中的代码进行优化，参考leetcode题解
发现如果求i位置的最大连续子数组的最大乘积只需要i-1位置的两个状态即可，
因此我们可以只使用两个变量保存i-1位置的两个状态

> 执行用时：4 ms, 在所有 Go 提交中击败了92.02%的用户
		内存消耗：2.6 ms, 在所有 Go 提交中击败了64.94%的用户


```go
func maxProduct(nums []int) int {
	//special case
	if len(nums) <= 0 {
		return 0
	} else if len(nums) == 1 {
		return nums[0]
	}

	//base case，可以简化
	maxs := nums[0]
	mins := nums[0]
	ret := nums[0]

	for i := 1; i < len(nums); i++ {
		maxs, mins = max(maxs*nums[i], nums[i], mins*nums[i]), min(maxs*nums[i], nums[i], mins*nums[i])
		if maxs > ret {
			ret = maxs
		}
	}

	return ret
}

func max(a, b, c int) int {
	ret := a
	if b > a {
		ret = b
	}

	if c > ret {
		ret = c
	}

	return ret
}

func min(a, b, c int) int {
	ret := a
	if a > b {
		ret = b
	}

	if ret > c {
		ret = c
	}

	return ret
}



```

