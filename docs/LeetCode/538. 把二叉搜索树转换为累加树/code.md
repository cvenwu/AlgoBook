# [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

## 方法一：

思路：采用右-根-左的方式遍历，每次遍历之后返回当前节点以及右边所有节点的值

 **注意：由于我们需要传递参数给左子树，因此必须添加一个参数，不能使用返回值给左子树传递，因为父节点先调用左子树，无法将结果返回给左子树**

> 执行用时：16 ms, 在所有 Go 提交中击败了81.03%的用户
> 		内存消耗：7 ms, 在所有 Go 提交中击败了43.73%的用户

```go

func convertBST(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}
	sum := 0
	postOrderTraverse(root, &sum)
	return root
}


func postOrderTraverse(root *TreeNode, sum *int) {
	if root == nil {
		return
	}

	//返回右子树的和
	postOrderTraverse(root.Right, sum)
	//当前根节点加上右子树的和，同时更新sum为右子树加上根节点的和
	*sum = *sum + root.Val
	root.Val = *sum
	//如何修改左子树的值？
	postOrderTraverse(root.Left, sum)
}
```

