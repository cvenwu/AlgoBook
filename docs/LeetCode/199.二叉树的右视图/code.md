# [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

## 方法：BFS

思路：层次遍历之后返回每一层最后一个元素

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 ms, 在所有 Go 提交中击败了28.44%的用户

```go

func rightSideView(root *TreeNode) []int {
	if root == nil {
		return nil
	}
	ret := []int{}
	levelTraverse(root, &ret)
	return ret
}

func levelTraverse(root *TreeNode, ret *[]int) {

	queue := []*TreeNode{root}
	for len(queue) > 0 {
		length := len(queue)
		for i := 0; i < length; i++ {
			node := queue[0]
			queue = queue[1:]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			if i == length-1 {
				*ret = append(*ret, root.Val)
			}
		}
	}
}
```

