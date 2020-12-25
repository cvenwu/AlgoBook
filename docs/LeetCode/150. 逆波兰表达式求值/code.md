# [150. 逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

## 方法一：自己的代码

思路：使用一个栈模拟

**记笔记：逆波兰表达式：逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。**
**平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) * ( 3 + 4 ) 。**
**该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) * ) 。**

**逆波兰表达式主要有以下两个优点：**

1. **去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + * 也可以依据次序计算出正确结果。**
2. **适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。**

> 执行用时：4 ms, 在所有 Go 提交中击败了88.42%的用户
		内存消耗：4.1 MB, 在所有 Go 提交中击败了42.85%的用户


```go
func evalRPN(tokens []string) int {
	var stack []int
	//遍历tokens，
	for _, token := range tokens {
		//如果遇到数字将数字进行转换为整数加入进去
		if token != "+" && token != "-" && token != "*" && token != "/" {
			num, _ := strconv.Atoi(token)
			stack = append(stack, num)
		} else { //如果遇到符号就进行计算，取出两个数进行计算，计算后的结果加入栈中
			val1 := stack[len(stack)-1]
			val2 := stack[len(stack)-2]
			stack = stack[:len(stack)-2]
			switch token {
			case "+":
				stack = append(stack, val1+val2)
				//注意顺序：如果是减号，则是前面一个数减去栈顶元素
			case "-":
				stack = append(stack, val2-val1)
			case "*":
				stack = append(stack, val1*val2)
				//注意顺序：如果是除法，则是前面一个数减去栈顶元素
			case "/":
				stack = append(stack, val2/val1)
			}

		}
	}

	return stack[0]
}
```

## 方法二：参考别人代码进行优化

```go

/*
参考别人的代码进行改进

执行用时：4 ms, 在所有 Go 提交中击败了88.42%的用户
内存消耗：4.1 MB, 在所有 Go 提交中击败了42.85%的用户
*/

func evalRPN(tokens []string) int {
	var stack []int
	//遍历tokens，
	for _, token := range tokens {
		//如果遇到数字将数字进行转换为整数加入进去

		switch token {
		case "+":
			stack[len(stack)-2] += stack[len(stack)-1]
			stack = stack[:len(stack)-1]

		case "-":
			stack[len(stack)-2] -= stack[len(stack)-1]
			stack = stack[:len(stack)-1]

		case "*":
			stack[len(stack)-2] *= stack[len(stack)-1]
			stack = stack[:len(stack)-1]

		case "/":
			stack[len(stack)-2] /= stack[len(stack)-1]
			stack = stack[:len(stack)-1]

		default:
			num, _ := strconv.Atoi(token)
			stack = append(stack, num)
		}

	}

	return stack[0]
}
```

