



# [111 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

## 方法一：递归

思路：从上到下递归
	**提交后报错：输入[1, 2]，我们输出1，预期输出2**

```go
func minDepth(root *TreeNode) int {
   if root == nil {
      return 0
   }

   //返回左右子树最小的深度值+1
   return min(minDepth(root.Left), minDepth(root.Right)) + 1
}


func min(num1, num2 int) int{
   if num1 < num2 {
      return num1
   }

   return num2
}
```

## 方法一修正：修改方法一

**注意：如果某个节点只有左叶子节点，那么该节点的深度为2而不是1。**
   因为要的是从根节点到叶子节点的最小深度
   因此我们要加上前面两个判断左右子树的条件

> 执行用时：8 ms, 在所有 Go 提交中击败了74.02%的用户
> 		内存消耗：5.3 ms, 在所有 Go 提交中击败了35.42%的用户

```go

func minDepth(root *TreeNode) int {
   if root == nil {
      return 0
   }

   if root.Left == nil {
      return minDepth(root.Right) + 1
   }

   if root.Right == nil {
      return minDepth(root.Left) + 1
   }

   //返回左右子树最小的深度值+1
   //这里不能直接这样写，因为如果某个节点只有左叶子节点，那么该节点的深度为2而不是1
   //因为要的是从根节点到叶子节点的最小深度
   //因此我们要加上前面两个判断左右子树的条件
   return min(minDepth(root.Left), minDepth(root.Right)) + 1
}


func min(num1, num2 int) int{
   if num1 < num2 {
      return num1
   }

   return num2
}
```

