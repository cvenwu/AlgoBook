# 队列的最大值


## 方法一：
!> 思路：使用两个切片，一个保存数据，另一个保存最大值

**push_back操作：**
1. 直接向数据队列加入我们的元素
2. 循环：
	1. 如果最大值队列有元素且最大值队列末尾的元素小于我们要加入的元素的值：不断从最大值队列尾部弹出元素
3. 将要插入的值加入到我们的最大值队列

**Pop_front操作：**
1. 如果有元素直接从数据队列弹出元素val
2. 如果val等于我们的最大值队列的头部元素，则从最大值队列弹出队头

```go

type MaxQueue struct {
	dataQueue []int
	maxQueue  []int
}

func Constructor() MaxQueue {
	return MaxQueue{
		dataQueue: []int{},
		maxQueue:  []int{},
	}
}

func (this *MaxQueue) Max_value() int {
	if len(this.dataQueue) == 0 {
		return -1
	}

	return this.maxQueue[0]
}

func (this *MaxQueue) Push_back(value int) {
	this.dataQueue = append(this.dataQueue, value)
	for len(this.maxQueue) > 0 && this.maxQueue[len(this.maxQueue)-1] < value {
		this.maxQueue = this.maxQueue[:len(this.maxQueue)-1]
	}
	this.maxQueue = append(this.maxQueue, value)
}

func (this *MaxQueue) Pop_front() int {
	if len(this.dataQueue) == 0 {
		return -1
	}

	val := this.dataQueue[0]
	this.dataQueue = this.dataQueue[1:]
	if val == this.maxQueue[0] {
		this.maxQueue = this.maxQueue[1:]
	}
	return val
}

```