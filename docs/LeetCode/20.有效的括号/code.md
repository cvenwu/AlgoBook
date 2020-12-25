# [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

## 方法一：借助栈+map进行匹配

思路：借助栈来进行匹配，同时使用map来存储右括号和左括号，其中键为右括号，值为左括号，以后遍历到当前字符为右括号，直接使用map[key]得到对应的左括号与栈顶元素相匹配。如果遍历到当前字符为左括号，直接将其压入栈中

注意：

1. 传入的字符串如果长度为奇数一定不是有效的括号
2. **当取栈顶的时候一定要确保栈里面有元素，如果没有直接返回false**


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 ms, 在所有 Go 提交中击败了16.17%的用户



```go
func isValid(s string) bool {
	//如果字符串长度为0返回true
	if len(s) == 0 {
		return true
	} else if len(s)&1 == 1 { //如果字符串长度为奇数直接返回false
		return false
	}

	mymap := map[string]string{
		")": "(",
		"]": "[",
		"}": "{",
	}

	stack := []string{}
	for _, val := range s {
		if string(val) == "(" || string(val) == "[" || string(val) == "{" {
			stack = append(stack, string(val))
		} else {
			//如果当前字符是右括号看下对应的左括号是否与栈顶一致
			//注意这里必须要加上一个条件len(stack)>0，因为很有可能一开始就是一个右括号
			if len(stack) > 0 && mymap[string(val)] == stack[len(stack)-1] { //如果一致就弹出栈顶
				stack = stack[:len(stack)-1]
			} else { //不一致就返回false
				return false
			}
		}

	}
	//最后检查栈是否为空
	return len(stack) == 0
}

```

