# 滑动窗口的最大值

!> 思路：使用一个切片模拟窗口以及一个切片保存滑动窗口每次的最大值

1. 初始时将前滑动窗口大小的元素中的元素按照如下规则加入最大值切片
   1. 若最大值切片没有元素，直接加入
   2. 若大于最大值切片的最后一个，则弹出最后一个元素，不断循环，直到最大值切片最后一个元素小于等于要加入的元素值
2. 之后在加入当前遍历到的元素时，需要确保如下几种情况
   1. 滑动窗口最左边的元素没有越界
   2. 加入的元素如果比之前的大，就弹出，直到最大值切片最后一个元素小于等于要加入的元素值
   3. 此时就可以直接加入

**注意：我们向窗口中加入的是元素的索引，判断的时候记得根据我们对应的索引取值**
```go
func maxSlidingWindow(nums []int, k int) []int {
	if len(nums) <= 0 {
		return nil
	}

	ret := make([]int, len(nums)-k+1)
	var window []int

	r := 0
	count := 0

	//开始不断遍历并移动
	for r < len(nums) {
		//如果窗口中有元素，并且第一个元素与当前要加入的元素距离等于k则弹出
		if len(window) > 0 && r-window[0] == k {
			window = window[1:]
		}

		//如果窗口中有元素并且窗口中的元素比我们要加入的元素小，那么不断弹出
		for len(window) > 0 && nums[window[len(window)-1]] <= nums[r] {
			window = window[:len(window)-1]
		}
		//加入当前我们要加入的元素
		window = append(window, r)

		//也就是初始化完成之后我们就可以让我们的结果切片加入内容了
		if r >= k-1 {
			ret[count] = nums[window[0]]
			count++
		}
		r++
	}

	return ret
}

```
