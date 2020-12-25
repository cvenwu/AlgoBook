# [946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)

## 方法：


思路：按照题目中说明的，我们直接使用一个空栈进行模拟即可

步骤：因为题目中说明了每个值都是不重复的，所以一旦我们加入元素的值等于popped的值我们就弹出
1. 遍历pushed中的每个元素，
   如果与popped当前元素不相等，我们直接加入
   如果与popped当前元素相等，我们使用for循环依次判断当前栈顶元素与popped元素是否相同，如果相同就一直弹出同时Popped元素也要加1，
2. 如果pushed遍历完了，且栈不为空，说明原来不是空栈，我们直接返回false


> 执行用时：8 ms, 在所有 Go 提交中击败了86.42%的用户
内存消耗：3.8 MB, 在所有 Go 提交中击败了28.85%的用户


```go
func validateStackSequences(pushed []int, popped []int) bool {
	stack := []int{}


	curPopped := 0
	for _, v := range pushed {
		//如果popped当前元素与我们准备加入栈的元素相同我们直接不加入，跳过该元素，同时使用for循环检查接下来的元素是否与栈顶元素相同并弹出
		if v == popped[curPopped] {
			curPopped += 1
			for len(stack) > 0 && stack[len(stack)-1] == popped[curPopped] {
				stack = stack[:len(stack)-1]
				curPopped++
			}
		} else {
			stack = append(stack, v)
		}
	}


	return len(stack) == 0
}
```

