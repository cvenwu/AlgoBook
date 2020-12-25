# 226 翻转二叉树



## 方法一：从上到下递归反转

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 内存消耗：2.1 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	root.Left, root.Right = root.Right, root.Left
	invertTree(root.Left)
	invertTree(root.Right)


	return root
}
```



## 方法二：从下到上递归反转



> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了65.09%的用户






```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	root.Left, root.Right = invertTree(root.Right), invertTree(root.Left)


	return root
}
```

## 总结

从上到下与从下到上的区别就是：从上到下，先执行操作，最后递归调用即可。而从下到上则是先递归到下面，执行操作之后，需要上面接收下面执行操作后的返回值。例如上面的翻转二叉树，从下到上则是先翻转左右子树，之后采用当前节点的两个节点接收后进行翻转。