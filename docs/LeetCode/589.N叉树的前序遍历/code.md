# [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

## 方法一：递归

> 执行用时：4 ms, 在所有 Go 提交中击败了59.25%的用户
> 		内存消耗：5.8 ms, 在所有 Go 提交中击败了17.95%的用户

```go
func preorder(root *Node) []int {
	if root == nil {
		return []int{}
	}

	ret := []int{root.Val}


	for _, node := range root.Children {
		ret = append(ret, preorder(node)...)
	}


	return ret
}


```

## 方法二：迭代

思路：写了二叉树的非递归前序遍历后，其实这个题目类似于二叉树的非递归前序遍历，因此参照写就可以了

注意：我们遍历当前节点子树的时候，需要先从右边开始遍历，然后再遍历到左边，所以需要逆序遍历，而正好root.Children是一个切片，我们可以获取长度逆序遍历

> 虽然较难理解，但其实只要写好了二叉树的非递归前序遍历就知道如何进行N叉树的非递归前序遍历了



> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
			内存消耗：3.9 ms, 在所有 Go 提交中击败了86.36%的用户

```go
func preorder(root *Node) []int {
	if root == nil {
		return []int{}
	}

	ret := []int{}
	stack := []*Node{root}

	for len(stack) != 0 {
		root = stack[len(stack)-1]
		ret = append(ret, root.Val)
		stack = stack[:len(stack)-1]

		for i := len(root.Children)-1; i >= 0; i-- {
			stack = append(stack, root.Children[i])
		}
	}


	return ret
}
```

