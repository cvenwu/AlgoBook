# 队列的最大值

> *// 执行用时：96 ms, 在所有 Go 提交中击败了 96.81% 的用户*
>
> *// 内存消耗：7.8 MB, 在所有 Go 提交中击败了 53.33% 的用户*



思路：使用两个切片，一个保存数据，另一个保存最大值

1. 当加入数据到队列时，如果比最大值切片中的最后一个元素大，则弹出最后一个，不断循环，直到最大值切片中最后一个元素大于等于要加入的数据值(类似于滑动窗口的最大值)。此时再加入数据值到数据切片与最大值切片。反之直接加入
2. 当弹出数据元素的时候，如果该元素等于最大值切片的第一个元素，最大值切片也要弹出，反之只弹出数据切片中的元素





```go
type MaxQueue struct {
	maximum []int
	data []int
}


func Constructor() MaxQueue {
	return MaxQueue{[]int{}, []int{}}
}


func (this *MaxQueue) Max_value() int {
	if len(this.maximum) <= 0 {
		return -1
	}
	return this.maximum[0]
}


func (this *MaxQueue) Push_back(value int)  {
	length := len(this.maximum)
	for length != 0 && value > this.maximum[length - 1] {  //弹出小的,因为要加入的值比前面的值大
		this.maximum = this.maximum[:length - 1]
		length -= 1
	}
	this.data = append(this.data, value)
	this.maximum = append(this.maximum, value)
}


func (this *MaxQueue) Pop_front() int {
	if len(this.data) <= 0 {
		return -1
	}
	val := this.data[0]
	this.data = this.data[1:]
	if val == this.maximum[0] {
		this.maximum = this.maximum[1:]
	}
	return val
}
```

