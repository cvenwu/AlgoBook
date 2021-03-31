# 剑指 Offer 53 - II. 0～n-1中缺失的数字

## 方法一：使用map统计

## 方法二：求和0~n-1的所有数字的和，然后减去数组中所有元素的和

## 方法三：两两比较，如果不相等就返回

## 【推荐】方法四：二分

思路：因为给定的数组是有序的，并且题目可以简化为找到第一个元素值与对应下标不同的那个元素值，假设为num，那么返回num-1即可。（也就是使用二分查找第一个不满足条件(元素值和对应下标不同)的元素）

注意：如果数组缺少的是最后一个数字，我们需要特别处理。


```go
func missingNumber(nums []int) int {
    if  len(nums) == nums[len(nums)-1]+1 {
        return len(nums)
    }

	l, r := 0, len(nums)

	for l < r {
		mid := l + (r-l)>>1
		if nums[mid] > mid {
			r = mid
		} else {
			l = mid + 1
		}
	}

	return nums[l] - 1
}
```