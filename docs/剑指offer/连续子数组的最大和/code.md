# 连续子数组的最大和



## 方法一：找规律

*思路：剑指offer*

1. 如果前面一个数的连续子数组最大和大于0，则将其加上当前数，并与已经保存的最大连续子数组的和比较，如果大就替换*

2. 如果前面一个数的连续子数组最大和小于0，则将连续子数组的最大和赋值为当前数，并与已经保存的最大连续子数组的和比较，如果大就替换*

```go
func maxSubArray(nums []int) int {
	if nums == nil || len(nums) <= 0 { //非法情况
		return -9999999
	}

	//greatesSubArraySum用来求连续子数组的最大和
	greatesSubArraySum, curSum := nums[0], nums[0]

	for i := 1; i < len(nums); i++ {
		if curSum > 0 {
			curSum += nums[i]
		} else {
			curSum = nums[i]
		}
		if curSum > greatesSubArraySum {
			greatesSubArraySum = curSum
		}
		
	}

	return greatesSubArraySum
}
```





## 方法二：dp

用函数f(i)表示以第i个数字结尾的子数组的最大和，那么我们最后的结果便是max(f[i]), 其中 0<=i<=n

>  f[i] = pData[i]  当 i == 0 || f[i-1] <= 0

>  f[i] = f[i-1] + pData[i] 当i > 0 && f[i-1] > 0

这个公式的意义便是：

1. 当以第i-1个数字结尾的子数组的最大和为小于0时，如果将第i个数字与第i-1个数字结尾的子数组的最大和相加将会小于第i个数字，所以这种情况下，第i个数字结尾的连续子数组最大和便是自己

2. 如果第i-1个数字结尾的子数组的最大和大于0，那么第i个数字累加就得到以第i个数字结尾的子数组中所有数字的和

```go
func max(nums []int) int {
	max := nums[0]
	for i := 0; i < len(nums); i++ {
		if max < nums[i] {
			max = nums[i]
		}
	}
	return max
}

func maxSubArray(nums []int) int {
	if nums == nil || len(nums) <= 0 { //非法情况
		return -9999999
	}

	//之后采用dp
	dpArray := []int{nums[0]}
	for i := 1; i < len(nums); i++ {
		if dpArray[i-1] < 0 {
			dpArray = append(dpArray, nums[i])
		} else {
			dpArray = append(dpArray, dpArray[i-1]+nums[i])
		}
	}
	return max(dpArray)
}
```

**2021-03-04代码进行精简**
```go
func maxSubArray(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}

	dp := make([]int, len(nums))
	dp[0] = nums[0]
	ret := dp[0]

	for i :=1; i < len(nums); i++ {
		dp[i] = max(dp[i-1]+nums[i], nums[i])
		if ret < dp[i] {
			ret = dp[i]
		}
	}

	return ret
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}
```


## 【状态压缩】方法三：dp

```go
func maxSubArray(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}

	cur := nums[0]
	ret := cur

	for i :=1; i < len(nums); i++ {
		cur = max(cur+nums[i], nums[i])
		ret = max(ret, cur)
	}

	return ret
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}
```

