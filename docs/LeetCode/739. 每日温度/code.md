# [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

## 方法：单调栈

思路：一旦当前气温遇到一个更高温度的就弹出，我们使用一个单调栈，一旦遇到一个大的就不断弹出小的并进行结算


如果元素相等，我们直接加入栈中，到时候遇到一个大的，直接计算下标差返回即可


> 执行用时：64 ms, 在所有 Go 提交中击败了62.38%的用户
		内存消耗：7.2 ms, 在所有 Go 提交中击败了32.57%的用户


```go
func dailyTemperatures(T []int) []int {
	if len(T) < 0 {
		return nil
	}

	stack := []int{0}

	//建立并初始化我们返回的结果
	ret := make([]int, len(T))

	//将所有数都加入栈中
	for i := 1; i < len(T); i++ {
		//如果栈不空，并且要进入的元素大于当前栈顶元素，需要弹出栈顶元素
		for len(stack) > 0 && T[stack[len(stack)-1]] < T[i] {
			val := stack[len(stack)-1]
			ret[val] = i - val
			stack = stack[:len(stack)-1]
		}
		stack = append(stack, i)
	}

	//如果所有数都加入栈了之后，那么剩下的元素都是没有比他大的所以就默认值就可以了


	return ret
}

```

