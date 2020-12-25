# [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

方法三适合处理海量数据，方法四(时间复杂度：O(N)。空间复杂度：O(1))需要修改传入的数组

思路：

1. ![aLjufN](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/aLjufN.png)

2. ![b8s2H2](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/b8s2H2.png)



## 方法一：

思路：直接用最快的排序方法排好序后取前k个或者后k个元素即可

时间复杂度：O(n * logn)

空间复杂度：O(1)

```go

func getLeastNumbers(arr []int, k int) []int {
	return QuickSort(arr)[:k]
}

func QuickSort(nums []int) []int {
	quickSort(nums, 0, len(nums)-1)
	return nums
}

func quickSort(nums []int, low int, high int) {
	var pivotkey int
	//优化1：尾递归优化
	for low < high {
		//pivotkey 为枢轴防止的最终下标
		pivotkey = partition(nums, low, high)
		quickSort(nums, low, pivotkey-1)
		low = pivotkey + 1
	}
}

func partition(nums []int, low int, high int) int {
	//优化2：使用随机数选择划分枢轴
	//设置随机数种子
	rand.Seed(time.Now().UnixNano())
	//选择[0,n)
	//rand.Intn(n)
	//选择划分元素
	dummyIndex := rand.Intn(high-low+1) + low
	//将随机选择的元素放置到low位置上，以后便于在高处找到比划分元素小的时候我们可以直接让high赋值给low
	swap(nums, low, dummyIndex)
	dummyVal := nums[low]
	for low < high {
		for low < high && dummyVal <= nums[high] {
			high--
		}
		//优化3：我们使用替换而不是交换
		//高处找到小的直接赋值给low
		nums[low] = nums[high]
		for low < high && dummyVal >= nums[low] {
			low++
		}
		//低处找到大的直接赋值给high
		nums[high] = nums[low]
	}
	//最后low == high
	nums[low] = dummyVal
	return low
}

func swap(nums []int, i int, j int) {
	nums[i], nums[j] = nums[j], nums[i]
}
```



## 方法二：

思路：冒泡排序每次都会将1个元素放置到最终位置上，我们可以冒泡k趟

时间复杂度：O(k * n)

空间复杂度：O(1)

```go
func getLeastNumbers(arr []int, k int) []int {
	//需要冒泡k次
	for i := 1; i <= k; i++ {
		//每次从倒数第2个元素开始一直到前面的第i-1个元素，例如第1趟是到第0个(因为每次是和后面的元素进行比较)
		for j := len(arr) - 2; j >= i-1; j-- {
			if arr[j] > arr[j+1] {
				arr[j+1], arr[j] = arr[j], arr[j+1]
			}
		}
	}
	return arr[:k]
}

```

## 方法三：

思路：构建一个容量为k的堆，不断遍历数据，插入后进行调整，最后依次返回组合成我们要的结果

***\*****适合处理海量数据*****\***

时间复杂度：O(n*logk)

空间复杂度：O(K)



```go
type IntHeap []int

//Heap Interface
//type Interface interface {
//    sort.Interface
//    Push(x interface{}) // 向末尾添加元素
//    Pop() interface{}   // 从末尾删除元素
//}

func (h *IntHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

// Sort Interface
//type Interface interface {
//    // Len方法返回集合中的元素个数
//    Len() int
//    // Less方法报告索引i的元素是否比索引j的元素小
//    Less(i, j int) bool
//   // Swap方法交换索引i和j的两个元素
//    Swap(i, j int)
//}

func (h IntHeap) Len() int {
	return len(h)
}

//表明是一个小顶堆
func (h IntHeap) Less(i, j int) bool {
	return h[i] < h[j]
}

func (h IntHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}

func getLeastNumbers(arr []int, k int) []int {
	h := &IntHeap{}
	for _, val := range arr {
		heap.Push(h, val)
	}

	ret := []int{}
	for i := 0; i < k; i++ {
		ret = append(ret, heap.Pop(h).(int))
	}
	return ret
}

```

## 方法四：

思路：随机选择，通过减治的思想利用快速排序中的partition方法可以对我们要求的那个划分轴进行快速选择，

**算法导论中提过，我们有成熟的时间复杂度为O(n)的算法来获取任意第k大的数，也就是基于partition的随机选择方法**

时间复杂度：O(N)

空间复杂度：O(1)

***\*****会对传入的切片进行修改*****\***

```go
func partition(arr []int, start int, end int) int {
	rand.Seed(time.Now().UnixNano())

	index := rand.Intn(end-start+1) + start
	val := arr[index]
	arr[index], arr[start] = arr[start], arr[index]

	for start < end {
		for start < end && val <= arr[end] {
			end--
		}
		arr[start] = arr[end]
		for start < end && val >= arr[start] {
			start++
		}
		arr[end] = arr[start]
	}
	arr[start] = val
	return start
}

func getLeastNumbers(arr []int, k int) []int {
	//特殊情况
	if len(arr) == 0 || k <= 0 {
		return nil
	} else if k >= len(arr) {
		return arr
	}

	index := partition(arr, 0, len(arr)-1)
	for index != k-1 {
		if index > k-1 { //说明在左边
			index = partition(arr, 0, index-1)

		} else if index < k-1 {
			index = partition(arr, index+1, len(arr)-1)
		}
	}
	//到这里说明partition划分为index就是第k个元素，直接返回即可
	return arr[:k]
}
```

## 参考

[58沈剑讲解](https://mp.weixin.qq.com/s/FFsvWXiaZK96PtUg-mmtEw)