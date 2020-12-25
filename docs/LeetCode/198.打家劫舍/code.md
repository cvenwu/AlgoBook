# [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

## 方法一：自己的dp

步骤：
1. 明确base case：dp[0][0]=0,
               dp[0][1]=nums[0]
2. 状态：dp[i][0]，
         其中第2维取值0，1分别表示不偷，偷。
         其中第1维表示偷到第i个房屋时可以获取的最大偷窃金额
3. 选择：偷 或 不偷
4. 状态转移方程：
      dp[i][0] = max(dp[i-1][0], dp[i-1][1])
      dp[i][1] = dp[i-1][0] + nums[i]


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了5.44%的用户

```go
func rob(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}

	dp := [][]int{{0, nums[0]}}

	for i := 1; i < len(nums); i++ {
		dp = append(dp, []int{max(dp[i-1][0], dp[i-1][1]), dp[i-1][0] + nums[i]})
	}

	return max(dp[len(nums)-1][0], dp[len(nums)-1][1])

}
func max(num, num2 int) int {
	if num > num2 {
		return num
	}

	return num2
}
```



## 方法二：LeetCode上的dp


步骤：
1. 明确base case：dp[0]=nums[0],
2. 状态：dp[i]表示前 i 间房屋能偷窃到的最高总金额
3. 选择：偷 或 不偷
4. 状态转移方程：
      dp[i] = max(dp[i-1], dp[i-2]+nums[i])

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了57.77%的用户

```go
func rob(nums []int) int {
	if len(nums) <= 0 {
		return 0
	}

	dp := []int{nums[0]}

	for i := 1; i < len(nums); i++ {
		dp = append(dp, max(nums[i] + dp[i-2], dp[i-1]))
	}

	return dp[len(nums)-1]

}

func max(num, num2 int) int {
	if num > num2 {
		return num
	}

	return num2
}
```



## 方法三：参考小浩算法

`dp[i]`表示偷盗至第i个房屋时，已经累计获取的最大偷窃金额

后来自己也参考思路改进了

```go
func rob(nums []int) int {

	if len(nums) <= 0 {
		return 0
	}

	if len(nums) == 1 {
		return nums[0]
	}

	//记得更新数组中第2个元素的值
	if len(nums) == 2 {
		return max(nums[0], nums[1])
	}
	nums[1] = max(nums[0], nums[1])
	//状态转移方程
	for i := 2; i < len(nums); i++ {
		nums[i] = max(nums[i-1], nums[i-2]+nums[i])
	}

	//返回最大值
	return nums[len(nums)-1]
}

func max(num, num2 int) int {
	if num > num2 {
		return num
	}

	return num2
}

```



自己上次提交的错误代码：

错误原因：之前自己提交的这一行代码给nums[1]赋值只会在数组长度为2的时候进行赋值

```go
func rob(nums []int) int {

	if len(nums) <= 0 {
		return 0
	}

	if len(nums) == 1 {
		return nums[0]
	}

	//记得更新数组中第2个元素的值
	if len(nums) == 2 {
		nums[1] = max(nums[0], nums[1])
	}

	//状态转移方程
	for i := 2; i < len(nums); i++ {
		nums[i] = max(nums[i-1], nums[i-2]+nums[i])
	}

	//返回最大值
	return nums[len(nums)-1]
}
```

