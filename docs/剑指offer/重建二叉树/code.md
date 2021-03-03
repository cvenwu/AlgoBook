# 重建二叉树

## 方法一：递归建立左右子二叉树


```go
type TreeNode struct {
	Val         int
	Left, Right *TreeNode
}

func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) != len(inorder) || len(preorder) == 0 || len(inorder) == 0 {
		return nil
	}
	return buildTreeCore(preorder, inorder, 0, len(preorder)-1, 0, len(inorder)-1)
}

func buildTreeCore(preorder []int, inorder []int, pStart, pEnd, inStart, inEnd int) *TreeNode {
	if pStart > pEnd || inStart > inEnd {
		return nil
	}

	//找到根的值
	rootVal := preorder[pStart]

	//找到左子树的长度
	l := 0
	for i := inStart; i <= inEnd; i++ {
		if inorder[i] == rootVal {
			l = i - 1
			break
		}
	}
	//返回当前建立的根
	return &TreeNode{
		Val:   rootVal,
		Left:  buildTreeCore(preorder, inorder, pStart+1, pStart+1+l-inStart, inStart, l),
		Right: buildTreeCore(preorder, inorder, pStart+1+l-inStart+1, pEnd, l+2, inEnd),
	}
}

```

## 【推荐】方法二：直接使用当前函数递归建立

```go
//方法二：直接使用当前函数
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) != len(inorder) || len(preorder) == 0 {
		return nil
	}

	rootVal := preorder[0]
	l := 0
	for i := 0; i < len(inorder); i++ {
		if inorder[i] == rootVal {
			l = i-1
			break
		}
	}

	return &TreeNode{
		Val: rootVal,
		Left: buildTree(preorder[1:l+2], inorder[:l+1]),
		Right: buildTree(preorder[l+2:], inorder[l+2:]),
	}

}
```