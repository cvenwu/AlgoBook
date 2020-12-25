# [1022. 从根到叶的二进制数之和](https://leetcode-cn.com/problems/sum-of-root-to-leaf-binary-numbers/)

## 方法：DFS + 树的前序遍历

> 具体解法同leetcode-129


> 执行用时：4 ms, 在所有 Go 提交中击败了79.55%的用户
		内存消耗：3.2 MB, 在所有 Go 提交中击败了43.33%的用户


```go
func sumRootToLeaf(root *TreeNode) int {
	if root == nil {
		return 0
	}

	sum := 0
	dfs(root, &sum, 0)
	return sum
}


func dfs(root *TreeNode, sum *int, currentVal int) {

	currentVal = currentVal<<1 + root.Val

	//如果为叶子节点就加入到sum中
	if root.Left == nil && root.Right == nil {
		*sum += currentVal
	}

	//如果左子树不空
	if root.Left != nil {
		dfs(root.Left, sum, currentVal)
	}

	//如果右子树不空
	if root.Right != nil {
		dfs(root.Right, sum, currentVal)
	}
}

```

