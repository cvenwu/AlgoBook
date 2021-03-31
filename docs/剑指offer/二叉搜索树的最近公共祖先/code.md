# [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)



## 方法一：

思路：
1. 如果根节点为空，并且传入的两个节点的值有一个与我们的根节点的值相等，直接返回root
2. 如果两个节点都比根小，则递归到根节点左侧，若两个节点都比根大，则递归到根节点右侧，否则(说明一个比根节点小一个比根节点的值大)返回根节点
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
	//如果根为空，说明没有公共节点返回nil
	//如果root的值等于p的值或者等于q的值，直接返回root
	//上面两种情况总和之后就是这样写
	if root == nil || root.Val == p.Val || root.Val == q.Val {
		return root
	}

	//如果都比根小
	if root.Val > p.Val && root.Val > q.Val {
		return lowestCommonAncestor(root.Left, p, q)
	} else if root.Val < p.Val && root.Val < q.Val { //如果都比根大
		return lowestCommonAncestor(root.Right, p, q)
	} else { //如果一个比根大，一个比根小
		return root
	}
}

```

**python写法**
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = Nonehttps://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 如果当前根节点为空，直接返回true
        if root is None:
            return None
        # 如果当前节点不为空
        # 1.1 如果p和q都小于root，递归到左子树
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        # 1.2 如果p和q都大于root，递归到右子树
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        # 1.3 如果p和q一个大于等于root一个小于等于q，直接返回root
        else:
            return root
            
```

