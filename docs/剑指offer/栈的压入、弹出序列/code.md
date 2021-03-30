# 栈的压入、弹出序列



> 思路：使用一个辅助栈模拟真实栈的操作
>
> 步骤如下：
>
> 1. 如果需要加入的切片不为空，就加入
> 2. 辅助栈栈顶元素与弹出的元素一致，则弹出，循环此步骤直到辅助栈栈顶元素与弹出的元素不一致
> 3. 循环1，2步骤，直到所有元素加入完成。
> 4. 此时判断辅助栈是否为空，如果为空则返回true否则返回false



## 代码一：

> *// 执行用时：8 ms, 在所有 Go 提交中击败了 82.92% 的用户*
>
> *// 内存消耗：3.8 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func validateStackSequences(pushed []int, popped []int) bool {
	//记录输入两个slice的长度
	lengthPushed, lengthPopped := len(pushed), len(popped)
	//如果两个长度不一致
	if lengthPushed != lengthPopped {
		return false
	}

	//使用一个栈来模拟栈的行为
	stack, length := []int{}, 0
	
	
	//将pushed所有元素压入
	for lengthPushed > 0 {
		//元素压入栈
		stack, length = append(stack, pushed[0]), length + 1
		//pushed删除该元素
		pushed, lengthPushed = pushed[1:], lengthPushed - 1

		//push元素之后比较压入的元素与pop元素是否同，如果同就弹出，否则继续压入
		//不用判断lengthPopped是否为0，因为如果为0肯定stack也没有元素
        for (length != 0) && (stack[length - 1] == popped[0]) {
			stack, length, popped, lengthPopped = stack[:length - 1], length - 1, popped[1:], lengthPopped - 1
		}
		
	}

	//如果所有元素压入之后，stack的栈顶依然与popped的第1个元素不同，直接返回false
	//上述条件等价于如果栈中还有元素，直接返回false
	if length != 0 {
		return false
	} 

	return true
}
```

## 代码二：

**优点：相比于代码一，使用for-range将pushed元素加入到stack**

> *// 执行用时：4 ms, 在所有 Go 提交中击败了 99.69% 的用户*
>
> *// 内存消耗：3.8 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func validateStackSequences(pushed []int, popped []int) bool {
	//记录输入两个slice的长度
	lengthPushed, lengthPopped := len(pushed), len(popped)
	//如果两个长度不一致
	if lengthPushed != lengthPopped {
		return false
	}

	//使用一个栈来模拟栈的行为
	stack, length := []int{}, 0

	for _, value := range pushed {
		//元素压入栈
		stack, length = append(stack, value), length + 1

		//当前栈顶元素与popped的第一个元素一致，则删除
		//不用判断lengthPopped是否为0，因为如果为0肯定stack也没有元素
		for (length > 0) && (stack[length - 1] == popped[0]) {
			stack, length, popped, lengthPopped = stack[:length-1], length - 1, popped[1:], lengthPopped - 1
		}

	}
    
	if length != 0 {
		return false
	}

	return true
}
```

## 【推荐】代码三：

步骤：遍历Pushed数组中的每个元素：
1. 每次加入元素之后
2. 不断循环：如果栈顶有元素并且栈顶的元素等于我们指向的弹出的元素就弹出
```go
func validateStackSequences(pushed []int, popped []int) bool {
	stack, i := []int{}, 0
	for j := 0; j < len(pushed); j++ {
		stack = append(stack, pushed[j])
		//如果栈元素个数大于0并且栈顶一直与Popped相等就一直弹出
		for len(stack) > 0 && stack[len(stack)-1] == popped[i] {
			stack, i = stack[:len(stack)-1], i+1
		}
	}

	return len(stack) == 0
}
```