# [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

## 方法：dfs

思路：题目中说明了树中节点值唯一，并且两个节点都存在于树中

1. 如果当前节点的左右子树都不空，直接返回当前节点
2. 如果左子树为空，直接返回右节点
3. 如果右子树为空，直接返回左节点

关键点：

1. 如果当前的节点为左节点或右节点，我们直接返回当前节点。

​		2. 如果当前节点为空，直接返回当前节点

小问题：自己之前很不理解的就是如果两个节点都在当前节点的左子树最下方，那么这个函数不是返回当前节点么？
		解释：首先如果两个节点都在当前节点的孙子节点，那么返回的是当前节点的儿子节点而不是孙子节点，因为我们使用dfs一层一层遍历，当遍历到孙子那层返回两个节点，但是之后递归回到了当前节点的孩子那一层，此时发现左右两个子树节点满足条件，所以返回当前节点的孩子节点，也就是最终答案楼

> 执行用时：16 ms, 在所有 Go 提交中击败了 63.91% 的用户
> 		内存消耗：7.6 MB, 在所有 Go 提交中击败了 17.10% 的用户

### Go解法

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil || root == p || root == q {
		return root
	}

	//递归遍历到左子树
	left := lowestCommonAncestor(root.Left, p, q)
	//递归遍历到右子树
	right := lowestCommonAncestor(root.Right, p, q)

	//如果左边的不空
	if left != nil {
		if right != nil { //说明左节点在左边，右节点在右边
			return root
		}
		//说明左子树那边不空(且此时left为其中的一个节点)，右子树那边没有p和q
		return left
	}

	//	
	//原因：题目说明了两个节点都出现了并且树中所有节点的值都不相同
	return right
}
```

!> **2021-03-30改进**
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	if root == nil || root.Val == p.Val || root.Val == q.Val {
		return root
	}

	//这个函数表示两层意思 
	//第一层意思： p，q的最近公共祖先
	//第二层意思： p，q出现的那个节点
	l, r := lowestCommonAncestor(root.Left, p, q), lowestCommonAncestor(root.Right, p, q)

	//如果左侧为空，说明右侧不为空，说明右侧已经找到了最近公共祖先直接返回
	if l == nil {
		return r
	} else if r == nil { //如果右侧为空，说明左侧不为空，说明左侧已经找到了最近公共祖先直接返回
		return l
	} else { //左右两侧都不为空，说明一个在左，一个在右，直接返回当前根
		return root
	}
}
```


### python解法

```python
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if root is None or root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left is not None:
            if right is not None:
                return root
            return left
        return right
```

