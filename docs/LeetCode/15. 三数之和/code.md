# [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

## 方法：

**注意避免重复**

思路：排序+固定一个数之后使用双指针转换成两数之和

> 执行用时：40 ms, 在所有 Go 提交中击败了49.23%的用户
> 		内存消耗：7.3 MB, 在所有 Go 提交中击败了70.28%的用户


```go
/*

 */
func threeSum(nums []int) [][]int {
   //special case
   if len(nums) < 3 {
      return nil
   }

   //1. 对数组进行排序
   sort.Ints(nums)

   //定义返回的结果
   ret := [][]int{}
   //2. 递归求解
   for i := 0; i <= len(nums)-3; i++ {



      //这个for循环是求两数之和等于-nums[i]的两个元素
      l, r := i+1, len(nums)-1
      for l < r {
         //如果遇到重复的就跳过，只要l不重复r就肯定不会重复
         //记录左右上次左右两侧访问过的数字，下次直接跨过去
         left, right := nums[l], nums[r]

         //之后进行判断
         if nums[l]+nums[r] > 0-nums[i] {
            for l < r && right == nums[r] {
               r--
            }
         } else if nums[l]+nums[r] < 0-nums[i] {
            for l < r && left == nums[l] {
               l++
            }
         } else if nums[l]+nums[r] == 0-nums[i] {
            ret = append(ret, []int{nums[i], nums[l], nums[r]})
            for l < r && left == nums[l] {
               l++
            }
            for l < r && right == nums[r] {
               r--
            }
         }
      }

      for i <= len(nums)-3 && nums[i+1] == nums[i] {
         i++
      }
   }
   return ret
}
```