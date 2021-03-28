# 二叉树的镜像

## 【推荐】方法一：递归
>  思路：每次不断反转当前节点的左右子树，直到当前节点为空
**对代码进行改进(2021-03-03)：**
```go
func mirrorTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	root.Left, root.Right = mirrorTree(root.Right), mirrorTree(root.Left)
	return root
}
```

## 方法二：采用先序遍历反转

思路：

1. 采用先序遍历的方式，先弹出队列的队首，然后将当前节点的左右反转

2. 如果当前节点的左边不为空，则将当前节点的左边加入队列

3. 如果当前节点的右边不为空，则将当前节点的右边加入队列

> *// 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户*
>
> *// 内存消耗：2.1 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func mirrorTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	queue := []*TreeNode{}

	queue = append(queue, root)

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		//反转node的左右
		node.Left, node.Right = node.Right, node.Left
		//如果左不为空，则加入队列
		if node.Left != nil {
			queue = append(queue, node.Left)
		}
		//如果右不为空，则加入队列
		if node.Right != nil {
			queue = append(queue, node.Right)
		}
	}
	return root
}
```

