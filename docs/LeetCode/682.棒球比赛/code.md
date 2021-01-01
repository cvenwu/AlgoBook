# [682. 棒球比赛](https://leetcode-cn.com/problems/baseball-game/)

## 方法：使用一个栈来模拟

思路：使用一个栈来模拟，如果遇到对应的符号执行对应的操作，最后传入的参数都遍历完成之后，我们栈中所有的元素求和并返回结果

>  执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.7 ms, 在所有 Go 提交中击败了23.19%的用户


```go
func calPoints(ops []string) int {

	sumScore := 0
	stack := []int{}
	for _, v := range ops {
		switch v {
		case "C":
			stack = stack[:len(stack)-1]
		case "D":
			stack = append(stack, stack[len(stack)-1]>>1)
		case "+":
			stack = append(stack, stack[len(stack)-1]+stack[len(stack)-2])
		default:
			vInt, _ := strconv.Atoi(v)
			stack = append(stack, vInt)
		}
	}

	for _, v := range stack {
		sumScore += v
	}


	return sumScore
}
```

