# [55.跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

## 方法一：递归

!> 例如：f(i)可以递归到f(i-(1~nums[i]))，只要里面有一个为true最终f(i)就可以到达

## 方法二：dp
!> 由于上面的递归涉及到了大量的重复计算，因此我们改用dp减少重复计算，但是发现dp效果提交之后不是很好




改用dp->因为判断是否可以到达最后一个位置，如果从前往后进行计算将会无法利用题目中的条件->所以从后往前进行计算->因此转换为f(0)是否可以到达我们的第一个位置

状态定义：f(i)表示i位置是否可以到达最后一个位置
状态转移方程：f(i) = f(i+(1~nums[i]))，如果有1个为true就返回true
            f(i)默认为false,但是最后一个为true
dp反向：从右向左

```go
// 执行用时：248 ms, 在所有 Go 提交中击败了10.56%的用户
// 内存消耗：4.1 MB, 在所有 Go 提交中击败了21.13%的用户

func canJump(nums []int) bool {
	if len(nums) == 0 {
		return false
	}

	dp := make([]bool, len(nums))

	dp[len(dp)-1] = true
	for i := len(dp)-2; i >= 0; i-- {
		for j := 1; j <= nums[i]; j++ {
			if dp[i+j] {
				dp[i] = true
				break
			}
		}
	}

	return  dp[0]
}

```



## 【推荐】方法三：贪心

!> 参考官方题解：使用一个变量代表最远可以到达位置，如果遍历数组过程中，最远可以到达位置大于等于数组长度，那么返回true


![mVDxOq](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/mVDxOq.png)




```go

//方法三：参考官方题解，最远可以到达距离
// 执行用时：8 ms, 在所有 Go 提交中击败了89.75%的用户
// 内存消耗：4 MB, 在所有 Go 提交中击败了68.41%的用户
func canJump(nums []int) bool {

	maxDis := 0
	for i := 0; i < len(nums); i++ {
		if maxDis >= i {
			maxDis = max(maxDis, i+nums[i])
			if maxDis >= len(nums)-1 {
				return true
			}
		}
	}


	return false
}


func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```