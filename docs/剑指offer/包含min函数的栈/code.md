# 包含min函数的栈

!> 根据slice或container/list实现还有push的两种可能一共有4种解法

思路：根据加入的当前元素，在push时有如下两种可能
1. 当加入的元素大于最小值栈顶元素，最小值栈顶元素不加入，否则加入 。弹出的时候需要将弹出的元素与最小值栈顶元素比较，如果相等，都弹出，否则只弹出数据栈的元素
2. 当加入的元素大于最小值栈顶元素，最小值栈顶元素加入当前的最小值栈顶元素。弹出的时候一起弹出

## 方法一：使用container/list

!> 思路：当加入的元素大于最小值栈顶元素，最小值栈顶元素加入当前的最小值栈顶元素


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

!> 思路：当加入的元素大于最小值栈顶元素，最小值栈顶元素不加入，否则加入 。弹出的时候需要将弹出的元素与最小值栈顶元素比较，如果相等，都弹出，否则只弹出数据栈的元素
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


```c++
class MinStack {
public:
    //存放数据的栈
    stack<int> dataStack;
    //存放最小值的栈
    stack<int> minStack;
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int x) {
        //加入数据到数据栈中
        this->dataStack.push(x);
        //如果最小栈没有值或者加入的小于等于最小栈栈顶就加入
        if (this->minStack.empty() || (x <= this->minStack.top()))
            this->minStack.push(x);
    }
    
    void pop() {
        if (this->dataStack.empty()) return;

        int ret = this->dataStack.top();
        this->dataStack.pop();
        if (ret == this->minStack.top()) this->minStack.pop();
    }
    
    int top() {
        return this->dataStack.top();
    }
    
    int min() {
        //返回最小栈的栈顶
        return this->minStack.top();
    }
};
```