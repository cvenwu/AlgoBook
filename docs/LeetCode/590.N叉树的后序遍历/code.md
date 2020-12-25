# [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)



## 方法一：递归


思路：
1. 递归遍历该节点的子节点，并依次将结果集加入到ret中
2. 将当前节点的值加入到ret中

> 执行用时：4 ms, 在所有 Go 提交中击败了66.50%的用户
> 		内存消耗：5.7 ms, 在所有 Go 提交中击败了29.91%的用户

```go
func postorder(root *Node) []int {
	if root == nil {
		return []int{}
	}

	ret := []int{}


	for _, node := range root.Children {
		ret = append(ret, postorder(node)...)
	}

	ret = append(ret, root.Val)
	return ret
}
```

## 方法二：迭代

