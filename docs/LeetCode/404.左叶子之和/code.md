# [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)



## 方法一：递归

思路：遍历当前节点的左右子树，之后如果当前节点的左节点是左叶子节点就加上

>执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
			内存消耗：2.7 ms, 在所有 Go 提交中击败了53.85%的用户

```go
func sumOfLeftLeaves(root *TreeNode) int {
	if root == nil {
		return 0
	}

	sum := 0
	//遍历左右子树
	sum += sumOfLeftLeaves(root.Left)
	sum += sumOfLeftLeaves(root.Right)

	//如果root的左结点为叶子节点
	if root.Left != nil && root.Left.Left == nil && root.Left.Right == nil {
		sum += root.Left.Val
	}

	return sum
}
```

