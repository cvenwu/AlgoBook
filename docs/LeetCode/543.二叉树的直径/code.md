
# [543.二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

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