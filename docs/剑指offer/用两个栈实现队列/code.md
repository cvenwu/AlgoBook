# 用两个栈实现队列



**思路：**

- 准备1个输入数据的栈，1个输出数据的栈

- 当加入元素的时候直接加入到输入数据栈中

- 当弹出元素的时候，分为如下几种情况，

	1. - 如果输出数据栈不为空，直接弹出
 	2. - 如果输出数据栈为空，将**输入数据栈所有元素**加入到输出数据栈中，再返回输出数据栈栈顶元素

## 方法一：使用container/list

> *//执行用时：252 ms, 在所有 Go 提交中击败了 54.16% 的用户*
>
> *//内存消耗：8.1 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
type CQueue struct {
	InputStack, OutputStack *list.List
}


func Constructor() CQueue {
	return CQueue{list.New(), list.New()}
}


func (this *CQueue) AppendTail(value int)  {
	//直接将元素加入到输入栈中
	this.InputStack.PushBack(value)
}


func (this *CQueue) DeleteHead() int {
	//如果输出栈有元素，直接删除
	//否则需要将输入栈全部元素导入到输出栈，然后删除输出栈的元素
	if this.OutputStack.Len() == 0 {
		//以下3行代码必须这样写
		//或者可以写成this.OutputStack.PushBack(this.InputStack.Remove(this.InputStack.Back()))
        for this.InputStack.Len() > 0 {
			this.OutputStack.PushBack(this.InputStack.Remove(this.InputStack.Back()))
        }
    }
	if this.OutputStack.Len() != 0 {
		return this.OutputStack.Remove(this.OutputStack.Back()).(int)
    }
	return -1  //说明输入栈以及输出栈没有任何内容
}
```



## 方法二：使用切片

> *//执行用时：256 ms, 在所有 Go 提交中击败了 47.47% 的用户*
>
> *//内存消耗：8.3 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go

type CQueue struct {
	InputSlice []int
	OutputSlice []int
}


func Constructor() CQueue {
	return CQueue{
		make([]int, 0),
		make([]int, 0),
	}
}


func (this *CQueue) AppendTail(value int)  {
	this.InputSlice = append(this.InputSlice, value)
}


func (this *CQueue) DeleteHead() int {
	//得到InputSlice以及OutputSlice的大小
	lengthInputSlice, lengthOutputSlice := len(this.InputSlice), len(this.OutputSlice)
	//如果输出有元素直接输出
	if lengthOutputSlice > 0 {
		ele := this.OutputSlice[lengthOutputSlice-1]
		this.OutputSlice = this.OutputSlice[0:lengthOutputSlice-1]
		return ele
	} else {
		//如果输出没有元素将输入的元素全部加入进去
		for lengthInputSlice > 0 {
			this.OutputSlice = append(this.OutputSlice, this.InputSlice[lengthInputSlice-1])
			this.InputSlice = this.InputSlice[0:lengthInputSlice-1]
            lengthInputSlice, lengthOutputSlice = lengthInputSlice - 1, lengthOutputSlice + 1
		}
	}

	if lengthOutputSlice > 0 {
		ele := this.OutputSlice[lengthOutputSlice-1]
		this.OutputSlice = this.OutputSlice[0:lengthOutputSlice-1]
        lengthOutputSlice = lengthOutputSlice - 1
		return ele
	}

	//如果输入也没有元素返回-1
	return -1
}

```


## 【推荐】



!> **2021-03-03自己写的代码**
```go
type CQueue struct {
	stack1 []int
	stack2 []int
}

func Constructor() CQueue {
	return CQueue{
		stack1: []int{},
		stack2: []int{},
	}
}

func (this *CQueue) AppendTail(value int)  {
	//直接加入到栈1
	this.stack1 = append(this.stack1, value)
}

func (this *CQueue) DeleteHead() int {
	//如果栈2里面有就直接删除，否则就将栈1里面所有的内容导入到栈2
	if len(this.stack2) > 0 {
		val := this.stack2[len(this.stack2)-1]
		this.stack2 = this.stack2[:len(this.stack2)-1]
		return val
	}

	//将栈1里面的所有内容加入到栈2
	for len(this.stack1) != 0 {
		val := this.stack1[len(this.stack1)-1]
		this.stack2 = append(this.stack2, val)
		this.stack1 = this.stack1[:len(this.stack1)-1]
	}

	//然后再从栈2里面移除
	if len(this.stack2) > 0{
		val := this.stack2[len(this.stack2)-1]
		this.stack2 = this.stack2[:len(this.stack2)-1]
		return val
	}

	//说明非法操作
	return -1
}
```


!> **2021-03-27自己写的代码**

```go
type CQueue struct {
	inputStack  []int
	outputStack []int
}

func Constructor() CQueue {
	return CQueue{
		inputStack:  []int{},
		outputStack: []int{},
	}
}

func (this *CQueue) AppendTail(value int) {
	this.inputStack = append(this.inputStack, value)
}

func (this *CQueue) DeleteHead() int {
	//如果输出栈没有元素
	if len(this.outputStack) == 0 {
		//将输入栈的所有元素全部加入
		this.outputStack = append(this.outputStack, this.inputStack...)
		//输入栈将所有元素弹出
		this.inputStack = this.inputStack[0:0]
	}

	//此时如果输出栈有元素就弹出，否则返回-1
	if len(this.outputStack) != 0 {
		val := this.outputStack[0]
		this.outputStack = this.outputStack[1:]
		return val
	}

	return -1
}

/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```