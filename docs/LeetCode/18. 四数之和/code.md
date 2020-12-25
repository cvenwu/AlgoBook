# [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

## 方法：

类似于三数之和，首先固定一个数，之后再固定一个数，然后转换为双指针求两数之和

**注意点：列在了代码中**

>  执行用时：16 ms, 在所有 Go 提交中击败了56.72%的用户
> 		内存消耗：2.8 MB, 在所有 Go 提交中击败了52.10%的用户

**注意避免重复**

```go

func fourSum(nums []int, target int) [][]int {
   //special case
   if len(nums) < 4 {
      return nil
   }

   //1. 对数组的元素进行排序
   sort.Ints(nums)
   ret := [][]int{}
   //2. 枚举第1个元素可能出现的位置
   for i := 0; i <= len(nums)-4; i++ {

      //枚举第2个元素可能出现的位置，此时是求三数之和
      for j := i + 1; j <= len(nums)-3; j++ {
         //此时枚举第3个元素可能出现的位置，此时是求两数之和
         l, r := j+1, len(nums)-1

         for l < r {
            //注意点1：使用left以及right是为了避免重复
            left, right := nums[l], nums[r]
            if nums[l]+nums[r]+nums[i]+nums[j] < target {
               //注意点2：为了避免重复

               for l < r && nums[l] == left {
                  l++
               }
            } else if nums[l]+nums[r]+nums[i]+nums[j] > target {
               //注意点3：为了避免重复

               for l < r && nums[r] == right {
                  r--
               }
            } else if nums[l]+nums[r]+nums[i]+nums[j] == target {
               ret = append(ret, []int{i, j, l, r})
               //注意点4：为了避免重复

               for l < r && nums[l] == left {
                  l++
               }
               for l < r && nums[r] == right {
                  r--
               }
            }
         }
         //注意点5：为了避免重复，先求出之后，再写这个，比如-1,-1,2如果放在前面直接就跳过第1个-1

         for j <= len(nums)-3 && nums[j] == nums[j+1] {
            j++
         }
      }
      //注意点5：为了避免重复，同注意点5，注意顺序
      for i+1 <= len(nums)-3 && nums[i] == nums[i+1] {
         i++
      }
   }

   return ret

}
```