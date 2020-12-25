# [515. 在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

## 方法一：BFS

> 执行用时：8 ms, 在所有 Go 提交中击败了80.00%的用户
> 		内存消耗：6.3 MB, 在所有 Go 提交中击败了7.46%的用户

```go
func largestValues(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	ret := []int{}
	queue := []*TreeNode{root}
	for len(queue) != 0 {
		max := queue[0].Val
		currentLevelLength := len(queue)
		for currentLevelLength > 0 {
			node := queue[0]
			queue = queue[1:]
			currentLevelLength -= 1
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			if max < node.Val {
				max = node.Val
			}
		}
		ret = append(ret, max)
	}
	return ret
}
```

## 方法二：DFS

> 执行用时：4 ms, 在所有 Go 提交中击败了99.02%的用户
> 		内存消耗：6.1 MB, 在所有 Go 提交中击败了32.84%的用户

```go

func largestValues(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	ret := []int{}
	dfs(root, 1, &ret)

	return ret
}

func dfs(root *TreeNode, level int, ret *[]int) {
	if len(*ret) < level {  //如果当前层没有记录
		*ret = append(*ret, root.Val)
	} else {	//如果当前层有记录，与记录比，更新最大值
		if root.Val > (*ret)[level-1] {
			(*ret)[level-1] = root.Val
		}
	}

	if root.Left != nil {
		dfs(root.Left, level+1, ret)
	}

	if root.Right != nil {
		dfs(root.Right, level+1, ret)
	}
}

```

