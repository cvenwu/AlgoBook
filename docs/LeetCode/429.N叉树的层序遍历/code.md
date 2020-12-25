# [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

## 方法一：直接使用BFS进行遍历


>执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
>		内存消耗：4.3 ms, 在所有 Go 提交中击败了53.03%的用户

```go

func levelOrder(root *Node) [][]int {
	if root == nil {
		return [][]int{}
	}

	queue := []*Node{root}
	ret := [][]int{}
	for len(queue) != 0 {
		length := len(queue)
		levelRet := []int{}
		for i := 0; i < length; i++ {
			node := queue[0]
			queue = queue[1:]
			levelRet = append(levelRet, node.Val)
			for _, child := range node.Children {
				queue = append(queue, child)
			}
		}
		ret = append(ret, levelRet)
	}

	return ret
}

```

