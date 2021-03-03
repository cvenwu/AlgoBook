# 包含min函数的栈

> 根据slice或container/list实现还有push的两种可能一共有4种解法

> 思路：根据加入的当前元素，在push时有如下两种可能
>
> 1. 当加入的元素大于最小值栈顶元素，最小值栈顶元素不加入，否则加入 。弹出的时候需要将弹出的元素与最小值栈顶元素比较，如果相等，都弹出，否则只弹出数据栈的元素
>
> 2. 当加入的元素大于最小值栈顶元素，最小值栈顶元素加入当前的最小值栈顶元素。弹出的时候一起弹出

## 方法一：使用container/list



> 思路：当加入的元素大于最小值栈顶元素，最小值栈顶元素加入当前的最小值栈顶元素

> // 执行用时：20 ms, 在所有 Go 提交中击败了 79.64% 的用户
>        // 内存消耗：7.7 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go
type MinStack struct {
	//一个数据栈，一个最小值栈
	DataStack, MinValueStack *list.List
}


/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{list.New(), list.New()}
}


func (this *MinStack) Push(x int)  {
	this.DataStack.PushBack(x)

	//如果最小值栈没有元素直接加入
	if this.MinValueStack.Len() == 0 {
		this.MinValueStack.PushBack(x)
		return
	}
	//最小值栈不为空的情况
	//如果x超过最小值，也要将最小值栈栈顶重新加入最小值栈栈顶
	minValueStackTop := this.MinValueStack.Back().Value.(int)
	if x > minValueStackTop {
		this.MinValueStack.PushBack(minValueStackTop)
	} else {
		this.MinValueStack.PushBack(x)
	}
}

func (this *MinStack) Pop()  {
	//如果数据栈没有内容直接不弹出
	if this.DataStack.Len() != 0 {
		this.DataStack.Remove(this.DataStack.Back())
		this.MinValueStack.Remove(this.MinValueStack.Back())
	}
}


func (this *MinStack) Top() int {
	return this.DataStack.Back().Value.(int)
}


func (this *MinStack) Min() int {
	return this.MinValueStack.Back().Value.(int)
}
```



## 方法二：使用slice

> 思路：当加入的元素大于最小值栈顶元素，最小值栈顶元素不加入，否则加入 。弹出的时候需要将弹出的元素与最小值栈顶元素比较，如果相等，都弹出，否则只弹出数据栈的元素

> *// 执行用时：24 ms, 在所有 Go 提交中击败了 45.81% 的用户*
>
> *// 内存消耗：8.1 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
type MinStack struct {
	DataStack, MinValueStack []int
}


/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{[]int{}, []int{}}
}


func (this *MinStack) Push(x int)  {
	this.DataStack = append(this.DataStack, x)
	//如果当前最小值栈为空，直接加入
	if len(this.MinValueStack) == 0 {
		this.MinValueStack = append(this.MinValueStack, x)
		return
	}

	//如果x大于当前最小栈栈顶元素就不加入
	if x <= this.MinValueStack[len(this.MinValueStack) - 1] {
		this.MinValueStack = append(this.MinValueStack, x)
	}
}


func (this *MinStack) Pop()  {
	//数据栈的元素个数大于0
	if len(this.DataStack) > 0 {
		//并且当数据栈栈顶元素等于最小值栈顶元素就弹出
		if this.MinValueStack[len(this.MinValueStack) - 1] == this.DataStack[len(this.DataStack) - 1] {
			this.MinValueStack = this.MinValueStack[:len(this.MinValueStack) - 1]
		}
		this.DataStack = this.DataStack[:len(this.DataStack) - 1]
	}
}


func (this *MinStack) Top() int {
	return this.DataStack[len(this.DataStack) - 1]
}


func (this *MinStack) Min() int {
	return this.MinValueStack[len(this.MinValueStack) - 1]
}

```


## 改进：
!> **2021-03-03代码**
```go

type MinStack struct {
	data []int
	min  []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{
		data: []int{},
		min:  []int{},
	}
}

func (this *MinStack) Push(x int) {
	this.data = append(this.data, x)

	//如果栈顶有元素并且比x小就不加入，否则就加入x
	if len(this.min) > 0 && this.min[len(this.min)-1] < x {
		return
	}

	this.min = append(this.min, x)
}

func (this *MinStack) Pop() {
    if len(this.data) == 0 {
        return
    }
	val := this.data[len(this.data)-1]
	this.data = this.data[:len(this.data)-1]

	//如果最小栈栈顶元素等于val则弹出，否则不弹出
	if val == this.min[len(this.min)-1] {
		this.min = this.min[:len(this.min)-1]
	}
}

func (this *MinStack) Top() int {
	return this.data[len(this.data)-1]
}

func (this *MinStack) Min() int {
	return this.min[len(this.min)-1]
}


/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.Min();
 */
```
