
# [543.二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

> 思路：二叉树直径 = 一颗二叉树的直径长度就是这个树所有节点中左子树深度+右子树深度的最大值

1. 因为最长路径和一定经过某个节点，该节点的左子树的叶子节点就是该路径的一个端点，该节点右子树的叶族节点就是该路径的另一个端点
2. 所以该节点的直径 = 该节点的左子树深度 + 该节点的右子树深度

但是树中所有节点都有可能成为这个节点，于是我们要求树中所有节点的左子树深度+右子树深度的最大值（通过向下递归的时候不断更新这个最大值）

**总结：**
1. 经过某一结点的最长路径为 左子树高度+右子树高度
2. 遍历每一个树中每一个节点，求得其左右子树高度并计算路径，取最大值作为树的直径

## 方法一：

!> 思路：定义一个局部变量，然后求每个节点的深度：因为根节点的最大路径长度一定来自于树中某个节点的最大路径长。之后不断从下向上遍历，遍历的过程中某个节点的最大路径可能是左子树，可能是右子树，也可能经过某个节点，所以我们使用一个变量在遍历的过程中不断更新，这个变量代表了我们目前搜索到的最大路径长度，最后返回这一个变量即可

```go
package main

/**
 * @Author: yirufeng
 * @Date: 2021/1/5 10:29 上午
 * @Desc: 543.二叉树的直径
 **/


/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func diameterOfBinaryTree(root *TreeNode) int {
	if root == nil {
		return 0
	}

	max := 0

	depth(root, &max)
	return max
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func depth(root *TreeNode, maxPath *int) int {
	if root == nil {
		return 0
	}

	lDepth := depth(root.Left, maxPath)
	rDepth := depth(root.Right, maxPath)


	//查看是否可以更新我们的全局最大值
	if *maxPath < lDepth + rDepth {
		*maxPath = lDepth + rDepth
	}

	return max(lDepth, rDepth) + 1
}

```

## 【推荐】方法二：
> 通过函数变量定义一个变量为函数，对代码进行了简化


注意：不可以使用`depth := func......`只能先声明再赋值
```go
func diameterOfBinaryTree(root *TreeNode) int {

	//最终返回的结果
	var ret int

	//不可以使用下一行注释的代码
	//depth := func......

	//只能这样写
	var depth func(root *TreeNode) int
	depth = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		lDepth, rDepth := depth(root.Left), depth(root.Right)

		//动态更新我们要的结果
		if ret < lDepth + rDepth {
			ret = lDepth + rDepth
		}

		//最后返回深度
		if lDepth > rDepth {
			return lDepth+1
		}
		return rDepth+1
	}

	depth(root)
	return ret
}


```