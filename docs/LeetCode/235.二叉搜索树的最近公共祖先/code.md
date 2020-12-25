# [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

思路：利用二叉搜索树的特性，左子树节点小于等于根，右子树节点大于等于根

1. 如果两个节点的值都大于根，则说明最近公共最先应该往根的右边找
2. 如果两个节点的值都小于根，则说明最近公共最先应该往根的左边找
3. 如果两个节点的值一个大于根，一个小于根，则说明最近公共祖先就是当前的根节点


## 方法一：递归



时间复杂度：O(N)

空间复杂度：O(N)



```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {

	if root == nil {
		return nil
	}

	//如果p,q都在一侧则一定都比roo大或者都比root小
	if p.Val > root.Val && q.Val > root.Val {
		return lowestCommonAncestor(root.Right, p, q)
	} else if p.Val < root.Val && q.Val < root.Val {
		return lowestCommonAncestor(root.Left, p, q)
	} else { //说明位于root的左右两侧
		return root
	}
}
```



## 方法二：迭代

时间复杂度：O(N)

空间复杂度：O(1)



```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	for root != nil {
		//如果p,q都在一侧则一定都比roo大或者都比root小
		if p.Val > root.Val && q.Val > root.Val {
			root = root.Right
		} else if p.Val < root.Val && q.Val < root.Val {
			root = root.Left
		} else { //说明位于root的左右两侧
			return root
		}
	}

	return nil
}
```

