# [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

## 方法：最小堆+最大堆

> 执行用时：96 ms, 在所有 Go 提交中击败了 99.07% 的用户
> 		内存消耗：13.9 MB, 在所有 Go 提交中击败了 88.00% 的用户


思路：我们初始化的时候将第1个元素加入最大堆，之后每次元素个数相同的时候再加入我们都要让最大堆比最小堆多1个
1. 如果此时加入的元素比最大堆的堆顶大，
1. 如果两个堆元素相同，加入最小堆，之后最小堆调整后弹出堆顶加入最大堆
2. 如果最大堆比最小堆多1个，加入最小堆
2. 如果此时加入的元素比最大堆的堆顶小，
  1. 如果两个堆元素相同，加入最大堆
  2. 如果最大堆比最小堆已经多了1个，则加入最大堆，调整之后弹出最大堆的堆顶加入到最小堆


```go

type MinHeap []int
type MaxHeap []int

//*************************************小根堆
func (h MinHeap) Len() int {
	return len(h)
}

//小根堆，前面比后面小
func (h MinHeap) Less(i, j int) bool {
	return h[i] < h[j]
}

func (h MinHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *MinHeap) Pop() interface{} {
	val := (*h)[len(*h)-1]
	*h = (*h)[:len(*h)-1]
	return val
}

//********************************************大根堆
func (h MaxHeap) Len() int {
	return len(h)
}

//大根堆，前面比后面大
func (h MaxHeap) Less(i, j int) bool {
	return h[i] > h[j]
}

func (h MaxHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}

func (h *MaxHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *MaxHeap) Pop() interface{} {
	val := (*h)[len(*h)-1]
	*h = (*h)[:len(*h)-1]
	return val
}

type MedianFinder struct {
	minHeap *MinHeap
	maxHeap *MaxHeap
}

/** initialize your data structure here. */
func Constructor() MedianFinder {
	minHeap := &MinHeap{}
	maxHeap := &MaxHeap{}
	heap.Init(&MinHeap{})
	heap.Init(&MaxHeap{})

	return MedianFinder{
		minHeap: minHeap,
		maxHeap: maxHeap,
	}
}

func (this *MedianFinder) AddNum(num int) {
	//将元素加入最小堆，


	//如果两个堆都没有元素就直接加入
	if this.minHeap.Len() == 0 && this.maxHeap.Len() == 0 {
		heap.Push(this.maxHeap, num)
		return
	}

	//
	if num >= (*this.maxHeap)[0] {
		if this.minHeap.Len() == this.maxHeap.Len() {
			heap.Push(this.minHeap, num)
			heap.Push(this.maxHeap, heap.Pop(this.minHeap))
		} else {
			heap.Push(this.minHeap, num)
		}
	} else if num < (*this.maxHeap)[0] { //如果最大堆比最小堆多1个元素还多
		if this.minHeap.Len() == this.maxHeap.Len() {
			heap.Push(this.maxHeap, num)
		} else {
			heap.Push(this.maxHeap, num)
			heap.Push(this.minHeap, heap.Pop(this.maxHeap))
		}
	}

}

func (this *MedianFinder) FindMedian() float64 {
	//两个堆，一个最大堆一个最小堆
	//如果两个的数字和为奇数，那么返回最小值的堆顶，也就是说最小值的堆顶比最大值会多1个
	//如果两个堆大小的和为偶数，那么直接返回两个堆顶的均值

	//若两个堆的大小相等，直接求两个堆堆顶的平均值
	if this.minHeap.Len() == this.maxHeap.Len() {
		return float64((*this.minHeap)[0] + (*this.maxHeap)[0]) / 2.0
	} else { //否则因为我们默认要让最大堆比最小堆多1个元素，所以要返回最大堆的堆顶
		return float64((*this.maxHeap)[0])
	}

}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```

