# [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## 方法一：递归的简洁写法

> 执行用时：4 ms, 在所有 Go 提交中击败了96.59%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了29.47%的用户

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) == 0 || len(inorder) == 0 {
		return nil
	}

	//根节点的值
	rv := preorder[0]

	//左子树的长度
	left := 0

	for i := 0; i < len(preorder); i++ {
		if inorder[i] == rv {
			left = i
			break
		}
	}

	return &TreeNode{
		Val: rv,
		Left: buildTree(preorder[1:left+1], inorder[:left]),
		Right: buildTree(preorder[left+1:], inorder[left+1:]),
	}
}

```

## 方法二：递归

> 执行用时：8 ms, 在所有 Go 提交中击败了56.43%的用户
> 		内存消耗：4 MB, 在所有 Go 提交中击败了17.37%的用户

思路：上面的方法每次都会对传入的数组进行切片，而这里我们传递区间的起始值来锁定中序序列以及先序序列所在的区间

- 此时左子树长度为：left-startInorder
  右子树长度为：endInorder-left
- 此时left会指向根
- 此时我们遍历左子树的
  - 先序从startPreorder+1->startPreorder+左子树长度(left-startInorder)
  - 中序从startInorder->left-1
- 此时我们遍历右子树的
  - 先序从startPreorder+左子树长度(left-startInorder)+1->endPreorder
  - 中序从left+1->endInorder

**改进：不对传入的先序与中序序列进行切分**

关键点：**正确计算左右子树的区间起始值**


```go
func buildTree(preorder []int, inorder []int) *TreeNode {

	if len(preorder) == 0 || len(inorder) == 0 || len(preorder) != len(inorder) {
		return nil
	}

	return buildTreeCore(preorder, inorder, 0, len(preorder)-1, 0, len(inorder)-1)
}


func buildTreeCore(preorder []int, inorder []int, startPreorder, endPreorder, startInorder, endInorder int) *TreeNode {
	if startPreorder > endPreorder || startInorder > endInorder {
		return nil
	}

	//找到根节点的值
	rootVal := preorder[startPreorder]

	//找到左子树的长度
	left := startInorder
	for ; left <= endInorder; left++ {
		if inorder[left] == rootVal {
			break
		}
	}

	//建立根节点
	root := &TreeNode{Val: rootVal}


	//建立左子树
	if left > startInorder { //说明有左子树
		root.Left = buildTreeCore(preorder, inorder, startPreorder+1, startPreorder+left-startInorder, startInorder, left-1)
	}

	//建立右子树
	if endInorder-left > 0 { //说明有右子树
		root.Right = buildTreeCore(preorder, inorder, startPreorder+left-startInorder+1, endPreorder, left+1, endInorder)
	}


	return root
}

```

