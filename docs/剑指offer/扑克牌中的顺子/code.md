# [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

因为数组长度很小，所以使用方法一二的效率都差不多。

## 方法一：快排

时间复杂度：O(NlogN)

空间复杂度：O(1)

思路：


1. 首先对传入的数组进行排序
2. 排序之后因为是从小到大升序，首先统计0的元素的个数
3. 统计非0元素个数之间的空隙
4. 如果空隙的个数大于0说明无法填充直接返回false，否则返回true


> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗 2.2 MB, 在所有 Go 提交中击败了 17.76% 的用户


```go
func isStraight(nums []int) bool {
	//1. 对传入的数据进行排序
	sort.Ints(nums)
	//2. 数一下0的个数
	numsOfZero := 0
	for i := 0; i < len(nums) && nums[i] == 0; i++ {
		if nums[0] == 0 {
			numsOfZero += 1
		}
	}
	//3. 计算缺失数字的个数，同时如果中间有两个数相等，直接返回false
	temp := numsOfZero
	gap := 0
	for i := temp; i < len(nums)-1; i++ {
		if nums[i+1] == nums[i] {
			return false
		} else {
			gap += nums[i+1] - nums[i] - 1
		}
	}
	//4. 如果缺失数字的个数小于等于0的个数返回true
	if gap <= numsOfZero {
		return true
	}
	//说明缺失数字个数太多否则返回false
	return false
}

```

## 方法二：使用set + 最大值最小值规律法

时间复杂度：O(N)

空间复杂度：O(N)

[参考](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/solution/mian-shi-ti-61-bu-ke-pai-zhong-de-shun-zi-ji-he-se/)

1. 遍历数组的时候，我们动态维护一个最大值和最小值，同时我们不断更新map，如果有重复值直接返回false，并且统计0的个数

2. 遍历之后，我们得到了最大值max，最小值min，以及一个没有重复值的map，同时我们还获得了0的个数count

3. 此时max - min为最大值和最小值的差值，非0的元素个数为5-count，如果为顺子正常的差值为4-count。

4. 因此我们需要进行判断，如果max - min - (4 - count)得到的便是gap数量，如果这个值小于等于count那么就可以填充成顺子

```go
func isStraight(nums []int) bool {
	mymap := make(map[int]bool)
	//遍历传入的数组
	//1. 如果为0直接跳过，并且count+=1
	//统计0出现的次数
	count := 0
	//记录5张牌中的最大值和最小值
	max, min := -1, 20

	for i := 0; i < len(nums); i++ {
		if nums[i] == 0 {
			count += 1
		} else {
			if _, exists := mymap[nums[i]]; exists {
				return false
			}
			mymap[nums[i]] = true
			if nums[i] > max {
				max = nums[i]
			}
			if nums[i] < min {
				min = nums[i]
			}
		}
	}

	//如果最大值减去(除0以外的最小值)差值小于5，那么一定可以搭建成一个顺子
	if (max-min) < 5 {
		return true
	}

	return false
}


```

