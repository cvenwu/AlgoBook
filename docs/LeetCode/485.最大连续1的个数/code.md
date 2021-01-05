# [485.最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)


## 方法一：双指针


!> 思路：分别指向连续1的开始和结束，统计1的个数，然后找到下一个连续的1

```go
func findMaxConsecutiveOnes(nums []int) int {
    max, l, r := 0, 0, 0
    for r != len(nums) {
        //让l走到连续的1的开头
        for l < len(nums) && nums[l] != 1 {
            l++
        }
        r = l
        //让r走到连续的1的末尾的下一个
        for r < len(nums) && nums[r] == 1 {
            r++
        }

        //更新最大值
        if r - l > max {
            max = r - l
        }
        
        //重新开始找下一个连续的1，这里不会发生死循环，如果一旦r超过了范围，下一次赋值之后一定会立马跳出for循环
        l = r
    }

    return max
}
```


## 【很好理解】方法二：常规的计数

[一次遍历进行计数](https://leetcode-cn.com/problems/max-consecutive-ones/solution/zui-da-lian-xu-1de-ge-shu-by-leetcode/)