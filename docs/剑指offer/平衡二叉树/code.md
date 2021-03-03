# 平衡二叉树



## 方法一：递归

1. 获取当前节点左右子树的高度差是否小于等于1且大于等于1
2. 再判断当前节点的左右子树是否平衡

该方法在获取高度差之后又要判断左右子树是否平衡，同时遍历了两次左右子树，因此效率不高。

> *// 执行用时：8 ms, 在所有 Go 提交中击败了 88.31% 的用户*
>
> *// 内存消耗：5.7 MB, 在所有 Go 提交中击败了 100.00% 的用户*



```go
func isBalanced(root *TreeNode) bool {
	//需要注意的是空树也是平衡二叉树
	if root == nil {
		return true
	}

	//获取左子树的深度，以及右子树的深度，然后判断差值的绝对值是否在-1 - 1之间
	leftSubTreeDepth, rightSubTreeDepth := maxDepth(root.Left),  maxDepth(root.Right)
	
	val := leftSubTreeDepth - rightSubTreeDepth
	if val >= -1 && val <= 1 {  //说明当前节点是一个平衡
		//此时我们再递归判断左右子树是否都是平衡二叉树
		return isBalanced(root.Left) && isBalanced(root.Right)
	}

	return false

}

func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	
	//此时当前节点的深度为左右子树深度的最大值+1
	leftSubTreeDepth, rightSubTreeDepth := maxDepth(root.Left), maxDepth(root.Right)
	if leftSubTreeDepth > rightSubTreeDepth {
		return leftSubTreeDepth + 1
	}
	return rightSubTreeDepth + 1
}
```







## 方法二：递归

相对于方法一，我们采用了后序遍历，当遍历到当前节点时，我们先判断左右子树是否平衡，在判断子树平衡的时候我们已经获取了左右子树的高度，此时根据差值判断是否平衡。

>*// 执行用时：4 ms, 在所有 Go 提交中击败了 98.69% 的用户*
>
>*// 内存消耗：5.7 MB, 在所有 Go 提交中击败了 44.44% 的用户*

```go
func isBalancedCore(root *TreeNode, height *int) bool {
	//需要注意的是空树也是平衡二叉树
	if root == nil {
		return true
	}

	leftSubTreeDepth, rightSubTreeDepth := 0, 0

	//如果左子树平衡并且右子树平衡。如果都平衡，我们同时获取了左子树和右子树的深度
	if (isBalancedCore(root.Left, &leftSubTreeDepth) && isBalancedCore(root.Right, &rightSubTreeDepth)) {
		diff := leftSubTreeDepth - rightSubTreeDepth
		if diff >= -1 && diff <= 1 {
			if diff > 0 {
				*height = leftSubTreeDepth + 1
			} else {
				*height = rightSubTreeDepth + 1
			}
			return true
		}
	}
	return false
}

func isBalanced(root *TreeNode) bool {
	//需要注意的是空树也是平衡二叉树
	if root == nil {
		return true
	}

	height := 0
	return isBalancedCore(root, &height)

}
```

总结：

相比于方法一，方法二采用了后序遍历的方式，只需要遍历一次左右子树，不需要重复遍历。因此效率会比方法一高。方法一采用了自上向下的方式，包含了大量重复计算。

## 【推荐】方法三

**2021-03-03写法**
```go
type RetType struct {
	isBalanced bool
	depth int
}

func isBalanced(root *TreeNode) bool {
	return isBalancedCore(root).isBalanced
}

func isBalancedCore(root *TreeNode) RetType {
	if root == nil {
		return RetType{
			isBalanced: true,
			depth: 0,
		}
	}

	lRet, rRet := isBalancedCore(root.Left), isBalancedCore(root.Right)

	isBalan := true
	depth := max(lRet.depth, rRet.depth)
	//高度差超过1
	if depth - lRet.depth > 1 || depth - rRet.depth > 1 {
		isBalan = false
	}
	//左右子树高度差小于1并且当前左右子树是平衡的
	return RetType{
		isBalanced: lRet.isBalanced && rRet.isBalanced && isBalan,
		depth: depth+1,
	}
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```