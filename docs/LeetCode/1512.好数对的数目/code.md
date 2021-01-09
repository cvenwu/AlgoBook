# [1512.好数对的数目](https://leetcode-cn.com/problems/number-of-good-pairs/s)


## 方法一：
!> 暴力查找，时间复杂度O(n^2)

## 方法二：
!> 排序后查找相邻元素，时间复杂度O(NlogN)

## 方法三：
!> 使用map对每个元素进行计数，之后根据"某个元素出现n次，则该元素可以产生好数对`(1 + n-1)*(n-1)/2`"

```go
//方法一：使用map对每个元素进行计数
//核心：某个元素出现n次，则该元素可以产生好数对(1 + n-1)*(n-1)/2
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2 MB, 在所有 Go 提交中击败了17.92%的用户
func numIdenticalPairs(nums []int) int {
	if len(nums) <= 1 {
		return 0
	}

	sum := 0
	timesMap := make(map[int]int)
	for i := 0; i < len(nums); i++ {
		timesMap[nums[i]] += 1
	}

	//某个元素出现n次，则该元素可以产生好数对(1 + n-1)*(n-1)/2
	for _, v := range timesMap {
		sum += ((v-1)*v)>>1
	}

	return sum
}
```

## 方法四：

!> 因为题目中说明了数据状况，1 <= nums[i] <= 100，此时我们可以将我们方法一种的map改为一个长度为100的切片