# [116 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)



## 方法一：BFS遍历进行修改

思路：
1. 每次左子树不空就设置左子树的Next为右子树
2. 每次右子树不空就设置右子树的Next为nil

注意：题目中要求只能使用常数级别的空间复杂度

> 执行用时：4 ms, 在所有 Go 提交中击败了94.43%的用户
> 内存消耗：6.2 MB, 在所有 Go 提交中击败了16.30%的用户

```go
func connect(root *Node) *Node {
	if root == nil {
		return nil
	}

	////设置root的Next指向
	//root.Next = nil
	queue := []*Node{root}

	for len(queue) != 0 {
		length := len(queue)
		for i := 0; i < length; i++ {
			node := queue[0]
			if i < length-1 {
				node.Next = queue[1].Next
			} else {
				node.Next = nil
			}

			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			queue = queue[1:]
		}
	}


	return root
}

```

