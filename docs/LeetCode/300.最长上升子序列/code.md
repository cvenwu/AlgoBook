# [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

## 方法一：

思路：**虽然是一个子序列，但是没有说连续，所以可能不连续**

步骤：

1. base case : dp[0] = 1
2. 状态定义：dp[i] 表示子序列以第i个元素结尾时的长度
3. 状态方程：dp[i] = max(dp[j], dp[k].....前面比第i个元素小的值对应的dp值) + 1
4. 额外的一步：分析状态转移方程不能满足的特殊情况
   最后返回的结果：遍历dp数组返回最大值即可


> 执行用时：12 ms, 在所有 Go 提交中击败了44.35%的用户
> 		内存消耗：2.4 ms, 在所有 Go 提交中击败了28.76%的用户

```go
func lengthOfLIS(nums []int) int {
    if len(nums) <= 1 {
        return len(nums)
    }
    //定义dp数组，dp[i]表示以i为索引的元素在子序列中结尾
    dp := make([]int, len(nums))
    //初始将所有dp元素置为1
    for i := 0; i < len(nums); i++ {
        dp[i] = 1
    }
    max := 0
    for i := 1; i < len(nums); i++ {
        //找到当前比自己前面小的所有元素，并更新自己的dp
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                if dp[j] + 1 > dp[i] {
                    dp[i] = dp[j] + 1
                }
            }
        }
        if max < dp[i] {
            max = dp[i]
        }
    }
    
    //最后找到dp中的最大值
    return max
}
```

