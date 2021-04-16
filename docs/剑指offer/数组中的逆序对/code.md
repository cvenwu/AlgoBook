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

## 【推荐】方法二：改进
!> 思路：**合并两个有序数组的时候我们使用从后往前的方法来合并**，这样便于我们统计逆序对的个数。如果从前往后，我们不知道要统计到哪个为止，甚至我们可能会面临重复统计逆序数的问题。

```go
/**
 * @Author: yirufeng
 * @Date: 2021/4/16 9:24 上午
 * @Desc: 剑指 Offer 51. 数组中的逆序对

注意点：每次我们合并的时候是从末尾合并到开头，因为从开头合并的时候，会重复计算部分逆序的数，甚至不确定我们加上多少个逆序数，
			所以从末尾到开头这个方向进行合并将会避免我们的重复逆序数计算

比如：0,93, 36,28  逆序数是3

过程：
1. 0,93 | 28,36
2. 0,93 | 28,36 将93放到我们help最后一个数上，同时逆序数个数加上我们的2
3. 0,93 | 28,36 将36放到我们help
4. 0,93 | 28,36 将28放到我们help
5. 0,93 | 28,36 将0放到我们help

但是如果从前往后：
1. 0,93,101 | 28,36
2. 0,93,101 | 28,36 将0放到我们help第一个数上
3. 0,93,101 | 28,36 将28放到我们help上，逆序数加上1
4. 0,93,101 | 28,36 将36放到我们help上，逆序数加上1还是2呢，此时是无法确定的，并且及时确定下来，那么加到哪个范围呢，36后面如果还有比93更大的数呢？
5. 0,93,101 | 28,36 将93放到我们help上
6. 0,93,101 | 28,36 将101放到我们help上

 **/

//思路：合并的时候统计我们的逆序对的个数
func reversePairs(nums []int) int {
	count := 0
	mergeSort(nums, 0, len(nums)-1, &count)
	return count
}

func mergeSort(nums []int, l, r int, count *int) {
	if l < r {
		mid := l + (r-l)>>1
		//分开
		mergeSort(nums, l, mid, count)
		mergeSort(nums, mid+1, r, count)
		//合并
		merge(nums, l, mid, r, count)
	}
}

//合并区间[l,mid]与区间[mid+1,r]
func merge(nums []int, l, mid, r int, count *int) {
	cur := r - l
	lCur, rCur := mid, r
	help := make([]int, r-l+1)
	for lCur >= l && rCur >= mid+1 {
		if nums[lCur] > nums[rCur] {
			help[cur], cur, lCur = nums[lCur], cur-1, lCur-1
			*count += rCur - mid
		} else if nums[lCur] < nums[rCur] {
			help[cur], cur, rCur = nums[rCur], cur-1, rCur-1
		} else if nums[lCur] == nums[rCur] {
			help[cur], cur, rCur = nums[rCur], cur-1, rCur-1
		}
	}

	for lCur >= l {
		help[cur], cur, lCur = nums[lCur], cur-1, lCur-1
	}
	for rCur >= mid+1 {
		help[cur], cur, rCur = nums[rCur], cur-1, rCur-1
	}
	for i := 0; i < len(help); i++ {
		nums[l+i] = help[i]
	}
}

```