# [1047. 删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)

## 方法一：【自己写的代码】栈


思路：
1. 循环：
   当遇到一个元素就看一下是否与栈顶相同，
         如果相同就弹出栈顶，
         如果不同就加入栈中，
   之后移动到下一个元素

两个注意点已经列在了代码注释中，一定要注意越界情况

> 执行用时：4 ms, 在所有 Go 提交中击败了78.00%的用户
> 		内存消耗：6.6 ms, 在所有 Go 提交中击败了27.19%的用户


```go
func removeDuplicates(S string) string {
	stack := []int32{}
	for i := 0; i < len(S); i++ {
		//注意点1：这里要注意越界，使用i<len(S)
		for len(stack) > 0 && i < len(S) && stack[len(stack)-1] == int32(S[i]) {
			stack = stack[:len(stack)-1]
			i++
		}

		//注意点2：这里要注意越界，使用i<len(S)：例如输入6个a
		if i < len(S) {
			stack = append(stack, int32(S[i]))
		}
	}

	return string(stack)
}

```

## 【推荐，参考别人的代码】方法二：参考超时100%代码

```go
func removeDuplicates(S string) string {
	ret := make([]byte, len(S))
	for i := range S {
		//需要向栈中添加元素
		if len(ret) == 0 || ret[len(ret)-1] != S[i] {
			ret = append(ret, S[i])
		} else { //需要将栈中的元素弹出
			ret = ret[:len(ret)-1]
		}
	}
	return string(ret)
}

```

