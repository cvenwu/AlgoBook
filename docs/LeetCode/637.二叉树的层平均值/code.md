

# [637 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

## 方法：BFS

思路：
1. 获取每层的和后除以每层的个数来得到平均值将其加入ret中

> 执行用时：8 ms, 在所有 Go 提交中击败了96.21%的用户
> 		内存消耗：6.3 ms, 在所有 Go 提交中击败了11.54%的用户

```go

func averageOfLevels(root *TreeNode) []float64 {
   if root == nil {
      return []float64{}
   }

   queue := []*TreeNode{root}
   ret := []float64{}
   for len(queue) != 0 {
      length := len(queue)
      var levelRet float64
      for i := 0; i < length; i++ {
         node := queue[0]
         levelRet += float64(node.Val)
         if node.Left != nil {
            queue = append(queue, node.Left)
         }
         if node.Right != nil {
            queue = append(queue, node.Right)
         }
         queue = queue[1:]
      }
      ret = append(ret, levelRet/float64(length))
   }

   return ret
}
```