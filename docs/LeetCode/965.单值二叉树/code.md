# [965. 单值二叉树](https://leetcode-cn.com/problems/univalued-binary-tree/)





## 方法一：递归遍历

思路：

满足以下条件就不是单值二叉树
      1. 左子树存在，左子树不是单值二叉树
      2. 左子树是单值二叉树，但是左子树的根与根节点值不同
      3. 右子树存在，右子树不是单值二叉树
      4. 右子树是单值二叉树，但是右子树的根与根节点值不同

>   执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
>    		内存消耗：2.3 ms, 在所有 Go 提交中击败了92.86%的用户


```go
func isUnivalTree(root *TreeNode) bool {
   if root == nil {
      return true
   }

   if (root.Left != nil && ((root.Val != root.Left.Val) ||
      !isUnivalTree(root.Left))) ||
      (root.Right != nil && ((root.Val != root.Right.Val) ||
         !isUnivalTree(root.Right))){
      return false
   }
   return true
}
```

