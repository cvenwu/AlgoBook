#  [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

## 方法一：brute-force

暴力：直接穷举所有区间的起点和终点，计算区间内所有元素的和与最大值比较

> 执行用时：108 ms, 在所有 Go 提交中击败了5.02%的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了100.00%的用户

时间复杂度：O(N^2)
		空间复杂度：O(1)

```go
func maxSubArray(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}
	max := nums[0]
	sum := 0
	for i := 0; i < len(nums); i++ {
		sum = 0
		for j := i; j < len(nums); j++ { //i,j分别为区间的起始和结束
			sum += nums[j]
			if sum > max {
				max = sum
			}
		}
	}
	return max
}

```





## 方法二：dp

时间复杂度：O(N)
		空间复杂度：O(N)

> 执行用时：8 ms, 在所有 Go 提交中击败了58.79%的用户
> 		内存消耗：3.5 MB, 在所有 Go 提交中击败了17.46%的用户

步骤：


   1. basecase dp[0] = nums[0]
   2. 状态： dp[i] = 连续数组在结束为止为i时的最大和
   3. 选择： 区间结束位置为i，是否需要前面的子序和呢，如果当前值要上前面的子序和变小就不要
   4. dp数组： dp[i] = max(dp[i-1]+nums[i], nums[i])
      最后的结果也就是找dp数组的最大值即可

```go
func maxSubArray(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}
	dp := make([]int, len(nums))
	//初始化状态
	dp[0]=nums[0]
	max := nums[0]

	//进行状态转移
	for i := 1; i < len(nums); i++ {
		if dp[i-1]+nums[i] > nums[i] {
			dp[i] = dp[i-1] + nums[i]
		} else {
			dp[i] = nums[i]
		}

		//如果比最大子序和还要大
		if max < dp[i] {
			max = dp[i]
		}
	}
	return max
}
```

## 方法三：dp进行状态压缩

改进：相比方法二进行改进，复用我们传入进来的数组

时间复杂度：O(N)
		空间复杂度：O(1)

> 执行用时：8 ms, 在所有 Go 提交中击败了58.79%的用户
> 		内存消耗：3.3 MB, 在所有 Go 提交中击败了72.71%的用户

```go
func maxSubArray(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}
	max := nums[0]
	for i := 1; i < len(nums); i++ {
		if nums[i-1]+nums[i] > nums[i] {
			nums[i] = nums[i-1] + nums[i]
		}

		//如果比最大子序和还要大
		if max < nums[i] {
			max = nums[i]
		}
	}
	return max
}
```

