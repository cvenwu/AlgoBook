# [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

## 方法：单调栈

思路：将nums2中的所有元素求出右边比它大的元素的下标，放在map中（key为nums2元素对应的值，因为没有重复元素所以不用担心），之后遍历nums1从map中查找

**注意代码中提及到的注意点**

> 执行用时：4 ms, 在所有 Go 提交中击败了89.84%的用户
>  		内存消耗：3.4 MB, 在所有 Go 提交中击败了32.81%的用户

```go

func nextGreaterElement(nums1 []int, nums2 []int) []int {
   nextGreaterNums2 := make(map[int]int)
   stack := []int{}
   ret := make([]int, len(nums1))
   for i := 0; i < len(nums2); i++ {
      //如果要加入的元素大于我们栈顶元素，需要不断弹出栈顶，直到栈顶元素小于等于要加入的元素为止
      //注意点1：这里是nums2[stack[len(stack)-1]]因为栈里面我们存的是元素的下标，所以一定要注意
      for len(stack) > 0 && nums2[stack[len(stack)-1]] < nums2[i] {
         nextGreaterNums2[nums2[stack[len(stack)-1]]] = nums2[i]
         //弹出栈顶
         stack = stack[:len(stack)-1]
      }
      stack = append(stack, i)
   }


   //之后遍历我们的nums1，然后加入我们最终的结果中
   for i, v := range nums1 {
      if val, ok := nextGreaterNums2[v]; ok {
         ret[i] = val
      } else {
         ret[i] = -1
      }
   }

   return ret
}
```