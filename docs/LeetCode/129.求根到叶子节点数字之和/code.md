# [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

## 方法：DFS+前序遍历

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.5 MB, 在所有 Go 提交中击败了18.80%的用户


```go
func sumNumbers(root *TreeNode) int {
	//之前提交的错误点：注意输入为nil
	if root == nil {
		return 0
	}

	sum := 0
	dfs(root, &sum, 0)
	return sum
}

//第2个值存储所有到叶子路径的和，也就是最终结果
//第3个值代表当前的值
func dfs(root *TreeNode, sum *int, currentVal int) {
	currentVal = currentVal * 10 + root.Val

	//回溯结束条件：如果当前节点为叶子节点
	if root.Left == nil && root.Right == nil {
		*sum += currentVal
		fmt.Println(currentVal)
	}

	//如果当前节点的左子树不空
	if root.Left != nil {
		dfs(root.Left, sum, currentVal)
	}
	//如果当前节点的右子树不空
	if root.Right != nil {
		dfs(root.Right, sum, currentVal)
	}
}
```

