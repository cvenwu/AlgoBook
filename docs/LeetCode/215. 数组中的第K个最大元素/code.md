



# [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

> 有很多其他方法可以解决，例如堆排，快排的partition等，这里只列举最优解

## 方法：最优解BFPRT

```go
func min(a, b int) int {
	if a > b {
		return b
	}
	return a
}

//采用类似于荷兰国旗问题的划分思路，划分为小于等于与大于三个区间
func partition(nums []int, start int, end int, target int) (int, int) {
	//l, r分别表示小于target已经有序的区域和大于target已经有序的区域
	l, r := start-1, end+1
	cur := start
	for cur < r {
		if nums[cur] > target {
			nums[r-1], nums[cur] = nums[cur], nums[r-1]
			r--
		} else if nums[cur] < target {
			nums[cur], nums[l+1] = nums[l+1], nums[cur]
			l++
			cur++
		} else if nums[cur] == target {
			cur++
		}
	}
	return l + 1, r - 1
}

//获取传入数组nums在start->end之间的中位数，包括两个边界
func getMedianOfNums(nums []int, start int, end int) int {
	//对nums[start:end]进行直接插入排序
	var temp, j int
	for i := start + 1; i <= end; i++ {
		temp = nums[i]
		//注意点1：nums[j] > temp
		for j = i - 1; j >= start && nums[j] > temp; j-- {
			nums[j+1] = nums[j]
		}
		nums[j+1] = temp
	}
	return nums[start+(end-start)>>1]
}

//选取中位数数组的中位数并进行返回
func medianOfMedians(nums []int, start int, end int) int {

	//首先将传入的数组有效区间内的元素每5个划分为1组，不足5个的不划分
	//注意点2：是float64(end - start + 1)不是float64(len(nums))
	medianNums := make([]int, int(math.Ceil(float64(end-start+1)/5.0)))

	//找到每个数组的中位数，添加到我们的中位数数组中
	for i := 0; i < len(medianNums); i++ {
		val := getMedianOfNums(nums, start+i*5, min(start+i*5+4, end))
		medianNums[i] = val
	}

	//返回中位数数组的中位数
	return getMedianOfNums(medianNums, 0, len(medianNums)-1)
}

func BFPRT(nums []int, start int, end int, k int) int {
	//递归结束条件
	if start == end {
		return nums[start]
	}

	//获取nums每5个一组后划分的中位数数组的中位数
	//之后根据pivot进行partition
	//注意点3：这里是medianOfMedians(nums, start, end)而不是nums[medianOfMedians(nums, start, end)]
	//因为我们medianOfMedians(nums, start, end)返回的就是划分值
	l, r := partition(nums, start, end, medianOfMedians(nums, start, end))

	//判断k是否在划分后的两个索引
	if k >= l && k <= r {
		return nums[l]
	} else if k < l {
		return BFPRT(nums, start, l-1, k)
	} else {
		return BFPRT(nums, r+1, end, k)
	}
}

func findKthLargest(nums []int, k int) int {
	tempNums := make([]int, len(nums))
	copy(tempNums, nums)
	return BFPRT(tempNums, 0, len(nums)-1, len(nums)-k)
}
```

