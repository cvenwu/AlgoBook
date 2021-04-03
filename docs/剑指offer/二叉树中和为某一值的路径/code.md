# 二叉树中和为某一值的路径

## 方法一：回溯
!> 题目中的两个坑：
1. 目标值可能为负数
2. 打印的是根节点到叶子节点的路径，如果target为4，并且树为如下，那么最终返回的结果只有[[2,3,-1]]并没有[2,2]因为最后一个2不是叶节点
```
				2
		3				2
	-1				1
```

**backtrack函数过程如下：**
1. 首先检查root是否为空，若为空直接返回，**注意：这里不检测target < 0或root.Val<target，因为目标值可能为负数，并且节点值也可能为负数**
2. 将当前节点加入路径（此时root不可能为空，因为我们第一步已经判断了）
3. 如果当前节点没有左右子树并且当前节点的值与我们的目标值一致，则将路径加入我们的结果中
4. 继续遍历左子树以及右子树
5. 到达这里说明左右子树符合条件的路径已经打印完成，此时我们从路径中去除当前节点并且将和减去当前节点的值
>*// 执行用时：4 ms, 在所有 Go 提交中击败了 92.53% 的用户*
>
>*// 内存消耗：4.5 MB, 在所有 Go 提交中击败了 87.96% 的用户*

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func pathSum(root *TreeNode, target int) [][]int {
	var ret [][]int
	backtrack(root, target, []int{}, &ret)
	return ret
}

func backtrack(root *TreeNode, target int, path []int, ret *[][]int) {
	if root == nil {
		return
	}

	path = append(path, root.Val)

	//如果root左右节点都没并且root的值等于target,则将加入ret
	if root.Left == nil && root.Right == nil && root.Val == target {
		temp := make([]int, len(path))
		copy(temp, path)
		*ret = append(*ret, temp)
		return
	}

	backtrack(root.Left, target - root.Val, path, ret)
	backtrack(root.Right, target - root.Val, path, ret)
	path = path[:len(path)-1]
}

```





