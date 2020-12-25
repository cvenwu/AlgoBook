# [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

## 方法一：DFS+前序遍历


思路：
1. 如果有左右子树就加上->，之后递归到左右子树，递归回来之后记得去掉->
2. 如果没有左右子树，直接将path加入到ret中


> 执行用时：0ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.4 MB, 在所有 Go 提交中击败了59.58%的用户

```go
func binaryTreePaths(root *TreeNode) []string {
	if root == nil {
		return nil
	}
	ret := []string{}
	path := ""
	dfs(root, &ret, path)
	return ret
}

func dfs(root *TreeNode, ret *[]string, path string) {
	//如果有左右子树就加上—>，之后递归到左右子树
	//如果没有左右子树，直接将path加入到ret中
	path = path + strconv.Itoa(root.Val)
	if root.Left == nil && root.Right == nil {
		*ret = append(*ret, path)
	}

	if root.Left != nil {
		path = path + "->"
		dfs(root.Left, ret, path)
		path = path[:len(path)-2]
	}

	if root.Right != nil {
		path = path + "->"
		dfs(root.Right, ret, path)
		path = path[:len(path)-2]
	}
}
```

## 方法二：对方法一的代码优化

> 执行用时：0ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.4 MB, 在所有 Go 提交中击败了43.92%的用户

```go
func binaryTreePaths(root *TreeNode) []string {
	if root == nil {
		return nil
	}
	ret := []string{}
	path := ""
	dfs(root, &ret, path)
	return ret
}

func dfs(root *TreeNode, ret *[]string, path string) {
	//如果有左右子树就加上—>，之后递归到左右子树
	//如果没有左右子树，直接将path加入到ret中
	path = path + strconv.Itoa(root.Val)
	if root.Left == nil && root.Right == nil {
		*ret = append(*ret, path)
	}
	path = path + "->"

	if root.Left != nil {
		dfs(root.Left, ret, path)
	}

	if root.Right != nil {
		dfs(root.Right, ret, path)
	}
}
```

