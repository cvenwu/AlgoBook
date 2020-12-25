# [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

## 方法：荷兰国旗问题

思路：采用荷兰国旗问题思路进行解决即可

```go
func sortColors(nums []int)  {
    if len(nums) < 2 {
        return
    }
    
    left, right, cur := -1, len(nums), 0
    for cur < right {
        if nums[cur] < 1 {
            nums[left+1], nums[cur] = nums[cur], nums[left+1]
            left++
            cur++
        } else if nums[cur] > 1 {
            nums[right-1], nums[cur] = nums[cur], nums[right-1]
            right--
        } else {
            cur++
        }
    }
}
```

