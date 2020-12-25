# [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

## 方法：merge的时候进行统计

思路：[参考自己的博客文章](http://www.sivan.tech/2020/09/23/%E7%AE%97%E6%B3%95-%E7%BB%9F%E8%AE%A1%E9%80%86%E5%BA%8F%E5%AF%B9/)

> 执行用时：172 ms, 在所有 Go 提交中击败了 16.19% 的用户
		内存消耗：8.5 MB, 在所有 Go 提交中击败了 31.28% 的用户


```go
func reversePairs(nums []int) int {
	return MergeSort(nums)
}

func MergeSort(nums []int) int {
	if nums == nil || len(nums) < 2 {
		return 0
	}
	return mergeSort(nums, 0, len(nums)-1)
}

func mergeSort(nums []int, l, r int) int {
	//递归结束条件
	if l == r {
		return 0
	}

	//注意这个写法：可以避免越界同时使用>>1加速运算
	mid := l + (r-l)>>1
	//分治
	return mergeSort(nums, l, mid) + mergeSort(nums, mid+1, r) + merge(nums, l, mid, r)

	//合并两部分[l,mid] [mid+1,r]
}

func merge(nums []int, l, mid, r int) int {
	i, j := l, mid+1
	//申请一个辅助数组
	help := make([]int, r-l+1)
	index := 0
	ret := 0
	//谁小填谁移谁
	for i <= mid && j <= r {
		if nums[i] > nums[j] { //产生逆序对
			help[index] = nums[j]
			ret += (mid - i + 1)
			j++
		} else {
			help[index] = nums[i]
			i++
		}
		index++
	}

	//移动剩下的一部分
	//注意下面两个是for循环，不是if
	//另外注意的是下面两个for循环有且只有一个可以被执行
	for i <= mid {
		help[index] = nums[i]
		index++
		i++
	}

	for j <= r {
		help[index] = nums[j]
		index++
		j++
	}

	//此时help已经填充好我们排序后的数字了，我们需要将且拷贝到原数组的l->r
	for i := l; i <= r; i++ {
		nums[i] = help[i-l]
	}
	return ret
}
```



