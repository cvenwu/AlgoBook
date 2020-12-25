# [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## 方法一：递归

思路：直接找到根，划分为左右子树，递归建立左右子树即可。不需要将原来的中序与后序的数组修改，

> 执行用时：8 ms, 在所有 Go 提交中击败了56.82%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了15.69%的用户

```go
func buildTree(inorder []int, postorder []int) *TreeNode {
	//说明节点非法，不需要构造二叉树
	if len(inorder) == 0 || len(postorder) == 0 || len(postorder) != len(inorder) {
		return nil
	}

	return buildTreeCore(inorder, postorder, 0, len(inorder)-1, 0, len(postorder)-1)

}

func buildTreeCore(inorder []int, postorder []int, startInorder, endInorder, startPostorder, endPostorder int) *TreeNode {
	if startInorder > endInorder || startPostorder > endPostorder {
		return nil
	}

	//找到根节点
	root := &TreeNode{Val: postorder[endPostorder]}

	//左子树的长度
	leftLength := 0
	for i := startInorder; i <= endInorder; i++ {
		if inorder[i] == root.Val {
			leftLength = i - startInorder
			break
		}
	}

	rightLength := endInorder - startInorder - leftLength

	//如果有左子树
	if leftLength > 0 {
		root.Left = buildTreeCore(inorder, postorder, startInorder, startInorder+leftLength-1,
			startPostorder, startPostorder+leftLength-1)
	}

	//如果有右子树
	if rightLength > 0 {
		root.Right = buildTreeCore(inorder, postorder, startInorder+leftLength+1, endInorder,
			startPostorder+leftLength, endPostorder-1)
	}

	return root
}


```

## 方法二：参考LeetCode其他解法

思路：直接找到根，划分为左右子树，递归建立左右子树即可

[参考](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

> 执行用时：4 ms, 在所有 Go 提交中击败了96.59%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了29.41%的用户

```go
func buildTree(inorder []int, postorder []int) *TreeNode {
	if len(inorder) == 0 || len(postorder) == 0 {
		return nil
	}


	//根节点的值
	rv := postorder[len(postorder)-1]

	//用来记录左子树长度
	left := 0

	//找到左子树
	for i := 0; i < len(inorder); i++ {
		if rv == inorder[i] {
			left = i
			break
		}
	}

	return &TreeNode{
		Val: rv,
		Left: buildTree(inorder[:left], postorder[:left]),
		Right: buildTree(inorder[left+1:], postorder[left:len(postorder)-1]),
	}
}
```

