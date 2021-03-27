# 二叉树的深度

## 方法一：DFS

```go
func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	//否则返回左右子树的深度的最大值+1
	return max(maxDepth(root.Left), maxDepth(root.Right)) + 1
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}

```