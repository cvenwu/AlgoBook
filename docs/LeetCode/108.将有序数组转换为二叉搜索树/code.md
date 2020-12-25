# [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

## 方法一：递归

思路：每次找到中间节点之后，将其一分为二，之后递归建立左右子树

> 执行用时：4 ms, 在所有 Go 提交中击败了85.06%的用户
> 		内存消耗：4.4 ms, 在所有 Go 提交中击败了100.00%的用户

```go
func sortedArrayToBST(nums []int) *TreeNode {
	if len(nums) <= 0 {
		return nil
	}
	mid := len(nums) >> 1
	root := &TreeNode{
		Val: nums[mid],
	}
	if mid > 0 { //说明有左子树
		root.Left = sortedArrayToBST(nums[:mid])
	}

	if mid < len(nums)-1 { //说明有右子树
		root.Right = sortedArrayToBST(nums[mid+1:])
	}

	return root
}
```

