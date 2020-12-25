



# [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)



## 方法：DFS+前序遍历

> 执行用时：4 ms, 在所有 Go 提交中击败了94.63%的用户
> 		内存消耗：4.6 MB, 在所有 Go 提交中击败了38.30%的用户

```go
func pathSum(root *TreeNode, sum int) [][]int {
	if root == nil {
		return nil
	}

	ret := [][]int{}
	path := []int{}
	dfs(root, &ret, &path, sum)
	return ret
}

func dfs(root *TreeNode, ret *[][]int, path *[]int, sum int)  {
	*path = append(*path, root.Val)

	//加入的条件，到了叶子节点并且叶子节点的值等于sum
	if sum == root.Val && root.Left == nil && root.Right == nil {
		temp := make([]int, len(*path))
		copy(temp, *path)
		*ret = append(*ret, temp)
		return
	}

	if root.Left != nil {
		dfs(root.Left, ret, path, sum-root.Val)
		*path = (*path)[:len(*path)-1]
	}

	if root.Right != nil {
		dfs(root.Right, ret, path, sum-root.Val)
		*path = (*path)[:len(*path)-1]
	}
}

```

