# 从上到下打印二叉树



**思路：直接使用BFS 即可**

## 使用container/list

> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.7 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func levelOrder(root *TreeNode) []int {
	//如果根节点为空
	if root == nil {
		return []int{}
	}

	queue := list.New()
	queue.PushBack(root)
	ret := []int{}

	for queue.Front() != nil {
		node := queue.Remove(queue.Front())
		ret = append(ret, node.(*TreeNode).Val)
		if node.(*TreeNode).Left != nil {
			queue.PushBack(node.(*TreeNode).Left)
		}
		if node.(*TreeNode).Right != nil {
			queue.PushBack(node.(*TreeNode).Right)
		}
	}

	return ret
}
```





## 使用slice

> *// 执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户*
>
> *// 内存消耗：2.7 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func levelOrder(root *TreeNode) []int {
	//如果根节点为空
	if root == nil {
		return nil
	}

	queue := []*TreeNode{root}
	ret := []int{}

	for len(queue) != 0 {
		node := queue[0]
		ret = append(ret, node.Val)
		if node.Left != nil {
			queue = append(queue, node.Left)
		}
		if node.Right != nil {
			queue = append(queue, node.Right)
		}
		queue = queue[1:]
	}
	return ret
}
```

