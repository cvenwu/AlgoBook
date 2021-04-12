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

**2021-02-25更新的代码**

```go
// 执行用时：8 ms, 在所有 Go 提交中击败了90.10%的用户
// 内存消耗：4 MB, 在所有 Go 提交中击败了100.00%的用户
//设置一个变量表示最远可以到达的位置
//最终判断这个变量是否可以到达最后一个位置

func canJump(nums []int) bool {
	ret := 0  //设置一个变量表示最远可以到达的位置
	i := 0

	//如果当前位置没有越界，并且当前位置在可以最远到达的位置里面就可以遍历
	//遍历的时候要动态更新最远可以到达的位置
	for i < len(nums) && i <= ret {
		//当前走到的位置是i，表示可以到达i
		//动态更新处于当前位置我们可以到达的最远位置
		if ret < i + nums[i] {
			ret = i + nums[i]
		}
		i++
	}

	//最终如果最远可以到达的位置是超过最后一个元素所在的位置就返回true
	return ret >= len(nums)-1
}
```

## 【推荐】自己写的题解
```go
func canJump(nums []int) bool {
	index := nums[0] //表示当前可以跳到的最大索引
	//坑点：注意我们的i可能会比我们数组长度大，因为可能会跑到很远，所以我们要避免越界
	for i := 0; i <= index && i <= len(nums)-1; i++ {
		//因为我们可以到达i位置，并且i位置可以到达的最远的地方是比我们目前可以到达的最远地方大就更新
		if i+nums[i] > index {
			index = i + nums[i]
		}
	}
	return index >= len(nums)-1
}

```