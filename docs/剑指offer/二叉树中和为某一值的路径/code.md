# 二叉树中和为某一值的路径



思路：关键点。采用先序遍历遍历树，同时记录路径，用来帮助我们返回

1. 首先路径中加入当前节点，并将当前节点的值加入到和中
2. 如果当前节点没有左右子树并且和与我们的目标值一致，则打印路径到返回的结果中
3. 如果有左子树，则继续遍历左子树
4. 如果有右子树，则继续遍历右子树，
5. 到达这里说明左右子树符合条件的路径已经打印完成，此时我们从路径中去除当前节点并且将和减去当前节点的值



>*// 执行用时：4 ms, 在所有 Go 提交中击败了 92.53% 的用户*
>
>*// 内存消耗：4.5 MB, 在所有 Go 提交中击败了 79.49% 的用户*

```go
func pathSum(root *TreeNode, sum int) [][]int {
	//因为传入的节点值可能为负数并且最后求的和也会是负数，所以不需要判断sum <= 0
	if root == nil {
		return nil
	}

	ret, path := [][]int{}, []*TreeNode{}
	pathSumCore(root, sum, 0, &path, &ret)
	return ret
}

func pathSumCore(root *TreeNode, sum, currentSum int, path *[]*TreeNode, ret *[][]int) {
	//路径中加入当前节点
	*path = append(*path, root)
	//当前和加入当前节点的值
	currentSum += root.Val

	//如果到达了叶子节点且当前和等于我们的目标则打印到结果中
	if root.Left == nil && root.Right == nil && currentSum == sum {
		temp := []int{}
		for _, node := range *path {
			temp = append(temp, node.Val)
		}
		*ret = append(*ret, temp)
	}
	//如果左子树不为空
	if root.Left != nil {
		pathSumCore(root.Left, sum, currentSum, path, ret)
	}

	//如果右子树不为空
	if root.Right != nil {
		pathSumCore(root.Right, sum, currentSum, path, ret)
	}

	//到达这里说明左右子树都遍历完，此时需要剔除该节点
	*path = (*path)[:len(*path)-1]
	currentSum -= root.Val

}

```





