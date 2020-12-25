





# [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

## 方法一：递归合并

思路：从上到下进行合并

> 执行用时：20 ms, 在所有 Go 提交中击败了78.92%的用户
> 		内存消耗：6.3 ms, 在所有 Go 提交中击败了72.63%的用户

```go
func mergeTrees(t1 *TreeNode, t2 *TreeNode) *TreeNode {
	if t1 == nil {
		return t2
	}

	if t2 == nil {
		return t1
	}

	//如果两个树都存在，那么合并到t1上
	t1.Val += t2.Val

	t1.Left = mergeTrees(t1.Left, t2.Left)
	t1.Right = mergeTrees(t1.Right, t2.Right)


	return t1
}
```

