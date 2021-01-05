# [1365.有多少小于当前数字的数字](https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number/)


## 方法一：暴力统计
时间复杂度：O(N^2)


## 方法二：排序后统计

时间复杂度：O(NlogN)


## 方法三：计数排序

!> 计数排序，然后将次数数组转换成次数前缀和然后统计到ret中

>执行用时：4 ms, 在所有 Go 提交中击败了94.31%的用户
>内存消耗：3.1 MB, 在所有 Go 提交中击败了91.70%的用户


```go
func smallerNumbersThanCurrent(nums []int) []int {
    if len(nums) <= 0 {
        return nil
    }

    ret := make([]int, len(nums))
    times := make([]int, 101)
    for i := 0; i < len(nums); i++ {
        times[nums[i]]++
    }

    //将times转换成次数前缀和
    for i := 1; i <= len(times)-1; i++ {
        times[i] += times[i-1]
    }

    //写入到ret中
    for i := 0; i < len(ret); i++ {
        if nums[i] != 0 {
            ret[i] = times[nums[i]-1]
        }
    }

    return ret
}
```