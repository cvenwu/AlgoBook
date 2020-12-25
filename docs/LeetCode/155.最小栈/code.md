# [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

## 方法一：

思路：

1. 使用两个栈来保存，一个为数据栈用来保存数据，另一个为最小值栈用来保存最小值
2. 当加入元素的时候，数据栈直接加入，但是如果元素小于等于最小栈的栈顶则加入到最小栈中
3. 当弹出元素的时候，数据栈直接弹出，但是如果弹出的元素等于最小栈的栈顶则从最小栈中弹出

> 执行用时：36 ms, 在所有 Go 提交中击败了97.98%的用户
> 		内存消耗：6.3 MB, 在所有 Go 提交中击败了60.28%的用户

```go
type MinStack struct {
	dataStack []int
	minStack  []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{
		dataStack: []int{},
		minStack:  []int{},
	}
}

func (this *MinStack) Push(x int) {
	this.dataStack = append(this.dataStack, x)
	//这里得特别注意最小值栈为空的情况
	if len(this.minStack) == 0 {
		this.minStack = append(this.minStack, x)
		return
	}
	//如果加入的元素小于等于最小栈栈顶，则加入
	if x <= this.minStack[len(this.minStack)-1] {
		this.minStack = append(this.minStack, x)
	}
}

func (this *MinStack) Pop() {
	//如果数据栈元素为空，直接不弹出
	if len(this.dataStack) == 0 {
		return
	}
	val := this.dataStack[len(this.dataStack) - 1]
	this.dataStack = this.dataStack[:len(this.dataStack) - 1]
	//如果弹出的元素值等于最小栈栈顶，就从最小栈栈顶弹出
	if val == this.minStack[len(this.minStack) - 1] {
		this.minStack = this.minStack[:len(this.minStack) - 1]
	}
}

func (this *MinStack) Top() int {
	return this.dataStack[len(this.dataStack) - 1]
}

func (this *MinStack) GetMin() int {
	return this.minStack[len(this.minStack) - 1]
}
```



## 方法二：

思路：

1. 使用两个栈来保存，一个为数据栈用来保存数据，另一个为最小值栈用来保存最小值
2. 当加入元素的时候，数据栈直接加入，但是如果元素大于最小栈的栈顶则重复加入最小栈栈顶到最小栈中
3. 当弹出元素的时候，数据栈与最小栈直接弹出


> 执行用时：20 ms, 在所有 Go 提交中击败了74.85%的用户
> 		内存消耗：7.8 MB, 在所有 Go 提交中击败了26.47%的用户

```go
type MinStack struct {
	dataStack []int
	minStack  []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{
		dataStack: []int{},
		minStack:  []int{},
	}
}

func (this *MinStack) Push(x int) {
	this.dataStack = append(this.dataStack, x)
	//这里得特别注意最小值栈为空的情况
	if len(this.minStack) == 0 {
		this.minStack = append(this.minStack, x)
		return
	}
	//如果加入的元素大于最小栈栈顶，则重复加入最小栈栈顶,否则加入x
	if x > this.minStack[len(this.minStack)-1] {
		this.minStack = append(this.minStack, this.minStack[len(this.minStack)-1])
	} else {
		this.minStack = append(this.minStack, x)
	}
}

func (this *MinStack) Pop() {
	if len(this.dataStack) == 0 {
		return
	}
	this.dataStack = this.dataStack[:len(this.dataStack) - 1]
	this.minStack = this.minStack[:len(this.minStack) - 1]
}

func (this *MinStack) Top() int {
	return this.dataStack[len(this.dataStack) - 1]
}

func (this *MinStack) GetMin() int {
	return this.minStack[len(this.minStack) - 1]
}

```

