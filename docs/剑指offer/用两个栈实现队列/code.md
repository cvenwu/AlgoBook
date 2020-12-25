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



